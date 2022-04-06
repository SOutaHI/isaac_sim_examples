# 概要
Isaac Sim上に配置したLiDARからのlazer_scanをros topicとして発行します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_ros_sensors.html

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
Isaac Sim上に配置したLiDARからのlazer_scanをros topicとして発行します。
シーンファイルとして、Turtlebotを追加したシーンファイルを使用します。
シーンファイルはこちらからダウンロードできます。

1. シーンのロード
2. ROS LiDARの追加
3. Topicの確認

## 1. シーンのロード
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンをロードする
Isaac Simの下部にあるContentの中から、”ダウンロードしたディレクトリ” > simple_room_apriltag_with_camera.usdをダブルクリックします。
![](https://storage.googleapis.com/zenn-user-upload/1a2ec20e8638-20220406.png)
![](https://storage.googleapis.com/zenn-user-upload/962396b7c7f1-20220406.png)

## 2. ROS LiDARの追加
### 2.1 LiDARを追加する
メニューバーのCreate > Isaac > Sensors > Lidar > Rotatingを選択します。
![](https://storage.googleapis.com/zenn-user-upload/bda05619aa39-20220406.png)

右側のStageの中で追加したLiDARをWorld > turtlebot3_burger > base_scanの下にドラッグアンドドロップします。
![](https://storage.googleapis.com/zenn-user-upload/f95efce39599-20220406.png)

右側のStageの中で追加したLiDARを選択します。
Stage下部のTransformの値を次の通りに変更します。

- translateのXを0.0にする
- translateのYを0.0にする
- translateのZを0.0にする
![](https://storage.googleapis.com/zenn-user-upload/2d9220e6fb28-20220406.png)

Stage下部のpropertyのRaw USD propertiesを開きます。
設定値の中で次の値を変更します。

- maxRangeに25を設定します
- drawLinesにチェックを入れる
![](https://storage.googleapis.com/zenn-user-upload/e39560e0af34-20220406.png)

この状態で、Viewportの左側のPLAYボタンを押すと、LiDARのレーザ光が出力されます。
![](https://storage.googleapis.com/zenn-user-upload/652402114268-20220406.png)

### 2.2 ROS LiDARを追加する
メニューバーのCreate > Isaac > ROS > Lidarを選択します。
![](https://storage.googleapis.com/zenn-user-upload/c233d8c6cb0d-20220406.png)

右側のStageの中で、追加したROS_LiDARを選択します。
![](https://storage.googleapis.com/zenn-user-upload/7d6e3f3d3552-20220406.png)

選択した状態で、Stage下部のpropertyのRaw USD propertiesを開きます。
Raw USD propertiesの中で、lidarPrimを選択し、Stageの中の/world/turtlebot3_burger/base_scan/Lidarを選択します。
![](https://storage.googleapis.com/zenn-user-upload/5c7428264de1-20220406.png)

設定値の中で次の値を変更します。

- pointCloudEnabledにチェックを入れる
- lazerScanEnabledにチェックを入れる
![](https://storage.googleapis.com/zenn-user-upload/68272701e0fe-20220406.png)

また、ROS Camera、ROS Camera_1, ROS_LIDARのRaw USD propertiessのframeIdを”turtle”に変更します。

## 3. Topicの確認
新たにterminalを開き、roscoreを起動します。
![](https://storage.googleapis.com/zenn-user-upload/3af54e779012-20220406.png)

この状態で、Viewportの左側のPLAYボタンを押すと、各種topicが発行されます。
![](https://storage.googleapis.com/zenn-user-upload/bbe03b079852-20220406.png)

また、新たにterminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ cd ~/.local/share/ov/pkg/isaac_sim-2021.2.1/ros_workspace/
$ rosdep install -i --from-paths ./src/
$ caktin b
$ source devel/setup.bash
$ rviz -d src/isaac_tutorials/rviz/camera_lidar.rviz
~~~

rvizにカメラとLiDARから取得できる情報が表示されます。
![](https://storage.googleapis.com/zenn-user-upload/3a1f7e9e075c-20220406.png)



