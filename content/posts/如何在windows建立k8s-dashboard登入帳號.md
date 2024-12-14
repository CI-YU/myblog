---
title: "如何在windows建立k8s Dashboard登入帳號"
date: 2024-12-14T15:26:07+08:00
draft: false
tags: ["kubernetes"]
---

### 前言

練習時發現文章是使用 mac 的操作指令,windows 用 powershell 建立登入 dashboard 認證時會遇到困難,所以寫下來幫助大家

### 安裝 dashboard

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.5.0/aio/deploy/recommended.yaml
```

> https://github.com/kubernetes/dashboard 可以自行查看當下最新版本

### 檢查是否有成功安裝,如果成功會顯示 kubernetes-dashboard 的名稱

```shell
#檢查是否有成功安裝
kubectl get pod -n kubernetes-dashboard
```

### 透過代理人讓網頁可以使用 dashboard,接著點擊[連結](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/) 確認是否有成功開啟畫面

```shell
kubectl proxy
```

### 接下來開啟新的 powershell 開始依序輸入下列指令

> 切記 kubectl proxy 畫面不要關閉

```shell
#意思是我要在 kubernetes-dashboard 建立一組帳號名稱是admin-user
kubectl create serviceaccount admin-user -n kubernetes-dashboard
#查看有沒有建立成功,如果有成功會顯示 admin-user的帳號
kubectl -n kubernetes-dashboard get serviceaccount
#意思是我要在 kubernetes-dashboard 建立的帳號 admin-user 新增 admin 權限
kubectl create clusterrolebinding admin-user --clusterrole=cluster-admin --serviceaccount=kubernetes-dashboard:admin-user
#取得 token 增加--duration=168h 就可以生效一周,預設的期限很短不方便練習
kubectl create token admin-user -n kubernetes-dashboard --duration=168h
#最後會有一串 token,將他複製到畫面中就可以使用
```

### 參考

[從異世界歸來發現只剩自己不會 Kubernetes](https://ithelp.ithome.com.tw/articles/10287935)
