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

    * ds4_twist_pub.cpp  
     : PS4のコントローラーからTwist型のメッセージを送信するためのノード


* /stl : STLファイルの保存場所
    * omni_barrel.stl  
     : オムニホイールのバレル部分
    * omni_housing.stl  
     : オムニホイールの本体
    * stl_part.stl  
     : なんのパーツかは不明．ただ表示させるためだけに使用



* /urdf

    * arm_robot_gazebo.urdf  
     : アームロボをgazeboで表示するためのファイル
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

    * diff_wheel_robo1.xacro  
     : 差動二輪ロボのモデル．rvizで確認できる

    ```
    $ cd gazebo_tutorial/xacro
    $ roslaunch gazebo_tutorial urdf_display.launch model:=diff_wheel_robo1.xacro
    ```

    * diff_wheel_robo2.xacro  
     : 上のファイルにgazebo用の追記をしたもの．
    これでgazebo上に生成できる．

    ```
    $ roslaunch gazebo_tutorial diff_wheel_robo_gzb.launch
    ```

    * diff_wheel_robo3.xacro  
     : gazeboとrosの通信をさせる．ros_controlというgazeboプラグインを使用してシミュレーターとして動かす．
    `<robotNameSpace>`でロボットの名前（ファイルのはじめに宣言した名前）を入れる．プラグインを使うときのおまじない
    ```
    $ roslaunch gazebo_ttorial diff_wheel_robo_gzb3.launch
    ```

    * move_macro.xacro  
     : センサ取り付け用の土台となるモデル

    * laser_macro.xacro  
     : LiDARシミュレーション用のxacro:macroを記述してある．`<min_angle>`と`<max_angle>`が視野角の設定部分．`<range>/<min>&<max>`の部分が測定距離の部分．単位はメートル．
    `<sensor type="">`と`<plugin name="">`の名前を変えることでGPUを使うかどうか変えられる．2Dのときはさほど変わらないが，3DLiDARになると重くなるので有効化が推奨．`<visualize>`を`true`にするとgazebo上でLiDARの視界が表示される．rvizで点群が得られないときは，これを`true`にして対象物に照射されているか確認する．2DLiDARの高さがあっていなくて赤外線が当たっていないということもある．

    * camera_macro.xacro  
     : カメラシミュレーション用のxacro:macroが記述されている．gazeboの仕様ではカメラがカメラ座標系のX軸方向を向く．一方，xacroファイルではhead_camera_linkの座標系をX軸を右に，Y軸を下に，Z軸を前方に取るように設定してある．この食い違いをすり合わせるために，`<pose>`にてカメラの向く方向を合わせている．

    * diff_robo_lidar.xacro  
     : move_macro.xacroとlaser_macro.xacroから1個のモデルを作るマクロ．

    * diff_robo_camera.xacro  
     : movw_macro.xacroとcamera_macro.xacroから1個のモデルを作るマクロ．

    * diff_robo_cam_li.xacro  
     : movw_macro.xacroとcamera_macro.xacroとlaser_macro.xacroから1個のモデルを作るマクロ．

    * odom_common.xacro  
     : xacroで使用する色の設定をする．

    * omni_wheel_set1.xacro  
     : オムニホイール用のxacro

    * single_omni.xacro  
     : オムニホイールを生成するxacro


* /launch
    * arm_robot_gazebo.launch
    * diff_wheel_robo_gzb.launch

    * diff_wheel_robo_gzb3.launch  
     : xacroをgazeboに表示する．16行目以降で設定ファイルの読み込みを行う．
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

    * diff_wheel_robo_gzb4.launch  
     : gazebo上での位置(tf)をパブリッシュしてrvizで表示する．上のコマンドで移動させる．rvizでFixed Frameをodomにしないと，モデルが移動しない

    * gazebo_display.launch
    * joint_publisher1.launch
    * robot_sim.launch
    * tf2_broadcast_lister.launch
    * urdf_display.launch
    * xacro_display.launch
    * diff_gzb_sensor.launch  
     : 各種センサを取り付けたモデルをgazeboとrvizで表示する．

    * diff_robo_sensor.launch  
         : LiDARを使う場合
        ```
        $ roslaunch gazebo_tutorial diff_gzb_sensor.launch
        ```  
         : カメラを使う場合
        ```
        $ roslaunch gazebo_tutorial diff_gzb_sensor.launch model:=camera
        ```

    * ds4_turtlesim.launch  
     : PS4コントローラーでturtlesimを動かすのを試してみる．