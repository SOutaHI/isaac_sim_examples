# 概要
Hello robotのチュートリアルを進めます。
このチュートリアルはmulti robotのチュートリアルの6部の内の2部目です。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_required_hello_robot.html

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

hellow worldのExampleのソースコードに処理を追加し、シーンにNvidia Jetbotを追加します。
まず、Extentsionの機能を用いて、以前作成したAwesome ExampleのコードにNvidia Jetobotを追加します。
次の手順を進めます。

1. Awesome Exampleのソースコードを開く
2. Nviida Jetbotの処理を追加する

## 1. Awesome Exampleのソースコードを開く
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 Awesome Exampleのソースコードを表示
メニューバーのIsaac Examples > Awesome Exampleを選択します。

次に、Awesome Examplesのウィンドウの右上にある3つのボタンの内、一番左側のOpen Source Codeボタンを選択します。
選択すると、がVScodeが開き、Hello Worldのソースコードが表示されます。

## 2. Nvidia Jetbotの処理を追加する 
vscode上で、hello_world.pyを編集します。

## 2.1 Jetbotの追加
hello_world.pyのsetup_sceneメソッドに次の処理を追加します。

~~~ hello_world.py:Python3

from omni.isaac.examples.base_sample import BaseSample
from omni.isaac.core.utils.nucleus import find_nucleus_server
from omni.isaac.core.utils.stage import add_reference_to_stage
from omni.isaac.core.robots import Robot
import carb

class HelloWorld(BaseSample):
    def __init__(self) -> None:
        super().__init__()
        return

    def setup_scene(self):
        world = self.get_world()
        world.scene.add_default_ground_plane()
        result, nucleus_server = find_nucleus_server()
        if result is False:
            carb.log_error("Could not find nucleus server with /Isaac folder")
        asset_path = nucleus_server + "/Isaac/Robots/Jetbot/jetbot.usd"
        add_reference_to_stage(usd_path=asset_path, prim_path="/World/Fancy_Robot")
        jetbot_robot = world.scene.add(Robot(prim_path="/World/Fancy_Robot", name="fancy_robot"))
        self.log_info("Num of degrees of freedom before first reset: " + str(jetbot_robot.num_dof)) # prints None
        return

    async def setup_post_load(self):
        self._world = self.get_world()
        self._jetbot = self._world.scene.get_object("fancy_robot")
        self.log_info("Num of degrees of freedom after first reset: " + str(self._jetbot.num_dof)) # prints 2
        self.log_info("Joint Positions after first reset: " + str(self._jetbot.get_joint_positions()))
        returnfrom omni.isaac.examples.base_sample import BaseSample
import numpy as np
# Can be used to create a new cube or to point to an already existing cube in stage.
from omni.isaac.core.objects import DynamicCuboid ## add code

class HelloWorld(BaseSample):
    def __init__(self) -> None:
        super().__init__()
        return

    def setup_scene(self):
        world = self.get_world()
        world.scene.add_default_ground_plane()
        ## add code
        fancy_cube = world.scene.add(
            DynamicCuboid(
                prim_path="/World/random_cube", # The prim path of the cube in the USD stage
                name="fancy_cube", # The unique name used to retrieve the object from the scene later on
                position=np.array([0, 0, 100.0]), # Using the current stage units which is cms by default.
                size=np.array([50.15, 50.15, 50.15]), # most arguments accept mainly numpy arrays.
                color=np.array([0, 0, 1.0]), # RGB channels, going from 0-1
            ))
        return
~~~


追加後、Ctrl＋Saveとhot reloadが実行されます。

メニューバーのIsaac Examples > Awesome Exampleを選択し、Loadを選択すると、ソースコードの変更部分が反映された状態で表示されます。
この状態で、Viewportの左側のPLAYボタンを押すと、Cubeが重力により落下します。

## 2.2 Jetbotを動かす
JetbotのJoint部にPD Controllerを追加し、速度制御で動かします。

cubeの位置と速度をロード字の1回のみ取得する場合には、次の処理を追加します。
hello_world.pyを次の様に編集します。

~~~ hello_world.py:Python3
from omni.isaac.examples.base_sample import BaseSample
from omni.isaac.core.utils.nucleus import find_nucleus_server
from omni.isaac.core.utils.stage import add_reference_to_stage
from omni.isaac.core.robots import Robot
from omni.isaac.core.utils.types import ArticulationAction
import carb
import numpy as np


class HelloWorld(BaseSample):
    def __init__(self) -> None:
        super().__init__()
        return

    def setup_scene(self):
        world = self.get_world()
        world.scene.add_default_ground_plane()
        result, nucleus_server = find_nucleus_server()
        if result is False:
            carb.log_error("Could not find nucleus server with /Isaac folder")
        asset_path = nucleus_server + "/Isaac/Robots/Jetbot/jetbot.usd"
        add_reference_to_stage(usd_path=asset_path, prim_path="/World/Fancy_Robot")
        jetbot_robot = world.scene.add(Robot(prim_path="/World/Fancy_Robot", name="fancy_robot"))
        return

    async def setup_post_load(self):
        self._world = self.get_world()
        self._jetbot = self._world.scene.get_object("fancy_robot")
        self._jetbot_articulation_controller = self._jetbot.get_articulation_controller()
        self._world.add_physics_callback("sending_actions", callback_fn=self.send_robot_actions)
        return


    def send_robot_actions(self, step_size):
        self._jetbot_articulation_controller.apply_action(ArticulationAction(joint_positions=None,
                                                                            joint_efforts=None,
                                                                            joint_velocities=5 * np.random.rand(2,)))
        return
~~~

追加後、Ctrl＋Saveとhot reloadが実行されます。

メニューバーのIsaac Examples > Awesome Exampleを選択し、Loadを選択すると、ソースコードの変更部分が反映された状態で表示されます。
この状態で、Viewportの左側のPLAYボタンを押すと、Jetbotが直進します。

## 2.3 Jetbot Classの使用

Isaac Simでは、カスタマイズ性が高く、シンプルな特定のロボットのExtensionsが用意されています。
Jetbotにおいては、Jetbotのクラスが用意されており、上記で記述した処理をより容易に実現することが可能です。

hello_world.pyを次の様に編集します。

~~~ hello_world.py:Python3
from omni.isaac.examples.base_sample import BaseSample
from omni.isaac.jetbot import Jetbot
from omni.isaac.core.utils.types import ArticulationAction
import numpy as np


class HelloWorld(BaseSample):
    def __init__(self) -> None:
        super().__init__()
        return

    def setup_scene(self):
        world = self.get_world()
        world.scene.add_default_ground_plane()

        jetbot_robot = world.scene.add(Jetbot(prim_path="/World/Fancy_Robot", name="fancy_robot"))
        return

    async def setup_post_load(self):
        self._world = self.get_world()
        self._jetbot = self._world.scene.get_object("fancy_robot")
        self._world.add_physics_callback("sending_actions", callback_fn=self.send_robot_actions)
        return


    def send_robot_actions(self, step_size):
        self._jetbot.apply_wheel_actions(ArticulationAction(joint_positions=None,
                                                            joint_efforts=None,
                                                            joint_velocities=5 * np.random.rand(2,)))
        return
~~~

追加後、Ctrl＋Saveとhot reloadが実行されます。

メニューバーのIsaac Examples > Awesome Exampleを選択し、Loadを選択すると、ソースコードの変更部分が反映された状態で表示されます。
この状態で、Viewportの左側のPLAYボタンを押すと、Jetbotが直進します。


