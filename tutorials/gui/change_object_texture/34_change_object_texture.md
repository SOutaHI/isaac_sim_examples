# 概要
Isaac SimのGUI上でオブジェクトのテクスチャを変更します。


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

1. オブジェクトのテクスチャを変更
2. テクスチャを変更したオブジェクトの保存

物体認識等の学習モデル用の正解データを作成する際に、オブジェクトのテクスチャを変更したいと思います。
このような場合に、GUIから任意のテクスチャに変更する方法を紹介します。

## 1. オブジェクトのテクスチャを変更
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンをロードする
メニューバーのCreate > Physics > Groud Planeを選択します。
![](https://storage.googleapis.com/zenn-user-upload/27de02f2ba54-20220423.png)

### 1.3 オブジェクトの追加
シーン内にオブジェクトを追加します。
今回は、前回作成したウォーターサーバーと机をまとめたオブジェクトを追加します。

Viewportの下にあるContentの中で、omniverse/localhost/Isaac/Enviroments/Simple_Room/Props/water_server_on_desk.usdを選択します。
選択した状態で、選択したUSDファイルをドラッグアンドドロップでViewport内に移動します。
移動すると、シーン内にウォーターサーバーと机をまとめたオブジェクトが追加されます。
![](https://storage.googleapis.com/zenn-user-upload/4e8e0a5edec6-20220423.png)

### 1.4 オブジェクトのテクスチャを変更する
右側のStageタブの中で、water_server_on_desk > SM_WaterDispenser_01a > MI_WaterDispenser_01aを選択します。
選択すると、Stageタブ下のPropeertyの中に、Material Shader > Inputsが表示されます。
![](https://storage.googleapis.com/zenn-user-upload/ebd8b5abe14e-20220423.png)
![](https://storage.googleapis.com/zenn-user-upload/75649ca1285a-20220423.png)

マテリアルについての詳細な説明は、Omniverseのマニュアルに記載されています。
https://docs.omniverse.nvidia.com/prod_materials-and-rendering/prod_materials-and-rendering/materials.html

今回は、Inputs:Normal, Inputs:Albedo, Inputs:RMAをそれぞれ、空の風景のテクスチャに変更します。

まず、Inputs:Normalを変更します。
Inputs:Normalの横にあるフォルダマークをクリックし、ポップアップしたウィンドウにおいて、T_Sky_Blue.pngを選択します。
![](https://storage.googleapis.com/zenn-user-upload/8e85c0667eca-20220423.png)
![](https://storage.googleapis.com/zenn-user-upload/8eacc999a744-20220423.png)

次に、Inputs:Albedoを変更します。
Inputs:Albedoの横にあるフォルダマークをクリックし、ポップアップしたウィンドウにおいて、T_Sky_Blue.pngを選択します。
![](https://storage.googleapis.com/zenn-user-upload/5f43b963dbec-20220423.png)
![](https://storage.googleapis.com/zenn-user-upload/43a2b8836b2b-20220423.png)

最後に、Inputs:RMAを変更します。
Inputs:RMAの横にあるフォルダマークをクリックし、ポップアップしたウィンドウにおいて、T_Sky_Blue.pngを選択します。
![](https://storage.googleapis.com/zenn-user-upload/b839f0c2fc5b-20220423.png)


## 2. テクスチャを変更したオブジェクトの保存
### 2.１ GroupをUSDファイルとしてエクスポートする

右側のStageタブの中で、Groupを選択します。
選択した状態で、右クリックし、Export Selectedを選択します。
![](https://storage.googleapis.com/zenn-user-upload/267b7af13093-20220423.png)

ポップアップしたウィンドウにて、任意の保存名と保存先を指定します。
![](https://storage.googleapis.com/zenn-user-upload/9ec597d9d8b5-20220423.png)


### 2.2 保存したUSDファイルをインポートする
今回はomniverse/localhost/Isaac/Enviroments/Simple_Room/Props/water_server_with_texture_on_desk.usdとして保存しました。
1.3の手順と同様にViewport内に該当のUSDファイルをドラッグアンドドロップすると、机の上にウォーターサーバーが設置されているオブジェクトが追加されます。
![](https://storage.googleapis.com/zenn-user-upload/4d980a189912-20220423.png)
![](https://storage.googleapis.com/zenn-user-upload/042fd3deecbd-20220423.png)