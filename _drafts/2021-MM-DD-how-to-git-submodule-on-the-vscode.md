---
layout: default
title:  "git submoduleをvscodeで使う"
date:   2021-10-12
categories: git submodule vscode
---

# git submoduleをvscodeで使う

vscodeでのgit submoduleの基本的な使い方について

## 手順

1. `git submodule add -b <branch name> <repository url>`でサブモジュールとしてリポジトリを追加
2. `git checkout <commit_id or branch_name>`でコミット番号，もしくはブランチを切り替える
3. 以下のようなファイルがvscodeのgitで表示されるので，変更したければコミットをする．
```
diff --git a/sample_repo b/sample_repo
index dc2e835..86fbb67 160000
--- a/sample_repo
+++ b/sample_repo
@@ -1 +1 @@
-Subproject commit dc2e835xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx(commit id)
+Subproject commit 86fbb67xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx(commit id)
```
4. `git submodule`でコミットIDが自分が選択したものと一致しているかを確認する．

# 参考
- [git-submodule - Initialize, update or inspect submodules](https://git-scm.com/docs/git-submodule)
- [Git submodule の基礎](https://qiita.com/sotarok/items/0d525e568a6088f6f6bb)
- [Git submoduleの押さえておきたい理解ポイントのまとめ](https://qiita.com/kinpira/items/3309eb2e5a9a422199e9)
- [git submoduleを別のブランチに切り替える](https://gozuk16.hatenablog.com/entry/2017/09/26/224342)
- [Visual Studio Code の git 連携機能と git コマンドについて (2018/05/23)](https://qiita.com/satokaz/items/4660ce57ca8eb456a096)
