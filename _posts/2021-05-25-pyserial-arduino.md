---
layout: default
title:  "pythonでArduinoとPC間でシリアル通信する"
date:   2021-05-25
categories: python arduino
---

# pythonでArduinoとPC間でシリアル通信する

raspiとarduinoを想定（試した環境はUbuntu18.04）

## PC => Arduino

PCからの命令でLEDを光らせる．

Arduino側（PCからの命令受け取り）↓

```cpp
byte data = 0;

void setup(){
  Serial.begin(115200);
  pinMode(13, OUTPUT);
  digitalWrite(13, LOW);  //  初期化
}

void loop(){
  if (Serial.available() > 0){
    data = (byte)Serial.read();
    if(data == 123){
      digitalWrite(13, HIGH);
      delay(3000);
    }
    else{
      digitalWrite(13, HIGH);   
      delay(1000);
      digitalWrite(13, LOW);
    }    
  }
}
```

PC側（Arduinoへの命令送信）↓

```py
import serial
import time

ser = serial.Serial("/dev/ttyACM0", 115200, timeout = 0.1)
time.sleep(2)           // Arduino側との接続のための待ち時間が必要
ser.write(chr(123))     // char型で送信し，Arduino側でbyte型へ変換すれば数字をそのまま扱える．
ser.close()
```

## Arduino => PC

Arduino内の数値をPCに送る

Arduino側（PCへのデータ送信）↓

```cpp
//Arduino
int val = 0;
int x = 100;
int y = 100;
void setup(){
  Serial.begin(9600);
}
void loop(){
  val = analogRead(1);
  // Serial.println(val);
  Serial.println((String)"x:"+x+" y:"+y);
  delay(100);
}
```

PC側（Arduinoからのデータ受信）↓

```py
# coding: utf-8

import serial
ser = serial.Serial('/dev/ttyACM0', 9600) 
not_used = ser.readline()
while True:
    val_arduino = ser.readline()
    # val_decoded = float(repr(val_arduino.decode())[1:-5])
    val_decoded = val_arduino
    print(val_decoded)

ser.close()
```

## 注意点

### arduino=>pc，pc=>arduinoでデータをやり取りする際の注意点

```py
ser = serial.Serial('/dev/ttyACM0', 9600) 
while True:
    ser.write(123)
```

ではなく

```py
ser = serial.Serial('/dev/ttyACM0', 9600) 
while True:
    ser.readline()
    ser.write(123)
```

とするとうまくいく．  
arduinoへの書き込みに対して，読込を常に行う必要がある．（バッファの関係？クロックなんちゃらのせい？）

## 参考
- [【PySerial】Python×Arduinoで制御してみる1【Lチカ】](https://gangannikki.hatenadiary.jp/entry/2018/11/15/025849)
- [(python⇔Arduino)シリアル通信で数値をやりとり](https://haizairenmei.com/2019/11/13/pyserial-arduino/)
- [【Arduino】Arduino と python でシリアル通信](https://shizenkarasuzon.hatenablog.com/entry/2018/09/29/180625)
- [PC⇔Arduinoのシリアル通信をPython3でやってみた](https://qiita.com/k_zoo/items/cbeda6736d727113b7cd)
