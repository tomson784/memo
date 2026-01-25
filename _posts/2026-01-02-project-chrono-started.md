---
layout: default
title:  "Project Chronoの始め方"
date:   2026-01-02
categories: chrono simulator ros 
---

# Project Chrono

[Project Chrono](https://projectchrono.org/)は、車両・ロボティクス等と土壌・その他流体などの相互作用(テラメカニクス)をシミュレーションすることができるオープンソースソフトウェアです。
試しに動かしてみました。GPUなしのPCでやりました。

「Dockerで動かす場合」と「ローカル環境(Ubuntu24.04、CPUのみ)」と「ローカル環境(WSL2、Ubuntu22.04、NvidiaGPUあり)」で動かす3パターンで試しました。

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

### 備考

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


cmakeをアップデートした方がよい。
私はcmake version 4.2.1にアップデートをした。
次のサイトを参考にした（[【cmake】最新版CMakeをapt installする方法【Ubuntu】](https://qiita.com/sunrise_lover/items/810977fede4b979c382b)）。

cuda-12.3をインストールし、それでも一部関数が対応していなかったので、DEMプログラムの一部を以下のスクリプトで書き換えるとコンパイルが進むようになった
```
grep -rl "cuda::std::terminate" src/chrono_dem/cuda   | xargs sed -i 's/cuda::std::terminate()/asm("trap;")/g'
```

cudaが認識されていない場合は`ccmake`画面で`[t] Toggle advanced mode`を用いると隠れたcmakeの変数が表示されるので、その状態で以下のようにパスを設定する。
```
CMAKE_CUDA_COMPILER              /usr/local/cuda/bin/nvcc
```
また、以下のコマンドを実施する。
```
echo "export PATH="/usr/local/cuda/bin:$PATH"
export LD_LIBRARY_PATH="/usr/local/cuda/lib64:$LD_LIBRARY_PATH"
" >> ~/.bashrc
apt install cuda-toolkit-12-3
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

### 備考

## Docker(+NVIDIA)を用いてやる方法(DEMの実行まで)

ローカルのUbuntu24.04でやろうとすると、Nvidia-driverやcudaのバージョン管理が面倒に感じたので、nvidiaのdockerをカスタムして実施した。

まとめるのが面倒なので、とりあえず殴り書きまで（後ほどインストールしたパッケージのコミットIDも書く）。
基本的にこれまでの環境構築は実施し、DockerコンテナでDEMを起動するための追加設定を記述していきます。


下記、実際に設定したコマンド
`src`の`#include <irrlicht.h>`を`#include <irrlicht/irrlicht.h>`に変更
```sh
grep -Rl '#include <irrlicht.h>' . | xargs sed -i 's|#include <irrlicht.h>|#include <irrlicht/irrlicht.h>|'
```
```cpp
#include "chrono/core/ChRotation.h"   // QuatFromAngleX/Y/Z 等を定義
#include "chrono/assets/ChVisualSystem.h"
#ifdef CHRONO_VSG
    #include "chrono_dem/visualization/ChDemVisualizationVSG.h"
    #include "chrono_vsg/ChVisualSystemVSG.h"
    using namespace chrono::vsg3d;
#endif
#ifdef CHRONO_IRRLICHT
    #include "chrono_irrlicht/ChVisualSystemIrrlicht.h"
    using namespace chrono::irrlicht;
#endif
```

ただし、DEMでは`VSG`のみの対応であり、irrlichtで描画が実装されてなかった。
実際は以下の変更だけでいいはず。
```cpp
#include "chrono/core/ChRotation.h"   // QuatFromAngleX/Y/Z 等を定義
#include "chrono/assets/ChVisualSystem.h"
#ifdef CHRONO_VSG
    #include "chrono_dem/visualization/ChDemVisualizationVSG.h"
    #include "chrono_vsg/ChVisualSystemVSG.h"
    using namespace chrono::vsg3d;
#endif
```

```cpp
// sampler->maxLod = data->properties.maxNumMipmaps;
sampler->maxLod = static_cast<float>(data->properties.mipLevels);
```

VSG(Vulkan)をインストールするために下記をそれぞれmake install
```
git clone https://github.com/vsg-dev/VulkanSceneGraph.git
git clone https://github.com/vsg-dev/vsgXchange.git
git clone https://github.com/vsg-dev/vsgImGui.git
```

vsgのもろもろをインストールした後に、chronoをビルドし、描画ありのDEMサンプルを起動したら以下のエラーが発生した。
```
FATAL: VulkanSceneGraph not compiled with GLSLang, unable to compile shaders.
terminate called after throwing an instance of 'vsg::Exception'
```

Vulkanを起動するにはほかにもいろいろと依存パッケージがあるそう。
調べてみるとvsgのそれぞれのパッケージのビルド時に以下のワーニングが出ているのが原因らしい。glslcがインストールされていないことが原因らしい。
```sh
CMake Warning at CMakeLists.txt:52 (message): glslang not found. ShaderCompile support disabled.
```

以下のコマンドを実施してインストールした。

```
apt install -y glslang-dev glslang-tools spirv-tools
apt install -y wget gnupg lsb-release

wget -qO- https://packages.lunarg.com/lunarg-signing-key-pub.asc \
 | gpg --dearmor > /usr/share/keyrings/lunarg.gpg

echo "deb [signed-by=/usr/share/keyrings/lunarg.gpg] \
https://packages.lunarg.com/vulkan/1.3.280 jammy main" \
> /etc/apt/sources.list.d/lunarg-vulkan.list

apt update
apt install -y vulkan-sdk
```

以下のコマンドでインストールを確認
```sh
glslc --version
```
`glslc`が入っていると確認したら、vsg関連のパッケージをビルド、インストールをやり直す。
そして、chronoをビルドしなおす。

そうするとdemのサンプルをguiありで実行することができた。

![project_chrono_dem]({{site.baseurl}}/images/demo_DEM_mixer.gif)

## 参考

- [ccmakeのすすめ](https://alt-native.hatenadiary.org/entry/20121224/1356319986)
- [chrono_tutorials(ishigami-lab)](https://github.com/ishigami-lab/chrono_tutorials)
- [A Review of Physics Simulators for Robotic Applications](https://ieeexplore.ieee.org/abstract/document/9386154)
- https://developer.nvidia.com/cuda-downloads
- https://developer.nvidia.com/designworks/optix/downloads/legacy
- https://forums.developer.nvidia.com/t/problem-running-optix-7-6-in-wsl/239355
- https://forums.developer.nvidia.com/t/optix-on-wsl-windows-subsystem-for-linux/140297/23?page=2