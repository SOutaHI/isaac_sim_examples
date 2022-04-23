# 概要
Isaac SimのGUI上でサンプルのオブジェクトを複数まとめて、USDファイルとして保存します。


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

1. オブジェクトをシーンに追加
2. Groupにまとめたオブジェクトの保存

シミュレーション上でロボットを動作させる環境を作成する場合、決まったオブジェクトはある程度まとめて設置したいと思います。
そのような場合には、事前にオブジェクトをまとめたオブジェクト群を1つのUSDファイルとして、保存しておくと便利です。
実際に、そのオブジェクト群を使用する場合には、１つのUSDファイルのインポートで実現することができます。

## 1. オブジェクトをシーンに追加
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンをロードする
メニューバーのCreate > Physics > Groud Planeを選択します。
![](https://storage.googleapis.com/zenn-user-upload/27126beca004-20220423.png)

### 1.3 オブジェクトの追加
シーン内にオブジェクトを追加します。
今回は、ウォーターサーバーと机を追加します。

Viewportの下にあるContentの中で、omniverse/localhost/Isaac/Enviroments/Hospital/Props/SM_WaterDispenser_01a.usdを選択します。
選択した状態で、選択したUSDファイルをドラッグアンドドロップでViewport内に移動します。
移動すると、シーン内にウォーターサーバーが追加されます。
![](https://storage.googleapis.com/zenn-user-upload/4755dd93f613-20220423.png)

次に、机を追加します。
Viewportの下にあるContentの中で、omniverse/localhost/Isaac/Enviroments/Simple_Room/Props/table_low.usdを選択します。
選択した状態で、選択したUSDファイルをドラッグアンドドロップでViewport内に移動します。
移動すると、シーン内に机が追加されます。
![](https://storage.googleapis.com/zenn-user-upload/07c0197b2c7c-20220423.png)

### 1.4 オブジェクト間の相対位置を変更する
追加したウォーターサーバーと机の位置を変更します。
今回は、机の上にウォーターサーバーを置きます。

右側のStageタブの中で、SM_WaterDispenser_01aを選択し、Viewport内の座標系の座標面を選択し、机の上に設置するように移動します。
![](https://storage.googleapis.com/zenn-user-upload/4585205ec8b8-20220423.png)


## 2. Groupにまとめたオブジェクトの保存
### 2.1 Groupとしてまとめる

右側のStageタブの中で、SM_WaterDispenser_01aとtable_lowを選択します。
![](https://storage.googleapis.com/zenn-user-upload/606d67826a37-20220423.png)

選択した状態で、右クリックし、Group Selectedを選択します。
![](https://storage.googleapis.com/zenn-user-upload/2dab1e63930a-20220423.png)

### 2.2 GroupをUSDファイルとしてエクスポートする

右側のStageタブの中で、Groupを選択します。

選択した状態で、右クリックし、Export Selectedを選択します。
ポップアップしたウィンドウにて、任意の保存名と保存先を指定します。
![](https://storage.googleapis.com/zenn-user-upload/364da759e93d-20220423.png)

### 2.3 保存したUSDファイルをインポートする
今回はomniverse/localhost/Isaac/Enviroments/Simple_Room/Props/water_server_on_desk.usdとして保存しました。
1.3の手順と同様にViewport内に該当のUSDファイルをドラッグアンドドロップすると、机の上にウォーターサーバーが設置されているオブジェクトが追加されます。
![](https://storage.googleapis.com/zenn-user-upload/b72d2c96b592-20220423.png)
