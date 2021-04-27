---
layout: default
title:  "ROSデータの参照方法"
date:   2021-04-09
categories: ros
---

# ROSのデータを扱う

rosでは複数のセンシングデータを扱う．  
大量のデータを扱うので，スムーズに参照する必要がある．  
そのためのツール群の使い方に関するメモ．

## rosdep

rosプロジェクトの依存パッケージを自動でインストール．  
たぶん`package.xml`に従ってインストールを行う（あとで調べる）．

```
rosdep install -i --from-paths <path-to-ros-package>
```

## rosbag record

rostopicのデータを同時系列で保存する．
以下のコマンドで全トピックの保存．
```
rosbag record -a
```

指定したトピックだけを保存
```
rosbag record /topic1 /topic2
```

正規表現でトピックを指定
```
rosbag record -e topic
```

rosbagデータをファイル名を指定して保存
```
rosbag record -O <file.bag>
```

rosbagデータをcsv形式で保存するためののシェルスクリプト（ただし，画像データなどはどうなるかわからない．それに気をつけてトピックを指定して保存）  
`$1`は引数を表す．
```bash
#!/bin/bash
rostopic echo -b $1.bag -p /sensing_value > $1.csv
```

## rosbag play

rosbagデータを再生する．主に再生した状態でrvizなどで観察する．
```
rosbag play <bagfile.bag> 
```

n倍速でrosbagを再生する．
```
rosbag play <bagfile.bag> -r <speed> --clock
```

## rqt_bag

GUIでrosbagデータを開く．特定の時間を指定して観察したいときなど．
```
rqt_bag <bagfile.bag>
```

## rviz

センシングデータなどを可視化するためのツール．  

### 基本的な使い方

rvizを開いたあと

### Global Options

`Fixed Frame`を始めに設定する必要がある．
使い方の詳細はまだわからない

点群を扱うセンサの場合に`Fixed Frame`を指定されている．  
以下のコマンドで配列を除いたtopic情報が表示されるので，ヘッダー情報が見やすくなる．

```
rostopic echo --noarr <topic name>
```

### ROSのネットワーク接続

- 端末を同じネットワーク（ローカルネットワーク）で接続（VPNを使えば外部ネットワークとの接続も可能）
- `roscore`起動時のポートは標準で11311となっている

```
ROS_MASTER_URI=http://<ros_master_ip_address>:11311
ROS_MASTER=http://<this_pc_ip_address>
```

- 一見つながっているように見えても，データが取得できないときがある．
- 自分のPCに相手のPC（逆もしかり）を正しく認識させる必要がある．
- `/etc/hosts`を編集する
    - トピックをやり取りするPCのホスト名とIPアドレスを記述


## 参考
- [ROS講座29 rosbagを使う](https://qiita.com/srs/items/f6e2c36996e34bcc4d73)
- [データの記録と再生](http://wiki.ros.org/ja/rosbag/Tutorials/Recording%20and%20playing%20back%20data)
- [ROSのTips（初心者向け）](https://qiita.com/yukkysaito/items/e2f714c254bf7799677e)