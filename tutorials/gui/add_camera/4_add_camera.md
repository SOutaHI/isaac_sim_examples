# 概要
Nvidia Issac simのGUIの機能を使用し、簡単な2輪ロボットのモデルにカメラを追加します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_gui_camera_sensors.html#isaac-sim-app-tutorial-gui-camera-sensors

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

1. シーンにカメラを追加
2. 2輪ロボットにカメラを追加

以下内容のシーンファイルはこちらからダウンロードできます。
()[]

## 1. シーンにカメラを追加
こちらのページに記載してある手順を進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_gui_camera_sensors.html#getting-started

### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンをロードする
以前作成した2輪のロボットモデルをロードします。
シーンファイルは[こちらからダウンロード]()できます。
メニューバーのFile ＞ Open を選択します。
該当ファイルを選択し、シーンをロードします。


## 1.3 カメラの追加
シーン内にカメラを追加します。
メニューバーのCreate ＞ Physics > Cameraを選択します。

選択後、右側のStageタブの中にCameraが追加されていることを確認します。


## 2. 2輪ロボットにカメラを追加
右側のStageタブの中でcameraを右クリックし、renameを選択します。
renameにより次の名称に変更します。
- camera > car_camera

### 2.1 Viewportの追加
カメラで撮影した画像を映し出すViewportを作成します。
メニューバーのWindow > New Viewport Windowを選択します。

選択後、生成したWindowをGUI内でドラッグアンドドロップで移動し、任意の位置に配置します。

配置したウィンドウ中のPerspectiveをクリックし、Cameraをcar_cameraにします。

### 2.2 2輪ロボットへのアタッチ
右側のStageタブの中でcar_cameraをドラッグアンドドロップでBodyの下に移動させます。


Bodyの下に追加したcar_cameraを選択します。
選択した状態で、右下にあるPropertyの中から、次の値を変更します。
- transformationのtransformのXを20にする
- transformationのtransformのYを0にする
- transformationのtransformのZを200にする
- transformationのorientationのXを0にする
- transformationのorientationのYを70にする
- transformationのorientationのZを90にする


カメラの位置および姿勢の設定が完了すると、ロボットの前のシーンがViewportに写し出されます。


この状態で、側のツールバーのシミュレーションPlayボタンを押し、ロボットが進んでいる状態でカメラ画像が変化していることを確認します。

