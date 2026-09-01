## 发布静态tf
### 创建功能包并编译

```bash
cd ~/ros_ws/src
ros2 pkg create pkg_09_tf --build-type ament_python --license Apache-2.0
cd ~/ros_ws
colcon build
```
![[Pasted image 20260827133816.png]]

### 编写代码

在/ros_ws/src/pkg_09_tf/pkg_09_tf下新建static_tf_bro.py
```python
import rclpy                                                                
from rclpy.node import Node                                                  
from geometry_msgs.msg import TransformStamped                              
import tf_transformations                                                   
from tf2_ros.static_transform_broadcaster import StaticTransformBroadcaster  

class StaticTFBroadcaster(Node):
    def __init__(self, name):
        super().__init__(name)                                                  
        self.tf_broadcaster = StaticTransformBroadcaster(self)                 
        static_transformStamped = TransformStamped()                           
        static_transformStamped.header.stamp = self.get_clock().now().to_msg()  
        static_transformStamped.header.frame_id = 'world'                      
        static_transformStamped.child_frame_id  = 'house'                       
        static_transformStamped.transform.translation.x = 10.0                 
        static_transformStamped.transform.translation.y = 5.0                    
        static_transformStamped.transform.translation.z = 0.0
        quat = tf_transformations.quaternion_from_euler(0.0, 0.0, 0.0)          
        static_transformStamped.transform.rotation.x = quat[0]                  
        static_transformStamped.transform.rotation.y = quat[1]
        static_transformStamped.transform.rotation.z = quat[2]
        static_transformStamped.transform.rotation.w = quat[3]
        self.tf_broadcaster.sendTransform(static_transformStamped)             

def main(args=None):
    rclpy.init(args=args)                                
    node = StaticTFBroadcaster("static_tf_bro") 
    rclpy.spin(node)                                     
    rclpy.shutdown()
```
![[Pasted image 20260827134323.png]]

### 配置依赖

setup.py
```python
    entry_points={
        'console_scripts': [
            'static_tf_bro = pkg_09_tf.static_tf_bro:main',
        ],
    },
```
![[Pasted image 20260827134450.png]]

### 编译运行
```bash
colcon build
ros2 run pkg_09_tf static_tf_bro
```
输出一下world 到 house的关系
`ros2 run tf2_ros tf2_echo world house`
![[Pasted image 20260827135150.png]]


## 发布动态tf

### 编写代码

在/ros_ws/src/pkg_09_tf/pkg_09_tf下新建dynamic_tf_bro.py
```python
import rclpy                                                                
from rclpy.node import Node                                                  
from geometry_msgs.msg import TransformStamped                              
import tf_transformations                                                   
from tf2_ros import TransformBroadcaster  

class DynamicTFBroadcaster(Node):
    def __init__(self, name):
        super().__init__(name)                                                  
        self.tf_broadcaster = TransformBroadcaster(self)   
        self.timer_ = self.create_timer(0.01, self.publish_dynamic_tf)     

    def publish_dynamic_tf(self):
        dynamic_transformStamped = TransformStamped()                           
        dynamic_transformStamped.header.stamp = self.get_clock().now().to_msg()  
        dynamic_transformStamped.header.frame_id = 'house'                      
        dynamic_transformStamped.child_frame_id  = 'work'                       
        dynamic_transformStamped.transform.translation.x = 20.0                 
        dynamic_transformStamped.transform.translation.y = 10.0                    
        dynamic_transformStamped.transform.translation.z = 0.0
        quat = tf_transformations.quaternion_from_euler(0.0, 0.0, 0.0)          
        dynamic_transformStamped.transform.rotation.x = quat[0]                  
        dynamic_transformStamped.transform.rotation.y = quat[1]
        dynamic_transformStamped.transform.rotation.z = quat[2]
        dynamic_transformStamped.transform.rotation.w = quat[3]
        self.tf_broadcaster.sendTransform(dynamic_transformStamped)    
                 
def main():
    rclpy.init()                                
    node = DynamicTFBroadcaster("dynamic_tf_bro") 
    rclpy.spin(node)                                     
    rclpy.shutdown()
```
![[Pasted image 20260901130806.png]]

### 配置依赖

setup.py
```python
    entry_points={
        'console_scripts': [
            'static_tf_bro = pkg_09_tf.static_tf_bro:main',
            'dynamic_tf_bro = pkg_09_tf.dynamic_tf_bro:main',
        ],
    },
```
![[Pasted image 20260901130117.png]]

### 编译运行
```bash
colcon build
ros2 run pkg_09_tf dynamic_tf_bro
```
输出一下house 到 work的关系
`ros2 run tf2_ros tf2_echo house work`
![[Pasted image 20260901130623.png]]



## 查询TF关系

### 编写代码

在/ros_ws/src/pkg_09_tf/pkg_09_tf下新建tf_listener.py
```python
import rclpy
from rclpy.node import Node
import rclpy.time
from tf2_ros import TransformListener, Buffer # 坐标监听器
import tf_transformations
import math

class TFlistener(Node):
    def __init__(self):
        super().__init__('tf_listener')
        self.buffer_ = Buffer()
        self.listener_ = TransformListener(self.buffer_, self)
        self.timer_ = self.create_timer(1.0, self.get_transform)

    def get_transform(self):
        try:
            result = self.buffer_.lookup_transform('world', 'work', 
                    rclpy.time.Time(seconds=0), rclpy.time.Duration(seconds=1.0))
            transform = result.transform
            self.get_logger().info(f'平移:{transform.translation}')
            self.get_logger().info(f'旋转:{transform.rotation}')
            rotation_euler = tf_transformations.euler_from_quaternion([
                transform.rotation.x,
                transform.rotation.y,
                transform.rotation.z,
                transform.rotation.w]
            )
            self.get_logger().info(f'旋转RPY:{rotation_euler}')
        except Exception as e:
            self.get_logger().warn(f'获取坐标变换失败:原因{str(e)}')

def main():
    rclpy.init()
    node = TFlistener()
    rclpy.spin(node)
    rclpy.shutdown()
```
![[Pasted image 20260901132606.png]]


### 编译运行
```bash
colcon build
ros2 run pkg_09_tf static_tf_bro
ros2 run pkg_09_tf dynamic_tf_bro
ros2 run pkg_09_tf tf_listener
```
![[Pasted image 20260901132423.png]]

输出一下world 到 work的关系
`ros2 run tf2_ros tf2_echo world work`

![[Pasted image 20260901133129.png]]