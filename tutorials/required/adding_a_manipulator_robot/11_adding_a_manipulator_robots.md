# 概要
Adding a manipulator robotのチュートリアルを進めます。
このチュートリアルはmulti robotのチュートリアルの6部の内の4部目です。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_required_adding_manipulator.html

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

Jetbotを追加したAwesome worldのExampleのソースコードに処理を追加し、シーンにFrankaを追加します。
また、シーンに追加したFrankaを用いて、CubeをPick-and-Placeするタスクを実行します。
次の手順を進めます。

1. Awesome Exampleのソースコードを開く
2. Frankaを追加する

## 1. Awesome Exampleのソースコードを開く
### 1.1 OmniverseからIssac Simを起動する
![](https://storage.googleapis.com/zenn-user-upload/a1927915e055-20220213.png)

### 1.2 Awesome Exampleのソースコードを表示
メニューバーのIsaac Examples > Awesome Exampleを選択します。
![](https://storage.googleapis.com/zenn-user-upload/4571181c5f85-20220312.png)

次に、Awesome Examplesのウィンドウの右上にある3つのボタンの内、一番左側のOpen Source Codeボタンを選択します。
![](https://storage.googleapis.com/zenn-user-upload/366767d02577-20220312.png)

選択すると、がVScodeが開き、Hello Worldのソースコードが表示されます。
![](https://storage.googleapis.com/zenn-user-upload/8e09ad29950b-20220312.png)

## 2. Frankaを追加する 
vscode上で、hello_world.pyを編集します。

## 2.1 Frankaの追加
シーンにFrankaを追加します。
hello_world.pyのsetup_sceneメソッドに次の処理を追加します。

~~~ hello_world.py:Python3
from omni.isaac.examples.base_sample import BaseSample
from omni.isaac.franka import Franka
from omni.isaac.core.objects import DynamicCuboid
import numpy as np

class HelloWorld(BaseSample):
    def __init__(self) -> None:
        super().__init__()
        return

    def setup_scene(self):
        world = self.get_world()
        world.scene.add_default_ground_plane()
        franka = world.scene.add(Franka(prim_path="/World/Fancy_Franka", name="fancy_franka"))
        world.scene.add(
            DynamicCuboid(
                prim_path="/World/random_cube",
                name="fancy_cube",
                position=np.array([30, 30, 30]),
                size=np.array([5.15, 5.15, 5.15]),
                color=np.array([0, 0, 1.0]),
            )
        )
        return
~~~

追加後、Ctrl＋Saveとhot reloadが実行されます。

メニューバーのIsaac Examples > Awesome Exampleを選択し、Loadを選択すると、ソースコードの変更部分が反映された状態で表示されます。


## 2.2 PickAndPlace Controllerの追加

FrankaのクラスのControllerに含まれているPickAndPlace Controllerを追加します。
hello_world.pyを次の様に編集します。

~~~ hello_world.py:Python3
from omni.isaac.examples.base_sample import BaseSample
from omni.isaac.franka import Franka
from omni.isaac.core.objects import DynamicCuboid
from omni.isaac.franka.controllers import PickPlaceController
import numpy as np


class HelloWorld(BaseSample):
    def __init__(self) -> None:
        super().__init__()
        return

    def setup_scene(self):
        world = self.get_world()
        world.scene.add_default_ground_plane()
        franka = world.scene.add(Franka(prim_path="/World/Fancy_Franka", name="fancy_franka"))
        world.scene.add(
            DynamicCuboid(
                prim_path="/World/random_cube",
                name="fancy_cube",
                position=np.array([30, 30, 30]),
                size=np.array([5.15, 5.15, 5.15]),
                color=np.array([0, 0, 1.0]),
            )
        )
        return

    async def setup_post_load(self):
        self._world = self.get_world()
        self._franka = self._world.scene.get_object("fancy_franka")
        self._fancy_cube = self._world.scene.get_object("fancy_cube")
        self._controller = PickPlaceController(
            name="pick_place_controller",
            gripper_dof_indices=self._franka.gripper.dof_indices,
            robot_prim_path=self._franka.prim_path,
        )
        self._world.add_physics_callback("sim_step", callback_fn=self.physics_step)
        await self._world.play_async()
        return

    async def setup_post_reset(self):
        self._controller.reset()
        await self._world.play_async()
        return

    def physics_step(self, step_size):
        cube_position, _ = self._fancy_cube.get_world_pose()
        goal_position = np.array([-30, -30, 5.15 / 2.0])
        current_joint_positions = self._franka.get_joint_positions()
        actions = self._controller.forward(
            picking_position=cube_position,
            placing_position=goal_position,
            current_joint_positions=current_joint_positions,
        )
        self._franka.apply_action(actions)
        if self._controller.is_done():
            self._world.pause()
        return
~~~

追加後、Ctrl＋Saveとhot reloadが実行されます。

メニューバーのIsaac Examples > Awesome Exampleを選択し、Loadを選択すると、ソースコードの変更部分が反映された状態で表示されます。
この状態で、Viewportの左側のPLAYボタンを押すと、FrankatがCubeをPickし、Goal PositionにPlaceします。

## 2.3 Taskクラスの使用

Isaac Simには、Taskクラスがあり、このクラスを継承したクラスを使用することによって、より複雑なシーンを表現することが可能です。
上記で示したFrakaによるPick and  Placeをtaskクラスによって記述します。 
hello_world.pyを次の様に編集します。

~~~ hello_world.py:Python3
from omni.isaac.examples.base_sample import BaseSample
from omni.isaac.franka import Franka
from omni.isaac.core.objects import DynamicCuboid
from omni.isaac.franka.controllers import PickPlaceController
from omni.isaac.core.tasks import BaseTask
import numpy as np

class FrankaPlaying(BaseTask):
    def __init__(self, name):
        super().__init__(name=name, offset=None)
        self._goal_position = np.array([-30, -30, 5.15 / 2.0])
        self._task_achieved = False
        return

    # Here we setup all the assets that we care about in this task.
    def set_up_scene(self, scene):
        super().set_up_scene(scene)
        scene.add_default_ground_plane()
        self._cube = scene.add(DynamicCuboid(prim_path="/World/random_cube",
                                            name="fancy_cube",
                                            position=np.array([30, 30, 30]),
                                            size=np.array([5.15, 5.15, 5.15]),
                                            color=np.array([0, 0, 1.0])))
        self._franka = scene.add(Franka(prim_path="/World/Fancy_Franka",
                                        name="fancy_franka"))
        return

    def get_observations(self):
        cube_position, _ = self._cube.get_world_pose()
        current_joint_positions = self._franka.get_joint_positions()
        observations = {
            self._franka.name: {
                "joint_positions": current_joint_positions,
            },
            self._cube.name: {
                "position": cube_position,
                "goal_position": self._goal_position
            }
        }
        return observations

    def pre_step(self, control_index, simulation_time):
        cube_position, _ = self._cube.get_world_pose()
        if not self._task_achieved and np.mean(np.abs(self._goal_position - cube_position)) < 2:
            self._cube.get_applied_visual_material().set_color(color=np.array([0, 1.0, 0]))
            self._task_achieved = True
        return


class HelloWorld(BaseSample):
    def __init__(self) -> None:
        super().__init__()
        return

    def setup_scene(self):
        world = self.get_world()
        world.add_task(FrankaPlaying(name="my_first_task"))
        return

    async def setup_post_load(self):
        self._world = self.get_world()
        self._franka = self._world.scene.get_object("fancy_franka")
        self._controller = PickPlaceController(
            name="pick_place_controller",
            gripper_dof_indices=self._franka.gripper.dof_indices,
            robot_prim_path=self._franka.prim_path,
        )
        self._world.add_physics_callback("sim_step", callback_fn=self.physics_step)
        await self._world.play_async()
        return

    async def setup_post_reset(self):
        self._controller.reset()
        await self._world.play_async()
        return

    def physics_step(self, step_size):
        current_observations = self._world.get_observations()
        actions = self._controller.forward(
            picking_position=current_observations["fancy_cube"]["position"],
            placing_position=current_observations["fancy_cube"]["goal_position"],
            current_joint_positions=current_observations["fancy_franka"]["joint_positions"],
        )
        self._franka.apply_action(actions)
        if self._controller.is_done():
            self._world.pause()
        return
~~~

追加後、Ctrl＋Saveとhot reloadが実行されます。

メニューバーのIsaac Examples > Awesome Exampleを選択し、Loadを選択すると、ソースコードの変更部分が反映された状態で表示されます。
この状態で、Viewportの左側のPLAYボタンを押すと、FrankatがCubeをPickし、Goal PositionにPlaceします。

## 2.4 PickPlaceクラスの使用

また、FrankaクラスのTasksの中に、PickPlaceというクラスが存在します。
Frankaが元々持っているクラスを使用するため、ここで記述するコードは上記の2つの方法より少なくなります。 
hello_world.pyを次の様に編集します。

~~~ hello_world.py:Python3
from omni.isaac.examples.base_sample import BaseSample
from omni.isaac.franka.tasks import PickPlace
from omni.isaac.franka.controllers import PickPlaceController
import numpy as np

class HelloWorld(BaseSample):
    def __init__(self) -> None:
        super().__init__()
        return

    def setup_scene(self):
        world = self.get_world()
        world.add_task(PickPlace(name="awesome_task"))
        return

    async def setup_post_load(self):
        self._world = self.get_world()
        task_params = self._world.get_task("awesome_task").get_params()
        self._franka = self._world.scene.get_object(task_params["robot_name"]["value"])
        self._cube_name = task_params["cube_name"]["value"]
        self._controller = PickPlaceController(
            name="pick_place_controller",
            gripper_dof_indices=self._franka.gripper.dof_indices,
            robot_prim_path=self._franka.prim_path,
        )
        self._world.add_physics_callback("sim_step", callback_fn=self.physics_step)
        await self._world.play_async()
        return

    async def setup_post_reset(self):
        self._controller.reset()
        await self._world.play_async()
        return

    def physics_step(self, step_size):
        current_observations = self._world.get_observations()
        actions = self._controller.forward(
            picking_position=current_observations[self._cube_name]["position"],
            placing_position=current_observations[self._cube_name]["target_position"],
            current_joint_positions=current_observations[self._franka.name]["joint_positions"],
        )
        self._franka.apply_action(actions)
        if self._controller.is_done():
            self._world.pause()
        return
~~~

追加後、Ctrl＋Saveとhot reloadが実行されます。

メニューバーのIsaac Examples > Awesome Exampleを選択し、Loadを選択すると、ソースコードの変更部分が反映された状態で表示されます。
この状態で、Viewportの左側のPLAYボタンを押すと、FrankatがCubeをPickし、Goal PositionにPlaceします。