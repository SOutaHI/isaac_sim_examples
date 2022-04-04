# 概要
学習用のデータセットを作成し、Replicator Playgroudからモデルの学習を行います。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_replicator_playground.html

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
学習用のデータセットを作成し、Replicator Playgroudからモデルの学習を行います。
今回の例はサンプルとして説明されており、学習したモデルを保存することはできません、、、、
合成データの作成から学習までGUIの操作で完結しているため、操作の利便性はかなり高いと感じました。

1. 合成データの作成
2. Replicator Playgroudを用いた学習

## 1. 合成データの作成


















### 1.1 Exampleコードを実行する
terminalで次のコマンドを実行します。

~~~ bash:shell
$ cd ~/.local/share/ov/pkg/isaac
$ ./python.sh standalone_examples/replicator/offline_generation.py --scenario omniverse://localhost/Isaac/Samples/Synthetic_Data/Stage/warehouse_with_sensors.usd --num_frames 10 --max_queue_size 500
~~~

offline_generation.pyの詳細は次のURLにから確認することが可能です（説明は追記します）。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_replicator_offline_generation.html#loading-the-environment


### 2.2 保存したデータの確認
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






