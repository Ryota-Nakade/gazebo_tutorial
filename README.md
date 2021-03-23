# gazebo_tutorial
my urdf &amp; gazebo tutorial

## 注意
Gazebo関連のファイルを実行する前に以下のコマンドを実行
```
sudo apt install ros-noetic-gazebo-ros
sudo apt install ros-noetic-gazebo-ros-control 
sudo apt install ros-noetic-ros-control
sudo apt install ros-noetic-ros-controllers
sudo apt install ros-noetic-position-controllers
sudo apt install ros-noetic-joint-state-controller

```

## 中身
* /config
    * diff_drive_converter.yaml
    * joint_state_controller.yaml

* /rviz

* /src
    * joint_publisher.cpp
    * robot_sim.cpp
    * tf2_broadcaster.cpp
    * tf2_listener.cpp
    * odom_tf_converter.cpp

* /stl : STLファイルの保存場所

* /urdf

    * arm_robot_gazebo.urdf : アームロボをgazeboで表示するためのファイル
    * robot_sim.urdf
    * simple_body1.urdf
    * simple_body2.urdf
    * simple_body3.urdf
    * simple_body4.urdf
    * simple_body1_gazebo.urdf
    * stl_body.urdf

* /xacro
    * basic1.xacro
    * basic2.xacro
    * basic3.xacro
    * basic4.xacro
    * basic5_h.xacro
    * basic6.xacro

    * diff_wheel_robo1.xacro : 差動二輪ロボのモデル．rvizで確認できる

    ```
    $ cd gazebo_tutorial/xacro
    $ roslaunch gazebo_tutorial urdf_display.launch model:=diff_wheel_robo1.xacro
    ```

    * diff_wheel_robo2.xacro : 上のファイルにgazebo用の追記をしたもの．
    これでgazebo上に生成できる．

    ```
    $ roslaunch gazebo_tutorial diff_wheel_robo_gzb.launch
    ```

    * diff_wheel_robo3.xacro : gazeboとrosの通信をさせる．ros_controlというgazeboプラグインを使用してシミュレーターとして動かす．
`<robotNameSpace>`でロボットの名前（ファイルのはじめに宣言した名前）を入れる．プラグインを使うときのおまじない
    ```
    $ roslaunch gazebo_ttorial diff_wheel_robo_gzb3.launch
    ```

* /launch
    * arm_robot_gazebo.launch
    * diff_wheel_robo_gzb.launch

    * diff_wheel_robo_gzb3.launch : xacroをgazeboに表示する．16行目以降で設定ファイルの読み込みを行う．
    以下のコマンドでモデルに移動指示を出せる．

    ```
    $ rostopic pub -r 10 /dtw_robot/diff_drive_controller/cmd_vel geometry_msgs/Twist "linear:
    x: 0.4
    y: 0.0
    z: 0.0
    angular:
    x: 0.0
    y: 0.0
    z: 0.0"
    ```

    * diff_wheel_robo_gzb4.launch : gazebo上での位置(tf)をパブリッシュしてrvizで表示する．上のコマンドで移動させる．rvizでFixed Frameをodomにしないと，モデルが移動しない

    * gazebo_display.launch
    * joint_publisher1.launch
    * robot_sim.launch
    * tf2_broadcast_lister.launch
    * urdf_display.launch
    * xacro_display.launch