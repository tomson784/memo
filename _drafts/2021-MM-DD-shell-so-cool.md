---
layout: default
title:  "ターミナルをかっこよくするための環境構築"
date:   2021-05-11
categories: shell
---

# 手っ取り早いpowerline-shellの実装
https://github.com/b-ryan/powerline-shell より

```
pip install powerline-shell
```

下記を`~/.bashrc`に追加

```
function _update_ps1() {
    PS1=$(powerline-shell $?)
}
if [[ $TERM != linux && ! $PROMPT_COMMAND =~ _update_ps1 ]]; then
    PROMPT_COMMAND="_update_ps1; $PROMPT_COMMAND"
fi
```

これだけではフォント不足で表示されないので下記のコマンドを実行

```
sudo apt-get install fonts-powerline
```


vscodeの表示がおかしいままなので，以下のサイト通りに設定を変更する．
https://tworks55.hatenablog.com/entry/2020/04/29/165659

## 参考
- [Bashのコンソールをカスタマイズ](https://zenn.dev/kotokaze/articles/bash-console)
- [bashのプロンプトを変更するには](https://www.atmarkit.co.jp/flinux/rensai/linuxtips/002cngprmpt.html)
