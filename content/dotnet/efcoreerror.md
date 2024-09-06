---
title: "dotnet ef Error"
date: 2024-09-06T23:23:56+08:00
draft: false
tags: ["dotnet8", "EFCore"]
---

當使用者使用 dotnet cli 輸入 ef core 指令時會遇到下列錯誤訊息時，肯定會很納悶為什麼會錯誤。

> dotnet ef 指令需先透過<font color=#800000>dotnet tool install --global dotnet-ef</font>進行全域安裝。

```
Your startup project 'MicroRabbit.Banking.Api' doesn't reference Microsoft.EntityFrameworkCore.Design. This package is required for the Entity Framework Core Tools to work. Ensure your startup project is correct, install the package, and try again.
```

找到執行專案的 csproj 檔案，將 Microsoft.EntityFrameworkCore.Design 的 PrivateAssets 註解掉，再重新執行 dotnet ef migrations add 就可以執行了

```c#
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="8.0.8">
      <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
      <!-- <PrivateAssets>all</PrivateAssets> -->
    </PackageReference>
```

## 參考

[stackoverflow](https://stackoverflow.com/questions/52536588/your-startup-project-doesnt-reference-microsoft-entityframeworkcore-design)
