
# 概要
シミュレーション内で投射パターンを自由に変更することが可能なLIDARを使用します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_advanced_range_sensor_generic.html#

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

GUI上からの操作と、Python APIからシーンにLIDARを追加します。

1. Generic Range Sensorの追加（GUI）
2. Generic Range Sensorの設定を変更する


## 1. Generic Range Sensorの追加（GUI）
こちらのページに記載してある手順を進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_advanced_range_sensor_lidar.html#getting-started

### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンを作成する
メニューバーのCreate ＞ Physics > Phisics Sceneを選択します。
![](https://storage.googleapis.com/zenn-user-upload/7e39de2cccd2-20220219.png)


### 1.3 LIDARの追加
作成したシーン内に、Generic Range Sensorを追加します。
メニューバーのIsaac Examples > Sensors > Generic Range Sensorを選択します。
![](https://storage.googleapis.com/zenn-user-upload/e3f16e423da1-20220402.png)

ポップアップしたWindowにおいて、"Load Sensor"を選択します。
![](https://storage.googleapis.com/zenn-user-upload/44128040d7ac-20220402.png)

次に、ポップアップしたWindowにおいて、”Load Scene”を選択します。
![](https://storage.googleapis.com/zenn-user-upload/ae6b4678a770-20220402.png)

”Set Sensor Pattern”をすると、ExampleのSensor Patternが読み込まれます。
![](https://storage.googleapis.com/zenn-user-upload/3e744d3cffd6-20220402.png)

この状態で、Viewportの左側のPLAYボタンを押すと、Exampleのレーザパターンが照射されます。
![](https://storage.googleapis.com/zenn-user-upload/945aa72e761b-20220402.png)

## 2. Generic Range Sensorの設定を変更する
## 2.1 Generic Range Sensorのサンプルコードを開く

ポップアップしているウィンドウの右上にある3つのボタンの内、一番左側のOpen Source Codeボタンを選択します。
![](https://storage.googleapis.com/zenn-user-upload/116bfcb5b221-20220402.png)

選択すると、がVScodeが開き、Generic Range SensorのExampleのソースコードが表示されます。
![](https://storage.googleapis.com/zenn-user-upload/df198fa399af-20220402.png)

## 2.2 Generic Range Sensorの設定を変更する
ソースコードを変更し、レーザの照射レートを変更します。
Exampleの248行目を以下の設定に変更します。

~~~ change_prop_generic_range_sensor.py:Python3
frequency = 100 
~~~

ポップアップしたWindowにおいて、"Load Sensor" > ”Load Scene” > ”Set Sensor Pattern”を順番に選択します。
![](https://storage.googleapis.com/zenn-user-upload/8d93d1b17f69-20220402.png)

この状態で、Viewportの左側のPLAYボタンを押すと、照射レートが変更されたレーザパターンが照射されます。
![](https://storage.googleapis.com/zenn-user-upload/dec7dae68e3e-20220402.png)

