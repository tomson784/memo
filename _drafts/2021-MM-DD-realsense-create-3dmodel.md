---
layout: default
title:  "Realsenseの点群情報から3Dモデルを作成する"
date:   2021-06-21
categories: ros realsense 3dmodel
---

# 必要なもの

realsense2_camera: Follow the installation guide in: https://github.com/intel-ros/realsense.
imu_filter_madgwick: `sudo apt-get install ros-kinetic-imu-filter-madgwick`
rtabmap_ros: `sudo apt-get install ros-kinetic-rtabmap-ros`
robot_localization: `sudo apt-get install ros-kinetic-robot-localization`

## rosbagデータの保存

```bash
roslaunch realsense2_camera opensource_tracking.launch
```

```bash
rosbag record -O my_bagfile_1.bag /camera/aligned_depth_to_color/camera_info  camera/aligned_depth_to_color/image_raw /camera/color/camera_info /camera/color/image_raw /camera/imu /camera/imu_info /tf_static
```

## 点群データへの書き出し

```bash
roscore >/dev/null 2>&1 &
rosparam set use_sim_time true
rosbag play my_bagfile_1.bag --clock
roslaunch realsense2_camera opensource_tracking.launch offline:=true
```

```bash
rosrun pcl_ros pointcloud_to_pcd input:=/rtabmap/cloud_map
```

## 参考
- [SLAM with D435i](https://github.com/IntelRealSense/realsense-ros/wiki/SLAM-with-D435i)
- [3Dデータの修正方法](https://fabble.cc/fablabdazaifu/3dxxxxxxxx)
- [オブジェクトの3D モデル化](http://www.aerotap.com/DOCS/aeroCAM3DView.Help/aeroCAM3DView.Help/jp/3DModel.htm)
- [MeshLabでPCDデータをメッシュに変換、書き出し](http://www.pointcloud.jp/blog_n07/)
- [Virtual Tsukuba Challenge on Unity について](https://www.slideshare.net/UnityTechnologiesJapan002/virtual-tsukuba-challenge-on-unity-238911580)
