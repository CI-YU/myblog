---
title: "Opencc Memory Leak"
date: 2024-05-19T14:54:23+08:00
draft: false
tags: ["dotnet","記憶體洩漏","memoryleak"]
---
近期專案上有個需求,需要使用opencc來將接收到的資料將簡體轉繁體,參考了will保哥與黑暗執行緒的範例,其中保哥的文章指出有做**防止記憶體洩漏**的調整,但上線後發現一樣有記憶體洩漏的問題,最後找到的解決方案是,應該要使用opencc的函式做記憶體釋放,而非使用C#的函式。

## 調整前
```c#
using System.Runtime.InteropServices;
using System.Text;

public static class OpenCCHelper
{
    [DllImport(@"C:\Tools\opencc\bin\opencc.dll", EntryPoint = "opencc_open")]
    private static extern IntPtr opencc_open(string configFileName);

    [DllImport(@"C:\Tools\opencc\bin\opencc.dll", EntryPoint = "opencc_convert_utf8")]
    private static extern IntPtr opencc_convert_utf8(IntPtr opencc, IntPtr input, long length);

    public static string ConvertFromSimplifiedToTraditional(this string text, string config = "s2t")
    {
        return OpenCC(text, config: config);
    }

    public static string ConvertFromSimplifiedToTraditionalTaiwan(this string text, string config = "s2twp")
    {
        return OpenCC(text, config: config);
    }

    public static string ConvertFromTraditionalTaiwanToSimplified(this string text, string config = "tw2sp")
    {
        return OpenCC(text, config: config);
    }

    public static string ConvertFromTraditionalToSimplified(this string text, string config = "t2s")
    {
        return OpenCC(text, config: config);
    }

    public static string OpenCC(this string text, string config)
    {
        var configFile = @"C:\Tools\OpenCC\share\opencc\" + config + ".json";
        if (!File.Exists(configFile))
        {
            throw new FileNotFoundException("設定檔找不到", configFile);
        }

        IntPtr opencc = opencc_open(configFile);
        try
        {
            int len = Encoding.UTF8.GetByteCount(text);
            byte[] buffer = new byte[len + 1];
            Encoding.UTF8.GetBytes(text, 0, text.Length, buffer, 0);
            IntPtr inStr = Marshal.AllocHGlobal(buffer.Length);
            try
            {
                Marshal.Copy(buffer, 0, inStr, buffer.Length);
                IntPtr outStr = opencc_convert_utf8(opencc, inStr, -1);
                try
                {
                    int outLen = 0;
                    while (Marshal.ReadByte(outStr, outLen) != 0) ++outLen;
                    byte[] outBuffer = new byte[outLen];
                    Marshal.Copy(outStr, outBuffer, 0, outBuffer.Length);
                    return Encoding.UTF8.GetString(outBuffer);
                }
                finally
                {
                    Marshal.FreeHGlobal(outStr);
                }
            }
            finally
            {
                Marshal.FreeHGlobal(inStr);
            }
        }
        finally
        {
            Marshal.FreeHGlobal(opencc);
        }
    }
}
```

## 調整後
```C#
using System.Runtime.InteropServices;
using System.Text;

public static class OpenCCHelper
{
    [DllImport(@"C:\Tools\opencc\bin\opencc.dll", EntryPoint = "opencc_open")]
    private static extern IntPtr opencc_open(string configFileName);
    //傳入參數做調整
    [DllImport(@"C:\Tools\opencc\bin\opencc.dll", EntryPoint = "opencc_convert_utf8")]
    private static extern IntPtr opencc_convert_utf8(IntPtr opencc, IntPtr input, UIntPtr length);
    //新增
    [DllImport(@"C:\Tools\opencc\bin\opencc.dll", EntryPoint = "opencc_convert_utf8_free")]
    private static extern IntPtr opencc_convert_utf8_free(IntPtr str);
    //新增
    [DllImport(@"C:\Tools\opencc\bin\opencc.dll", EntryPoint = "opencc_close")]
    private static extern IntPtr opencc_close(IntPtr opencc);

    public static string ConvertFromSimplifiedToTraditional(this string text, string config = "s2t")
    {
        return OpenCC(text, config: config);
    }

    public static string ConvertFromSimplifiedToTraditionalTaiwan(this string text, string config = "s2twp")
    {
        return OpenCC(text, config: config);
    }

    public static string ConvertFromTraditionalTaiwanToSimplified(this string text, string config = "tw2sp")
    {
        return OpenCC(text, config: config);
    }

    public static string ConvertFromTraditionalToSimplified(this string text, string config = "t2s")
    {
        return OpenCC(text, config: config);
    }

    public static string OpenCC(this string text, string config)
    {
        var configFile = @"opencc\\" + config + ".json";
        if (!File.Exists(configFile))
        {
            throw new FileNotFoundException("設定檔找不到", configFile);
        }
        IntPtr opencc = opencc_open(configFile);
        try
        {
            IntPtr inputPtr = Marshal.StringToCoTaskMemUTF8(text);
            try
            {
                var input = text.ToCharArray();
                UIntPtr length = new((uint)Encoding.UTF8.GetByteCount(input));

                IntPtr resultPtr = opencc_convert_utf8(opencc, inputPtr, length);
                try
                {
                    string? result = Marshal.PtrToStringUTF8(resultPtr);
                    return result!;
                }
                finally
                {
                    //改為使用Opencc內建的釋放資源函數
                    opencc_convert_utf8_free(resultPtr);
                }
            }
            finally
            {
                Marshal.FreeCoTaskMem(inputPtr);
            }
        }
        finally
        {
            //改為使用Opencc內建的釋放資源函數
            opencc_close(opencc);
        }
    }
}
```
## 最終結果
改善前
![改善前](https://github.com/CI-YU/CI-YU/assets/19286751/3dfcbfa4-bbec-4270-99be-38c136333a58)
改善後
![改善後](https://github.com/CI-YU/CI-YU/assets/19286751/0c620807-5f79-48a6-aa1b-434c070f486f)

## 相關連結

OpenCC

[GitHub Repo](https://github.com/BYVoid/OpenCC)

黑暗執行緒

[OpenCC 中文繁簡體轉換工具](https://blog.darkthread.net/blog/opencc-notes-1/)

[使用 C# 整合 OpenCC 執行中文繁簡轉換](https://blog.darkthread.net/blog/call-opencc-with-csharp/)

Will保哥

[如何在 Windows 打造 OpenCC 中文繁簡轉換工具](https://blog.miniasp.com/post/2024/04/07/Useful-tool-OpenCC)
