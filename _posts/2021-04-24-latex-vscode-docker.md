---
layout: default
title:  "docker+vscodeでlatexを使う"
date:   2021-04-24
categories: vscode latex docker
---

# docker+vscodeでlatexを使う

## 実装環境

- wsl2(ubuntu-20.04)
- windows docker desktop (windows home)

## 手順

```json
{
    "editor.wordWrap": "on",
    "editor.wordSeparators": "./\\()\"'-:,.;<>~!@#$%^&*|+=[]{}`~?．。，、（）「」［］｛｝《》てにをはがのともへでや",
}
```

```json
{
    "latex-workshop.latex.recipe.default": "latexmk (latexmkrc)",
    "latex-workshop.docker.enabled": true,
    "latex-workshop.docker.image.latex": "paperist/alpine-texlive-ja",
    "latex-workshop.latex.autoBuild.run": "onFileChange",
    "latex-workshop.latex.autoClean.run": "onFailed",
    "latex-workshop.view.pdf.viewer": "tab",
}
```

`latexmkrc`

```perl
$latex = 'platex';
$bibtex = 'pbibtex';
$dvipdf = 'dvipdfmx %O -o %D %S';
$makeindex = 'mendex %O -o %D %S';
$pdf_mode = 3;
```

## 参考
- [LaTeX Workshop を使いこなす](https://qiita.com/Yarakashi_Kikohshi/items/a9357dd469320ffb65a0)
- [Docker+VS CodeでLaTeX環境構築 (latexmkrc依存版)](https://laptrinhx.com/docker-vs-codedelatex-huan-jing-gou-zhu-latexmkrc-yi-cun-ban-1015209527/)
