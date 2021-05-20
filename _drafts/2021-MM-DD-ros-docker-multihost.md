---
layout: default
title:  "異なるホスト間でROSネットワークを構築"
date:   2021-05-11
categories: docker ros network
---

# Docker+マルチホストでROS通信(セキュリティがあれなのでローカルネットワーク以外では非推奨)

環境構築の手間を省いて複数マシン間で通信するための方法．  
（商用で大規模なネットワークを構築するときなどはk8sやdocker swarmなどで
コンテナオーケストレーションなるものを行う？らしいがローカル環境でROSの実験をしたいときの手抜きのDocker+マルチホストです．）  

`docker network`のネットワークの中からhostを選択し，以下のように実行 

`--net host`

```
docker run -it --rm --net host tomson784/rosdoc-lite
```

その前にホストPCに`/etc/hosts`にローカルネットワーク内の他PCのホスト名と対応IPを記述しておく．

dockerのshellを起動し，ROS_MASTER_URIとROS_IPを変更する．

それによってROS通信が実行可能になる．

## 参考

- [Docker の bridge と host ネットワークについて勉強する](https://qiita.com/toshihirock/items/f5b9f7799ec8bf8c328e)
