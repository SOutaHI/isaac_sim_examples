# 概要
Jupyter notebook上からソースコードを実行し、Stageをインタラクティブに操作します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_advanced_jupyter.html


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
Jupyter notebook上からScriptを実行し、Stageをインタラクティブに操作します。
シミュレーション内のStageを、Jupyter notebookから操作する際には、Stageの設定を変更します。

1. Jupyter notebookからシミュレーションを実行する
2. Jupyter notebookからインタラクティブに操作する

## 1. Jupyter notebookからシミュレーションを実行する
こちらのページに記載してある手順を進めます。

### 1.1 Jupyter notebookを起動する
terminalで次のコマンドを実行します。

~~~ bash:shell
$ cd ~/.local/share/ov/pkg/isaac
$ ./jupyter_notebook.sh standalone_examples/notebooks/scene_generation.ipynb
~~~

./jupyter_notebook.shを実行すると、Jupyter notebookが起動します。

### 1.2 シーンをロードする
Jupyter notebook上で、1つ目のCellを実行します。


次に、Isaac Simの下部にあるContentのOmniverse > localhost > Users > "User名" > temp_jupyter_stage.usdをダブルクリックします。

## 1.3 Live SyncをEnableにする。
読み込んだシーンをJupyter notebookから操作できるように、設定を変更します。
Isaac Simの右上にあるLayerタブを選択し、Cloudアイコンをクリックします。

クリックすると、Isaac Simの上部に存在するLive Syncのアイコンの色が緑色に変化します。


## 1.4 Jupyter notebookから操作する
Jupyter NootebookのCell 1以外のCellを上から順番に実行します。
実行する前に、最後のCellに記載されている、SimulaterのShutdownコマンドをコメントアウトします。

最後のCellの1つ前のCellを実行すると、シーンに設置したカメラから取得されたRGB画像、Depth画像、Semantic Segmentaitonの結果が表示されます。

## 2. Jupyter notebookからインタラクティブに操作する

シーン内のオブジェクトをJupyter notebookから操作します。
まず、Viewport内で右クリックし、Create > Mesh > Cone選択します。

次に、SimulaterのShutdownコマンドをコメントアウトしたcellに次のコードを追加します。

~~~ add_process:Python3
carter_prim = stage.GetPrimAtPath(stage_path)
print(carter_prim)

xform_prim = XFormPrim(stage_path)
xform_prim.set_world_pose(position = np.array([0,100,0]))
simulation_world.render()

simulation_world.step()
cone_prim = stage.GetPrimAtPath('/Cone')
print(cone_prim)
~~~

追加したコードでは、ロボットの位置を変更し、シーン内に追加したConeのPrimを取得しています。
追加後、編集したCellを実行します。

実行後、最後のCellの1つ前のCellを再実行すると、Coneが追加されたシーンでの撮影結果が表示されます。








