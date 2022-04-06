# 概要
保存した合成データをVisualizeします。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_replicator_visualize_groundtruth.html

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
保存した合成データをVisualizeします。

1. Exampleコードの実行
2. 保存したデータの確認

## 1. Exampleコードの実行
### 1.1 Exampleコードを実行する
terminalで次のコマンドを実行します。

~~~ bash:shell
$ cd ~/.local/share/ov/pkg/isaac_sim-2021.2.1
$ ./python.sh standalone_examples/replicator/visualize_groundtruth.py
~~~

![](https://storage.googleapis.com/zenn-user-upload/6e764bd6d02b-20220406.png)
![](https://storage.googleapis.com/zenn-user-upload/0a3ad866dd51-20220406.png)

visualize_groundtruth.pyでは、Stageの設定及び、画像の取得、Visualizationの処理が記載されています。
詳細は次のURLにから確認することが可能です（説明は追記予定）。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_replicator_visualize_groundtruth.html#the-code

## 2.保存したデータの確認
### 2.1 保存したデータを確認する
保存されたデータを確認します。
terminalを開き、次のコマンドを入力します。

~~~ bash:shell
$ cd ~/.local/share/ov/pkg/isaac_sim-2021.2.1
$ nautlius ./ &
~~~

実行すると、保存先ディレクトリが表示されます。
保存したデータはこのディレクトリ直下に存在します。
![](https://storage.googleapis.com/zenn-user-upload/75c47be192ed-20220406.png)

該当データを開き、撮影されたデータが存在することを確認します。
![](https://storage.googleapis.com/zenn-user-upload/df4cb9cecd72-20220406.png)

