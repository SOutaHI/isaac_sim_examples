# 概要
シミュレーション内で2DのOccupacy Mapを作成します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_advanced_occupancy_map.html

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

GUI上からの操作によって、Occupacy Mapを作成します。
Occupacy Mapは時系列のセンサ情報をもとに事後確率として障害物の存在性を評価する際に使用されます。
[参考：菅沼、魚住、自動車の自律走行のためのOccupacy Grid Mapに基づく全方位障害物検出、2013](https://www.jstage.jst.go.jp/article/jsaeronbun/44/3/44_20134483/_pdf)

1. 2D Occupacy Mapの作成（GUI）

## 1. 2D Occupacy Mapの作成（GUI）
こちらのページに記載してある手順を進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_advanced_range_sensor_lidar.html#getting-started

### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンを作成する
メニューバーのCreate ＞ Physics > Phisics Sceneを選択します。
![](https://storage.googleapis.com/zenn-user-upload/7e39de2cccd2-20220219.png)


### 1.3 Warehouse環境のロード
Warehouseの環境をシーン内にロードします。
メニューバーのCreate -> Isaac -> Environments -> Warehouse Multiple Shelvesを選択します。
![](https://storage.googleapis.com/zenn-user-upload/765eb7855451-20220402.png)
![](https://storage.googleapis.com/zenn-user-upload/aa38728cdce9-20220402.png)

### 1.3 Occupacy Mapの作成
まず、Occupacy Map Generatorをロードします。
メニューバーのIsaac Utils -> Occupacy Mapを選択します。
![](https://storage.googleapis.com/zenn-user-upload/b47316f3f731-20220402.png)
![](https://storage.googleapis.com/zenn-user-upload/036df16c0010-20220402.png)

Viewportの下にあるOccupacy Map ExtensionのOriginの値を次の値に設定します。

- OriginのXを200にする
- OriginのYを0にする
- OriginのZを120にする
![](https://storage.googleapis.com/zenn-user-upload/77ba907ffb53-20220402.png)

Viwport内に２DのGridが作成されることを確認します。
![](https://storage.googleapis.com/zenn-user-upload/7cba50748d17-20220402.png)

次に、右側にあるStageの中で、ロードしたWarehouseを選択します。
この状態で、Viewportの下にあるOccupacy Map Extensionの”BOUND SELECTION”を選択します。
![](https://storage.googleapis.com/zenn-user-upload/95bb369af3f6-20220402.png)

Viewportの下にあるOccupacy MapのUpper Boundの値を次の値に設定します。

- Upper BoundのZを300にする
![](https://storage.googleapis.com/zenn-user-upload/34c25d4d9dba-20220402.png)

Occupacy Map Extensionの”CALCULATE” > "VISUALIZE IMAGE"を選択します。
選択すると、2DのOccupacy Mapが表示されます。
![](https://storage.googleapis.com/zenn-user-upload/3eeb76370eaf-20220402.png)









