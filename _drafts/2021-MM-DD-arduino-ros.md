# ROSでArduinoを使う

## Arduino Due

https://github.com/ros-drivers/rosserial/issues/284
の方法ではうまく行かなかった．

`ino`ファイルの`<ros.h>`の上に`#undef USBCON`を追記したらうまく行った．

