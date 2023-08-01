---
title: "Widget"
date: 2023-05-28T13:26:51+08:00
draft: false
tags: ["flutter"]
---

## 佈局的屬性
### Column
由上至下的排序
### Row
由左至右的排序
### Expanded
只能使用在繼承Flex以下的Widget，常使用在``Cloumn``或``row``的子項

因為有些widget不會限制長寬，要有expanded來將最大值限制在手機區塊內

### ListView
使用ListView是可以有滾動效果
可以透過builder提升效能
使用方法為
```flutter
ListView.builder(item: list.length, itemBuilder: (ctx,index){list[index].title})
```

### Spacer
可以在任何row,colume使用
佔用剩餘所有空間的意思

### TextField
輸入框

要記得關閉
### DropdownButton
下拉式選單
### Dismissable
key參數必要，才能確定要滑動刪除哪筆資料
也可以分為向左滑動與向右滑動，執行不同功能
滑動刪除效果
### Snackbar
會在螢幕下面調出一個訊息框
在Scaffold階層底下可以使用
```flutter
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        duration: Duration(seconds: 3),
        content: Text('Expense deleted'),
      ),
    );
```
### InkWell
可以讓沒有事件的元件增加點擊事件之類的功能
與GestureDetector的差別在於
`GestureDetector`提供更多的點擊事件，`InkWell`提供點擊特效

[兩者差異出處](https://stackoverflow.com/questions/56725308/flutter-inkwell-vs-gesturedetector-what-is-the-difference)

### Stack
類似`Column`與`Row`，但是可以讓所有子元件重疊在一起

### SwitchListTile
有大標題與子標題的開關

後續慢慢補上