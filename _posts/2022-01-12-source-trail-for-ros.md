---
layout: default
title:  "コードの結びつきを可視化する"
date:   2022-01-12
categories: ros code_reading
---

# コードの結びつきを可視化する

Source Trailを使ってROSプロジェクトの内容を理解する

手順
1. プロジェクト名称とプロジェクトの保存場所を設定
2. `Add Source Group`をクリックする
3. `SOURCE GROUP Type`の選択画面から、`C/C++ from Compilation Database`を選択してNextをクリック．
4. `Compilation Database`として`compile_commands.json`を設定する．
（`compile_commands.json`はプロジェクトの`CMakelists.txt`に`set(CMAKE_EXPORT_COMPILE_COMMANDS ON)`を追加して`catkin_make`を実行すると`~/catkin_ws/build/compile_commands.json`生成される．
5. `Header files & Directories to Index`でソースコードのディレクトリを指定する．
（指定するのはROSプロジェクトのルートでOK）
6. `Create`をクリックして完成

## 参考
- [SourcetrailでROSパッケージを読む](https://qiita.com/rkoyama1623/items/dcbdb3b96d3d384d4778)
- [rosのcatkin_makeでcompile_commands.jsonを生成する](https://amazingsoblin.hatenablog.com/entry/2018/07/02/232343)
