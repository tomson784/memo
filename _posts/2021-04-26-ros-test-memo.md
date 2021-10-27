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
- [ユニットテスト](https://industrial-training-jp.readthedocs.io/ja/latest/_source/session6_JP/Unit-Testing_JP.html)
- [「rostest」 の使い方](https://makemove.hatenablog.com/entry/2016/06/15/221708)
- [google testでrosパッケージの単体テストを行う方法](https://ittechnicalmemos.blogspot.com/2019/09/google-testros.html)
- [rostest](http://wiki.ros.org/rostest)

