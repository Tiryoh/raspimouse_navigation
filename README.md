# raspimouse_navigation

Raspberry Pi MouseでROSの[Navigation Stack](https://wiki.ros.org/navigation)を使ってナビゲーションをするためのパッケージです。

地図の生成には[rt-net/raspimouse_ros_examples](https://github.com/rt-net/raspimouse_ros_examples)にある`slam_gmapping.launch`を使うことができます。

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

#### SLAM

##### Raspberry Pi Mouse

Raspberry Pi Mouse上で次のコマンドでノードを起動します。

```sh
# URG
roslaunch raspimouse_ros_examples mouse_with_lidar.launch urg:=true port:=/dev/ttyACM0

# RPLIDAR
roslaunch raspimouse_ros_examples mouse_with_lidar.launch rplidar:=true port:=/dev/ttyUSB0

# LDS
roslaunch raspimouse_ros_examples mouse_with_lidar.launch lds:=true port:=/dev/ttyUSB0
```

##### Remote PC

1枚目の端末でRaspberry Pi Mouse操作のためのteleop用launchを起動します。

```sh
roslaunch raspimouse_ros_examples teleop.launch key:=true mouse:=false
```

2枚目の端末で地図を作成するためのlaunchを起動します。

```sh
roslaunch raspimouse_ros_examples slam_gmapping.launch 
```

地図作成後、3枚目の端末で地図を保存します。

```sh
roscd raspimouse_navigation/maps
rosrun map_server map_saver -f mymap
```

#### Navigation

##### Raspberry Pi Mouse

Raspberry Pi Mouse上で次のコマンドでノードを起動します。

```sh
roslaunch raspimouse_navigation robot.launch
```

###### Remote PC

Remote PC上で次のコマンドでノードを起動します。

`map_file`には保存した地図のファイルを指定します。

```sh
roslaunch raspimouse_navigation pc.launch map_file:="$(rospack find raspimouse_navigation)/maps/mymap.yaml"
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

#### SLAM

1枚目の端末でRaspberry Pi Mouse Simulatorを起動します。

```sh
roslaunch raspimouse_gazebo raspimouse_with_willowgarage.launch xacro_option:="lidar:=urg lidar_frame:=laser" 
```

2枚目の端末でRaspberry Pi Mouse操作のためのteleop用launchを起動します。

```sh
roslaunch raspimouse_ros_examples teleop.launch key:=true mouse:=false
```

3枚目の端末で地図を作成するためのlaunchを起動します。

```sh
roslaunch raspimouse_ros_examples slam_gmapping.launch 
```

地図作成後、4枚目の端末で地図を保存します。

```sh
roscd raspimouse_navigation/maps
rosrun map_server map_saver -f mymap
```

#### Navigation

1枚目の端末でRaspberry Pi Mouse Simulatorを起動します。

```sh
roslaunch raspimouse_gazebo raspimouse_with_willowgarage.launch xacro_option:="lidar:=urg lidar_frame:=laser" 
```

2枚目の端末で次のコマンドでノードを起動します。
`map_file`には保存した地図のファイルを指定します。

```sh
roslaunch raspimouse_navigation pc.launch map_file:="$(rospack find raspimouse_navigation)/maps/mymap.yaml"
```

## ライセンス

This repository is licensed under the MIT License, see [LICENSE](./LICENSE).