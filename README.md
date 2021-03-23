# gazebo_tutorial
my urdf &amp; gazebo tutorial

実行方法

/urdf
arm_robot_gazebo.urdf
アームロボをgazeboで表示するためのファイル

/xacro

diff_wheel_robo1.xacro
差動二輪ロボのモデル．rvizで確認できる
$ cd gazebo_tutorial/xacro
$ roslaunch gazebo_tutorial urdf_display.launch model:=diff_wheel_robo1.xacro

diff_wheel_robo2.xacro
上のファイルにgazebo用の追記をした．これでgazebo上に生成できる．
$ roslaunch gazebo_tutorial diff_wheel_robo_gzb.launch

diff_wheel_robo3.xacro
gazeboとrosの通信をさせる．ros_controlというgazeboプラグインを使用してシミュレーターとして動かす．
<robotNameSpace>でロボットの名前（ファイルのはじめに宣言した名前）を入れる．プラグインを使うときのおまじない
$ roslaunch gazebo_ttorial diff_wheel_robo_gzb3.launch

/launch
diff_wheel_robo_gzb3.launch
16行目以降で設定ファイルの読み込みを行う．