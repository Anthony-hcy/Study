## 小海龟参数示例
`ros2 run turtlesim turtlesim_node`启动海龟界面
![[Pasted image 20260819112858.png]]
`ros2 param list`查看具体都有哪些参数
`ros2 param describe /turtlesim background_r`查看参数的描述
`ros2 param get /turtlesim background_r`查看某个具体参数的值
![[Pasted image 20260819113137.png]]
`ros2 param set /turtlesim background_r 255`修改参数的值
![[Pasted image 20260819113302.png]]

## 创建功能包（parameter）
### 创建功能包并编译

```bash
cd ~/ros_ws/src
ros2 pkg create pkg_06_param --build-type ament_python --license Apache-2.0
cd ~/ros_ws
colcon build
```
![[Pasted image 20260819113702.png]]

### 编写代码

在/ros_ws/src/pkg_06_param/pkg_06_param下新建param_01_node.py
```python
import rclpy
from rclpy.node import Node

class ParamNode(Node):
    def __init__(self):
        super().__init__('param_node')
        self.declare_parameter('my_name','World')
        self.name = self.get_parameter('my_name').value
        self.get_logger().info(f'节点启动，当前名字: {self.name}')
        self.add_on_set_parameters_callback(self.param_callback)
        self.timer = self.create_timer(1.5,self.timer_callback)

    def timer_callback(self):
        self.get_logger().info(f'Hello {self.name}!')

    def param_callback(self,params):
        for param in params:
            if param.name == 'my_name':
                self.name = param.value
                self.get_logger().info(f'名字已更新为: {self.name}')
        return True

def main():
    rclpy.init()
    node = ParamNode()
    rclpy.spin(node)
    rclpy.shutdown()
```

![[Pasted image 20260819121059.png]]

### 配置依赖

setup.py
```python
    entry_points={
        'console_scripts': [
            'param_01_node = pkg_06_param.param_01_node:main',
        ],
    },
```
![[Pasted image 20260819115438.png]]

package.xml
```xml
  <depend>rclpy</depend>
```
![[Pasted image 20260819115646.png]]

### 编译运行

```bash
colcon build
ros2 run pkg_06_param param_01_node
```
![[Pasted image 20260819120040.png]]

新开终端，
`ros2 param list`查看当前节点有哪些参数
`ros2 param get /param_node my_name` 查看当前 'my_name' 的值
`ros2 param set /param_node my_name "ROS2"`修改参数值
![[Pasted image 20260819120954.png]]


```
ros2 param list							            查看参数列表
ros2 param get <node_name> <parameter_name>		    查看参数值
ros2 param set <node_name> <parameter_name> <值> 	运行时修改参数
ros2 param delete <node_name> <parameter_name>	    删除参数
```
