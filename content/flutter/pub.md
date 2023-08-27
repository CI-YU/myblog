---
title: "Pub"
date: 2023-05-28T14:30:29+08:00
draft: false
tags: ["flutter"]
---
# 快速介紹flutter安裝的函式庫

### intl
轉成指定地區的日期格式

> flutter pub add intl

### uuid
建立guid

> flutter pub add uuid

### transparent_image
當圖還沒有載入時可以有透明背景圖使用

> flutter pub add transparent_image

### riverpod
是一個狀態管理的工具，當開始寫flutter就會遇到跟現在流行的框架vue/react一樣的問題，當我要傳遞資料時要一直往子層傳參數，才能將資料傳進去，這時`riverpod`就是來解決這個問題的
另一個社群討論度高的是[Getx](https://pub.dev/packages/get)。

> flutter pub add riverpod

1. 要在需要的區塊上加上`ProviderScope`，下列範例就是所有區塊都要可以使用
```dart
void main() {
  runApp(const ProviderScope(
    child: MyApp(),
  ));
}
```
2. 新增一個檔案並建立供應方
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';

final demoProvider = Provider((ref) {
  return false;
});
```
3. ConsumerStatefulWidget & ConsumerWidget

StatelessWidget改成ConsumerWidget

StatefulWidget改成ConsumerStatefulWidget

4. 建立接收方
```dart
//在要使用資料的地方使用下列
//當資料不會改變時可以使用read來取代watch，但官方建議一律使用watch，以免當資料異動時，沒有修改到
final demo = ref.watch(demoProvider);
//接下來即可使用demo來做後續事情
```
### http
進行restful api，透過這個套件可以發送請求到api並接收回傳

> flutter pub add http

### image_picker
使用系統得相機功能，在ios中記得查看文件檔，將key放入文件所說明的plist檔案中

> flutter pub add image_picker

### location
取得使用者當下位置座標

> flutter location
