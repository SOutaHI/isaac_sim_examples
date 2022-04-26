# 概要
Isaac SimにUSDファイル以外のファイル形式の3Dモデルをインポートします。

Issac SimのExtension部分に上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/prod_extensions/ext_cad-importer.html

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

1. Cad Importerの有効化
2. オブジェクトのインポート（.stp）
3. オブジェクトの保存（USDファイル）

今回はstpファイルをインポートします。
他にもobj形式のモデル等もインポート可能です。

## 1. Cad Importerの有効化
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 Cad Importerを有効化する
メニューバーのWindow > Extensionsを選択します。
![](https://storage.googleapis.com/zenn-user-upload/5c84f2ce7a09-20220426.png)

ポップアップしたWindowの検索部分に”CAD”と入力します。
検索結果として、CAD Importerが表示されます。

ポップアップしたWindowの右上部分にExtensionの有効状態が表示されています。
緑色で”Enable”と表示されている場合には、有効な状態です。
![](https://storage.googleapis.com/zenn-user-upload/9bb426cc047c-20220426.png)

上記の状態になっていない場合には、トグルスイッチをクリックし、Enable状態にします。

## 2. オブジェクトのインポート（.stp）
### 2.1 シーンをロードする
メニューバーのCreate > Physics > Groud Planeを選択します。
![](https://storage.googleapis.com/zenn-user-upload/7469f77f09d8-20220426.png)
![](https://storage.googleapis.com/zenn-user-upload/27de02f2ba54-20220423.png)

### 2.2 オブジェクトのインポート
シーン内にオブジェクトを追加します。
今回は、.stpファイルをインポートします。
メニューバーのFile > Importを選択します。
![](https://storage.googleapis.com/zenn-user-upload/cf71469dc22f-20220426.png)

ポップアップしたWindowにおいて、任意のstpファイルを選択します。
![](https://storage.googleapis.com/zenn-user-upload/c878cc155da4-20220426.png)

選択し、Openをクリックすると、Viewport内にstpファイルに記述されているオブジェクトがインポートされます。
![](https://storage.googleapis.com/zenn-user-upload/2fa813717ece-20220426.png)

## 3. オブジェクトの保存（USDファイル）
### 3.1 オブジェクトをGropu化する
右側のStageタブの中で、先ほどインポートしたオブジェクトを選択します。
選択した状態で、右クリックし、Group Selectedを選択します。
![](https://storage.googleapis.com/zenn-user-upload/6e662343f255-20220426.png)

### 3.2 GroupをUSDファイルとしてエクスポートする
右側のStageタブの中で、Groupを選択します。
選択した状態で、右クリックし、Export Selectedを選択します。
![](https://storage.googleapis.com/zenn-user-upload/b11c84fff273-20220426.png)

ポップアップしたウィンドウにて、任意の保存名と保存先を指定します。


### 2.2 保存したUSDファイルをインポートする
今回はomniverse/localhost/Isaac/Enviroments/Simple_Room/Props/pallet_plastic.usdとして保存しました。
Viwportの下にあるContentの中から、Viewport内に該当のUSDファイルをドラッグアンドドロップすると、USDファイルとして先ほどインポートしたstpファイルがインポートされます。
![](https://storage.googleapis.com/zenn-user-upload/55a3ee681007-20220426.png)
