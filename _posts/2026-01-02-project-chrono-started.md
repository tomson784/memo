---
layout: default
title:  "Project Chronoの始め方"
date:   2026-01-02
categories: chrono simulator ros 
---

# Project Chrono

[Project Chrono](https://projectchrono.org/)は、車両・ロボティクス等と土壌・その他流体などの相互作用(テラメカニクス)をシミュレーションすることができるオープンソースソフトウェアです。
試しに動かしてみました。GPUなしのPCでやりました。

Dockerで動かす場合とローカル環境で動かす2パターンで試しました。

## Dockerを用いてやる方法(シミュレーション起動のみ)

以下URLのgithubリポジトリを参考にしました。<br>
https://github.com/ishigami-lab/chrono_tutorials

まずは、Dockerの起動
```sh
docker run -it --net=host --env=DISPLAY=$DISPLAY --env=QT_X11_NO_MITSHM=1 --volume="/home/$USER:/home/$USER" --volume="$HOME/.Xauthority:/root/.Xauthority" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" --name chrono_env00 uwsbel/projectchrono:latest
```

Dockerコンテナに入ってから、必要パッケージのインストール
```sh
apt update
apt install cmake-curses-gui gedit git
apt install libeigen3-dev
apt install libirrlicht-dev
```

`ccmake`することで、コマンドライン上で`cmake`のpathやbooleanの設定をします。
```sh
cd ~/Desktop/
mkdir chrono_build
cd chrono_build
ccmake ../chrono/
```
`ccmake`では`[c] Configure`と`[g] Generate`の手順で実行します。ただし、依存ファイルの不足などによりうまく行かない場合や、`ccmake`の処理によって新たに項目が増えたりもするので確認する必要あり

次のコマンドで実行ファイルが作成されます。時間かかります。
```sh
make
```
これが完了したら`chrono_build/bin/`にある`demo_*`のファイルを実行することでシミュレーションを実行できます。
ただし、Dockerを先程の方法で起動した場合はguiアプリが使えないのでdockerコンテナ起動前と起動後に以下のコマンドを挟んでください。
```sh
xhost +local:
docker run -it --net=host --env=DISPLAY=$DISPLAY --env=QT_X11_NO_MITSHM=1 --volume="/home/$USER:/home/$USER" --volume="$HOME/.Xauthority:/root/.Xauthority" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" --name chrono_env00 uwsbel/projectchrono:latest
xhost -local:
```


## ローカル環境でやる方法(シミュレーション起動・ROS2連携)

Ubuntu24.04, Fujitsu, intel core i7, メモリ32GBで試しました（CPUのみ）。

まずはシミュレーションソースコードのダウンロード
```sh
git clone https://github.com/projectchrono/chrono.git
```
必要パッケージのインストール
```sh
sudo apt install cmake-curses-gui
sudo apt install libirrlicht-dev
sudo apt install libeigen3-dev
```
ros2連携する場合はros2がインストールされていることが前提となる。
chronoとros2を連携するros2パッケージをコンパイルしておく必要あり。
```sh
cd <ros2_ws>/src
git clone https://github.com/projectchrono/chrono_ros_interfaces.git
cd ..
colcon build --symlink-install
```

```sh
mkdir ./chrono_build
cd ./chrono_build
ccmake ../chrono
```
`ccmake`の時にros2設定をONにしてから`[c] Configure`をするとros2のpath設定の設定一覧が増えます。その中からpathが未設定となっているものを探してください。
私の場合は`chrono_ros_interfaces`のpathが未設定だったので以下のpathを設定しました。
`<ros2_ws>/install/chrono_ros_interfaces/share/chrono_ros_interfaces/cmake/`

これら設定を追加できたら`ccmake`で`[c] Configure`と`[g] Generate`の手順で実行します。それができたら以下のコマンドで実行ファイルが作成されます。
```sh
make
```
ここまでの手順でシミュレーションサンプルの実行ファイルができているはずです。


ここからはchronoシミュレーションとros2の連携です。
以下のコマンドでchronoのシミュレーションを起動します。
```sh
./demo_ROS_two_managers 
```
別のターミナルで以下のtopicを出力します。
```sh
ros2 topic pub /hmmwv_1/input/driver_inputs chrono_ros_interfaces/msg/DriverInputs "{header: {stamp: {sec: 0, nanosec: 0}, frame_id: ''}, steering: 0.9, throttle: 1.5, braking: 0.0, clutch: 0.0}"
```
これによりchronoとros2が連携できていることが確認できました。


![project_chrono_ros]({{site.baseurl}}/images/project-chrono-connection-ros.png)

## 備考

`ccmake`コマンドで`configure`をしたい際に、DEMやSENSORをONにしたましたが、CUDAがありませんなどのメッセージが表示されました。
DEMなどの計算にはGPUが必須なのかもしれないです。
ここらへんの依存パッケージやコンパイルの条件などについてはパッケージそれぞれの`CMakeLists.txt`から確認できます。

## ローカル環境(GPUあり)でやる方法(シミュレーション起動・ROS2連携・WSL2)

- Project Chrono 9.0.1で実施
- Windows 11 home
- WSL2, Ubuntu22.04
- cuda-12.3

### 結論

前述のGPUなし環境におけるROS2連携に加え、DEMやSENSORのパッケージも含めてコンパイルできた。
ただし、その過程でインストールしたOptixを含めたコンパイルはできても、WSL2環境ではOptixを使えないそうです。以下、Nvidiaのフォーラムのスレッドを一部日本語にして引用したもの。
```
- フォーラムのスレッドで、ユーザーが WSL2（Ubuntu 22.04）上で OptiX 7.6 コードをビルド実行しようとしても、OptiX ランタイムライブラリ（libnvoptix.so.1）が見つからずエラーになるという報告があります。
- このスレッドでは、WSL2 ドライバー側に必要な OptiX ランタイムが含まれていないという指摘が出ています。
```

### インストール手順

まずは、CPUのみバージョンと同様にeigen3やirrlichtなどをインストールする。
DEMを利用するためにcudaをインストールする。
[このリンク](https://developer.nvidia.com/cuda-downloads)にcudaライブラリのインストール手順があるが、Project Chronoのバージョンによってcudaの対応バージョンが違う。
cuda-13はライブラリの中身の変更により、一部コンパイルが通らなかった。
cuda-12.3をインストールし、それでも一部関数が対応していなかったので、DEMプログラムの一部を以下のスクリプトで書き換えるとコンパイルが進むようになった
```
grep -rl "cuda::std::terminate" src/chrono_dem/cuda   | xargs sed -i 's/cuda::std::terminate()/asm("trap;")/g'
```

cudaが認識されていない場合は`ccmake`画面で`[t] Toggle advanced mode`を用いると隠れたcmakeの変数が表示されるので、その状態で以下のようにパスを設定する。
```
CMAKE_CUDA_COMPILER              /usr/local/cuda/bin/nvcc
```


コンパイルの最中に`OpenGL support CMake Error at /home/<user>/.local/lib/cmake/glfw3`と表示されることがある。
これについては、`.local`フォルダに入っているglfw3を削除して、以下のライブラリをインストール済みの状態で、`ccmake`で`configure`を実行すると`glfw3`へのパスが自動設定される。
`sudo apt install libglfw3-dev` or `sudo apt install libglfw3-dev libglew-dev libgl1-mesa-dev`

もしパスが更新されていない可能性があれば、`ccmake`画面で以下のようにパスを設定する。
```
glfw3_DIR t/usr/lib/x86_64-linux-gnu/cmake/glfw3
EIGEN3_INCLUDE_DIR  /usr/include/eigen3
```

ここまでやってコンパイルをすると、DEMのデモプログラムのコンパイルが通らなかった。
これはirrlichtのリンクができていないことのエラーだったが、これについてはリンクなどを確認しても原因がわからなかったので、DEMのDEMOプログラムをFALSEに設定してコンパイルした。

次にコンパイルしようとする際にOptixのコンパイルエラーが発生したので、Optixを`/opt`に[このリンク](https://developer.nvidia.com/designworks/optix/downloads/legacy)に従ってインストールした
```
chmod +x NVIDIA-OptiX-SDK-7.7.0-linux64-x86_64.sh
sudo ./NVIDIA-OptiX-SDK-7.7.0-linux64-x86_64.sh
```
`ccmake`を以下のように設定する
```
OptiX_INCLUDE       /opt/optix/include
OptiX_INSTALL_DIR   /opt/optix
```

ひとまず、DEMのデモなしでコンパイルすることができたが、一部バイナリを実行する際に以下のエラーが発生して、描画することができなかった。
```
Failed to create a ChOptixEngine, with error: OPTIX_ERROR_LIBRARY_NOT_FOUND: Library not found at <host path>/chrono/src/chrono_sensor/optix/ChOptixEngine.cpp:82
```

これについてはOptixの実行ファイルがWSL2に対応していないことが原因だと考えられている。
ここまでやると、フォークリフトなどのデモンストレーションを実行することができた。

## 参考

- [ccmakeのすすめ](https://alt-native.hatenadiary.org/entry/20121224/1356319986)
- [chrono_tutorials(ishigami-lab)](https://github.com/ishigami-lab/chrono_tutorials)
- [A Review of Physics Simulators for Robotic Applications](https://ieeexplore.ieee.org/abstract/document/9386154)
- https://developer.nvidia.com/cuda-downloads
- https://developer.nvidia.com/designworks/optix/downloads/legacy
- https://forums.developer.nvidia.com/t/problem-running-optix-7-6-in-wsl/239355
- https://forums.developer.nvidia.com/t/optix-on-wsl-windows-subsystem-for-linux/140297/23?page=2