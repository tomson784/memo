---
layout: default
title:  "Dockerを用いて異なるホスト間でROSネットワークを構築"
date:   2021-05-20
categories: docker ros network
---

# Docker+マルチホストでROS通信(セキュリティがあれなのでローカルネットワーク以外では非推奨)

環境構築の手間を省いて複数マシン間で通信するための方法．  
（商用で大規模なネットワークを構築するときなどはk8sやDockerSwarmなどでコンテナオーケストレーションなるものを行う？らしいが，ローカル環境でROSの実験をしたいときの手抜きのDocker+マルチホストです．）  

`docker network`のネットワークの中からhostを選択し，以下のように実行 

```
docker run -it --rm --net host ros:melodic
```

`--net host`というオプションによってホスト側のネットワークを利用する．  
その前にホストPCに`/etc/hosts`にローカルネットワーク内の他PCのホスト名と対応IPを記述しておく．
`/etc/hosts`を設定しておかないと，ホスト間のトピックのリストを見ることはできるが，トピックの中身を見ることができなかったので設定しておく必要がある．  
dockerのshellを起動し，ROS_MASTER_URIとROS_IPを変更する．  

それによってROS通信が実行可能になる．

## 参考

- [Docker の bridge と host ネットワークについて勉強する](https://qiita.com/toshihirock/items/f5b9f7799ec8bf8c328e)
- [docker-composeでホストとコンテナのIPアドレスを同じにする](https://hakengineer.xyz/2018/07/29/post-1475/)
- [dockerコマンドの詳しい解説](http://docs.docker.jp/v1.10/engine/reference/run.html#linux-lxc)
