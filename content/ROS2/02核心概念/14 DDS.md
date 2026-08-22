---
title: "DDS"
source: "https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/2.10_DDS/"
author:
published:
created: 2026-08-12
description: "图书资源·古月居"
tags:
  - "clippings"
---
## DDS

<iframe src="https://player.bilibili.com/player.html?aid=597395989&amp;bvid=BV16B4y1Q7jQ&amp;cid=746066234&amp;page=14&autoplay=0" width="800px" height="460px" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>

Hello，大家好，欢迎来到《ROS2入门21讲》，我是主讲人古月。

终于讲到ROS2中最为重大的变化—— **DDS** ，我们在前边课程中学习的话题、服务、动作，他们底层通信的具体实现过程，都是靠DDS来完成的，它相当于是 **ROS机器人系统中的神经网络** 。

## 通信模型

DDS的核心是通信，能够实现通信的模型和软件框架非常多，这里我们列出常用的四种模型。

![image-20220528020740057](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528020740057.jpg)

- 第一种， **点对点模型** ，许多客户端连接到一个服务端，每次通信时，通信双方必须建立一条连接。当通信节点增多时，连接数也会增多。而且每个客户端都需要知道服务器的具体地址和所提供的服务，一旦服务器地址发生变化，所有客户端都会受到影响。
- 第二种， **Broker模型** ，针对点对点模型进行了优化，由Broker集中处理所有人的请求，并进一步找到真正能响应该服务的角色。这样客户端就不用关心服务器的具体地址了。不过问题也很明显，Broker作为核心，它的处理速度会影响所有节点的效率，当系统规模增长到一定程度，Broker就会成为整个系统的性能瓶颈。更麻烦是，如果Broker发生异常，可能导致整个系统都无法正常运转。之前的ROS1系统，使用的就是类似这样的架构。
- 第三种， **广播模型** ，所有节点都可以在通道上广播消息，并且节点都可以收到消息。这个模型解决了服务器地址的问题，而且通信双方也不用单独建立连接，但是广播通道上的消息太多了，所有节点都必须关心每条消息，其实很多是和自己没有关系的。
- 第四种，就是 **以数据为中心的DDS模型** 了，这种模型与广播模型有些类似，所有节点都可以在DataBus上发布和订阅消息。但它的先进之处在于，通信中包含了很多并行的通路，每个节点可以只关心自己感兴趣的消息，忽略不感兴趣的消息，有点像是一个旋转火锅，各种好吃的都在这个DataBus传送，我们只需要拿自己想吃的就行，其他的和我们没有关系。

可见，在这几种通信模型中，DDS的优势更加明显。

## DDS

DDS并不是一个新的通信方式，在ROS2之前，DDS已经广泛应用在很多领域，比如航空，国防，交通，医疗，能源等。

![image-20220528020829304](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528020829304.jpg)

比如在自动驾驶领域，通常会存在感知，预测，决策和定位等模块，这些模块都需要非常高速和频繁地交换数据。借助DDS，可以很好地满足它们的通信需求。

### 什么是DDS

好啦，说了半天DDS，到底啥意思呢？我们来做一个完整的介绍

![image-20220528020842533](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528020842533.jpg)

DDS的全称是 **Data Distribution Service** ，也就是 **数据分发服务** ，2004年由 **对象管理组织OMG** 发布和维护，是一套专门为 **实时系统** 设计的 **数据分发/订阅标准** ，最早应用于美国海军， 解决舰船复杂网络环境中大量软件升级的兼容性问题，现在已经成为强制标准。

DDS强调 **以数据为中心** ，可以提供丰富的 **服务质量策略** ，以保障数据进行实时、高效、灵活地分发，可满足各种分布式实时通信应用需求。

这里也提一下对象管理组织OMG，成立于1989年，它的使命是开发技术标准，为数以千计的垂直行业提供真实的价值，比如大家课可能听说过的统一建模语言SYSML和UML，还有中间件标准CORBA等，当然还有DDS。

### DDS在ROS2中的应用

DDS在ROS2系统中的位置至关重要，所有上层建设都建立在DDS之上。在这个ROS2的架构图中，蓝色和红色部分就是DDS。

![image-20220528020937717](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528020937717.jpg)

刚才我们也提到，DDS是一种通信的标准，就像4G、5G一样，既然是标准，那大家都可以按照这个标准来实现对应的功能，所以华为、高通都有很多5G的技术专利，DDS也是一样， **能够按照DDS标准实现的通信系统很多** ，这里每一个红色模块，就是某一企业或组织实现的一种DDS系统。

既然可选用的DDS这么多，那我们该用哪一个呢？具体而言，他们肯定都符合基本标准，但还是会有性能上的差别，ROS2的原则就是尽量兼容，让用户根据使用场景选择，比如个人开发，我们选择一个开源版本的DDS就行，如果是工业应用，那可能得选择一个商业授权的版本了。

为了实现对多个DDS的兼容，ROS设计了一个 **Middleware中间件** ，也就是一个统一的标准，不管我们用那个DDS，保证上层编程使用的函数接口都是一样的。此时兼容性的问题就转移给了DDS厂商，如果他们想让自己的DDS系统进入ROS生态，就得按照ROS的接口标准，开发一个驱动，也就是这个部分。

