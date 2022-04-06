# 概要
Isaac Sim上でFrankaを配置し、Moveitを用いてRobotを動かします。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_ros_moveit.html

# 実行環境

- インストール実行環境

| unit       |       specification | 
|:-----------------:|:------------------:|
| CPU         | i9-11900H |  
| GPU         | GeForce RTX 3080 Laptop|  
| RAM         | 32GB | 
| OS         | Ubuntu 20.04.3 LTS  |

- Nvidia Driverバージョン
   - 510.39.01
- Issac simバージョン
   - 2021.2.1


# 手順
Isaac Sim上でFrankaを配置し、Moveitを用いてRobotを動かします。
moveitはros側から実行し、rviz上でmoveitを使用します。

1. moveitのインストール
2. シーンのロード
3. moveitの実行
4. トラブルシューティング

## 1. moveitのインストール
### 1.1 moveitをインストールする
terminalで次のコマンドを実行します。

~~~ bash:shell
$ sudo apt-get install -y ros-noetic-moveit
~~~

## 2. シーンのロード
### 2.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 2.2 シーンをロードする
メニューバーのIsaac Examples > ROS > MoveItを選択します。

この状態で、Viewportの左側のPLAYボタンを押します。


## 3. moveitの実行

新たにterminalを開き、次のコマンドを入力します

~~~ bash:shell
$ cd ~/.local/share/ov/pkg/isaac_sim_(version)/ros_workspace/
$ source devel/setup.bash
$ roslaunch isaac_moveit franka_isaac_execution.launch
~~~

最後のコマンドを実行すると、rvizが立ち上がります。


rviz上で、左側にある”ADD”をクリックし、ポップアップしたWindowの中から”MotionPlaning”を選択します。

rviz上で、左下にある”MotionPlanning”の”PlanningGroup”を”panda_arm”に変更します。

rviz上でArmの姿勢を変更し、”MotionPlanning”のPlan > Executeを選択すると、Isaac Sim上で動作することを確認できます。


## トラブルシューティング

次のコマンドを実行する際にエラーが発生し、実行できないことがありました。

~~~ bash:shell
$ roslaunch isaac_moveit franka_isaac_execution.launch
~~~

エラー内容は次の通りです。

このエラーはFrankaのPackageに含まれているlaunchのエラーであり、[該当launchファイル](https://answers.ros.org/question/384900/failed-to-lunch-this-command/
https://github.com/ros-planning/panda_moveit_config/blob/noetic-devel/launch/planning_context.launch})のエラー箇所を修正することで解決できました。

こちらを参考にしました。
https://answers.ros.org/question/384900/failed-to-lunch-this-command/


まず、frankaのpackageをクローンします。

~~~ bash:shell
$ cd ~/.local/share/ov/pkg/isaac_sim_(version)/ros_workspace/src/
$ git clone git@github.com:ros-planning/panda_moveit_config.git
~~~

エディタで該当launchファイルを開き、コードを変更します。
変更部分は11行目です。元の部分を次の内容に置き換えます。

~~~ xml:xml
<param if="$(eval arg('load_robot_description') and arg('load_gripper'))" name="$(arg robot_description)" command="$(find xacro)/xacro '$(find franka_description)/robots/panda_arm.urdf.xacro' hand:=true"/>
~~~

buildすると、実行できるようになります。

~~~ bash:shell
$ cd ~/.local/share/ov/pkg/isaac_sim_(version)/ros_workspace/
$ catkin b
$ source devel/setup.bash
$ roslaunch isaac_moveit franka_isaac_execution.launch
~~~