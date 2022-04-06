# 概要
Offlineで学習用のデータセットを作成します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_replicator_offline_generation.html

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
ランダムなシーンを作成し、シーンにおける合成データを連続して保存します。

1. Exampleコードの実行
2. 保存したデータの確認

## 1. Exampleコードの実行
### 1.1 Exampleコードを実行する
terminalで次のコマンドを実行します。

~~~ bash:shell
$ cd ~/.local/share/ov/pkg/isaac_sim.2021.2.1/
$ ./python.sh standalone_examples/replicator/offline_generation.py --scenario omniverse://localhost/Isaac/Samples/Synthetic_Data/Stage/warehouse_with_sensors.usd --num_frames 10 --max_queue_size 500
~~~
![](https://storage.googleapis.com/zenn-user-upload/df41665e123c-20220406.png)
![](https://storage.googleapis.com/zenn-user-upload/003a4fad037d-20220406.png)
![](https://storage.googleapis.com/zenn-user-upload/0d3d379be5db-20220406.png)]

offline_generation.pyの詳細は次のURLにから確認することが可能です（説明は追記します）。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_replicator_offline_generation.html#loading-the-environment


## 2. 保存したデータの確認
### 2.1 保存したデータを確認する
保存されたデータを確認します。
terminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ cd /home/"user名"/output/Viewport/
$ nautlius ./ &
~~~
![](https://storage.googleapis.com/zenn-user-upload/a38ae2584321-20220406.png)

実行すると、保存先ディレクトリが表示されます。
各ディレクトリの中のデータを開き、撮影されたデータが存在することを確認します。
![](https://storage.googleapis.com/zenn-user-upload/e156bce94f87-20220406.png)



