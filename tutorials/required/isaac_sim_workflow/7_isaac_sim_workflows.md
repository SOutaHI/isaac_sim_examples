# 概要
Isaac SimのWorkflowを紹介します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_required_workflows.html

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

Isaac simでは、次の3種類の開発方式が用意されています。

- Extensionts
- Standalone Applications
- Jupyter Notebook

上記の内、ExtensionsとStandalone Applicationについて紹介します。

1. Extensionsにおける操作
2. Standalone Applicationにおける操作


## 1. Extensionsにおける操作

ExtensionsはOmniverse kitのcore building blockです。
上記の様に、ExtensionsはOmniverse kitの要素として設計されている為、Issac simに限らず他のOmniverseのApplication内でも使用することが可能です。

Extensionsでは、Applicationは非同期に実行されるという特徴を持ちます。
これは、タイムステップの制御（例えば、phisics stepやrendering step）を行わないということを意味してます。

また、もう1つの特徴として、hot reloadingという機能を持っています。
Isaac simでは、Aplicationのソースコードを変更を、Aplicationのreloadによって確認することが可能です。
reloadはGUIの操作から実行することが可能であり、Issac sim自体のシャットダウンやリスタートは必要ありません。

今回はサンプルシーンをロードし、ロードしたシーン内にScript Editorからオブジェクトを追加します。
その後、サンプルシーンをリロードし、変更点の反映を確認します。


### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 Hello Worldのロード
メニューバーのIsaac Examples > Hello Worldを選択します。
![](https://storage.googleapis.com/zenn-user-upload/97c5049f9414-20220305.png)
![](https://storage.googleapis.com/zenn-user-upload/3db682aaa7a0-20220305.png)

ポップアップしたHello Worldのウィンドウの中のLoadを選択します。
選択すると、シーンがロードされます。
![](https://storage.googleapis.com/zenn-user-upload/9020c7a3d1a3-20220305.png)

このシーンのソースコードを変更する場合には、Hello Worldのウィンドウの右上にある3つのボタンの内、一番左側のボタンを選択します。
選択すると、VScodeが開き、Hello Worldのソースコードを編集することができます。
![](https://storage.googleapis.com/zenn-user-upload/ae109d07db08-20220305.png)

### 1.3 Script Editorからオブジェクトを追加
ポップアップしたHello WorldのウィンドウはViewportの下にドラッグアンドドロップで移動させます。
![](https://storage.googleapis.com/zenn-user-upload/7ca5e6a7cb2d-20220305.png)

メニューバーのWindow > Script Editorを選択します。
![](https://storage.googleapis.com/zenn-user-upload/bab4afe927af-20220305.png)
ポップアップしたScript Editorのウィンドウを、Viewportの下にドラッグアンドドロップで移動させます。
![](https://storage.googleapis.com/zenn-user-upload/244d04d6e109-20220305.png)

Script Editorに次のコードを記述します。

~~~ add_object:Python3


~~~

追加後、実行すると、シーン内にCubeが追加されます。
また、Hello Worldのウィンドウにおいて、Resetを選択し、その後loadを選択すると、Cubeが追加されたシーンがロードされます。


## 2. Standalone Applicationにおける操作

Applicationは、PythonのScriptとしても実行することが可能です。
Standalone Applicationでは、Applicationは同期して実行されます。
Extensionsとは異なり、タイムステップの制御（例えば、phisics stepやrendering step）を行うということを意味してます。

ターミナルで次のコマンドを実行します。

~~~ franka:bash

cd ~/.local/share/ov/pkg/isaac_sim-2021.2.1
./python.sh standalone_examples/api/omni.isaac.franka/follow_target.py

~~~

![](https://storage.googleapis.com/zenn-user-upload/7c73baa86c23-20220305.png)

実行すると、Isaac simが立ち上がり、FrankaのExampleシーンがロードされます。
Frankaのグリッパーの中心にあるTargetCubeを動かすと、Cubeの位置に合わせて、Frankaの手先が追従します。

![](https://storage.googleapis.com/zenn-user-upload/55f3e841cf61-20220305.png)
![](https://storage.googleapis.com/zenn-user-upload/fd2a9044d994-20220305.png)