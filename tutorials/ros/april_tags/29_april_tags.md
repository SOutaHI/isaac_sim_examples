# 概要
Isaac Sim上に配置したApril Tagsをros topicとして発行し, detectionの処理を実行します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_ros_apriltag.html

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
Isaac Sim上に配置したApril Tagsをros topicとして発行し, detectionの処理を実行します。
detectionの処理はros側で実行します。

1. シーンのロード
2. detection処理の実行
3. Topicの確認

## 1. シーンのロード
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンをロードする
メニューバーのIsaac Examples > ROS > April Tagを選択します。

新たにterminalを開き、roscoreを起動します。

この状態で、Viewportの左側のPLAYボタンを押すと、各種topicが発行されます。


## 2. detection処理の実行
### 2.1 detection用のLaunchを実行する

また、新たにterminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ cd ~/.local/share/ov/pkg/isaac_sim_2(version)/ros_workspace/
$ cd ./src
$ git clone https://github.com/AprilRobotics/apriltag.git   
$ git clone https://github.com/AprilRobotics/apriltag_ros.git 
$ caktin b
$ source devel/setup.bash
$ roslaunch isaac_tutorials apriltag_continuous_detection.launch
~~~


## 3. Topicの確認
Detectionの結果をRviz上で確認します。
新たにterminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ cd ~/.local/share/ov/pkg/isaac_sim_2(version)/ros_workspace/
$ rviz -d src/isaac_tutorials/rviz/apriltag_config.rviz
~~~

Detectionされていることが確認できます。





