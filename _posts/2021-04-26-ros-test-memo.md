---
layout: default
title:  "rosのユニットテスト"
date:   2021-04-26
categories: ros
---

# rosのためのユニットテスト

launchファイルでのテスト

```
catkin_make run_tests
```

実行ファイル単体のテスト

```
rosrun <package_name> <exefile>
```

その他オプション

- テスト一覧の表示
```
rosrun <package_name> <exefile> --gtest_list_tests
```


## 参考
- [gtest 実行時のオプション](http://dr-yellow.hatenablog.com/entry/2015/11/27/004234)
