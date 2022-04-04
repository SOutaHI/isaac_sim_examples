# 概要
Isaac Sim上に配置したカメラにおける画像をros topicとして発行します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_ros_camera.html

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

1. シーンのロード
2. ROS Cameraの追加
3. Topicの確認

## 1. シーンのロード
Isaac Simの下部にあるContentの中から、Isaac > Samples > ROS > Scenario > simple_room_apriltag.usd.をダブルクリックします。

## 2. ROS Cameraの追加
### 2.1 camera_1用のROS Cameraを追加する
メニューバーのCreate > Isaac > ROS > Cameraを選択します。

右側のStageの中で、追加したROS_Cameraを選択します。

選択した状態で、Stage下部のpropertyのRaw USD propertiesを開きます。


Raw USD propertiesの中で、cameraPrimを選択し、Stageの中の/world/Camera_1を選択します。


新たにterminalを開き、roscoreを起動します。

この状態で、Viewportの左側のPLAYボタンを押すと、各種topicが発行されます。

また、新たにterminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ rosrun rqt_image_viewer rqt_image_viewer
~~~

topicに/rgbを選択すると、カメラからの画像を取得することができます。

### 2.2 camera_2用のROS Cameraを追加する
次に2つ目のカメラのROS Cameraを追加します。
操作方法は1つ目のカメラと同様です。

まず、メニューバーのWindow > New Viewport windowを選択します。
ポップアップしたWindowを1つ目のViewportの隣に追加します。

追加した2つ目のViewportのカメラアイコンをｄクリックし、Camera_2を選択します。

メニューバーのCreate > Isaac > ROS > Cameraを選択します。

右側のStageの中で、追加したROS_Cameraを選択します。

選択した状態で、Stage下部のpropertyのRaw USD propertiesを開きます。


Raw USD propertiesの中で、cameraPrimを選択し、Stageの中の/world/Camera_2を選択します。

新たにterminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ rosrun rqt_image_viewer rqt_image_viewer
~~~

topicに/rgb2を選択すると、カメラからの画像を取得することができます。

### 2.3 Depth画像の発行
右側のStageの中で、ROS_Cameraを選択します。
選択した状態で、Stage下部のpropertyのRaw USD propertiesを開きます。

depthEnabledにチェックを入れます。

新たにterminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ rosrun rqt_image_viewer rqt_image_viewer
~~~

topicに/depthを選択すると、カメラからのdepth画像を取得することができます。


