---
layout: default
title:  "Arduino開発をvscodeで快適にする"
date:   2021-05-27
categories: arduino vscode
---

# Arduino開発をvscodeで快適にする

## 手順

- vscode，arduinoのインストール
- arduino拡張機能のインストール
- `setting.json`を編集
- verifyでコンパイル(右下の board select によりコンパイル環境の選択．それによって`arduino.json`というファイルができる．)
- コンパイルができる

## 注意点

いつの間にか`.vscode/c_cpp_properties.json`にarduinoの設定が書き込まれており，これを消すとコンパイルができなくなる．  
これを復活させる方法がわからないので，こまめに`c_cpp_properties.json`を確認して，バックアップを取っておいたほうが良い．


## 参考
- [Arduinoで遊ぶページ](https://garretlab.web.fc2.com/arduino/introduction/vscode/)
- [vscodeでArduinoを開発する@Ubuntu](https://qiita.com/mugimugi/items/7befd0bf561c96c43772)
- [VSCodeでArduinoの開発をする方法](https://www.mechatronahibi.com/arduino-vscode-dev/)
