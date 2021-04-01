---
layout: default
title:  "Bibtexの使い方"
date:   2021-03-20
categories: tex
---

# Bibtexとは

引用文献のデータベース化（SQL的なやつではない）  
tex内の引用文献の表示・非表示と順番を勝手に配置してくれる

## texliveなどの場合の例

出力された`****.aux`ファイルに対して以下のコマンドで
引用文献が表示できるようになる

```
pbibtex ****.aux
```

## overleafの場合の例

`latexmkrc`を作成してルートディレクトリに配置

```latexmkrc
$latex = 'platex';
$bibtex = 'pbibtex';
$dvipdf = 'dvipdfmx %O -o %D %S';
$makeindex = 'mendex -U %O -o %D %S';
$pdf_mode = 3; 
```

コンパイラをLatexにする

引用文献として以下ような感じで書く

```
\bibliographystyle{jplain} # jplainの意味はわからない
\bibliography{sample}      # sampleはsample.bibを指す

```
## 参考

http://mikilab.doshisha.ac.jp/dia/seminar/latex/doc/bib.html  
https://qiita.com/fujino-fpu/items/d92d185da730e25743cb  
