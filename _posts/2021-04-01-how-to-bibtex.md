---
layout: default
title:  "Bibtexの使い方"
date:   2021-03-20
categories: tex
---

# Bibtexとは

引用文献のデータベース化（SQL的なやつではない）  
tex内の引用文献の表示・非表示と順番を勝手に配置してくれる

Bibtexはこんな感じに記述されているファイル↓
```bibtex
@INPROCEEDINGS{7405113,
  author={T. {Hoshi} and T. {Takei}},
  booktitle={2015 IEEE/SICE International Symposium on System Integration (SII)}, 
  title={Simultaneous determination of optimized one unloading point and plural scooping points for wheel loader}, 
  year={2015},
  volume={},
  number={},
  pages={865-870},
  doi={10.1109/SII.2015.7405113}
}
```

`@種別`の種別の名前により，`{}`内のラベルの表示のされ方が変化するので，引用する資料の種類によって種別を変更して記述する必要がある．
論文の場合は`@Article{}`，Webページなどの場合は`@Misc{}`などと記述する．

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

## その他

引用文献として以下ような感じで書く

```
\bibliographystyle{jplain} % plain,jplain:アルファベット順．unsrt,junsrt:引用順
\bibliography{sample}      % sampleはsample.bibを指す
```

また，bibtexファイルを複数ファイルに分割して使いたい場合は以下のように記述する

```
\bibliography{sample_1, sample_2}
```

`sample_%%`はそれぞれ`sample_1.bib`,`sample_2.bib`となっている．


## 参考

- http://mikilab.doshisha.ac.jp/dia/seminar/latex/doc/bib.html  
- https://qiita.com/fujino-fpu/items/d92d185da730e25743cb  
- [BiBTeXとは](https://qiita.com/SUZUKI_Masaya/items/14f9727845e020f8e7e9)
- [Bibliographies from multiple .bib files](https://tex.stackexchange.com/questions/84099/bibliographies-from-multiple-bib-files)
- [BIBTEX の使い方 · 簡易資料](https://blackknight.ics.nara-wu.ac.jp/pub/doc/bibtex.pdf)
