---
layout: default
title:  "vscodeのプロジェクトフォルダごとに設定"
date:   2021-04-25
categories: vscode
---

# vscodeのプロジェクトフォルダごとに設定

プロジェクトフォルダに`.vscode/settings.json`を作成することで標準・拡張機能のon/offができる．

```json
{
  "workbench.colorCustomizations": {
    "titleBar.activeBackground": "#4aa1c4",
    "activityBarBadge.background": "#4aa1c4"
  }
}
```

## 参考
- [プロジェクトごとにVSCodeの色とテーマを変えて気持ちを切り替える](https://qiita.com/mottox2/items/a5813feeaf653ef3e2c3)
