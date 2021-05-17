---
layout: default
title:  "OpenMPの使い方"
date:   2021-05-10
categories: c++
---

# OpenMPとは

並列計算を行うためのライブラリ？ソフト？パッケージ？  
OpenMPはクロスプラットフォームとなっている

## インストールする必要があるもの

Ubuntu18.04のROSインストール済みだと追加で何かをインストール必要はなかった．

## cmakeでビルドする

以下のコマンドを加えればひとまずOK

```CMakeLists.txt
find_package(OpenMP REQUIRED)
if(OpenMP_FOUND)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()
```

## 参考

- [CMakeでOpenMPアプリケーション開発](https://qiita.com/iwatake2222/items/5800fda029019ce8a276)
- [CMakeの使い方（その１）](https://qiita.com/shohirose/items/45fb49c6b429e8b204ac)
