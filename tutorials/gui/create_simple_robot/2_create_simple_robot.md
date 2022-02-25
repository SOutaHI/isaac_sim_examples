
# 概要
Nvidia Issac simのGUIの機能を使用し、簡単な2輪ロボットのモデルを作成します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_gui_simple_robot.html

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

大まかな手順は次の通りです。

1. 環境設定
2. 2輪ロボットのモデル作成


以下内容のシーンファイルはこちらからダウンロードできます。(準備中)

## 1. 環境設定
こちらのページに記載してある手順を進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_gui_environment_setup.html


### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 シーンを作成する
メニューバーのCreate ＞ Physics > Phisics Sceneを選択します。
![](https://storage.googleapis.com/zenn-user-upload/7e39de2cccd2-20220219.png)

シーン作成後、右側にあるStageの中で、作成した”Phisical Scene”を選択します。
![](https://storage.googleapis.com/zenn-user-upload/c0a0213b9256-20220219.png)

選択した状態で、右下にあるPropertyの中から、”Enable GPU dynamics”のチェックを外し、”BoardPhase”をMBPにします。
![](https://storage.googleapis.com/zenn-user-upload/2ce1ec23aff4-20220219.png)

ここでは、物理演算をGPUからCPUで計算するように変更しています。シミュレーションするロボットが、何百個のBodyで構成されていない限りはGPUを使用する必要はありません。


## 1.3 Groud Planeの追加
作成したシーンないに、Groud planeを追加します。
メニューバーのCreate ＞ Physics > Ground Planeを選択します。
![](https://storage.googleapis.com/zenn-user-upload/deaf7d08255d-20220219.png)

Groud Planeにグリッドが表示されていない場合には、Viewport上部にあるEyeマークをクリックし、Gridにチェックを入れます。


## 1.4 ライト（光源）の追加
シーン内にライトを追加します。
メニューバーのCreate ＞ Light > Sphere Lightを選択します。
![](https://storage.googleapis.com/zenn-user-upload/dde19643f4a8-20220219.png)

ライト追加後、右側にあるStageの中で、作成した”Spherelightを選択します。
選択した状態で、右下にあるPropertyの中から、次の値を変更します。
- TransformのtranslateのＺを700にする
- TransformのrotationのＸ軸とＹ軸の値をそれぞれ0にする
- MainのColorを任意の色に設定する（左側の現在の設定色部分をクリックすると、色を選択することができます）
- MainのIntensityの値を1e6に設定する
- Raw USD propertiesのShapingのangleを45に設定する
- Raw USD propertiesのsharpingのsoftnessを0.05に設定する

![](https://storage.googleapis.com/zenn-user-upload/220fbf9e5bcc-20220219.png)
![](https://storage.googleapis.com/zenn-user-upload/a290b041220c-20220219.png)
![](https://storage.googleapis.com/zenn-user-upload/d5dad43257eb-20220219.png)

上記設定後、右側にあるStageの中で、作成した”defaultlightを選択します。
選択した状態で、右下にあるPropertyの中から、次の値を変更します。

- MainのIntensityの値を300に設定する

![](https://storage.googleapis.com/zenn-user-upload/6dfc9e709d9b-20220219.png)

上記の手順で環境設定は完了です。


## 2. 2輪ロボットモデルの作成
こちらのページに記載されている手順を進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_gui_simple_objects.html#isaac-sim-app-tutorial-gui-simple-objects


### 2.1 Rigid Bodyをシーン内に追加する
2輪ロボットは1つの直方体と、2つの円柱で構成します。
まず、直方体をシーンに追加します。
メニューバーのCreate ＞ Mesh > Cubeを選択します。

![](https://storage.googleapis.com/zenn-user-upload/2662dfe42ae3-20220219.png)
追加後、右側にあるStageの中で、作成した"Cube"を選択します。
選択した状態で、右下にあるPropertyの中から、次の値を変更します。
- TransformのtranslateのＺを100にする
- TransformのScaleのXを2.0、Zを0.5にする

次に2個の円柱を追加します。
メニューバーのCreate ＞ Mesh > Cylinderを選択します。
![](https://storage.googleapis.com/zenn-user-upload/ce2d86c777fb-20220219.png)
追加後、右側にあるStageの中で、作成した"Cylinder"を選択します。
選択した状態で、右下にあるPropertyの中から、次の値を変更します。
- Transformのtranslateのyを150、Zを100にする
- TransformのRotationのXを90にする

設定後、右側にあるStageの中で、作成した"Cylinder"を選択し、右クリックします。
右クリックした時に出てくるウィンドウの中から、”Deplicate”を選択します。

複製した"Cylinder"を選択します。
![](https://storage.googleapis.com/zenn-user-upload/e32058ecd2f8-20220219.png)
選択した状態で、右下にあるPropertyの中から、次の値を変更します。
- Transformのtranslateのyを-150にする


### 2.2 物理プロパティの追加
シーン内に追加したオブジェクトに物理プロパティを追加します。
右側にあるStageの中で、作成した3つのオブジェクトをCtrl+Shiftキーを押しながら選択します。

![](https://storage.googleapis.com/zenn-user-upload/c0164c8919d7-20220219.png)

選択したまま、右下のPropertyの”＋Add”をクリックし、Phisics ＞Rigid Body with Coliders Presetを選択します。

選択すると、それぞれのオブジェクトに重力等の物理プロパティと衝突判定のプロパティが追加されます。

この状態で、左側のツールバーからStartボタンを押すと、シミュレーションが開始され、オブジェクトに重力が適用されます。
![](https://storage.googleapis.com/zenn-user-upload/e3de64778056-20220219.png)
シミュレーションを終了する場合には、Stopボタンを選択します。

物理プロパティの削除や、衝突判定用のメッシュの可視化、接触/摩擦プロパティの追加については、tutorialに記載されています。

[物理プロパティの削除](https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_gui_simple_objects.html#adding-physics-properties)
![](https://storage.googleapis.com/zenn-user-upload/556c2177adc1-20220219.png)

[衝突判定用のメッシュの可視化](https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_gui_simple_objects.html#examine-collision-meshes)
![](https://storage.googleapis.com/zenn-user-upload/b63248d03145-20220219.png)
[接触/摩擦プロパティの追加](https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_gui_simple_objects.html#adding-contact-and-friction-parameters)

## 2.3　マテリアルプロパティの追加
オブジェクトのマテリアル情報を設定するプロパティを追加します。

メニューバーのCreate ＞ Materials > OmniPBRを選択します。
上記をもう1回繰り返します。
![](https://storage.googleapis.com/zenn-user-upload/94bc6f19a2e3-20220219.png)

右側にあるStageの中で、追加したOmniPBRをそれぞれ右クリックし、renameを選択します。
名称は次を指定します
- body
- wheel

右側にあるStageの中で、直方体のオブジェクトを選択します。
右下のPropertyの”Materials on selected models”の中のマテリアルの指定を先ほど作成した”body”のパスに変更します。

![](https://storage.googleapis.com/zenn-user-upload/33f0886c3a5f-20220219.png)

右側にあるStageの中で、円柱のオブジェクトを選択します。
右下のPropertyの”Materials on selected models”の中のマテリアルの指定を先ほど作成した”wheel”のパスに変更します。
もう一方の円柱のオブジェクトに対しても、同様の手順を行います。

右側にあるStageの中で、Looks > Bodyを選択します。
右下のPropertyの” Material and Shader”のAlbedoの中Colorを任意の色に変更します。
色を変更すると、シーン内の直方体の色が指定した色に変化します。

右側にあるStageの中で、Looks > Wheelを選択します。
bodyと同様の手順でオブジェクトの色を任意の色に変更します。

bodyとwheelの色選択の完了後、右側にあるStageの中で、defaultLightのEyeボタンを入り切りします。
それぞれのオブジェクトに色が設定されていることを確認します。

![](https://storage.googleapis.com/zenn-user-upload/11764ff804cd-20220219.png)