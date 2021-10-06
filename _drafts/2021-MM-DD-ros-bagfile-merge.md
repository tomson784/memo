---
layout: default
title:  "rosbagファイルの結合"
date:   2021-09-22
categories: ros
---

# rosbagファイルの結合

## やりかた
基本的には[このサイト](https://naoki-sh.github.io/articles/ros/rosbag)に書いてある通り

## 補足
rosbagで保存したデータにはタイムスタンプが一緒に保存されている．
タイムスタンプを確認すると10桁くらいの数字が記述されている．
この数字はunix時間を表しており，現在の時刻をタイムスタンプとして保存している．
[このサイト](https://keisan.casio.jp/exec/system/1526004418)でunix時間を現実の時間に変換することができる．
rosbagファイルをマージする際のデータの時系列はこのタイムスタンプに準拠しているので，結合したデータであっても時系列をある程度は保証することができる．

## 参考
