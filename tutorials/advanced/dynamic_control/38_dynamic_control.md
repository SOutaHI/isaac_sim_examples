# 概要
Dynamic Control Extensionを使用し、ロボットを動かします。

Issac SimのExtension部分に上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/ext_omni_isaac_dynamic_control.html#isaac-dynamic-control

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
Dynamic Control Extensionは、オブジェクトを制御する為の拡張機能です。
拡張機能には、位置制御、速度制御、トルク制御等の制御器が含まれています。
また、軸単位の制御や位置、速度の値の取得も可能です。

APIのReferenceはこちらに記載されています。
https://docs.omniverse.nvidia.com/py/isaacsim/source/extensions/omni.isaac.dynamic_control/docs/index.html

今回は、簡易的なデモとしてScript Editorからの実行及び、2つのExampleを実行します。

1. Script Editorからの実行
2. Read Articulations Exampleの実行
3. Joint Controller Exampleの実行


## 1. Script Editorからの実行
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 ロボットをロードする
メニューバーのcreate > Isaac > Robots > From Library > Manipulators > Frankaを選択します。
![](https://storage.googleapis.com/zenn-user-upload/9894fb8f0bae-20220507.png)

ロボットの読み込みの完了後、右側のツールバーの中のPlayボタンを選択し、シミュレーションを開始します。
![](https://storage.googleapis.com/zenn-user-upload/22a75b2ba601-20220507.png)

### 1.3　Script Editorの起動する
メニューバーのWindow > Script Editorを選択します。
![](https://storage.googleapis.com/zenn-user-upload/933af9468939-20220507.png)

### 1.4 制御器を追加する
今回は速度制御器を追加します。
位置制御やトルク制御の例は以下のURLに記載されています。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/ext_omni_isaac_dynamic_control.html#code-snippets


先ほど起動したScript Editorに以下のコードを追加します。

~~~ script editor:Python3
import omni
omni.timeline.get_timeline_interface().play()

from pxr import UsdPhysics
stage = omni.usd.get_context().get_stage()
for prim in stage.TraverseAll():
    prim_type = prim.GetTypeName()
    if prim_type in ["PhysicsRevoluteJoint" , "PhysicsPrismaticJoint"]:
        if prim_type == "PhysicsRevoluteJoint":
            drive = UsdPhysics.DriveAPI.Get(prim, "angular")
        else:
            drive = UsdPhysics.DriveAPI.Get(prim, "linear")
        drive.GetStiffnessAttr().Set(0)
from omni.isaac.dynamic_control import _dynamic_control
import numpy as np
dc = _dynamic_control.acquire_dynamic_control_interface()
#Note: getting the articulation has to happen after changing the drive stiffness
articulation = dc.get_articulation("/Franka")
dc.wake_up_articulation(articulation)
joint_vels = [-np.random.rand(9)*10]
dc.set_articulation_dof_velocity_targets(articulation, joint_vels)
~~~

![](https://storage.googleapis.com/zenn-user-upload/16bd0cc5ccdd-20220507.png)

### 1.5 追加した制御器からロボットを動かす
script Editor内で、RUNを選択すると、ロボットが動きます。

## 2. Read Articulations Exampleの実行
このExampleでは、ロボットの各種関節情報を取得することができます。

### 2.1 Exampleをロードする
メニューバーのIsaac Examples > Dynamic Control > Read Articulationsを選択します。
![](https://storage.googleapis.com/zenn-user-upload/0b3ee40c73f4-20220507.png)
![](https://storage.googleapis.com/zenn-user-upload/ba63100146eb-20220507.png)

### 2.2 ロボットをロードする
ポップアップしたウィンドウにおいて、”Load Robot”を選択します。
選択すると、ロボットがロードされます。
![](https://storage.googleapis.com/zenn-user-upload/0da6f5caee11-20220507.png)

ロボットの読み込みの完了後、右側のツールバーの中のPlayボタンを選択し、シミュレーションを開始します

### 2.3 関節情報を取得する
ポップアップしたウィンドウにおいて、”Get Articulation Information”を選択します。
選択すると、ロボットの関節情報がポップアップしたウィンドウに表示されます。
![](https://storage.googleapis.com/zenn-user-upload/1eb2cbea1de1-20220507.png)

## 3. Joint Controller Exampleの実行
このExampleでは、ロボットを動かしつつ、ロボットの各種関節情報を取得することができます。

### 3.1 Exampleをロードする
メニューバーのIsaac Examples > Dynamic Control > Joint Controllerを選択します。
![](https://storage.googleapis.com/zenn-user-upload/54e486b3e932-20220507.png)
![](https://storage.googleapis.com/zenn-user-upload/40492944a8f8-20220507.png)

### 3.2 ロボットをロードする
ポップアップしたウィンドウにおいて、”Load Robot”を選択します。
選択すると、ロボットがロードされます。
![](https://storage.googleapis.com/zenn-user-upload/5894318a7250-20220507.png)

ロボットの読み込みの完了後、右側のツールバーの中のPlayボタンを選択し、シミュレーションを開始します

### 3.3 関節情報を取得する
ポップアップしたウィンドウにおいて、”move”を選択します。
選択すると、ロボットが動き、ロボットの関節情報がポップアップしたウィンドウに表示されます。
![](https://storage.googleapis.com/zenn-user-upload/e5b8b5ac76f0-20220507.png)











