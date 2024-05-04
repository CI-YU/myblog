---
title: "Dockerslow"
date: 2024-05-05T01:24:52+08:00
draft: false
tags: ["docker","dotnet"]
---
# dotnet 6 部署到docker忽然變很慢
最近在公司將dotnet 6 用docker-compose部署到測試機時忽然要等3-5分鐘，最後查證是因為我們在linux環境上有做mount遠端磁碟造成部署時間過長

掛載的磁碟機沒有做資料夾分類所有檔案都在一個資料夾底下，在根目錄就有破百萬的檔案，導致container透過volume連到實體路徑時會讀取過久
