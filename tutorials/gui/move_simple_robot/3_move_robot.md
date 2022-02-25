# 概要
Nvidia Issac simのGUIの機能を使用し、簡単な2輪ロボットのモデルを動かします。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_gui_simple_robot.html

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

大まかな手順は次の通りです。

1. 2輪ロボットモデルにJoint設定を追加
2. 2輪ロボットにJointDriveを追加

以下内容のシーンファイルはこちらからダウンロードできます。(準備中）

## 1. 2輪ロボットモデルにJoint設定を追加
こちらのページに記載してある手順を進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_gui_simple_robot.html#isaac-sim-app-tutorial-gui-simple-robot

### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンをロードする
以前作成した2輪のロボットモデルをロードします。
シーンファイルは[こちらからダウンロード]()できます。
メニューバーのFile ＞ Open を選択します。
該当ファイルを選択し、シーンをロードします。

## 1.3 Primの追加
シーン内にPrimを追加します。Primはコンテナオブジェクトであり、Stageの中に名前空間を作ることができます。
右側のStageタブの中で右クリックし、Create＞Xformを選択します。
![](https://storage.googleapis.com/zenn-user-upload/59255c95ebaf-20220219.png)

追加したXformを右クリックし、Renameを選択します。
名称は次の名称に変更します。
- mock_robot

右側のStageタブの中で、CubeとCylinders, Physics Material,　Looksフォルダーを選択し、mock_robotの下にドラッグアンドドロップで移動させます。
![](https://storage.googleapis.com/zenn-user-upload/77ba046afe56-20220219.png)
![](https://storage.googleapis.com/zenn-user-upload/1b864e3db176-20220219.png)

移動後、CubeとCylindersをそれぞれ右クリックし、次の名称にrenameします。
- cube > body
- cylinders > wheel_left
- cylinders_01 > wheel_right

![](https://storage.googleapis.com/zenn-user-upload/d52c2837bab3-20220219.png)

## 1.4 Jointの追加
BodyとWheel間にJointを追加します。
右側にあるStageの中で、”wheel_left”を選択します。
選択した状態で、Ctrl＋Shiftを押しながら、bodyを選択します。

右側のStageタブの中で右クリックし、Create > Physics > Joints > Revoluteを選択します。
![](https://storage.googleapis.com/zenn-user-upload/8763dbb4b13d-20220219.png)
![](https://storage.googleapis.com/zenn-user-upload/0fa5e85b3b88-20220219.png)

Bodyの下のRevoluteJointを右クリックし、renameします。
- RevoluteJoint > wheel_joint_left

![](https://storage.googleapis.com/zenn-user-upload/003c0e8e0551-20220219.png)

右側のStageタブの中で、wheel_joint_leftを選択します。
選択した状態で、右下にあるPropertyの中から、次の値を変更します。
- AxisをYにする
- Local Rotation 1のXを-90にする

![](https://storage.googleapis.com/zenn-user-upload/e8174b3b0c48-20220219.png)

左タイヤに対して行ったJointの設定を右タイヤについても同様に行います。

![](https://storage.googleapis.com/zenn-user-upload/ef42b2ab1384-20220219.png)

各wheelの設定完了後、右側のツールバーのシミュレーションstartボタンを押すと、関節関係が保たれたまま、重力がかかり落下することを確認します。

![](https://storage.googleapis.com/zenn-user-upload/41b5026b28b7-20220219.png)

## 2. Joint Driveの追加
Jointを制御するための設定を追加します。
右側のStageタブの中で、wheel_joint_leftとwheel_joint_rightを選択します。
選択した状態で、右下のPropertyの”＋Add”をクリックし、Physics > Angular Driveを選択します。

![](https://storage.googleapis.com/zenn-user-upload/936887dd344a-20220219.png)

それぞれのJointにおいて、ダンパと指令各速度値を設定します。
右側のStageタブの中で、wheel_joint_leftを選択します。
選択した状態で、右下のPropertyの中のDrive ＞Angularの次の値を変更します。
- Dampingを1e4にする
- Target Velocityを2000にする
![](https://storage.googleapis.com/zenn-user-upload/c2d6a4ca37eb-20220219.png)

wheel_joint_rightにおいても同様の手順を行い、ダンパと指令各速度値を設定します。

両方のWheelにおいて設定が完了し、左側のツールバーのシミュレーションStartボタンを押すと、ロボットが進むことを確認します。

![](https://storage.googleapis.com/zenn-user-upload/657833fbba52-20220219.gif)