无论如何，ROS的宗旨不变，要提高软件代码的复用性，下边DDS任你边，上边的软件没影响。

![image-20220528020948738](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528020948738.jpg)

在ROS的四大组成部分中，由于DDS的加入，大大提高了分布式通信系统的综合能力，这样我们在开发机器人的过程中，就不需要纠结通信的问题，可以把更多时间放在其他部分的应用开发上。

### 质量服务策略QoS

DDS为ROS的通信系统提供提供了哪些特性呢？我们通过这个通信模型图来看下。

![image-20220528021108329](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528021108329.jpg)

DDS中的基本结构是 **Domain** ，Domain将各个应用程序绑定在一起进行通信，回忆下之前我们配置树莓派和电脑通信的时候，配置的那个DOMAIN ID，就是对全局数据空间的分组定义，只有处于同一个DOMAIN小组中的节点才能互相通信。这样可以避免无用数据占用的资源。

DDS中另外一个重要特性就是 **质量服务策略，QoS** 。

QoS是一种网络传输策略，应用程序指定所需要的网络传输质量行为，QoS服务实现这种行为要求，尽可能地满足客户对通信质量的需求，可以理解为 **数据提供者和接收者之间的合约** 。

![image-20220528021119863](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528021119863.jpg)

具体会有哪些策略？比如：

- **DEADLINE** 策略，表示通信数据必须要在每次截止时间内完成一次通信；
- **HISTORY** 策略，表示针对历史数据的一个缓存大小；
- **RELIABILITY** 策略，表示数据通信的模式，配置成BEST\_EFFORT，就是尽力传输模式，网络情况不好的时候，也要保证数据流畅，此时可能会导致数据丢失，配置成RELIABLE，就是可信赖模式，可以在通信中尽量保证图像的完整性，我们可以根据应用功能场景选择合适的通信模式；
- **DURABILITY** 策略，可以配置针对晚加入的节点，也保证有一定的历史数据发送过去，可以让新节点快速适应系统。

![image-20220528021133020](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528021133020.jpg)

所有这些策略在ROS系统中都可以通过类似这样的结构体配置，如果不配置的话，系统也会使用默认的参数。

举一个机器人的例子便于大家理解。

![image-20220528021146141](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528021146141.jpg)

比如我们遥控一个无人机航拍，如果网络情况不好的话，遥控器向无人机发送运动指令的过程，可以用reliable通信模式，保证每一个命令都可以顺利发送给无人机，但是可能会有一些延时，无人机传输图像的过程可以用best effort模式，保证视频的流畅性，但是可能会有掉帧。

![image-20220528021158778](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528021158778.jpg)

如果此时出现一个黑客黑入我们的网络，也没有关系，我们可以给ROS2的通信数据进行加密，黑客也没有办法直接控制无人机。

DDS的加入，让ROS2的通信系统焕然一新，多众多样的通信配置，可以更好的满足不同场景下的机器人应用。

好啦，DDS这么好，那该如何配置和使用呢？我们先带大家入个门。

## 案例一：在命令行中配置DDS

我们先来试一试在命令行中配置DDS的参数。

启动第一个终端，我们使用best\_effort创建一个发布者节点，循环发布任意数据，在另外一个终端中，如果我们使用reliable模型订阅同一话题，无法实现数据通信，如果修改为同样的best\_effort，才能实现数据传输。

```js
$ ros2 topic pub /chatter std_msgs/msg/Int32 "data: 42" --qos-reliability best_effort 
$ ros2 topic echo /chatter --qos-reliability reliable
$ ros2 topic echo /chatter --qos-reliability best_effort
```

![image-20220528021233993](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528021233993.jpg)

![image-20220528021243492](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528021243492.jpg)

如何去查看ROS2系统中每一个发布者或者订阅者的QoS策略呢，在topic命令后边跟一个"--verbose"参数就行了。

```js
$ ros2 topic info /chatter --verbose
```

![image-20220528021257958](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528021257958.jpg)

## 案例二：DDS编程示例

接下来，我们尝试在代码中配置DDS，以之前Hello World话题通信为例。

![image-20220528021328306](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528021328306.jpg)

### 运行效果

启动两个终端，分别运行发布者和订阅者节点：

```js
$ ros2 run learning_qos qos_helloworld_pub
$ ros2 run learning_qos qos_helloworld_sub
```

可以看到两个终端中的通信效果如下，和之前貌似并没有太大区别。

![image-20220528021427415](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528021427415.jpg)

