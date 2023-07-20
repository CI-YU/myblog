---
title: "Three Tree"
date: 2023-07-20T23:22:34+08:00
draft: false
tags: ["flutter"]
---
# 三棵樹
flutter共有三個核心
## widget tree
在flutter很常看到一層包一層，MaterialApp、Scaffold這類組件都是widget tree，我們通常只會對到這層。
## element tree
會將資料儲存在記憶體，並決定要不要重新繪製UI
## render tree
是由element tree控制，當element tree決定有必要修改，就會透過render tree進行UI修改