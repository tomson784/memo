---
layout: default
title:  "ROSのドキュメント自動生成のコメントの書き方"
date:   2021-05-17
categories: Doxygen c++ python ros
---

# ROSのドキュメント自動生成のコメントの書き方

`rosdoc_lite`のインストール方法

```
apt-get install ros-melodic-rosdoc-lite
```

`rosdoc_lite`でドキュメントを自動生成する．

```
rosdoc_lite <ros-project-path>
```

`rosdoc_lite`はデフォルトでDoxygenとなっているが，他の物を使うこともできる．  
もちろんDoxygen単体でも使用可能．

## コメントの文法


## ドキュメントに数式を加えたい場合

数式はMathjaxというものが使われるらしく，


## `rosdoc_lite`コマンドを手軽に扱う


[`Dockerfile`]()

https://github.com/tomson784/ros_tutorial/tree/main/rosdoc_lite_env

```
docker pull tomson784/rosdoc-lite:latest
```

### 注意点

ubuntu18をホストとしてdockerを起動したときは以下のコマンドだけでドキュメントの生成ができた．

```
docker run --rm -v "$(pwd)/<ros_project_dir_name>:/<ros_project_dir_name>" <rosdoc_lite_image> rosdoc_lite /<ros_project_dir_name>
```


## 参考
- [DoxygenでPythonコードを文書化](https://campkougaku.com/2020/02/20/python-doxygen/)
- [コードのドキュメント付け](http://www.doxygen.jp/docblocks.html)
- [【Ubuntu Linux】Doxygenをインストールして使う](https://www.yokoweb.net/2020/06/29/ubuntu-linux-doxygen-install/)
- [Doxygenを利用して、コードコメントからドキュメントを作ろう！ Githubとの連携も出来るよ！](https://qiita.com/developer-kikikaikai/items/3984606dbdbf2bbbe74e)
- [最近覚えた便利アプリ[doxygen]](https://qiita.com/wakaba130/items/faa6671bd5c954cb2d02)
