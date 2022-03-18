# 概要
Mutiple tasksのチュートリアルを進めます。
このチュートリアルはmulti robotのチュートリアルの6部の内の6部目です。

Issac Simのtutorialに上記の内容が記載されており、この内容に沿って進めます。
https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_required_multiple_tasks.html

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

FrankaとJetbotを追加したAwesome worldのExampleのソースコードに処理を追加し、複数のタスクを実行するように編集します。
次の手順を進めます。

1. Awesome Exampleのソースコードを開く
2. taskをパラメタライズする

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

## 2. taskをパラメタライズする 
vscode上で、hello_world.pyを編集します。

## 2.1 offsetパラメータの追加
Offsetパラメータを追加し、タスクのアセットをオフセット分だけ移動させます。
hello_world.pyを次の様に編集します。

~~~ hello_world.py:Python3
from omni.isaac.examples.base_sample import BaseSample
from omni.isaac.franka.tasks import PickPlace
from omni.isaac.franka.controllers import PickPlaceController
from omni.isaac.jetbot import Jetbot
from omni.isaac.motion_generation import WheelBasePoseController
from omni.isaac.jetbot.controllers import DifferentialController
from omni.isaac.core.tasks import BaseTask
from omni.isaac.core.utils.types import ArticulationAction
from omni.isaac.core.utils.string import find_unique_string_name        # Creates a unique prim path
from omni.isaac.core.utils.prims import is_prim_path_valid              # Checks if a prim path is valid
from omni.isaac.core.objects.cuboid import VisualCuboid
import numpy as np


class RobotsPlaying(BaseTask):
    # Adding offset to move the task objects with and the targets..etc
    def __init__(self, name, offset=None):
        super().__init__(name=name, offset=offset)
        self._task_event = 0
        self._jetbot_goal_position = np.array([130, 30, 0]) + self._offset #defined in the base task
        self._pick_place_task = PickPlace(cube_initial_position=np.array([10, 30, 5]),
                                        target_position=np.array([70, -30, 5.15 / 2.0]),
                                        offset=offset)
        return

    def set_up_scene(self, scene):
        super().set_up_scene(scene)
        #This will already translate the pick and place assets by the offset
        self._pick_place_task.set_up_scene(scene)
        # Find a unique scene name
        jetbot_name = find_unique_string_name(
            intitial_name="fancy_jetbot", is_unique_fn=lambda x: not self.scene.object_exists(x)
        )
        # Find a unique prim path
        jetbot_prim_path = find_unique_string_name(
            intitial_name="/World/Fancy_Jetbot", is_unique_fn=lambda x: not is_prim_path_valid(x)
        )
        self._jetbot = scene.add(Jetbot(prim_path=jetbot_prim_path,
                                        name=jetbot_name,
                                        position=np.array([0, 30, 0])))
        self._task_objects[self._jetbot.name] = self._jetbot
        pick_place_params = self._pick_place_task.get_params()
        self._franka = scene.get_object(pick_place_params["robot_name"]["value"])
        current_position, _ = self._franka.get_world_pose()
        self._franka.set_world_pose(position=current_position + np.array([100, 0, 0]))
        self._franka.set_default_state(position=current_position + np.array([100, 0, 0]))
        self._move_task_objects_to_their_frame()
        return

    def get_observations(self):
        current_jetbot_position, current_jetbot_orientation = self._jetbot.get_world_pose()
        observations= {
            "task_event": self._task_event,
            self._jetbot.name: {
                "position": current_jetbot_position,
                "orientation": current_jetbot_orientation,
                "goal_position": self._jetbot_goal_position
            }
        }
        observations.update(self._pick_place_task.get_observations())
        return observations

    def get_params(self):
        pick_place_params = self._pick_place_task.get_params()
        params_representation = pick_place_params
        params_representation["jetbot_name"] = {"value": self._jetbot.name, "modifiable": False}
        params_representation["franka_name"] = pick_place_params["robot_name"]
        return params_representation


    def pre_step(self, control_index, simulation_time):
        if self._task_event == 0:
            current_jetbot_position, _ = self._jetbot.get_world_pose()
            if np.mean(np.abs(current_jetbot_position[:2] - self._jetbot_goal_position[:2])) < 4:
                self._task_event += 1
                self._cube_arrive_step_index = control_index
        elif self._task_event == 1:
            if control_index - self._cube_arrive_step_index == 200:
                self._task_event += 1
        return

    def post_reset(self):
        self._task_event = 0
        return


