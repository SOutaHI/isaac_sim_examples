# 概要
Replicator Comporserを用いてデータセットを作成します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_replicator_composer.html

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
Replicator Composerを用いてデータセットを作成します。
Replicator Composerでは、Parameter FileとAsset Fileの2つのファイルからシーンを生成します。
シーンの設定では、Objectの種類や特性、環境の種類や特性をそれぞれ設定するこが可能です。

設定値の一覧はこちらのReferenceから確認できます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/manual_replicator_composer_parameter_list.html#isaac-sim-app-manual-replicator-composer-parameter-list

Replicator ComposerのTutorialとして用意されているParameter FileとAsset Fileを用いてデータセットを作成します。

また、Tutorialには、Launcherから実行する方法とDocker上で実行する方法が記載されています。
今回はLauncherから実行する方法によってデータセットを作成します。

流れを次に示します。

1. Isaac SimのインストールディレクトリでTerminalを開く
2. データ生成のコマンドの実行

## 1. Isaac SimのインストールディレクトリでTerminalを開く
### 1.1 OmniverseからIsaac Simを起動する
まず、OmniverseのLauncherからIsaac SimのLauncherを起動します。
![](https://storage.googleapis.com/zenn-user-upload/76d79bb8e0c2-20220416.png)

### 1.2 Isaac SimのインストールディレクトリでTerminalを開く
次に、Isaac SimのLauncherから”Open in Terminal”を選択します。
![](https://storage.googleapis.com/zenn-user-upload/55559222f3ed-20220416.png)
![](https://storage.googleapis.com/zenn-user-upload/e82f87680383-20220416.png)

## 2. データ生成のコマンドの実行
### 2.1 データ保存先ディレクトリを作成する
データ保存先ディレクトリを作成します。
開いたterminalで次のコマンドを実行する

~~~ bash:shell
$ mkdir -p ~/workspace/isaac/
~~~

### 2.2 tool/composer/main.pyを修正する
自分の環境において、[このコマンド](https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_replicator_composer.html#sample-input-parameterizations)を実行すると、エラーが発生しデータを生成することができませんでした。

エラー内容は次の様な内容でした。

こちらについて調べたところ、Isaacのある複数のパッケージはSimulationAppのインスタンスに依存しており、依存しているパッケージはインスタンス生成後にimportしないと、ImportErrorになるという報告がありました。
https://forums.developer.nvidia.com/t/omni-kit-error-isaac-sim-implementation/204275

そのため、実行ファイルであるtool/composer/main.pyを編集し、先にSimulationAppのインスタンスを生成するように変更しました。
まず、エディタで該当ファイルを開きます。

~~~ bash:shell
$ vim tool/composer/main.py
~~~
![](https://storage.googleapis.com/zenn-user-upload/e22516d0d11d-20220416.png)
![](https://storage.googleapis.com/zenn-user-upload/ff6e2ec6c50d-20220416.png)

16行目に次の処理を追加します。
~~~ Python:Python
kit = SimulationApp()
~~~

52行目をコメントアウトします。
~~~ Python:Python
#　self.sim_app = SimulationApp(config)kit = SimulationApp()
~~~

### 2.3 データ生成のコマンドを実行する
#### 2.3.1 Factory環境におけるデータ生成
Factory環境でデータを生成するコマンドを実行します。

~~~ bash:shell
$ ./python.sh tools/composer/src/main.py  --input parameters/warehouse.yaml --output */datasets/warehouse --num-scenes 10 --headless --mount ~/workspace/isaac/
~~~
![](https://storage.googleapis.com/zenn-user-upload/c1b32d38b85f-20220416.png)

実行が完了すると/workspace/isaac/datasets/warehouse/にデータが保存されます。
![](https://storage.googleapis.com/zenn-user-upload/750bebc2bda5-20220416.png)
![](https://storage.googleapis.com/zenn-user-upload/69c76eba0ad3-20220416.png)

#### 2.3.1  FlyingThings3Dを用いたデータ生成
 FlyingThings3Dを用いてデータを生成するコマンドを実行します。

~~~ bash:shell
$ ./python.sh tools/composer/src/main.py  --input parameters/flying_things_3d.yaml --output */datasets/flying_things_3d --num-scenes 10 --headless --mount ~/workspace/isaac/
~~~

実行が完了すると/workspace/isaac/datasets/flying_things_3d/にデータが保存されます。
![](https://storage.googleapis.com/zenn-user-upload/a856e68baf1c-20220416.png)

