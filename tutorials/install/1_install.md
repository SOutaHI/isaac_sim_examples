
# 概要
[Nvidia Issac sim](https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/overview.html)のインストール手順を記載します。
基本的には、公開されているインストール手順通りインストールすれば問題ありません。

"install issac sim"と検索すると古いバージョンのIssac Simのインストール手順が検索結果に出てきます。
最新バージョンをインストールする場合には注意します。

最新バージョンではNvidia Omniverse上からIssac Simをインストールする手順になっています。
従って、はじめにNvidia Omniverseをインストールします。
Omniverseインストール完了後にIssac Simをインストールします。

また、OmniverseおよびIssac Simには実行するための最小スペックが記載されています。
最小スペックに対して不十分なスペックのＰＣで実行する場合には、予期せぬエラーが発生する可能性があるため、注意します。

[Omniverse最小スペックおよび推奨スペック](https://docs.omniverse.nvidia.com/app_view_deprecated/app_view_deprecated/requirements.html)
[Issac Sim最小スペックおよび推奨スペック](https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/requirements.html)

# 実行環境

- インストール実行環境

| unit       |       specification | 
|:-----------------:|:------------------:|
| CPU         | i9-11900H |  
| GPU         | GeForce RTX 3080 Laptop|  
| RAM         | 32GB | 
| OS         | Ubuntu 20.04.3 LTS  |

- Nvidia Driverバージョン

- Issac simバージョン
   - 2021.2.1


# 手順

大まかな手順は次の通りです。

1. Nvidia Omniverseをインストールする
2. Cacheをインストールする
3. Nucleus のサービスをローカル環境に作成する
4. IssacSimをインストールする

## 1. Nvidia Omniverseをインストールする
こちらのページに記載してある手順を進めます。
https://docs.omniverse.nvidia.com/prod_install-guide/prod_install-guide/workstation.html

### 1.1 Omniverseのインストーラをダウンロードする
ダウンロードする際に、メールアドレスやOmniverseの使用目的について聞かれるので、適宜回答します。
![](https://storage.googleapis.com/zenn-user-upload/12934b97d828-20220213.png)

### 1.2 Omniverseのインストーラを起動する
インストーラを起動する前に、インストーラの実行権限を変更し、実行できるようにします。

```bash
cd /"インストーラをダウンロードしたディレクトリ"/
sudo chmod +x omniverse-launcher-linux.AppImage
```
インストーラを実行します

```bash
./omniverse-launcher-linux.AppImage
```

インストーラの実行すると、必要な情報（インストールディレクトリ等）を聞かれるので、適宜入力します。
インストーラの進め方はこちらに記載されています。
https://docs.omniverse.nvidia.com/prod_install-guide/prod_launcher/installing_launcher.html#launcher-setup


## 2. Cacheをインストールする
Cacheをインストールします。
インストーラの実行途中でも、[Cacheのインストール](https://docs.omniverse.nvidia.com/prod_install-guide/prod_launcher/installing_launcher.html#install-cache)について聞かれますが、Skipしてした場合には、次の手順でインストールします。

### 2.1 Omniverseを起動します
![](https://storage.googleapis.com/zenn-user-upload/5f584063cb6b-20220213.png)
### 2.2 Cacheをインストールします
ヘッダーの”Exchage”を選択します。選択すると、インストール可能なアプリケーションとプラグインが表示されます。
![](https://storage.googleapis.com/zenn-user-upload/e46b20f74570-20220213.png)

アプリケーションの中からOmniverse Cacheを選択します。
![](https://storage.googleapis.com/zenn-user-upload/90c4b374a31f-20220213.png)
バージョンを”2021.2.2"に指定し、INSRTALLを選択します。
![](https://storage.googleapis.com/zenn-user-upload/ae167199708c-20220213.png)
インストール後、””にアクセスし、Cacheのインストールを確認します。
![](https://storage.googleapis.com/zenn-user-upload/a4a3d1538161-20220213.png)

## 3. Nucleus のサービスをローカル環境に作成する
ヘッダーのNUCLEUSを選択し、”Add Local Nucleus Service"を選択します。
![](https://storage.googleapis.com/zenn-user-upload/f76712a434c5-20220213.png)
Data Pathに問題がなければ、NEXTを選択します。
![](https://storage.googleapis.com/zenn-user-upload/d973e2be378d-20220213.png)
サービスのAdiministrator情報を設定します。
![](https://storage.googleapis.com/zenn-user-upload/1c89e9f187f6-20220213.png)
サービスが立ち上がっていることを確認します。
![](https://storage.googleapis.com/zenn-user-upload/706017fd3913-20220213.png)

## 4. Issac simをインストールする
Issac Simのインストール手順はこちらに記載されています。この手順通りに進めると、インストールすことができます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/install_basic.html#isaac-sim-app-install-basic

ヘッダーの”Exchage”を選択します。選択すると、インストール可能なアプリケーションとプラグインが表示されます。
アプリケーションの中からISSAC SIMを選択します。
![](https://storage.googleapis.com/zenn-user-upload/0fa48f6af8e1-20220213.png)

バージョンを”2021.2.1"に設定し、”INSTALL”を選択します。
インストール時間は20分程かかります。
![](https://storage.googleapis.com/zenn-user-upload/5fd4a5bf96a5-20220213.png)
インストール完了後、バージョン横の”INSTALL”部分が”LAUNCH”に変わります。
この”LAUNCH”を選択します。
![](https://storage.googleapis.com/zenn-user-upload/c16dacff48d6-20220213.png)
”Issac Sim”を選択し、一番したにある”START"を選択します。
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)
”Issac Sim”の初回起動時に、Exampleのデータをダウンロードするためのフォルダーが存在しないという警告が出てきます。
”Delete folder before download"にチェックを入れて、”Download assets”を選択すれば、Exampleのデータをダウンロードするためのフォルダーが自動的に作成されます。
![](https://storage.googleapis.com/zenn-user-upload/9d39a4156ccd-20220213.png)
Assetsのダウンロードには、Issac simのインストールよりも長い時間が必要です（30分程度）
![](https://storage.googleapis.com/zenn-user-upload/842d76dfc50c-20220213.png)
Assetsのダウンロードが完了できれば、Issac Simのインストールは完了です。