class HelloWorld(BaseSample):
    def __init__(self) -> None:
        super().__init__()
        return

    def setup_scene(self):
        world = self.get_world()
        world.add_task(RobotsPlaying(name="awesome_task", offset=np.array([0, -100, 0])))
        VisualCuboid(prim_path="/new_cube_1",
                    name="visual_cube",
                    position=np.array([100, 0, 5]),
                    size=np.array([10, 10, 10]))
        return

    async def setup_post_load(self):
        self._world = self.get_world()
        task_params = self._world.get_task("awesome_task").get_params()
        self._franka = self._world.scene.get_object(task_params["franka_name"]["value"])
        self._jetbot = self._world.scene.get_object(task_params["jetbot_name"]["value"])
        self._cube_name = task_params["cube_name"]["value"]
        self._franka_controller = PickPlaceController(name="pick_place_controller",
                                                    gripper_dof_indices=self._franka.gripper.dof_indices,
                                                    robot_prim_path=self._franka.prim_path)
        self._jetbot_controller = WheelBasePoseController(name="cool_controller",
                                                        open_loop_wheel_controller=DifferentialController(name="open_loop_controller"))
        self._world.add_physics_callback("sim_step", callback_fn=self.physics_step)
        await self._world.play_async()
        return

    async def setup_post_reset(self):
        self._franka_controller.reset()
        self._jetbot_controller.reset()
        await self._world.play_async()
        return

    def physics_step(self, step_size):
        current_observations = self._world.get_observations()
        if current_observations["task_event"] == 0:
            self._jetbot.apply_wheel_actions(
                self._jetbot_controller.forward(
                    start_position=current_observations[self._jetbot.name]["position"],
                    start_orientation=current_observations[self._jetbot.name]["orientation"],
                    goal_position=current_observations[self._jetbot.name]["goal_position"]))
        elif current_observations["task_event"] == 1:
            self._jetbot.apply_wheel_actions(ArticulationAction(joint_velocities=[-10, -10]))
        elif current_observations["task_event"] == 2:
            self._jetbot.apply_wheel_actions(ArticulationAction(joint_velocities=[0.0, 0.0]))
            actions = self._franka_controller.forward(
                picking_position=current_observations[self._cube_name]["position"],
                placing_position=current_observations[self._cube_name]["target_position"],
                current_joint_positions=current_observations[self._franka.name]["joint_positions"])
            self._franka.apply_action(actions)
        if self._franka_controller.is_done():
            self._world.pause()
        return
~~~

追加後、Ctrl＋Saveとhot reloadが実行されます。

メニューバーのIsaac Examples > Awesome Exampleを選択し、Loadを選択すると、ソースコードの変更部分が反映された状態で表示されます。
この状態で、Viewportの左側のPLAYボタンを押すと、JetbotがCubeを移動し、FrankaがPickします。

## 2.2 複数タスクの実行

offsetパラメータを追加したクラスのインスタンスを複数生成し、複数のタスクを1つのシーン内で実行します。
hello_world.pyを次の様に編集します。

~~~ hello_world.py:Python3
from omni.isaac.examples.base_sample import BaseSample
from omni.isaac.franka.tasks import PickPlace
from omni.isaac.franka.controllers import PickPlaceController
from omni.isaac.jetbot import Jetbot
from omni.isaac.motion_generation import WheelBasePoseController
from omni.isaac.jetbot.controllers import DifferentialController
from omni.isaac.core.tasks import BaseTask
from omni.isaac.core.utils.types import ArticulationAction
from omni.isaac.core.utils.string import find_unique_string_name
from omni.isaac.core.utils.prims import is_prim_path_valid
import numpy as np


class RobotsPlaying(BaseTask):
    def __init__(self, name, offset=None):
        super().__init__(name=name, offset=offset)
        self._task_event = 0
        self._jetbot_goal_position = np.array([np.random.uniform(120, 160), 30, 0]) + self._offset
        self._pick_place_task = PickPlace(cube_initial_position=np.array([10, 30, 5]),
                                        target_position=np.array([70, -30, 5.15 / 2.0]),
                                        offset=offset)
        return

    def set_up_scene(self, scene):
        super().set_up_scene(scene)
        self._pick_place_task.set_up_scene(scene)
        jetbot_name = find_unique_string_name(
            intitial_name="fancy_jetbot", is_unique_fn=lambda x: not self.scene.object_exists(x)
        )
        jetbot_prim_path = find_unique_string_name(
            intitial_name="/World/Fancy_Jetbot", is_unique_fn=lambda x: not is_prim_path_valid(x)
        )
        self._jetbot = scene.add(Jetbot(prim_path=jetbot_prim_path,
                                        name=jetbot_name,
                                        position=np.array([0, 30, 0])))
        self._task_objects[self._jetbot.name] = self._jetbot
        pick_place_params = self._pick_place_task.get_params()
        self._franka = scene.get_object(pick_place_params["robot_name"]["value"])
        current_position, _ = self._franka.get_world_pose()
        self._franka.set_world_pose(position=current_position + np.array([100, 0, 0]))
        self._franka.set_default_state(position=current_position + np.array([100, 0, 0]))
        self._move_task_objects_to_their_frame()
        return

    def get_observations(self):
        current_jetbot_position, current_jetbot_orientation = self._jetbot.get_world_pose()
        observations= {
            self.name + "_event": self._task_event, #change task event to make it unique
            self._jetbot.name: {
                "position": current_jetbot_position,
                "orientation": current_jetbot_orientation,
                "goal_position": self._jetbot_goal_position
            }
        }
        observations.update(self._pick_place_task.get_observations())
        return observations

    def get_params(self):
        pick_place_params = self._pick_place_task.get_params()
        params_representation = pick_place_params
        params_representation["jetbot_name"] = {"value": self._jetbot.name, "modifiable": False}
        params_representation["franka_name"] = pick_place_params["robot_name"]
        return params_representation


    def pre_step(self, control_index, simulation_time):
        if self._task_event == 0:
            current_jetbot_position, _ = self._jetbot.get_world_pose()
            if np.mean(np.abs(current_jetbot_position[:2] - self._jetbot_goal_position[:2])) < 4:
                self._task_event += 1
                self._cube_arrive_step_index = control_index
        elif self._task_event == 1:
            if control_index - self._cube_arrive_step_index == 200:
                self._task_event += 1
        return

    def post_reset(self):
        self._task_event = 0
        return


