---
title: "Dapper PostgreSQL Error"
date: 2023-11-24T22:09:49+08:00
draft: false
tags: ["dotnet6","PostgreSQL"]
---
# 問題
當使用dapper呼叫postgresql時後,需要在in裡面查詢一批陣列會遇到下方的錯誤訊息
```C#
var Ids = [1,2,3,4];
DynamicParameters parameters = new();
parameters.Add("Ids", Ids);
var strSQL = "";
strSQL +="select * from Users where id in @Ids;"
await dapper.QueryAsync(strSQL,parameters);
```
> 42601: syntax error at or near "$1"\r\n\r\nPOSITION: 81
# 解法
改語法為any就可以解決這個問題。
```C#
var Ids = [1,2,3,4];
DynamicParameters parameters = new();
parameters.Add("Ids", Ids);
var strSQL = "";
strSQL +="select * from Users where id = any(@Ids);"
await dapper.QueryAsync(strSQL,parameters);

```
