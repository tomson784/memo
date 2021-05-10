---
layout: default
title:  "リポジトリごとに git config を設定する"
date:   2021-05-06
categories: git
---

# リポジトリごとに git config を設定する

1つのPCで複数のリモートリポジトリとやり取りしなければならないことがある．  
（例：組織内のリモートリポジトリ，個人のリモートリポジトリ，など）

それらのアカウント名が別々だと面倒なのでそれをさけるため，リポジトリごとにgitの設定を行う

# `git config --local`

```
git config --global user.name "メインアカウント名"
git config --global user.email "メインアカウントメールアドレス"
```

```
git config --local user.name "現在の作業リポジトリに用いるアカウント名"
git config --local user.email "現在の作業リポジトリに用いるメールアドレス"
```

## 参考

- [複数のgitアカウントを使い分ける](https://qiita.com/0084ken/items/f4a8b0fbff135a987fea)
