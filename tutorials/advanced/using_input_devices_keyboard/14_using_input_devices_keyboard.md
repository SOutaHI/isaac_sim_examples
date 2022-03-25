# 概要
keyboardからの入力を、シミュレーション内で使用するExampleを紹介します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_advanced_keyboard_control.html

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

JetbotのExampleとして、キーボードからの入力により、Jetbotの両輪の速度を制御します。

1. Jetbot Keyboardを実行する

## 1. Jetbot Keyboardを実行する
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 Jetbot Keyboardのソースコードを表示
メニューバーのIsaac Examples > Input Devices > Jetbot Keyboardを選択します。
![](https://storage.googleapis.com/zenn-user-upload/89320173039b-20220326.png)

次に、Jetbot keyboardのウィンドウの右上にある3つのボタンの内、一番左側のOpen Source Codeボタンを選択します。
![](https://storage.googleapis.com/zenn-user-upload/801941a678dd-20220326.png)

選択すると、がVScodeが開き、Jetbot keyboardのソースコードが表示されます。
![](https://storage.googleapis.com/zenn-user-upload/dcdce3903994-20220326.png)


Loadを選択すると、Jetbotが表示されます。
![](https://storage.googleapis.com/zenn-user-upload/69467fd54fd3-20220326.png)

この状態で、Viewportの左側のPLAYボタンを押すと、Keyboardからの入力を受け付け、Jetbotが移動します。

- W: 前進 
- S: 停止
- A: 左旋回
- D: 右旋回

![](https://storage.googleapis.com/zenn-user-upload/f13994d0b055-20220326.png)

