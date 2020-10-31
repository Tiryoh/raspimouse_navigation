# raspimouse_navigation

Raspberry Pi MouseでROSのNavigation Stackを使ってナビゲーションをするためのパッケージです。

地図の生成には[rt-net/raspimouse_ros_examples]にある`slam_gmapping.launch`を使うことができます。

[Raspberry Pi Mouse V3](https://rt-net.jp/products/raspberrypimousev3/)を用いた実機でのナビゲーションと
[rt-net/raspimouse_sim](https://github.com/rt-net/raspimouse_sim)を用いたシミュレータ上でのナビゲーション両方で動作確認をしています。

## 実機
### 動作環境
#### Raspberry Pi Mouse
##### ハードウェア

* [Raspberry Pi Mouse V3](https://rt-net.jp/products/raspberrypimousev3/)
  * with Raspberry Pi 3B, 4B
  * with LIDAR
    * RPLIDAR
    * LDS-01
    * URG-04LX-UG01

##### ソフトウェア

* OS
  * [Ubuntu 'classic' 18.04 LTS](https://wiki.ubuntu.com/ARM/RaspberryPi)
* Device Driver
  * [rt-net/RaspberryPiMouse](https://github.com/rt-net/RaspberryPiMouse)
* ROS
  * [Melodic](https://wiki.ros.org/melodic/Installation/Ubuntu)
* ROS Packages
  * [ryuichiueda/raspimouse_ros_2](https://github.com/ryuichiueda/raspimouse_ros_2)
  * [rt-net/raspimouse_ros_examples](https://github.com/rt-net/raspimouse_ros_examples)

#### Remote PC

* OS
  * [Ubuntu 18.04 LTS](https://www.ubuntulinux.jp/News/ubuntu1804-ja-remix)
* ROS
  * [Melodic](https://wiki.ros.org/melodic/Installation/Ubuntu)
* ROS Packages
  * [ryuichiueda/raspimouse_ros_2](https://github.com/ryuichiueda/raspimouse_ros_2)
  * [rt-net/raspimouse_ros_examples](https://github.com/rt-net/raspimouse_ros_examples)

### インストール

```sh
# ROSパッケージのダウンロード
cd ~/catkin_ws/src
git clone https://github.com/ryuichiueda/raspimouse_ros_2.git
git clone https://github.com/rt-net/raspimouse_ros_examples.git
git clone https://github.com/Tiryoh/raspimouse_navigation.git

# 依存パッケージのインストール
rosdep install -r -y -i --from-paths .

# make & install
cd ~/catkin_ws && catkin build
source devel/setup.bash
```

### 使い方

Raspberry Pi Mouse

```sh
roslaunch raspimouse_navigation robot.launch
```

Remote PC

```sh
roslaunch raspimouse_navigation pc.launch map_file:="/path/to/mapfile"
```

## シミュレータ
### 動作環境
#### ソフトウェア

* OS
  * [Ubuntu 18.04 LTS](https://www.ubuntulinux.jp/News/ubuntu1804-ja-remix)
* ROS
  * [Melodic](https://wiki.ros.org/melodic/Installation/Ubuntu)
* ROS Packages
  * [ryuichiueda/raspimouse_ros_2](https://github.com/ryuichiueda/raspimouse_ros_2)
  * [rt-net/raspimouse_ros_examples](https://github.com/rt-net/raspimouse_ros_examples)
  * [rt-net/raspimouse_sim](https://github.com/rt-net/raspimouse_sim)

### インストール

```sh
# ROSパッケージのダウンロード
cd ~/catkin_ws/src
git clone https://github.com/ryuichiueda/raspimouse_ros_2.git
git clone https://github.com/rt-net/raspimouse_ros_examples.git
git clone https://github.com/rt-net/raspimouse_sim.git
git clone https://github.com/Tiryoh/raspimouse_navigation.git

# 依存パッケージのインストール
rosdep install -r -y -i --from-paths .

# make & install
cd ~/catkin_ws && catkin build
source devel/setup.bash
```

### 使い方

```sh
roslaunch raspimouse_navigation pc.launch map_file:="/path/to/mapfile"
```

## ライセンス

This repository is licensed under the MIT License, see [LICENSE](./LICENSE).