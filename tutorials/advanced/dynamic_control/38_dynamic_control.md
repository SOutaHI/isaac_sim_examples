する# 概要
Dynamic Control Extensionを使用し、ロボットを動かします。

Issac SimのExtension部分に上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/ext_omni_isaac_dynamic_control.html#isaac-dynamic-control

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
Dynamic Control Extensionは、オブジェクトを制御する為の拡張機能です。
拡張機能には、位置制御、速度制御、トルク制御等の制御器が含まれています。
また、軸単位の制御や位置、速度の値の取得も可能です。

APIのReferenceはこちらに記載されています。
https://docs.omniverse.nvidia.com/py/isaacsim/source/extensions/omni.isaac.dynamic_control/docs/index.html

今回は、簡易的なデモとしてScript Editorからの実行及び、2つのExampleを実行します。

1. Script Editorからの実行
2. Read Articulations Exampleの実行
3. Joint Controller Exampleの実行










## 1. Leonardoを用いたデモの実行
このデモでは、マニピュレータがシーン内のトイブロックをスタックするタスクを実行します。

### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンをロードする
メニューバーのIsaac Examples > Demos > Leonardo Demoを選択します。
![](https://storage.googleapis.com/zenn-user-upload/98726aebe525-20220502.png)

次にポップアップしたWindowにおいて、”Create Scenario”を選択します。
選択すると、ロボットが読み込まれます。
![](https://storage.googleapis.com/zenn-user-upload/511fa644f73f-20220502.png)
![](https://storage.googleapis.com/zenn-user-upload/6e17150b67df-20220502.png)

読み込みの完了後、右側のツールバーの中のPlayボタンを選択し、シミュレーションを開始します。
![](https://storage.googleapis.com/zenn-user-upload/65630663277a-20220502.png)

### 1.3 デモを実行する
ポップアップしたウィンドウにおいて、”Perform Task”を選択します。
選択すると、タスクが実行されます。

![](https://storage.googleapis.com/zenn-user-upload/d086ea9fccca-20220502.png)
![](https://storage.googleapis.com/zenn-user-upload/64ce7f9439ef-20220502.png)

また、”Togggle Obstacle”を選択し、キューブをシーン内で移動させると、キューブを避けるようにマニピュレータの軌道が計算されます。

## 2. UR10を用いたデモの実行 
UR10を用いたデモは2つ用意されています。
一つは、UR10の手先に取り付けられているSuction Gripperでコンテナを持ち、そのコンテナの中ににランダムにオブジェクトを落下させるデモ（Bin fillデモ）です、
もう一つは、UR10を用いたパレタイジングのデモ（Stack binデモ）です。

### 2.1 Bin fillデモを実行する
### 2.1.1 シーンをロードする
メニューバーのIsaac Examples > Demos > UR10 Palletizingを選択します。

次にポップアップしたWindowにおいて、”Selected Scenario”の欄をクリックし、”fill bin”を選択します。
![](https://storage.googleapis.com/zenn-user-upload/a10cd4783ef4-20220502.png)

選択後、同様のWindowにおいて、”Create Scenario”を選択します。
選択すると、ロボットが読み込まれます。
![](https://storage.googleapis.com/zenn-user-upload/f497caede827-20220502.png)

読み込みの完了後、右側のツールバーの中のPlayボタンを選択し、シミュレーションを開始します。
![](https://storage.googleapis.com/zenn-user-upload/3e7f8cb4fbee-20220502.png)

### 2.1.2 デモを実行する
ポップアップしたウィンドウにおいて、”Perform Task”を選択します。
選択すると、タスクが実行されます。

![](https://storage.googleapis.com/zenn-user-upload/5050efefe5aa-20220502.png)

### 2.2 Stack binデモを実行する
### 2.2.1 シーンをロードする
メニューバーのIsaac Examples -> Demos -> UR10 Palletizingを選択します。

次にポップアップしたWindowにおいて、”Selected Scenario”の欄をクリックし、”Stack bin”を選択します。
![](https://storage.googleapis.com/zenn-user-upload/75252f8bc916-20220502.png)

選択後、同様のWindowにおいて、”Create Scenario”を選択します。
選択すると、ロボットが読み込まれます。
![](https://storage.googleapis.com/zenn-user-upload/a2af61a652d7-20220502.png)

読み込みの完了後、右側のツールバーの中のPlayボタンを選択し、シミュレーションを開始します。
![](https://storage.googleapis.com/zenn-user-upload/a39d86881711-20220502.png)

### 2.2.2 デモを実行する
ポップアップしたウィンドウにおいて、”Perform Task”を選択します。
選択すると、タスクが実行されます。

![](https://storage.googleapis.com/zenn-user-upload/af7bed8dcec5-20220502.png)
![](https://storage.googleapis.com/zenn-user-upload/ea03ffc7d42c-20220502.png)
![](https://storage.googleapis.com/zenn-user-upload/991dd0056e11-20220502.png)

## 3. Navigationデモの実行
このデモでは、AGVを使用します。
AGVの目標位置、目標姿勢を登録すると、その位置に向かってAGVが移動します。

### 3.1 シーンをロードする
メニューバーのIsaac Examples > Demos > Robot Navigationを選択します。

次にポップアップしたWindowにおいて、”Load”を選択します。
![](https://storage.googleapis.com/zenn-user-upload/784a1ac77a10-20220502.png)

選択すると、ロボットが読み込まれます。
![](https://storage.googleapis.com/zenn-user-upload/2b6b1a2d7464-20220502.png)

”Robot Type”をCarterに変更すると、Carterがロードされます。
![](https://storage.googleapis.com/zenn-user-upload/8d81949c7425-20220502.png)

ポップアップしたウィンドウにおいて、”Open Source Code”をクリックすると、ExampleのソースコードがVscode上に展開されます。

読み込みの完了後、右側のツールバーの中のPlayボタンを選択し、シミュレーションを開始します。


### 3.2 デモを実行する
ポップアップしたウィンドウにおいて、”Move Robot"の”Move”を選択すると、ロボットが前進します。また、"Spin Robot"の”Rotate”を選択すると、ロボットが回転します。

移動後、”target Pose"に値を入力し、"Move to Target"の”Move”を選択すると、ロボットが設定したTarget poseに移動します。
![](https://storage.googleapis.com/zenn-user-upload/0fe3e5bdbdf1-20220502.png)

Robot TypeをCarterに設定し、ロードした場合でも、同様にNavigationは実行可能です。
![](https://storage.googleapis.com/zenn-user-upload/bb23521a7e79-20220502.png)
![](https://storage.googleapis.com/zenn-user-upload/ca2b906076ed-20220502.png)











