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

次に、Jetbot keyboardのウィンドウの右上にある3つのボタンの内、一番左側のOpen Source Codeボタンを選択します。

選択すると、がVScodeが開き、Jetbot keyboardのソースコードが表示されます。

ソースコードの内容は次の通りです。

~~~ Jetbot_keyboard.py:Python3

~~~



## 2. taskをパラメタライズする 
vscode上で、hello_world.pyを編集します。

## 2.1 offsetパラメータの追加
Offsetパラメータを追加し、タスクのアセットをオフセット分だけ移動させます。
hello_world.pyを次の様に編集します。

~~~ hello_world.py:Python3

~~~

Loadを選択すると、Jetbotが表示されます。
この状態で、Viewportの左側のPLAYボタンを押すと、Keyboardからの入力を受け付け、Jetbotが移動します。

- W: 前進 
- S: 停止
- A: 左旋回
- D: 右旋回


