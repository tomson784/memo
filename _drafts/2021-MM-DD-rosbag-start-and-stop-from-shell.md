---
layout: default
title:  "rosbagをプログラム中の任意のタイミングで起動・停止する方法"
date:   2021-10-12
categories: ros rosbag shell
---

# rosbagをプログラム中の任意のタイミングで起動・停止する方法

ROSを使った実験が大規模（センサ数，ネットワーク）であったり，長時間に渡る場合にrosbagをそのまま使用すると，

- bagデータが非常に大きくなる．
- 実験のセクションごとに切り出したいときにbagデータが大きいと，bagデータを開くだけで手間がかかる．

これらを回避するためにシェルによってrosbagの自動起動，および自動停止する方法について説明

## 手法

rosbagの起動
```
rosbag record __name:=<record_node_name>
```

`<record_node_name>`はrosbagを保存するノードのノード名を決めることができる．
決めなかった場合は`record_xxxxxxxxxxxxxxxxxx`といったようなノード名となる（xは数字）．

rosbagの停止
```
rosnode kill /<record_node_name>
```

# 参考
- [start and kill rosbag record from bash shell script](https://answers.ros.org/question/275441/start-and-kill-rosbag-record-from-bash-shell-script/)
