# 概要
Isaac Sim上に配置したCameraのTFをros topicとして発行します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_ros_tf.html

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
Isaac Sim上に配置したCameraのTFをros topicとして発行します。
シーンファイルとして、Turtlebotを追加したシーンファイルを使用します。
シーンファイルはこちらからダウンロードできます。

1. シーンのロード
2. ROS PoseTreeの追加
3. Topicの確認

## 1. シーンのロード
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンをロードする
Isaac Simの下部にあるContentの中から、”ダウンロードしたディレクトリ” > simple_room_apriltag_with_turtlebot3_with_camera_and_lidar.usdをダブルクリックします。

## 2. ROS PoseTreeの追加
### 2.1 ROS PoseTreを追加する
メニューバーのCreate > Isaac > ROS > Pose Treeを選択します。

右側のStageの中で、追加したROS_PoseTreeRを選択します。

選択した状態で、Stage下部のpropertyのRaw USD propertiesを開きます。

Raw USD propertiesの中で、 targetPrimsを選択し、Stageの中のCamera_1とCamera_2 を選択します。


## 3. Topicの確認
新たにterminalを開き、roscoreを起動します。

この状態で、Viewportの左側のPLAYボタンを押すと、各種topicが発行されます。

また、新たにterminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ rostop list
~~~

TFが発行されていることが確認できます。







