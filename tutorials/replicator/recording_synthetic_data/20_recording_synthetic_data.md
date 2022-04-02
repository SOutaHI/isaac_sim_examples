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


## 2. Synthetic Data Recordeの使用
### 2.1 Viewportの設定を変更する

Viewportの中の上部にある目のアイコンをクリックします。

”Show By Type”の中で、CemaraとLightのチェックを外します。

次に、Viewportの中の上部にあるカメラのアイコンをクリックします。
Cameraをクリックし、”RandomCamera”にチェックを入れます。


### 2.2 Synthetic Data Recorderを使用する
メニューバーの　をクリックします。

ポップアップした”Synthetic Data Recoder”のWindowをstage下部のPropertyの右隣に追加します。


”Synthetic Data Recoder”の”Viewport: Sensor Settings”の中のすべての欄にチェックを入れます。


左側のツールバーのPLAYボタンを押し、Viewportに表示されるシーンが切り替わることを確認します。

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
各ディレクトリの中のデータを開き、撮影されたデータが存在することを確認します。






