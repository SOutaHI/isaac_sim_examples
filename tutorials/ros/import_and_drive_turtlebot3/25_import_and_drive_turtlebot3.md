# 概要
Isaac Sim上でTurtlebot3のURDFをロードし、ROSのTopicから車輪の速度指令値を発行し、TurtleBot3を動かします。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_ros_turtlebot.html

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
Isaac Sim上でTurtlebot3のURDFをロードし、ROSのTopicから車輪の速度指令値を発行し、TurtleBot3を動かします。

1. turtlebotのビルド
2. turtlebotのURDFの生成
3. turtlebotのURDFのインポート
4. Differential Drive Bridgeの追加
5. 車輪の速度指令値を発行する

## 1. turtlebotのビルド
はじめに、turtlebotのリポジトリをクローンします。

~~~ bash:shell
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3.git
~~~


## 2. turtlebotのURDFの生成
次に、turtlebotのxacroからurdfを生成します。

~~~ bash:shell
$ cd ./turtlebot3/turtlebot3_description/urdf
$ rosrun xacro xacro -o turtlebot3_burger.urdf turtlebot3_burger.urdf.xacro
~~~

上記のコマンドを実行すると、turtlebot3_burgerのurdfが生成されます。

## 3. turtlebotのURDFのインポート
### 3.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 3.2 ros extensionをEnableにする
メニューバーのwindow > Extensonsを選択します。

ポップアップしたWindowの左上の検索窓に”ros”と入力し、Enterキーを押します。

検索結果が表示され、ROS BRIDGEとROS UIのExtensionがEnable状態になっていることを確認します。

Enableになってない場合には、各Extensionの横のトグルバーをクリックし、Enable状態に変更します。

### 3.3 シーンのロード
Isaac Simの下部にあるContentの中から、Isaac > Samples > ROS > Scenario > simple_room_apriltag.usd.をダブルクリックします。

### 3.4 turtlebotのURFDをロードする
メニューバーのIsaac Utils > URDF Importerを選択します。

ポップアップした”URDF Importer”のWindowをViewport下部のConsoleの右隣に追加します。

URDF Importerの設定を次の通りに変更します。

- Fix Bsseのチェックを外す
- Joint Drive TypeにVelocityを指定する

次に、”SLECT AND INPORT”を選択し、先ほど生成したturtlebot3のURDFを指定します。

インポートすると、テーブルの上にturtlebotが現れます。
次に台車を制御するため、Viewport内でturtlebotをドラッグし、床面におろします。

また、インポート際に/World外にインポートした場合には、Stageの中でturtlebotをドラッグし、/worldの下側に移動させます。

## 4. Differential Drive Bridgeの追加
メニューバーのCreate > ROS > Differential Baseを選択します。


右側のStageの中で、追加したROS_DifferentialBaseを選択します。

選択した状態で、Stageの下側にあるPopertyの中で、Raw USD Propertiesを開きます。

chassisPrimを選択し、stage中のturtlebot3_burgerを選択します。

また、他の設定値を次の値に変更します。

- leftWheelJointNameに”wheel_left_joint”を入力する
- rightWheelJointNameに”wheel_right_joint”を入力する
- maxSpeedに0.22を入力する
- wheelBaseに0.16を入力する
- wheelRadiusに0.025を入力する


## 5. 車輪の速度指令値を発行する

新たにterminalを開き、roscoreを起動します。

この状態で、Viewportの左側のPLAYボタンを押すと、各種topicが発行されます。

また、新たにterminalを開き、以下のコマンドを実行し、cmd_vel topicを発行するnodeを立ち上げます。

~~~ bash:shell
$ rosrun teleop_twist_keyboard teleop_twist_keyboard.py
~~~

実行できない場合には、teleop-twist-keyboardをgithubからクローンしbuildするか、aptからインストールする必要があります。
実行すると、対話式のプログラムになっており、cmd_velのTopicがキーボードの入力から操作することができます。


キーを入力すると、turtlebotが動くことを確認できます。


