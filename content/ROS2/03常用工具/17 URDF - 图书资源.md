---
title: "URDF - 图书资源"
source: "https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/3.3_URDF/"
author:
published:
created: 2026-08-13
description: "图书资源·古月居"
tags:
  - "clippings"
---
## URDF：机器人建模方法

<iframe src="https://player.bilibili.com/player.html?aid=597395989&amp;bvid=BV16B4y1Q7jQ&amp;cid=746080385&amp;page=17&autoplay=0" width="800px" height="460px" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>

ROS是机器人操作系统，当然要给机器人使用啦，不过在使用之前，还得让ROS认识下我们使用的机器人，如何把一个机器人介绍给ROS呢？

为此，ROS专门提供了一种机器人建模方法—— **URDF** ，用来描述机器人外观、性能等各方面属性。

## 机器人的组成

建模描述机器人的过程中，我们自己需要先熟悉机器人的组成和参数，比如机器人一般是由 **硬件结构、驱动系统、传感器系统、控制系统** 四大部分组成，市面上一些常见的机器人，无论是移动机器人还是机械臂，我们都可以按照这四大组成部分进行分解。

![image-20220528144350325](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528144350325.png)

- 硬件结构就是底盘、外壳、电机等实打实可以看到的设备；
- 驱动系统就是可以驱使这些设备正常使用的装置，比如电机的驱动器，电源管理系统等；
- 传感系统包括电机上的编码器、板载的IMU、安装的摄像头、雷达等等，便于机器人感知自己的状态和外部的环境；
- 控制系统就是我们开发过程的主要载体了，一般是树莓派、电脑等计算平台，以及里边的操作系统和应用软件。

机器人建模的过程，其实就是按照类似的思路，通过建模语言，把机器人每一个部分都描述清楚，再组合起来的过程。

## URDF

![image-20220528144416270](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528144416270.png)

ROS中的建模方法叫做 **URDF** ，全称是 **统一机器人描述格式** ，不仅可以清晰描述机器人自身的模型，还可以描述机器人的外部环境，比如这里的桌子，也可以算作一个模型。

![image-20220528144424329](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528144424329.png)

URDF模型文件使用的是 **XML格式** ，右侧就是一个机器人的URDF描述，乍看上去，有点像网页开发的源代码，都是由一系列尖括号包围的标签和其中的属性组合而成。

如何使用这样一个文件描述机器人呢？比如这个机械臂，大家可以看下自己的手臂，我们的手臂是由大臂和小臂组成，他们独自是无法运动的，必须通过一个手肘关节连接之后，才能通过肌肉驱动，产生相对运动。

在建模中，大臂和小臂就类似机器人的这些独立的刚体部分，称为 **连杆Link** ，手肘就类似于机器人电机驱动部分，称为 **关节joint** 。

所以在URDF建模过程中，关键任务就是通过这里的\<link>和\<joint>，理清楚每一个连杆和关节的描述信息。

### 连杆Link的描述

\<link>标签用来描述机器人某个刚体部分的 **外观和物理属性** ，外观包括尺寸、颜色、形状，物理属性包括质量、惯性矩阵、碰撞参数等。

![image-20220528144534685](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528144534685.png)

以这个机械臂连杆为例，它的link描述如下：

![image-20220528144549092](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528144549092.png)

link标签中的name表示该连杆的名称，我们可以自定义，未来joint连接link的时候，会使用到这个名称。

link里边的\<visual>部分用来描述机器人的外观，比如：

- \<geometry>表示 **几何形状** ，里边使用\<mesh>调用了一个在三维软件中提前设计好的蓝色外观，就是这个stl文件，看上去和真实机器人是一致的
- \<origin>表示 **坐标系相对初始位置的偏移** ，分别是x、y、z方向上的平移，和roll、pitch、raw旋转，不需要偏移的话，就全为0。

第二个部分\<collision>，描述 **碰撞参数** ，里边的内容似乎和\<visual>一样，也有\<geometry>和\<origin>，看似相同，其实区别还是比较大的。

- \<visual>部分重在描述机器人看上去的状态，也就是视觉效果；
- \<collision>部分则是描述机器人运动过程中的状态，比如机器人与外界如何接触算作碰撞。

在这个机器人模型中，蓝色部分是通过\<visual>来描述的，在实际控制过程中，这样复杂的外观在计算碰撞检测时，要求的算力较高，为了简化计算，我们将碰撞检测用的模型简化为了绿色框的圆柱体，也就是\<collision>里边\<geometry>描述的形状。\<origin>坐标系偏移也是类似，可以描述刚体质心的偏移。

![image-20220528144603646](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528144603646.png)

如果是移动机器人的话，link也可以用来描述小车的车体、轮子等部分。

### 关节Joint描述

机器人模型中的刚体最终要通过关节joint连接之后，才能产生相对运动。

URDF中的关节有六种运动类型。

![image-20220528144655899](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528144655899.png)

1. continuous，描述旋转运动，可以围绕某一个轴无限旋转，比如小车的轮子，就属于这种类型。
2. revolute，也是旋转关节，和continuous类型的区别在于不能无限旋转，而是带有角度限制，比如机械臂的两个连杆，就属于这种运动。
3. prismatic，是滑动关节，可以沿某一个轴平移，也带有位置的极限，一般直线电机就是这种运动方式。
4. fixed，固定关节，是唯一一种不允许运动的关节，不过使用还是比较频繁的，比如相机这个连杆，安装在机器人上，相对位置是不会变化的，此时使用的连接方式就是Fixed。
5. Floating是浮动关节，第六种planar是平面关节，这两种使用相对较少。

![image-20220528144722751](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528144722751.png)

