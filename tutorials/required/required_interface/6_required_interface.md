# 概要
GUI上でExtensionを追加します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_required_interface.html
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

# 概要

Isaac Sim上では、GUI上からの操作性を向上するExtensionが容易されています。
今回は、Extensionの使用例として、”TimeSample Editor”を用いて、オブジェクトを操作します。
GUIでの操作はすべてPythonのAPIから操作することが可能です。

1. GUI内にObjectを追加
2. Extensionの追加


## 1. GUI内にObjectを追加
こちらのページに記載してある手順を進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_required_interface.html

### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンにObjectを追加
メニューバーのCCreate > Shapes > Cubeを選択します。
![](https://storage.googleapis.com/zenn-user-upload/fe359d9cdc63-20220305.png)

次に、球体をシーンに追加します。
メニューバーのCreate > Shapes > Sphereを選択します。

![](https://storage.googleapis.com/zenn-user-upload/da67857cf350-20220305.png)
![](https://storage.googleapis.com/zenn-user-upload/345e49ab3ab6-20220305.png)

### 1.3 Objectの関係
右側のStageタブの中でCubeをドラッグアンドドロップでSphereの下に移動させます。
![](https://storage.googleapis.com/zenn-user-upload/a10f229b2509-20220305.png)
この状態で、Sphereをシーン内で動かすと、CubeもSphereとの相対位置を保ったまま、動きます。
![](https://storage.googleapis.com/zenn-user-upload/232f6ffb86c9-20220305.png)

CubeとSphereを独立させる場合には、CubeをドラッグアンドドロップでWorldの下に移動させます。

![](https://storage.googleapis.com/zenn-user-upload/9a1436b681a5-20220305.png)


## 2. Extensionの追加

## 2.1 TimeSample Editorの有効化
シーンにExtensionを追加します。
TimeSample Editorは時間ごとのObjectの動きを記録し、再生するExtensionです。
メニューバーのWindow > Extensionsを選択します。
![](https://storage.googleapis.com/zenn-user-upload/0b337fb15d74-20220305.png)
![](https://storage.googleapis.com/zenn-user-upload/7fc2210c4b15-20220305.png)

ポップアップしたウィンドウのSearchの部分に“TimeSample Editor”を入力します。
![](https://storage.googleapis.com/zenn-user-upload/e3033196ba30-20220305.png)

検索結果の”TimeSample Editor”の右側のトグルボタンをクリックし、Enableにします。
Enableにすると、インストールされます。
![](https://storage.googleapis.com/zenn-user-upload/4b05200c674c-20220305.png)

インストール後、ポップアップされた”TimeSample Editor”のウィンドウを、右側のStageの下にドラッグアンドドロップで移動させます。
![](https://storage.googleapis.com/zenn-user-upload/815cc6a0d89a-20220305.png)

## 2.2 TimeSample Editorの設定
右側のStageの中のSphereを選択します。
![](https://storage.googleapis.com/zenn-user-upload/b66db30f131b-20220305.png)

Stageの下についかしたTimeSample Editorの”xformOp:translate”の”Add key”を選択します。
![](https://storage.googleapis.com/zenn-user-upload/aad72f26bb93-20220305.png)

Viewportの中のTimelineを50程度の位置に変更します。
この状態で、Sphereを動かします。
Viewportの中のTimelineを100程度の位置に変更します。
この状態で、Sphereを動かします。

![](https://storage.googleapis.com/zenn-user-upload/81b07d4abef4-20220305.png)

上記の設定の完了後、'Space'キーを押すと、Animationが開始されます。
