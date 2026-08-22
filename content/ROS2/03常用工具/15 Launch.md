---
title: "Launch"
source: "https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/3.1_Launch/"
author:
published:
created: 2026-08-13
description: "图书资源·古月居"
tags:
  - "clippings"
---
## Launch：多节点启动与配置脚本

<iframe src="https://player.bilibili.com/player.html?aid=597395989&amp;bvid=BV16B4y1Q7jQ&amp;cid=746068233&amp;page=15&autoplay=0" width="800px" height="460px" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>

到目前为止，每当我们运行一个ROS节点，都需要打开一个新的终端运行一个命令。机器人系统中节点很多，每次都这样启动好麻烦呀。有没有一种方式可以一次性启动所有节点呢？答案当然是肯定的，那就是 **Launch启动文件** ，它是ROS系统中多节点启动与配置的一种脚本。

## Launch文件

![image-20220528140958189](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.1_Launch/image-20220528140958189.png)

这是一个完整的Launch文件，乍看上去，好像Python代码呀，没错， **ROS2中的Launch文件就是基于Python描述的** 。

Launch的核心目的是 **启动节点** ，我们在命令行中输入的各种参数，在Launch文件中，通过类似这样的很多代码模版，也可以进行配置，甚至还可以使用Python原有的编程功能，大大丰富了启动过程中的多样化配置。

Launch文件在ROS系统中出现的频次相当之高，它就像粘合剂一样，可以自由组装和配置各个节点，那如何编写或者阅读一个Launch文件呢，我们通过一系列例程带领大家来了解。

## 多节点启动

先来看看如何启动多个节点。

### 运行效果

启动终端，使用ros2中的launch命令来启动第一个launch文件示例：

```js
$ ros2 launch learning_launch simple.launch.py
```

运行成功后，就可以在终端中看到发布者和订阅者两个节点的日志信息啦。

![image-20220530163837942](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.1_Launch/image-20220530163837942.png)

### 文件解析

这两个节点是如何启动的呢？我们来分析下这个launch文件。

learning\_launch/simple.launch.py

```js
from launch import LaunchDescription           # launch文件的描述类
from launch_ros.actions import Node            # 节点启动的描述类

def generate_launch_description():             # 自动生成launch文件的函数
    return LaunchDescription([                 # 返回launch文件的描述信息
        Node(                                  # 配置一个节点的启动
            package='learning_topic',          # 节点所在的功能包
            executable='topic_helloworld_pub', # 节点的可执行文件
        ),
        Node(                                  # 配置一个节点的启动
            package='learning_topic',          # 节点所在的功能包
            executable='topic_helloworld_sub', # 节点的可执行文件名
        ),
    ])
```

## 命令行参数配置

我们使用ros2命令在终端中启动节点时，还可以在命令后配置一些传入程序的参数，使用launch文件一样可以做到。

### 运行效果

比如我们想要运行一个Rviz可视化上位机，并且加载某一个配置文件，使用命令行的话，是这样的：

```js
$ ros2 run rviz2 rviz2 -d <PACKAGE-PATH>/rviz/turtle_rviz.rviz
```

命令后边还得跟一长串配置文件的路径，如果放在launch文件里，启动就优雅很多了：

```js
$ ros2 launch learning_launch rviz.launch.py
```

![image-20220530164738675](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.1_Launch/image-20220530164738675.png)

### 文件解析

命令行后边的参数是如何通过launch传入节点的呢？来看下这个launch文件。

learning\_launch/rviz.launch.py

```js
import os

from ament_index_python.packages import get_package_share_directory # 查询功能包路径的方法

from launch import LaunchDescription    # launch文件的描述类
from launch_ros.actions import Node     # 节点启动的描述类

def generate_launch_description():      # 自动生成launch文件的函数
   rviz_config = os.path.join(          # 找到配置文件的完整路径
      get_package_share_directory('learning_launch'),
      'rviz',
      'turtle_rviz.rviz'
      )

   return LaunchDescription([           # 返回launch文件的描述信息
      Node(                             # 配置一个节点的启动
         package='rviz2',               # 节点所在的功能包
         executable='rviz2',            # 节点的可执行文件名
         name='rviz2',                  # 对节点重新命名
         arguments=['-d', rviz_config]  # 加载命令行参数
      )
   ])
```

