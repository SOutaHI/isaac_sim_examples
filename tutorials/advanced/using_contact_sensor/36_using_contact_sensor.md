
# 概要
シミュレーション内で接触センサを使用します。

Issac SimのDocumentationのExtension部分に上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/ext_omni_isaac_contact_sensor.html

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

Isaac SimのExampleの中に、接触センサのExampleが存在します。
今回はこのExamdpleを実行します。

1. Exampleシーンのロード
2. シミュレーションの実行


## 1. Exampleシーンのロード

### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 Exampleシーンをロードする
メニューバーのIsaac Examples > Sensors > Contactを選択します。
![](https://storage.googleapis.com/zenn-user-upload/23eb114b035b-20220427.png)
![](https://storage.googleapis.com/zenn-user-upload/96bedfe5e95f-20220427.png)

ポップアップしたウィンドウにおいて、”Open Source Code”をクリックすると、ExampleのソースコードがVscode上に展開されます。
![](https://storage.googleapis.com/zenn-user-upload/312125a3516a-20220427.png)

## 2. シミュレーションの実行
## 2.1 シミュレーションを実行する
Viewportの左側のPLAYボタンを押すと、シミュレーションが開始されます。
センサ値はロボットの各足部に設定されており、取得されるセンサ値は、Exampleシーンロード時のポップアップウィンドウに表示されます。
![](https://storage.googleapis.com/zenn-user-upload/c0789c5c46a9-20220427.png)
![](https://storage.googleapis.com/zenn-user-upload/764aed4b0964-20220427.png)

センサ値はロボットの各足部に設定されており、取得されるセンサ値は、Exampleシーンロード時のポップアップウィンドウに表示されます。

