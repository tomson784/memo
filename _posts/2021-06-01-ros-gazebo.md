---
layout: default
title:  "rosでgazeboを使う"
date:   2021-06-01
categories: ros gazebo
---

# 内容

rosでgazeboを扱う場合の注意点など

## rosでgazeboが起動できない場合

以下のようなエラーが発生した場合の対処法

```
[gazebo-2] process has died [pid 4327, exit code 255 ...........................
```

以下のコマンドでなんとかなる場合がある

```
killall gzserver
killall gzclient
```

https://answers.gazebosim.org//question/9170/error-while-loading-gazebo/


## gazeboの環境を初期化する

強化学習に使用する場合，エージェントの状態によって環境をリセットしたい場合がある．

```
rosservice call /gazebo/reset_world "{}"
```

[turtlebot3でDQN](https://github.com/ROBOTIS-GIT/turtlebot3_machine_learning)

## 参考
- [[ROS Q&A] 014 - How to Reset Model Poses Programmatically in Gazebo](https://www.youtube.com/watch?v=ZSvM7dEilhk)