## 资源重映射

ROS社区中的资源非常多，当我们使用别人代码的时候，经常会发现通信的话题名称不太符合我们的要求，能否对类似的资源重新命名呢？

为了提高软件的复用性，ROS提供了资源重映射的机制，可以帮助我们解决类似的问题。

### 运行效果

启动一个终端，运行如下例程，很快会看到出现了两个小海龟仿真器界面；再打开一个终端，发布如下话题，让海龟1动起来，海龟2也会一起运动：

```js
$ ros2 launch learning_launch rviz.launch.py
$ ros2 topic pub --rate 1 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```

![image-20220530165536359](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.1_Launch/image-20220530165536359.png)

### 文件解析

为什么两个海龟都会动呢？这里要用到turtlesim功能包里另外一个节点，叫做mimic，它的功能是订阅某一个海龟的Pose位置，通过计算，变换成一个同样运动的速度指令，发布出去。

至于mimic节点订阅或者发布的话题名叫什么呢？我们就可以通过重映射修改成对应任意海龟的名字。

learning\_launch/remapping.launch.py

```js
from launch import LaunchDescription      # launch文件的描述类
from launch_ros.actions import Node       # 节点启动的描述类

def generate_launch_description():        # 自动生成launch文件的函数
    return LaunchDescription([            # 返回launch文件的描述信息
        Node(                             # 配置一个节点的启动
            package='turtlesim',          # 节点所在的功能包
            namespace='turtlesim1',       # 节点所在的命名空间
            executable='turtlesim_node',  # 节点的可执行文件名
            name='sim'                    # 对节点重新命名
        ),
        Node(                             # 配置一个节点的启动
            package='turtlesim',          # 节点所在的功能包
            namespace='turtlesim2',       # 节点所在的命名空间
            executable='turtlesim_node',  # 节点的可执行文件名
            name='sim'                    # 对节点重新命名
        ),
        Node(                             # 配置一个节点的启动
            package='turtlesim',          # 节点所在的功能包
            executable='mimic',           # 节点的可执行文件名
            name='mimic',                 # 对节点重新命名
            remappings=[                  # 资源重映射列表
                ('/input/pose', '/turtlesim1/turtle1/pose'),         # 将/input/pose话题名修改为/turtlesim1/turtle1/pose
                ('/output/cmd_vel', '/turtlesim2/turtle1/cmd_vel'),  # 将/output/cmd_vel话题名修改为/turtlesim2/turtle1/cmd_vel
            ]
        )
    ])
```

## ROS参数设置

ROS系统中的参数，也可以在Launch文件中设置。

### 运行效果

启动一个终端，运行如下命令：

```js
$ ros2 launch learning_launch parameters.launch.py
```

在启动的海龟仿真器中，我们看到背景颜色被改变了，这个颜色参数的设置就是在launch文件中完成的。

![image-20220530170726913](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.1_Launch/image-20220530170726913.png)

### 文件解析

我们看下在launch文件中如何来设置参数的。

learning\_launch/parameters.launch.py

```js
from launch import LaunchDescription                   # launch文件的描述类
from launch.actions import DeclareLaunchArgument       # 声明launch文件内使用的Argument类
from launch.substitutions import LaunchConfiguration, TextSubstitution

from launch_ros.actions import Node                    # 节点启动的描述类

def generate_launch_description():                     # 自动生成launch文件的函数
   background_r_launch_arg = DeclareLaunchArgument(
      'background_r', default_value=TextSubstitution(text='0')     # 创建一个Launch文件内参数（arg）background_r
   )
   background_g_launch_arg = DeclareLaunchArgument(
      'background_g', default_value=TextSubstitution(text='84')    # 创建一个Launch文件内参数（arg）background_g
   )
   background_b_launch_arg = DeclareLaunchArgument(
      'background_b', default_value=TextSubstitution(text='122')   # 创建一个Launch文件内参数（arg）background_b
   )

   return LaunchDescription([                                      # 返回launch文件的描述信息
      background_r_launch_arg,                                     # 调用以上创建的参数（arg）
      background_g_launch_arg,
      background_b_launch_arg,
      Node(                                                        # 配置一个节点的启动
         package='turtlesim',
         executable='turtlesim_node',                              # 节点所在的功能包
         name='sim',                                               # 对节点重新命名
         parameters=[{                                             # ROS参数列表
            'background_r': LaunchConfiguration('background_r'),   # 创建参数background_r
            'background_g': LaunchConfiguration('background_g'),   # 创建参数background_g
            'background_b': LaunchConfiguration('background_b'),   # 创建参数background_b
         }]
      ),
   ])
```