![image-20220528021440872](https://book.guyuehome.com/ROS2/2.%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5/image/2.10_DDS/image-20220528021440872.jpg)

看效果确实差不多，不过底层通信机理上可是有所不同的。

### 发布者代码解析

我们看下在代码中，如果加入QoS的配置。

learning\_qos/qos\_helloworld\_pub.py

```js
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
@作者: 古月居(www.guyuehome.com)
@说明: ROS2 QoS示例-发布“Hello World”话题
"""

import rclpy                     # ROS2 Python接口库
from rclpy.node import Node      # ROS2 节点类
from std_msgs.msg import String  # 字符串消息类型
from rclpy.qos import QoSProfile, QoSReliabilityPolicy, QoSHistoryPolicy # ROS2 QoS类

"""
创建一个发布者节点
"""
class PublisherNode(Node):

    def __init__(self, name):
        super().__init__(name)        # ROS2节点父类初始化

        qos_profile = QoSProfile(     # 创建一个QoS原则
            # reliability=QoSReliabilityPolicy.BEST_EFFORT,
            reliability=QoSReliabilityPolicy.RELIABLE,
            history=QoSHistoryPolicy.KEEP_LAST,
            depth=1
        )
        self.pub = self.create_publisher(String, "chatter", qos_profile) # 创建发布者对象（消息类型、话题名、QoS原则）
        self.timer = self.create_timer(0.5, self.timer_callback)         # 创建一个定时器（单位为秒的周期，定时执行的回调函数）

    def timer_callback(self):                                # 创建定时器周期执行的回调函数
        msg = String()                                       # 创建一个String类型的消息对象
        msg.data = 'Hello World'                             # 填充消息对象中的消息数据
        self.pub.publish(msg)                                # 发布话题消息
        self.get_logger().info('Publishing: "%s"' % msg.data)# 输出日志信息，提示已经完成话题发布

def main(args=None):                           # ROS2节点主入口main函数
    rclpy.init(args=args)                      # ROS2 Python接口初始化
    node = PublisherNode("qos_helloworld_pub") # 创建ROS2节点对象并进行初始化
    rclpy.spin(node)                           # 循环等待ROS2退出
    node.destroy_node()                        # 销毁节点对象
    rclpy.shutdown()                           # 关闭ROS2 Python接口
```

完成代码的编写后需要设置功能包的编译选项，让系统知道Python程序的入口，打开功能包的setup.py文件，加入如下入口点的配置：

```js
entry_points={
    'console_scripts': [
     'qos_helloworld_pub  = learning_qos.qos_helloworld_pub:main',
},
```

### 订阅者代码解析

订阅者中的QoS配置和发布者类似。

learning\_qos/qos\_helloworld\_sub.py

```js
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
@作者: 古月居(www.guyuehome.com)
@说明: ROS2 QoS示例-订阅“Hello World”话题消息
"""

import rclpy                                     # ROS2 Python接口库
from rclpy.node   import Node                    # ROS2 节点类
from std_msgs.msg import String                  # ROS2标准定义的String消息
from rclpy.qos import QoSProfile, QoSReliabilityPolicy, QoSHistoryPolicy  # ROS2 QoS类

"""
创建一个订阅者节点
"""
class SubscriberNode(Node):

    def __init__(self, name):
        super().__init__(name)         # ROS2节点父类初始化

        qos_profile = QoSProfile(      # 创建一个QoS原则
            # reliability=QoSReliabilityPolicy.BEST_EFFORT,
            reliability=QoSReliabilityPolicy.RELIABLE,
            history=QoSHistoryPolicy.KEEP_LAST,
            depth=1
        )

        self.sub = self.create_subscription(\
            String, "chatter", self.listener_callback, qos_profile) # 创建订阅者对象（消息类型、话题名、订阅者回调函数、QoS原则）

    def listener_callback(self, msg):                      # 创建回调函数，执行收到话题消息后对数据的处理
        self.get_logger().info('I heard: "%s"' % msg.data) # 输出日志信息，提示订阅收到的话题消息

def main(args=None):                               # ROS2节点主入口main函数
    rclpy.init(args=args)                          # ROS2 Python接口初始化
    node = SubscriberNode("qos_helloworld_sub")    # 创建ROS2节点对象并进行初始化
    rclpy.spin(node)                               # 循环等待ROS2退出
    node.destroy_node()                            # 销毁节点对象
    rclpy.shutdown()                               # 关闭ROS2 Python接口
```

完成代码的编写后需要设置功能包的编译选项，让系统知道Python程序的入口，打开功能包的setup.py文件，加入如下入口点的配置：

```js
entry_points={
    'console_scripts': [
     'qos_helloworld_pub  = learning_qos.qos_helloworld_pub:main',
     'qos_helloworld_sub  = learning_qos.qos_helloworld_sub:main',
    ],
},
```

DDS本身是一个非常复杂的系统，ROS2使用的也只是冰山一角，我们主要带领大家认识DDS，更多使用方法和相关内容，大家也可以参考下边的链接进行学习。

## 参考链接

[https://design.ros2.org/articles/ros\_on\_dds.html](https://design.ros2.org/articles/ros_on_dds.html)

[https://docs.ros.org/en/humble/Concepts/About-Different-Middleware-Vendors.html](https://docs.ros.org/en/humble/Concepts/About-Different-Middleware-Vendors.html)

[https://docs.ros.org/en/humble/How-To-Guides/Working-with-multiple-RMW-implementations.html](https://docs.ros.org/en/humble/How-To-Guides/Working-with-multiple-RMW-implementations.html)

[https://www.bilibili.com/video/BV12z4y167w2](https://www.bilibili.com/video/BV12z4y167w2)
