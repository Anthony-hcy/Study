## 多节点启动
### 创建功能包并编译

```bash
cd ~/ros_ws/src
ros2 pkg create pkg_08_launch --build-type ament_python --license Apache-2.0
cd ~/ros_ws
colcon build
```
![[Pasted image 20260825153001.png]]

### 编写代码

在/ros_ws/src/pkg_08_launch/launch下新建simple.launch.py
```python
from launch import LaunchDescription 
from launch_ros.actions import Node

def generate_launch_description():             
    return LaunchDescription([                 
        Node(                                  
            package='pkg_02_topic',          
            executable='topic_01_pub', 
        ),
        Node(                                  
            package='pkg_02_topic',          
            executable='topic_01_sub', 
        ),
    ])
```
![[Pasted image 20260825154450.png]]

### 配置依赖

setup.py
```python
import os
from glob import glob

data_files=[
        ('share/ament_index/resource_index/packages',
            ['resource/' + package_name]),
        ('share/' + package_name, ['package.xml']),
        (os.path.join('share', package_name, 'launch'), glob(os.path.join('launch', '*.launch.py'))),
    ],
```
![[Pasted image 20260825155019.png]]


### 编译运行
```bash
colcon build
ros2 launch pkg_08_launch simple.launch.py
```
![[Pasted image 20260825154320.png]]


## 传递参数

### 编写代码

在/ros_ws/src/pkg_08_launch/launch下新建param.launch.py
```python
import launch
import launch_ros

def generate_launch_description():
    # 1.声明一个launch参数
    action_declare_arg_background_g = launch.actions.DeclareLaunchArgument\
    ('launch_arg_bg', default_value='150')
    # 2.把launch的参数手动传递给某个节点
    """产生launch描述"""
    action_node_turtlesim_node = launch_ros.actions.Node(
        package='turtlesim',
        executable='turtlesim_node',
        parameters=[{'background_g': launch.substitutions.LaunchConfiguration('launch_arg_bg', default='150')}],      
    )
    action_node_topic_01_pub = launch_ros.actions.Node(
        package='pkg_02_topic',
        executable='topic_01_pub',    
    )
    action_node_topic_01_sub = launch_ros.actions.Node(
        package='pkg_02_topic',
        executable='topic_01_sub',     
    )
    return launch.LaunchDescription([
        # actions 动作
        action_declare_arg_background_g,
        action_node_turtlesim_node,
        action_node_topic_01_pub,
        action_node_topic_01_sub,
    ])
```
![[Pasted image 20260825160000.png]]

### 编译运行
```bash
colcon build
ros2 launch pkg_08_launch param.launch.py launch_arg_bg:=255 
```
![[Pasted image 20260825160219.png]]

