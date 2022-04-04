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
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンをロードする
メニューバーのCreate ＞ Synthetic Data > Replicator playgroudを選択します。


ポップアップした”Replicator playgroud”のWindowをViewport下部のConsoleの右隣に追加します。


追加したReplicator playgroudのScene and Randamization > Base Sceneをクリックし、”Simple Room”を選択します。

選択後、”Base Scene”の下の”LOAD”をクリックすると、”Simple Room”がロードされます。

### 1.3 シーンのColorを変更する

追加したReplicator playgroudのScene and Randamization > Select randamzationをクリックし、”Color”を選択します。

選択後、”Select randamzation”の下の”ADD”をクリックすると、ランダムなカラーが適用されます。
適用されたカラーは、”ADD”の下にある”Preview”を選択すると、表示されます。


### 1.4 合成データのデータセットを作成する
メニューバーのCreate ＞ Synthetic Data > Synthetic Data Recodarを選択します。
ポップアップした”Synthetic Data Recoda”のWindowをViewport下部のReplicator playgroudyの右隣に追加します。

Synthetic Data Recodar > Sensor Settingの中で、学習に必要なデータにチェックを入れます。
チェックを入れるデータは次の通りです。

- RGB ( status )
- Instance Segmentation (status, Save array)
- 2D Tight Bounding Box (status, Save array)

また、学習データの保存先ディレクトリは、Recodar Settingsの”output directory”において指定します。
チェック後、シミュレータ左側のツールバーの”PLAY”を押すと、シーンのカラーがランダムに変更されます。


Synthetic Data RecodarのRecodar Settingにある”Start Recording”を選択します。
10sec程度経過後、”Stop Recording”を選択します。

## 2. Replicator Playgroudを用いた学習

Replicator playgroudのTrainingの設定値を次の通りに変更します。

- "input directory"を先ほど学習データを生成したSynthetic Data Recodarの出力先ディレクトリに変更する
- "Training Scenario"を”instance Segmentation”に設定する
  - instance segmentationはMaskR-CNNが使用されています。

設定後、Replicator playgroudのTrainingの”LOAD”を選択します。


Replicator playgroudのTrainingの”GENERATEを選択すると、データセットの統計値のグラフ表示されます。


最後に、Replicator playgroudのTrainingの”START"を選択すると、学習が開始されます。

学習が経過するにつれて、学習できていることが確認できます。