class HelloWorld(BaseSample):
    def __init__(self) -> None:
        super().__init__()
        self._tasks = []
        self._num_of_tasks = 3
        self._franka_controllers = []
        self._jetbot_controllers = []
        self._jetbots = []
        self._frankas = []
        self._cube_names = []
        return

    def setup_scene(self):
        world = self.get_world()
        for i in range(self._num_of_tasks):
            world.add_task(RobotsPlaying(name="my_awesome_task_" + str(i), offset=100 * np.array([0, (i * 2) - 3, 0])))
        return

    async def setup_post_load(self):
        self._world = self.get_world()
        for i in range(self._num_of_tasks):
            self._tasks.append(self._world.get_task(name="my_awesome_task_" + str(i)))
            task_params = self._tasks[i].get_params()
            self._frankas.append(self._world.scene.get_object(task_params["franka_name"]["value"]))
            self._jetbots.append(self._world.scene.get_object(task_params["jetbot_name"]["value"]))
            self._cube_names.append(task_params["cube_name"]["value"])
            self._franka_controllers.append(PickPlaceController(name="pick_place_controller",
                                                                gripper_dof_indices=self._frankas[i].gripper.dof_indices,
                                                                robot_prim_path=self._frankas[i].prim_path,
                                                                events_dt=[0.008, 0.002, 0.5, 0.1, 0.05, 0.05, 0.0025, 1, 0.008, 0.08]))
            self._jetbot_controllers.append(WheelBasePoseController(name="cool_controller",
                                                                    open_loop_wheel_controller=
                                                                        DifferentialController(name="open_loop_controller")))
        self._world.add_physics_callback("sim_step", callback_fn=self.physics_step)
        await self._world.play_async()
        return

    async def setup_post_reset(self):
        for i in range(len(self._tasks)):
            self._franka_controllers[i].reset()
            self._jetbot_controllers[i].reset()
        await self._world.play_async()
        return

    def physics_step(self, step_size):
        current_observations = self._world.get_observations()
        for i in range(len(self._tasks)):
            if current_observations[self._tasks[i].name + "_event"] == 0:
                self._jetbots[i].apply_wheel_actions(
                    self._jetbot_controllers[i].forward(
                        start_position=current_observations[self._jetbots[i].name]["position"],
                        start_orientation=current_observations[self._jetbots[i].name]["orientation"],
                        goal_position=current_observations[self._jetbots[i].name]["goal_position"]))
            elif current_observations[self._tasks[i].name + "_event"] == 1:
                self._jetbots[i].apply_wheel_actions(ArticulationAction(joint_velocities=[-10, -10]))
            elif current_observations[self._tasks[i].name + "_event"] == 2:
                self._jetbots[i].apply_wheel_actions(ArticulationAction(joint_velocities=[0.0, 0.0]))
                actions = self._franka_controllers[i].forward(
                    picking_position=current_observations[self._cube_names[i]]["position"],
                    placing_position=current_observations[self._cube_names[i]]["target_position"],
                    current_joint_positions=current_observations[self._frankas[i].name]["joint_positions"])
                self._frankas[i].apply_action(actions)
        return

    def world_cleanup(self):
        self._tasks = []
        self._franka_controllers = []
        self._jetbot_controllers = []
        self._jetbots = []
        self._frankas = []
        self._cube_names = []
        return
~~~

追加後、Ctrl＋Saveとhot reloadが実行されます。

メニューバーのIsaac Examples > Awesome Exampleを選択し、Loadを選択すると、ソースコードの変更部分が反映された状態で表示されます。
この状態で、Viewportの左側のPLAYボタンを押すと、JetbotがCubeを移動し、FrankaがPickするtaskが3つ分実行されます。