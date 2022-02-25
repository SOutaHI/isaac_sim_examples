# 概要
PythonのAPIからシーンを操作します。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_gui_script_editor.html

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



# 概要

GUIでの操作はすべてPythonのAPIから操作することが可能です。
（逆にPython APIからのすべての操作がGUI上から実行できるわけではない為。注意します）

issac Sim上でのPython APIは、Omniverse USD APIとIssac Sim core APIが用意されています。
今回は、2つのAPIを用いて、シーン内に立方体を追加します。

1. GUI内にScript Editorを追加
2. USD APIでシーンを操作
3. Issac Sim Core APIでシーンを操作

以下内容のシーンファイルはこちらからダウンロードできます。
()[]

## 1. GUI内にScript Editorを追加
こちらのページに記載してある手順を進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_gui_script_editor.html

### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 Script Editorを追加
メニューバーのWindow > Script Editpを選択します。

![](https://storage.googleapis.com/zenn-user-upload/b117cde55aeb-20220226.png)
![](https://storage.googleapis.com/zenn-user-upload/a4c8f69922b6-20220226.png)
選択後、GUIの任意の位置にドラッグアンドドロップで配置します。
![](https://storage.googleapis.com/zenn-user-upload/9b3aae3ff2cb-20220226.png)

## 2. USD APIでシーンを操作
追加したScript EditorにUSD APIを用いてシーンを操作するScriptを記述し、実行します。
追加したScript Editorに次のコードを記述します。

~~~ usd_api.py:Python3
from pxr import UsdPhysics, PhysxSchema, Gf, PhysicsSchemaTools
import omni

stage = omni.usd.get_context().get_stage()

# Setting up Physics Scene
gravity = 980
scene = UsdPhysics.Scene.Define(stage, "/World/physics")
scene.CreateGravityDirectionAttr().Set(Gf.Vec3f(0.0, 0.0, -1.0))
scene.CreateGravityMagnitudeAttr().Set(gravity)
PhysxSchema.PhysxSceneAPI.Apply(stage.GetPrimAtPath("/World/physics"))
physxSceneAPI = PhysxSchema.PhysxSceneAPI.Get(stage, "/World/physics")
physxSceneAPI.CreateEnableCCDAttr(True)
physxSceneAPI.CreateEnableStabilizationAttr(True)
physxSceneAPI.CreateEnableGPUDynamicsAttr(False)
physxSceneAPI.CreateBroadphaseTypeAttr("MBP")
physxSceneAPI.CreateSolverTypeAttr("TGS")

# Setting up Ground Plane
PhysicsSchemaTools.addGroundPlane(stage, "/World/groundPlane", "Z", 1500, Gf.Vec3f(0,0,0), Gf.Vec3f(0.5))

# Adding a Cube
path = "/World/Cube"
cubeGeom = UsdGeom.Cube.Define(stage, path)
cubePrim = stage.GetPrimAtPath(path)
size = 50
offset = Gf.Vec3f(50,20,100)
cubeGeom.CreateSizeAttr(size)
cubeGeom.AddTranslateOp().Set(offset)

# Attach Rigid Body and Collision Preset
rigid_api = UsdPhysics.RigidBodyAPI.Apply(cubePrim)
rigid_api.CreateRigidBodyEnabledAttr(True)
UsdPhysics.CollisionAPI.Apply(cubePrim
~~~
![](https://storage.googleapis.com/zenn-user-upload/c551a814a5aa-20220226.png)
記述後、Ctrl+Enterを押すとScriptが実行され、シーン内に立方体が追加されます。

![](https://storage.googleapis.com/zenn-user-upload/0e78addf0eae-20220226.png)
左側のツールバーのシミュレーションStartボタンを押すと、重力により立方体が落下します。
![](https://storage.googleapis.com/zenn-user-upload/2f92d63285e7-20220226.png)

## 2. Issac Sim Core APIでシーンを操作
追加したScript EditorにIssac Sim Core APIを用いてシーンを操作するScriptを記述し、実行します。
Issac Sim Core APIでは、USD APIと比較して、よりロボットの操作が容易になるように設計されています。
その為、Issac Sim Core APIでは、上記で示したUSD APIでのScriptで実現したシーンを、より少ないコード量で実現することが可能です。

以下のスクリプトを実行する場合には、上記とは別の空のシーンを作成します。
![](https://storage.googleapis.com/zenn-user-upload/507e538f2ca3-20220226.png)

追加したScript Editorに次のコードを記述します。

~~~ issac_sim_core_api.py:Python3

import numpy as np
from omni.isaac.core.objects import DynamicCuboid
from omni.isaac.core.objects.ground_plane import GroundPlane

GroundPlane(prim_path="/World/groundPlane", size=1000, color=np.array([0.5, 0.5, 0.5]))
DynamicCuboid(prim_path="/World/cube",
    position=np.array([50, 20, 100.0]),
    size=np.array([50, 50, 50]),
    color=np.array([20,30,0]))
~~~

記述後、Ctrl+Enterを押すとScriptが実行され、シーン内に立方体が追加されます。
![](https://storage.googleapis.com/zenn-user-upload/c4d1fa15a538-20220226.png)

左側のツールバーのシミュレーションStartボタンを押すと、重力により立方体が落下します。