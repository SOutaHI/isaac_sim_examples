# 概要
合成データを連続して取得し、保存します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_replicator_recorder.html

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
GUI上での操作で、シーン内のカメラから合成データを連続して取得し、保存します。

1. テストシーンのロード
2. Synthetic Data Recordeの使用

## 1. テストシーンのロード
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 テストシーンをロードする
Isaac Simの下部にあるContentの中から、Isaac > Samples > Synthetic_Data > Stage > warehouse_with_sensors.usdをダブルクリックします。
![](https://storage.googleapis.com/zenn-user-upload/cac66c04e992-20220406.png)

## 2. Synthetic Data Recordeの使用
### 2.1 Viewportの設定を変更する
Viewportの中の上部にある目のアイコンをクリックします。
![](https://storage.googleapis.com/zenn-user-upload/61c8ab8eb500-20220406.png)

”Show By Type”の中で、CemaraとLightのチェックを外します。
![](https://storage.googleapis.com/zenn-user-upload/978be54cc55c-20220406.png)

次に、Viewportの中の上部にあるカメラのアイコンの表示名が”RandomCamera”になっているか確認します。

### 2.2 Synthetic Data Recorderを使用する
メニューバーのSynthetic Data > Synthetic Data Recodarをクリックします。
![](https://storage.googleapis.com/zenn-user-upload/218fcc7c9f88-20220406.png)

ポップアップした”Synthetic Data Recoder”のWindowをstage下部のPropertyの右隣に追加します。
![](https://storage.googleapis.com/zenn-user-upload/076f418fa52c-20220406.png)

”Synthetic Data Recoder”の”Viewport: Sensor Settings”の中のすべての欄にチェックを入れます。
![](https://storage.googleapis.com/zenn-user-upload/9cd2e5c5f9a1-20220406.png)

左側のツールバーのPLAYボタンを押し、Viewportに表示されるシーンが切り替わることを確認します。
![](https://storage.googleapis.com/zenn-user-upload/b8d4bdc21f66-20220406.png)

この状態で、”Synthetic Data Recoder”の”Start Recording”をクリックします。
10sec程度経過した後、”Synthetic Data Recoder”の”Stop Recording”をクリックします。

### 2.3 保存したデータを確認する
保存されたデータを確認します。
terminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ cd /home/"user名"/output/Viewport/
$ nautlius ./ &
~~~

実行すると、保存先ディレクトリが表示されます。
![](https://storage.googleapis.com/zenn-user-upload/06fa676b5591-20220406.png)

各ディレクトリの中のデータを開き、撮影されたデータが存在することを確認します。
![](https://storage.googleapis.com/zenn-user-upload/0500ef9323c8-20220406.png)
![](https://storage.googleapis.com/zenn-user-upload/7c661a76d664-20220406.png)





