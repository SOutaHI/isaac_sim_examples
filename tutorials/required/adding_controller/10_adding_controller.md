# 概要
Adding Controllerのチュートリアルを進めます。
このチュートリアルはmulti robotのチュートリアルの6部の内の3部目です。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_required_adding_controller.html

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

Jetbotを追加したAwesome worldのExampleのソースコードに処理を追加し、シーンにNvidia Jetbotを追加します。
次の手順を進めます。

1. Awesome Exampleのソースコードを開く
2. Custom Controllerを追加する

## 1. Awesome Exampleのソースコードを開く
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 Awesome Exampleのソースコードを表示
メニューバーのIsaac Examples > Awesome Exampleを選択します。

次に、Awesome Examplesのウィンドウの右上にある3つのボタンの内、一番左側のOpen Source Codeボタンを選択します。
選択すると、がVScodeが開き、Hello Worldのソースコードが表示されます。

## 2. Custom Controllerを追加する 
vscode上で、hello_world.pyを編集します。

## 2.1 Custom Controllerの追加
Jetbotの差動２輪にオープンループのControllerを追加します。
BaseControllerのクラスを継承したクラスを作成し、オープンループ制御のクラスを作成します。
hello_world.pyのsetup_sceneメソッドに次の処理を追加します。

~~~ hello_world.py:Python3

from omni.isaac.examples.base_sample import BaseSample
from omni.isaac.jetbot import Jetbot
from omni.isaac.core.utils.types import ArticulationAction
from omni.isaac.core.controllers import BaseController
import numpy as np

class CoolController(BaseController):
    def __init__(self):
        super().__init__(name="my_cool_controller")
        self._wheel_radius = 3
        self._wheel_base = 11.25
        return

    def forward(self, command):
        joint_velocities = [0.0, 0.0]
        joint_velocities[0] = ((2 * command[0]) - (command[1] * self._wheel_base)) / (2 * self._wheel_radius)
        joint_velocities[1] = ((2 * command[0]) + (command[1] * self._wheel_base)) / (2 * self._wheel_radius)
        return ArticulationAction(joint_velocities=joint_velocities)

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
        # Initialize our controller after load and the first reset
        self._my_controller = CoolController()
        return

    def send_robot_actions(self, step_size):
        self._jetbot.apply_wheel_actions(self._my_controller.forward(command=[20.0, np.pi/ 4]))
        return
~~~


追加後、Ctrl＋Saveとhot reloadが実行されます。

メニューバーのIsaac Examples > Awesome Exampleを選択し、Loadを選択すると、ソースコードの変更部分が反映された状態で表示されます。
この状態で、Viewportの左側のPLAYボタンを押すと、Jetbotが円を描きながら移動します。

## 2.2 Available Controllersの使用

汎用的なControllerはクラスとして用意されています。
今回は、DifferentialControllerクラスを使用し、上記で記述した処理をより容易に実現します。

hello_world.pyを次の様に編集します。

~~~ hello_world.py:Python3
from omni.isaac.examples.base_sample import BaseSample
from omni.isaac.jetbot import Jetbot
from omni.isaac.motion_generation import WheelBasePoseController
from omni.isaac.jetbot.controllers import DifferentialController
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
        self._my_controller = WheelBasePoseController(name="cool_controller",
                                                    open_loop_wheel_controller=DifferentialController(name="open_loop_controller"),
                                                    is_holonomic=False)
        return

    def send_robot_actions(self, step_size):
        position, orientation = self._jetbot.get_world_pose()
        self._jetbot.apply_wheel_actions(self._my_controller.forward(start_position=position,
                                                                     start_orientation=orientation,
                                                                     goal_position=np.array([80, 80])))
        return
~~~

追加後、Ctrl＋Saveとhot reloadが実行されます。

メニューバーのIsaac Examples > Awesome Exampleを選択し、Loadを選択すると、ソースコードの変更部分が反映された状態で表示されます。
この状態で、Viewportの左側のPLAYボタンを押すと、Jetbotが回転し、直進します。
