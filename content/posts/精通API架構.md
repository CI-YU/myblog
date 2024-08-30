---
title: "精通API架構"
date: 2024-07-13T11:45:36+08:00
draft: true
tags: ["書籍分享"]
---
# 書籍心得：精通API架構
## 推薦給誰
適合已經脫離初階工程師想邁向資深工程師
## 測試
### 服務測試
契約測試[Pact](https://pact.io/)與元件測試的差異性
## 比較反向代理、負載平衡器、API閘道
|功能|反向代理|負載平衡器|API閘道|
|-|:-:|:-:|:-:|
|單一後端(Single Backend)|＊|＊|＊|
|TIS/SSL||＊|＊|
|多重後端(Multiple Backends)||＊|＊|
|服務探索(Service Discovery)||＊|＊|
|API合併(Composition)|||＊|
|授權(Authorization)|||＊|
|重試邏輯(Retry)|||＊|
|速率限制(Rate Limiting)|||＊|
|記錄和追蹤(Logging and Tracing)|||＊|
|斷路機制(Circuit Breaking)|||＊|

負載平衡器
+ [YARP](https://microsoft.github.io/reverse-proxy/)
+ [Nginx](https://nginx.org/)

API閘道
+ [Apache APISIX](https://apisix.apache.org/zh/)
+ [Kong](https://konghq.com/)
+ [Ocelot](https://www.nuget.org/packages/Ocelot#readme-body-tab)
建議使用apisix除了是apache有保障以外，下列文章有更加詳細的說明[已經有Kong為什麼還需要APISI？](https://apisix.apache.org/zh/blog/2022/07/30/why-we-need-apache-apisix/)
## 結論
本書包含很多基礎範例,並從頭開始說明api的演化
