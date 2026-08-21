# 创建发布者（Publisher）
## 创建功能包并编译

```bash
cd ~/ros_ws/src
ros2 pkg create pkg_02_topic --build-type ament_python --license Apache-2.0
cd ~/ros_ws
colcon build
```

![[Pasted image 20260817112852.png]]

%% ## 生效环境变量

```bash
echo "source ~/ros_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
 %%

## 编写代码

在/ros_ws/src/pkg_02_topic/pkg_02_topic下新建topic_01_pub.py
```python
import rclpy
from rclpy.node import Node
from example_interfaces.msg import String

class Publisher(Node):
    def __init__(self,node_name):
        super().__init__(node_name)
        self.get_logger().info(f"{node_name}, 启动")
        self.publisher_ = self.create_publisher(String,'topic',10)
        self.count_ = 0
        self.timer_ = self.create_timer(0.5,self.timer_callback)

    def timer_callback(self):
        msg = String()
        msg.data = f"Hell, ROS 2! {self.count_}"
        self.count_ += 1
        self.publisher_.publish(msg)
        self.get_logger().info(f'Publishing: "{msg.data}"')

def main():
    rclpy.init()
    node = Publisher('topic_pub')
    rclpy.spin(node)
    rclpy.shutdown()
```

![[Pasted image 20260817144930.png]]

## 配置依赖

setup.py
```python
    entry_points={
        'console_scripts': [
            'topic_01_pub = pkg_02_topic.topic_01_pub:main',
        ],
    },
```
![[Pasted image 20260817122936.png]]

package.xml
```xml
  <depend>rclpy</depend>
  <depend>example_interfaces</depend>
```
![[Pasted image 20260817123114.png]]

## 编译运行

```bash
colcon build
ros2 run pkg_02_topic topic_01_pub
```
![[Pasted image 20260817123251.png]]

新开终端，`ros2 topic list`即可查看当前运行的话题，`ros2 topic echo /topic`即可查看话题的内容
![[Pasted image 20260817123634.png]]

---

# 创建订阅者（Subscriber）
%% ## 创建功能包并编译

```bash
cd ~/ros_ws/src
ros2 pkg create pkg_02_topic --build-type ament_python --license Apache-2.0
cd ~/ros_ws
colcon build
```

 ## 生效环境变量

```bash
echo "source ~/ros_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
  %%

## 编写代码

在/ros_ws/src/pkg_02_topic/pkg_02_topic下新建topic_01_sub.py
```python
import rclpy
from rclpy.node import Node
from example_interfaces.msg import String

class Subscriber(Node):
    def __init__(self,node_name):
        super().__init__(node_name)
        self.get_logger().info(f"{node_name}, 启动")
        self.subscriber_ = self.create_subscription(String,'topic',self.Sub_callback,100)
        self.get_logger().info("Waiting pub...")

    def Sub_callback(self,msg):
        self.get_logger().info(f"收到：'{msg.data}'")

def main():
    rclpy.init()
    node = Subscriber('topic_sub')
    rclpy.spin(node)
    rclpy.shutdown()
```

![[Pasted image 20260817145109.png]]

## 配置依赖

setup.py
```python
    entry_points={
        'console_scripts': [
            'topic_01_pub = pkg_02_topic.topic_01_pub:main',
            'topic_01_sub = pkg_02_topic.topic_01_sub:main',
        ],
    },
```
![[Pasted image 20260817145152.png]]

package.xml
```xml
  <depend>rclpy</depend>
  <depend>example_interfaces</depend>
```
![[Pasted image 20260817123114.png]]

## 编译运行

```bash
colcon build
ros2 run pkg_02_topic topic_01_sub
```

当Publisher未启动时，Subscriber处于等待中
![[Pasted image 20260817145358.png]]

新开终端，`ros2 run pkg_02_topic topic_01_pub`发布话题
![[Pasted image 20260817145645.png]]

同样，`ros2 topic list`即可查看当前运行的话题，
`ros2 topic echo <topic_name>`即可查看话题的数据
`ros2 topic info <topic_name>`查看话题信息
`ros2 topic pub <topic_name> <msg_type> <msg_data>`发布话题信息