> [!tip]
> launch文件中出现的argument和parameter，虽都译为“参数”，但含义不同： - argument：仅限launch文件内部使用，方便在launch中调用某些数值； - parameter：ROS系统的参数，方便在节点见使用某些数值。


### 加载参数文件

以上例程我们在launch文件中一个一个的设置参数，略显麻烦，当参数比较多的时候，建议使用参数文件进行加载。

learning\_launch/parameters\_yaml.launch.py

```js
import os

from ament_index_python.packages import get_package_share_directory  # 查询功能包路径的方法

from launch import LaunchDescription   # launch文件的描述类
from launch_ros.actions import Node    # 节点启动的描述类

def generate_launch_description():     # 自动生成launch文件的函数
   config = os.path.join(              # 找到参数文件的完整路径
      get_package_share_directory('learning_launch'),
      'config',
      'turtlesim.yaml'
      )

   return LaunchDescription([          # 返回launch文件的描述信息
      Node(                            # 配置一个节点的启动
         package='turtlesim',          # 节点所在的功能包
         executable='turtlesim_node',  # 节点的可执行文件名
         namespace='turtlesim2',       # 节点所在的命名空间
         name='sim',                   # 对节点重新命名
         parameters=[config]           # 加载参数文件
      )
   ])
```

## Launch文件包含

在复杂的机器人系统中，launch文件也会有很多，此时我们可以使用类似编程中的include机制，让launch文件互相包含。

### 文件解析

learning\_launch/namespaces.launch.py

```js
import os

from ament_index_python.packages import get_package_share_directory  # 查询功能包路径的方法

from launch import LaunchDescription                 # launch文件的描述类
from launch.actions import IncludeLaunchDescription  # 节点启动的描述类
from launch.launch_description_sources import PythonLaunchDescriptionSource
from launch.actions import GroupAction               # launch文件中的执行动作
from launch_ros.actions import PushRosNamespace      # ROS命名空间配置

def generate_launch_description():                   # 自动生成launch文件的函数
   parameter_yaml = IncludeLaunchDescription(        # 包含指定路径下的另外一个launch文件
      PythonLaunchDescriptionSource([os.path.join(
         get_package_share_directory('learning_launch'), 'launch'),
         '/parameters_nonamespace.launch.py'])
      )

   parameter_yaml_with_namespace = GroupAction(      # 对指定launch文件中启动的功能加上命名空间
      actions=[
         PushRosNamespace('turtlesim2'),
         parameter_yaml]
      )

   return LaunchDescription([                        # 返回launch文件的描述信息
      parameter_yaml_with_namespace
   ])
```

## 功能包编译配置

```js
...

data_files=[
    ('share/ament_index/resource_index/packages',
        ['resource/' + package_name]),
    ('share/' + package_name, ['package.xml']),
    (os.path.join('share', package_name, 'launch'), glob(os.path.join('launch', '*.launch.py'))),
    (os.path.join('share', package_name, 'config'), glob(os.path.join('config', '*.*'))),
    (os.path.join('share', package_name, 'rviz'), glob(os.path.join('rviz', '*.*'))),
],

...
```

## 参考链接

[https://docs.ros.org/en/humble/Tutorials/Launch/Launch-Main.html](https://docs.ros.org/en/humble/Tutorials/Launch/Launch-Main.html)

[https://docs.ros.org/en/humble/Tutorials/Launch/Using-ROS2-Launch-For-Large-Projects.html](https://docs.ros.org/en/humble/Tutorials/Launch/Using-ROS2-Launch-For-Large-Projects.html)
