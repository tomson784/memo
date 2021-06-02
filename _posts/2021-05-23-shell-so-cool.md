---
layout: default
title:  "ターミナルをかっこよくするための環境構築"
date:   2021-05-23
categories: shell
---


# ターミナルの見た目を変更する場合

## powerline-shellの実装

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

vscodeの表示がおかしいままなので，以下のサイト通りに設定を変更する．↓  
https://tworks55.hatenablog.com/entry/2020/04/29/165659

これによってターミナルがpowerline-shellの表示となる．

## 環境変数`PS1`を編集する場合

ターミナルの表示を細かく変更する場合の編集方法

shellのはじめに改行を加えるだけの場合↓

```bash
if [ "$color_prompt" = yes ]; then
    # PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\n\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

shellのはじめの改行＋gitのブランチの表示をしたい場合↓

```bash
if [ "$color_prompt" = yes ]; then
    # PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]$(__git_ps1)\n\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

## 参考
- [Bashのコンソールをカスタマイズ](https://zenn.dev/kotokaze/articles/bash-console)
- [bashのプロンプトを変更するには](https://www.atmarkit.co.jp/flinux/rensai/linuxtips/002cngprmpt.html)
- [Ubuntu bashのプロンプトHack!!!!（gitブランチ表示）](https://qiita.com/ryosukue/items/bc1eae639e3950790eb9)
