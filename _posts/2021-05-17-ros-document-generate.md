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

最低限あった方がいいものについてメモ

### C++の場合

コメント分の記法(c++)
```
/**
 * 
 */
```
```
/*!
 * 
 */
 ```
```
/*!

*/
```
```
///
///
///
```
```
//!
//!
//!
```

ファイルの先頭
```
/**
 * @file ファイル名
 * @brief 簡単な説明
 * @details 詳細な説明
 */
```

関数の上
```
/**
 * @brief 簡単な説明（～する関数）
 * @param[in] 引数名 引数の説明
 * @param[out] 引数名 引数の説明
 * @return 戻り値の型 戻り値の説明
 * @details 詳細な説明
 * ここから下にも記述可能．
 * Markdown形式でコンパイルされる．texが使えるならMathJaxも可
 */
```

短い説明用（変数など，関数でも別に問題ない）
```
//! 説明
```

### Pythonの場合

ファイルの先頭
```
##
# @file ファイル名
# @brief 簡単な説明
# @details 詳細な説明
```

短い説明用（変数など，関数でも別に問題ない）
```
## 説明
```

## ドキュメントに数式を加えたい場合

数式はMathjaxというものが使われるらしく，以下のような記法で記述できる．

```
\f$ tex記法 \f$
```

## `rosdoc_lite`コマンドを手軽に扱う


rosdoc_lite(latexコンパイルあり)をdockerで手軽に扱うための`Dockerfile`を
[このリンク](https://github.com/tomson784/ros_tutorial/blob/main/rosdoc_lite_env/Dockerfile)に公開する．

また，dockerhubのrepositoryにも上げているので以下のコマンドでdockerイメージがダウンロードできる．

```
docker pull tomson784/rosdoc-lite:latest
```

### 注意点

ubuntu18をホストとしてdockerを起動したときは以下のコマンドだけでドキュメントの生成ができた．

```
docker run --rm -v "$(pwd)/<ros_project_path_on_host>:/<ros_project_path_on_docker>" --workdir="/<ros_project_path_on_docker>" tomson784/rosdoc-lite rosdoc_lite .
```

~~しかし，wsl2のUbunut20.04内でコマンドを実行した場合は，`rosdoc_lite`の処理はうまくいくが肝心のドキュメントがホスト上に生成されていないので，修正する必要がある．~~

~~wsl2でもdockerでshellを起動し，そこで`rosdoc_lite`を起動すると普通に生成されてホスト上にも共有された．~~

wsl2，Ubuntu18の両方で実行できることを確認．

## 参考
- [DoxygenでPythonコードを文書化](https://campkougaku.com/2020/02/20/python-doxygen/)
- [コードのドキュメント付け](http://www.doxygen.jp/docblocks.html)
- [【Ubuntu Linux】Doxygenをインストールして使う](https://www.yokoweb.net/2020/06/29/ubuntu-linux-doxygen-install/)
- [Doxygenを利用して、コードコメントからドキュメントを作ろう！ Githubとの連携も出来るよ！](https://qiita.com/developer-kikikaikai/items/3984606dbdbf2bbbe74e)
- [最近覚えた便利アプリ[doxygen]](https://qiita.com/wakaba130/items/faa6671bd5c954cb2d02)
- [式のインクルード](http://www.doxygen.jp/formulas.html)
