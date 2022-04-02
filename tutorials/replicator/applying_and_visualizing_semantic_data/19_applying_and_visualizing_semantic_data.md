# 概要
GUI上での操作で、シーン内のカメラから合成データを取得します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_replicator_semantics.html

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
GUI上での操作で、シーン内のカメラから合成データを取得します。

1. テストシーンのロード
2. 個別のオブジェクトのSemantic dataの生成
3. Stage全体のSemantic dataの生成

## 1. テストシーンのロード
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 テストシーンをロードする
Isaac Simの下部にあるContentの中から、Isaac > Environments > Simple_Room > simple_room.usdをダブルクリックします。


次に、メニューバーの　を選択します。
ポップアップしたSemantic Schema EditorのWindowをドラッグし、Stageの下のPropertyの右隣に追加します。

## 2. 個別のオブジェクトのSemantic dataの生成
### 2.1 個別のオブジェクトのSemantic dataを生成する
Viwportの中で、左上にある目のアイコンの隣のアイコンを選択します。

表示されたWindowの中で、RGBとSemantic Segmentaitonにチェックを入れます。

チェック後に、”Visualize”を選択すると、シーン内のカメラで撮影されたRGB画像が表示されます。

次に、シーン内のテーブルをSemantic Segmentationの結果で認識するように設定を変更します。
Stageの中でRoot > table_low_327 > table_lowを選択します。

選択した状態で、Stageの下部にあるPropertyの”Raw USD properties”の中の”semantic:Semantics:params:semanticData”と”semantic:Semantics:params:semanticType”の値を確認します。

確認した値を、Semantic Schema Editorの”Apply Semantic data on selected objects”のTableのTypeとDataの値にそれぞれ入力します。

追加後、Viwportの中で、左上にある目のアイコンの隣のアイコンを選択します。
再度、”Visualize”を選択すると、シーン内のカメラで撮影されたRGB画像とTableが認識されたSemantic Segmentationの結果が表示されます。


## 3. Stage全体のSemantic dataの生成
### 3.1 Stage全体のSemantic dataを生成する

Semantic Schema Editorの設定値を次の様に変更します。

- "Prim types to label"に”Mesh”を入力する
- "Class list"に”table,floor,wall”を入力する

入力後、"Prim types to label"欄の中で、”Generate Labels”を選択します。

追加後、Viwportの中で、左上にある目のアイコンの隣のアイコンを選択します。
再度、”Visualize”を選択すると、シーン内のカメラで撮影されたRGB画像とTableが認識されたSemantic Segmentationの結果が表示されます。