在URDF模型中，每一个link都使用这样一段xml内容描述，比如关节的名字叫什么，运动类型是哪一种。

![image-20220528144729633](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528144729633-16537204524521.png)

- parent标签：描述父连杆；
- child标签：描述子连杆，子连杆会相对父连杆发生运动；
- origin：表示两个连杆坐标系之间的关系，也就是图中红色的向量，可以理解为这两个连杆该如何安装到一起；
- axis表示关节运动轴的单位向量，比如z等于1，就表示这个旋转运动是围绕z轴的正方向进行的；
- limit就表示运动的一些限制了，比如最小位置，最大位置，和最大速度等。
Info

ROS中关于平移的默认单位是m，旋转是弧度（不是度），所以这里的3.14就表示可以在-180度到180度之间运动，线速度是m/s，角速度是rad/s。

### 完整机器人模型

![image-20220528144900705](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528144900705.png)

最终所有的link和joint标签完成了对机器人每个部分的描述和组合，全都放在一个robot标签中，就形成了完整的机器人模型。

![image-20220528144824234](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528144824234.png)

所以大家在看某一个URDF模型时，先不着急看每一块代码的细节，先来找link和joint，看下这个机器人是由哪些部分组成的，了解完全局之后，再看细节。

## 创建机器人模型

好啦，讲了这么多，还是要看一个完整的示例。

![image-20220528145019891](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528145019891.png)

我们以这款移动机器人模型为例，一起看下它的URDF建模过程。

### 功能包结构

机器人的模型放置在learning\_urdf功能包中，功能包中包含的文件夹如下：

![image-20220528145030778](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528145030778.png)

- urdf：存放机器人模型的URDF或xacro文件
- meshes：放置URDF中引用的模型渲染文件
- launch：保存相关启动文件
- rviz：保存rviz的配置文件

### 模型可视化效果

我们先来看下这个模型的效果，尝试逆向分下一下机器人的结构。

```js
$ ros2 launch learning_urdf display.launch.py
```

很快就可以看到Rviz中显示的机器人模型啦，大家可以使用鼠标拖拽观察。

![image-20220530175131389](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220530175131389.png)

从可视化的效果来看，这个机器人由五个link和4个joint组成。

### 查看URDF模型结构

我们分析的对不对呢，可以在模型文件的路径下，使用urdf\_to\_graphviz这个小工具来分析下。

```js
$ urdf_to_graphviz mbot_base.urdf  # 在模型文件夹下运行
```

运行成功后会产生一个pdf文件，打开之后就可以看到URDF模型分析的结果啦，是不是和我们的猜测完全相同呢！

![image-20220528145132111](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.3_URDF/image-20220528145132111.png)

### 模型文件解析

具体URDF模型什么样的，还是要打开模型来研究。

learning\_urdf/urdf/mbot\_base.urdf

```js
<?xml version="1.0" ?>
<robot name="mbot">

    <link name="base_link">
        <visual>
            <origin xyz=" 0 0 0" rpy="0 0 0" />
            <geometry>
                <cylinder length="0.16" radius="0.20"/>
            </geometry>
            <material name="yellow">
                <color rgba="1 0.4 0 1"/>
            </material>
        </visual>
    </link>

    <joint name="left_wheel_joint" type="continuous">
        <origin xyz="0 0.19 -0.05" rpy="0 0 0"/>
        <parent link="base_link"/>
        <child link="left_wheel_link"/>
        <axis xyz="0 1 0"/>
    </joint>

    <link name="left_wheel_link">
        <visual>
            <origin xyz="0 0 0" rpy="1.5707 0 0" />
            <geometry>
                <cylinder radius="0.06" length = "0.025"/>
            </geometry>
            <material name="white">
                <color rgba="1 1 1 0.9"/>
            </material>
        </visual>
    </link>

    <joint name="right_wheel_joint" type="continuous">
        <origin xyz="0 -0.19 -0.05" rpy="0 0 0"/>
        <parent link="base_link"/>
        <child link="right_wheel_link"/>
        <axis xyz="0 1 0"/>
    </joint>

    <link name="right_wheel_link">
        <visual>
            <origin xyz="0 0 0" rpy="1.5707 0 0" />
            <geometry>
                <cylinder radius="0.06" length = "0.025"/>
            </geometry>
            <material name="white">
                <color rgba="1 1 1 0.9"/>
            </material>
        </visual>
    </link>

    <joint name="front_caster_joint" type="continuous">
        <origin xyz="0.18 0 -0.095" rpy="0 0 0"/>
        <parent link="base_link"/>
        <child link="front_caster_link"/>
        <axis xyz="0 1 0"/>
    </joint>

    <link name="front_caster_link">
        <visual>
            <origin xyz="0 0 0" rpy="0 0 0"/>
            <geometry>
                <sphere radius="0.015" />
            </geometry>
            <material name="black">
                <color rgba="0 0 0 0.95"/>
            </material>
        </visual>
    </link>

    <joint name="back_caster_joint" type="continuous">
        <origin xyz="-0.18 0 -0.095" rpy="0 0 0"/>
        <parent link="base_link"/>
        <child link="back_caster_link"/>
        <axis xyz="0 1 0"/>
    </joint>

    <link name="back_caster_link">
        <visual>
            <origin xyz="0 0 0" rpy="0 0 0"/>
            <geometry>
                <sphere radius="0.015" />
            </geometry>
            <material name="black">
                <color rgba="0 0 0 0.95"/>
            </material>
        </visual>
    </link>

</robot>
```

## 参考链接

[https://docs.ros.org/en/humble/Tutorials/URDF/URDF-Main.html](https://docs.ros.org/en/humble/Tutorials/URDF/URDF-Main.html)

[![图片1](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/footer.png)](https://www.guyuehome.com/)