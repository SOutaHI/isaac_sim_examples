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
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンをロードする
Isaac Simの下部にあるContentの中から、Isaac > Samples > ROS > Scenario > simple_room_apriltag.usd.をダブルクリックします。
![](https://storage.googleapis.com/zenn-user-upload/113998f065a2-20220406.png)

## 2. ROS Cameraの追加
### 2.1 camera_1用のROS Cameraを追加する
メニューバーのCreate > Isaac > ROS > Cameraを選択します。
![](https://storage.googleapis.com/zenn-user-upload/316535e41f0b-20220406.png)

右側のStageの中で、追加したROS_Cameraを選択します。
![](https://storage.googleapis.com/zenn-user-upload/9ea167db017e-20220406.png)

選択した状態で、Stage下部のpropertyのRaw USD propertiesを開きます。
![](https://storage.googleapis.com/zenn-user-upload/f754637b26e8-20220406.png)

Raw USD propertiesの中で、cameraPrimを選択し、Stageの中の/world/Camera_1を選択します。
![](https://storage.googleapis.com/zenn-user-upload/6dc747c139e7-20220406.png)

新たにterminalを開き、roscoreを起動します。
![](https://storage.googleapis.com/zenn-user-upload/b26772ee6a88-20220406.png)

この状態で、Viewportの左側のPLAYボタンを押すと、各種topicが発行されます。
また、新たにterminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ rosrun rqt_image_viewer rqt_image_viewer
~~~
topicに/rgbを選択すると、カメラからの画像を取得することができます。
![](https://storage.googleapis.com/zenn-user-upload/464eb7ea7dd7-20220406.png)

### 2.2 camera_2用のROS Cameraを追加する
次に2つ目のカメラのROS Cameraを追加します。
操作方法は1つ目のカメラと同様です。

まず、メニューバーのWindow > New Viewport windowを選択します。
![](https://storage.googleapis.com/zenn-user-upload/313594b1a098-20220406.png)

ポップアップしたWindowを1つ目のViewportの隣に追加します。
![](https://storage.googleapis.com/zenn-user-upload/5a58d0007883-20220406.png)

追加した2つ目のViewportのカメラアイコンをｄクリックし、Camera_2を選択します。
![](https://storage.googleapis.com/zenn-user-upload/0015d2baccc6-20220406.png)

メニューバーのCreate > Isaac > ROS > Cameraを選択します。
![](https://storage.googleapis.com/zenn-user-upload/440f4b8d53c6-20220406.png)

右側のStageの中で、追加したROS_Cameraを選択します。
選択した状態で、Stage下部のpropertyのRaw USD propertiesを開きます。
Raw USD propertiesの中で、cameraPrimを選択し、Stageの中の/world/Camera_2を選択します。
![](https://storage.googleapis.com/zenn-user-upload/62eaa986cf5b-20220406.png)
![](https://storage.googleapis.com/zenn-user-upload/16f584e19f11-20220406.png)

新たにterminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ rosrun rqt_image_viewer rqt_image_viewer
~~~

topicに/rgb2を選択すると、カメラからの画像を取得することができます。
![](https://storage.googleapis.com/zenn-user-upload/91cb0ec98246-20220406.png)

### 2.3 Depth画像の発行
右側のStageの中で、ROS_Camera_01を選択します。
選択した状態で、Stage下部のpropertyのRaw USD propertiesを開きます。

depthEnabledにチェックを入れます。
![](https://storage.googleapis.com/zenn-user-upload/e0b0d99898fb-20220406.png)

新たにterminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ rosrun rqt_image_viewer rqt_image_viewer
~~~

topicに/depthを選択すると、カメラからのdepth画像を取得することができます。
![](https://storage.googleapis.com/zenn-user-upload/fa972de725ad-20220406.png)
