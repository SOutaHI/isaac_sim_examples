# 概要
Isaac Sim上でSceneと複数のRobotを配置し、ROS Nvigation Stackを用いてRobotを動かします。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_ros_multi_navigation.html

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
Isaac Sim上でSceneと複数のRobotを配置し、ROS Nvigation Stackを用いてRobotを動かします。
まず、ROS Nvigation Stack上で使用するOccupacy MapをIssac Sim上で作成します。
作成したOccupacy Mapを用いて、Navigationを実行します。

1. ROS Navigationのインストール
2. Occupacy Mapの生成
3. Navigationの実行

## 1. ROS Navigationのインストール
### 1.1 ROS Navigationをインストールする
terminalで次のコマンドを実行します。

~~~ bash:shell
$ sudo apt-get install -y ros-noetic-navigation
~~~

## 2. Occupacy Mapの生成
### 2.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 2.2 シーンをロードする
メニューバーのIsaac Examples > ROS > Navigation > Hospitalを選択します。
![](https://storage.googleapis.com/zenn-user-upload/1be3f9b9171b-20220406.png)
![](https://storage.googleapis.com/zenn-user-upload/c86d3baa7346-20220406.png)

この状態で、Viewportの左側のPLAYボタンを押します。

viewportのカメラアイコンをクリックし、Perspectiveを選択します。
また、ドロップダウンしたメニューの中から”Top”を選択します。
![](https://storage.googleapis.com/zenn-user-upload/9c45918b02bb-20220406.png)

### 2.3 Occupacy Mapを生成する
メニューバーのIsaac Utils > Occupancy Mapを選択します。
![](https://storage.googleapis.com/zenn-user-upload/f293a51edca6-20220406.png)

ポップアップしたWindowをViwport下部にあるConsoleの右隣にドラッグし、追加します。


Occupacy Mapの設定を次の通りに変更します。

- originのXを0.0にする
- originのYを0.0にする
- originのZを0.0にする
- Lower BpundのZを10.0にする
- Upprer BoundのZを62.0にする


右側のStage上で、Hospitalを選択します。
![](https://storage.googleapis.com/zenn-user-upload/ba4004387397-20220406.png)

この状態で、Occupacy Mapの”BOUND SELECTION”をクリックします。
クリックすると、map parametersがHospitalに合うようにアップデートされます。
![](https://storage.googleapis.com/zenn-user-upload/a5b52e09aa28-20220406.png)

次に、Occupacy Mapの”CALCULATE” > "VISUALIZE IMAG"をクリックします。
![](https://storage.googleapis.com/zenn-user-upload/9577d41a02c4-20220406.png)

表示されたOccupacy MapのWindowの中で次の設定を変更します。

- rotationを180にする
- Coordinate Typeを”ROS Occupancy Map Parameters File (YAML)”にする

設定後、Occupacy Mapの”RE-GENERATE IMAGE”を選択する。
![](https://storage.googleapis.com/zenn-user-upload/ee08f37216b4-20220406.png)

生成したImage（Occupacy Map）を保存します。
Imageの保存名は”carter_hospital_navigation.yaml”に設定します。

この状態で、Viewportの左側のSTOPボタンを押します。

## 3. Navigationの実行
### 3.1 シーンをロードする
メニューバーのIsaac Examples > ROS > Multi Robot Navigation > Hospitalを選択します。
![](https://storage.googleapis.com/zenn-user-upload/ddbf76f2395e-20220406.png)
![](https://storage.googleapis.com/zenn-user-upload/65fcc1d5c98f-20220406.png)

新たにterminalを開き、roscoreを起動します。
この状態で、Viewportの左側のPLAYボタンを押します。
![](https://storage.googleapis.com/zenn-user-upload/a1e30e5fd3fc-20220406.png)

### 3.2 NavigationのLaunchを実行する

新たにterminalを開き、次のコマンドを入力します

~~~ bash:shell
$ cd ~/.local/share/ov/pkg/isaac_sim-2021.2.1/ros_workspace/
$ source devel/setup.bash
$ roslaunch carter_2dnav multiple_robot_carter_navigation.launch env_name:=hospital
~~~
![](https://storage.googleapis.com/zenn-user-upload/dcfea88d0422-20220406.png)

rviz上で、2D Nav Goalを使用すると、ロボットが動くことが確認できます。
![](https://storage.googleapis.com/zenn-user-upload/ddcdafb326dd-20220406.png)
![](https://storage.googleapis.com/zenn-user-upload/93911e0aabb8-20220406.png)







