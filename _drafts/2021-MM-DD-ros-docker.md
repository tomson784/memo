---
layout: default
title:  "論文を書くための環境準備"
date:   2021-05-11
categories: ros docker 
---

# Dockerを用いてROSを扱う

rosで分散処理やエッジデバイスによる通信を行う場合，環境構築やrosのバージョン合わせをしたい場合がある．
その場合のdockerの起動方法

```
docker run \
    -it --rm \
    --volume=/tmp/.X11-unix:/tmp/.X11-unix:rw \
    --volume=$HOME/.Xauthority:$HOME/.Xauthority:rw \
    --volume=(mounted directory in host):(mounted directory in docker):rw \
    --env="XAUTHORITY=$HOME/.Xauthority" \
    --env="DISPLAY=${DISPLAY}" \
    --env="USER_ID=$(id -u)" \
    --privileged \
    --net=host \
    $IMAGE \
```

rosでマルチホストネットワークを構築する場合は`ROS_IP`と`ROS_MASTER_URI`を記述することを忘れない．

場合によっては`runtime`も実装する．

## 参考
- [LaTeX Workshop を使いこなす](https://qiita.com/Yarakashi_Kikohshi/items/a9357dd469320ffb65a0)
- [Docker+VS CodeでLaTeX環境構築 (latexmkrc依存版)](https://laptrinhx.com/docker-vs-codedelatex-huan-jing-gou-zhu-latexmkrc-yi-cun-ban-1015209527/)
