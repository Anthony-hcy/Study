## 创建工作空间

```bash
mkdir -p ~/ros_ws/src
```
![[Pasted image 20260816124446.png]]

## 创建功能包并编译

```bash
cd ~/ros_ws/src
ros2 pkg create pkg_01_node --build-type ament_python --license Apache-2.0
cd ~/ros_ws
colcon build
```

![[Pasted image 20260816124821.png]]
![[Pasted image 20260816125028.png]]


## 生效环境变量

```bash
echo "source ~/ros_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

![[Pasted image 20260816125151.png]]

## 编写代码

在/ros_ws/src/pkg_01_node/pkg_01_node下新建node_01.py
```python
import rclpy
from rclpy.node import Node

def main():
    rclpy.init()
    node = Node('node_01')
    node.get_logger().info('Hello Ros2 !')
    node.get_logger().warn('Hi Ros2 !')
    rclpy.spin(node)
    rclpy.shutdown()
```

![[Pasted image 20260816125831.png]]

## 配置依赖

setup.py
```python
entry_points={
        'console_scripts': [
            'node_01 = pkg_01_node.node_01:main'
        ],
    },
```
![[Pasted image 20260816125427.png]]

package.xml
```xml
<depend>rclpy</depend>
```
![[Pasted image 20260816125529.png]]

## 编译运行

```bash
colcon build
ros2 run pkg_01_node node_01
```
![[Pasted image 20260816125931.png]]
新开终端，`ros2 node list`即可查看当前运行的节点
![[Pasted image 20260816130303.png]]

