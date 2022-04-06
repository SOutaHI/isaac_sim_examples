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
![](https://storage.googleapis.com/zenn-user-upload/8ed1b3754388-20220406.png)

ポップアップした”Replicator playgroud”のWindowをViewport下部のConsoleの右隣に追加します。
![](https://storage.googleapis.com/zenn-user-upload/75077bd3a211-20220406.png)

追加したReplicator playgroudのScene and Randamization > Base Sceneをクリックし、”Simple Room”を選択します。
選択後、”Base Scene”の下の”LOAD”をクリックすると、”Simple Room”がロードされます。
![](https://storage.googleapis.com/zenn-user-upload/80fb6136c215-20220406.png)

### 1.3 シーンのColorを変更する

追加したReplicator playgroudのScene and Randamization > Select randamzationをクリックし、”Color”を選択します。
選択後、”Select randamzation”の下の”ADD”をクリックすると、ランダムなカラーが適用されます。
適用されたカラーは、”ADD”の下にある”Preview”を選択すると、表示されます。
![](https://storage.googleapis.com/zenn-user-upload/2c0f5bebfa5b-20220406.png)

### 1.4 合成データのデータセットを作成する
メニューバーのCreate ＞ Synthetic Data > Synthetic Data Recodarを選択します。
ポップアップした”Synthetic Data Recoda”のWindowをViewport下部のReplicator playgroudyの右隣に追加します。
![](https://storage.googleapis.com/zenn-user-upload/036d8f952526-20220406.png)

Synthetic Data Recodar > Sensor Settingの中で、学習に必要なデータにチェックを入れます。
チェックを入れるデータは次の通りです。

- RGB ( status )
- Instance Segmentation (status, Save array)
- 2D Tight Bounding Box (status, Save array)

また、学習データの保存先ディレクトリは、Recodar Settingsの”output directory”において指定します。
![](https://storage.googleapis.com/zenn-user-upload/279a5aba92e1-20220406.png)

チェック後、シミュレータ左側のツールバーの”PLAY”を押すと、シーンのカラーがランダムに変更されます。
![](https://storage.googleapis.com/zenn-user-upload/f04ab4c7137b-20220406.png)

Synthetic Data RecodarのRecodar Settingにある”Start Recording”を選択します。
![](https://storage.googleapis.com/zenn-user-upload/69b3bf596957-20220406.png)

10sec程度経過後、”Stop Recording”を選択します。

## 2. Replicator Playgroudを用いた学習

Replicator playgroudのTrainingの設定値を次の通りに変更します。

- "input directory"を先ほど学習データを生成したSynthetic Data Recodarの出力先ディレクトリに変更する
- "Training Scenario"を”instance Segmentation”に設定する
  - instance segmentationはMaskR-CNNが使用されています。

![](https://storage.googleapis.com/zenn-user-upload/f177e13e2bb9-20220406.png)

設定後、Replicator playgroudのTrainingの”LOAD”を選択します。
Replicator playgroudのTrainingの”GENERATEを選択すると、データセットの統計値のグラフ表示されます。また、”Visualize”を選択すると、学習データを可視化することができます。
![](https://storage.googleapis.com/zenn-user-upload/6815709b5e80-20220406.png)


最後に、Replicator playgroudのTrainingの”START"を選択すると、学習が開始されます。
![](https://storage.googleapis.com/zenn-user-upload/bf4ecad0ecff-20220406.png)

学習が経過するにつれて、学習できていることが確認できます。
![](https://storage.googleapis.com/zenn-user-upload/7b4769f03e0d-20220406.png)
![](https://storage.googleapis.com/zenn-user-upload/88fb1984dbb8-20220406.png)
![](https://storage.googleapis.com/zenn-user-upload/8ba2cacbd6ba-20220406.png)