## 在命令行中配置DDS
启动第一个终端，我们使用`best_effort`创建一个发布者节点，循环发布任意数据
```bash
ros2 topic pub /chatter std_msgs/msg/Int32 "data: 42" --qos-reliability best_effort
```

在另外一个终端中，如果我们使用`reliable`模型订阅同一话题，无法实现数据通信；
修改为同样的`best_effort`，才能实现数据传输
```bash
ros2 topic echo /chatter std_msgs/msg/Int32 --qos-reliability reliable
ros2 topic echo /chatter std_msgs/msg/Int32 --qos-reliability best_effort
```

![[Pasted image 20260825115852.png]]

![[Pasted image 20260825115900.png]]

查看ROS2系统中每一个发布者或者订阅者的QoS策略
```bash
ros2 topic info /chatter --verbose
```

![[Pasted image 20260825120118.png]]

## DDS编程
### 编写代码
在/ros_ws/src/pkg_07_dds/pkg_07_dds下新建dds_01_pub.py
```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import String
from rclpy.qos import QoSProfile, QoSReliabilityPolicy, QoSHistoryPolicy

class Publisher(Node):
    def __init__(self,node_name):
        super().__init__(node_name)
        self.get_logger().info(f"{node_name}, 启动")
        qos_profile = QoSProfile(
            reliability=QoSReliabilityPolicy.RELIABLE,
            history=QoSHistoryPolicy.KEEP_LAST,
            depth=1)
        self.publisher_ = self.create_publisher(String,'dds',qos_profile)
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
    node = Publisher('dds_pub')
    rclpy.spin(node)
    rclpy.shutdown()
```
![[Pasted image 20260825123716.png]]

在/ros_ws/src/pkg_07_dds/pkg_07_dds下新建dds_01_sub.py
```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import String                
from rclpy.qos import QoSProfile, QoSReliabilityPolicy, QoSHistoryPolicy

class Subscriber(Node):
    def __init__(self,node_name):
        super().__init__(node_name)
        self.get_logger().info(f"{node_name}, 启动")
        qos_profile = QoSProfile(
                    reliability=QoSReliabilityPolicy.RELIABLE,
                    history=QoSHistoryPolicy.KEEP_LAST,
                    depth=1)
        self.subscriber_ = self.create_subscription(String,'dds',self.Sub_callback,qos_profile)
        self.get_logger().info("Waiting pub...")

    def Sub_callback(self,msg):
        self.get_logger().info(f"收到：'{msg.data}'")

def main():
    rclpy.init()
    node = Subscriber('dds_sub')
    rclpy.spin(node)
    rclpy.shutdown()
```
![[Pasted image 20260825123804.png]]

### 配置依赖

setup.py
```python
    entry_points={
        'console_scripts': [
            'dds_01_pub = pkg_07_dds.dds_01_pub:main',
            'dds_01_sub = pkg_07_dds.dds_01_sub:main',
        ],
    },
```
![[Pasted image 20260825122730.png]]

package.xml
```xml
  <depend>rclpy</depend>
  <depend>std_msgs</depend>
```
![[Pasted image 20260825122519.png]]

### 编译运行

启动两个终端，分别运行发布者和订阅者节点：
```bash
colcon build
ros2 run pkg_07_dds dds_01_pub
ros2 run pkg_07_dds dds_01_sub
```
![[Pasted image 20260825123636.png]]
