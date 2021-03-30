---
layout: default
title:  "rustのwasmテンプレート"
date:   2021-03-18
categories: rust wasm
---

# RustでWebAssembly

## プロジェクトの構築手順

1. nvmのインストール
2. `nvm install --lts`で`npm`のインストール
3. `cargo install cargo-generate`
4. `cargo generate --git https://github.com/rustwasm/wasm-pack-template`
5. `curl https://rustwasm.github.io/wasm-pack/installer/init.sh -sSf | sh`
6. `cd <project name>`でプロジェクトに移動し，`wasm-pack build`でビルド
7. `npm init wasm-app www`

## プロジェクトの実行

```
wasm-pack build
cd www
npm run start
```

### 参考
[実践Rustプログラミング入門](https://www.shuwasystem.co.jp/book/9784798061702.html)