# 创建服务端（Server）
## 创建功能包并编译

```bash
cd ~/ros_ws/src
ros2 pkg create pkg_03_service --build-type ament_python --license Apache-2.0
cd ~/ros_ws
colcon build
```

![[Pasted image 20260817180701.png]]

%% ## 生效环境变量

```bash
echo "source ~/ros_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
 %%

## 编写代码

在/ros_ws/src/pkg_03_service/pkg_03_service下新建service_01_srv.py
```python
import rclpy
from rclpy.node import Node
from example_interfaces.srv import AddTwoInts

class Server(Node):
    def __init__(self,node_name):
        super().__init__(node_name)
        self.srv = self.create_service(AddTwoInts,'add_two_ints',self.callback)
        self.get_logger().info(f'{node_name}已启动，等待请求...')

    def callback(self,request,response):
        response.sum = request.a + request.b
        self.get_logger().info(f'收到：{request.a} + {request.b} = {response.sum}')
        return response

def main():
    rclpy.init()
    node = Server('service_server')
    rclpy.spin(node)
    rclpy.shutdown()
```

![[Pasted image 20260817182524.png]]

## 配置依赖

setup.py
```python
    entry_points={
        'console_scripts': [
            'service_01_srv=pkg_03_service.service_01_srv:main',
        ],
    },
```
![[Pasted image 20260817181750.png]]

package.xml
```xml
  <depend>rclpy</depend>
  <depend>example_interfaces</depend>
```
![[Pasted image 20260817181842.png]]

## 编译运行

```bash
colcon build
ros2 run pkg_03_service service_01_srv
```
![[Pasted image 20260817182459.png]]

新开终端，
`ros2 node list`           查看当前运行的节点，
`ros2 service list`      查看当前运行的服务，
`ros2 service type <service_name> `  查看服务类型
`ros2 service call <service_name> <service_type> <service_data>`发送服务请求
输入`ros2 service call /add_two_ints example_interfaces/srv/AddTwoInts "{a: 10,b: 20}"`发送服务请求
![[Pasted image 20260817183102.png]]

---

# 创建客户端（Client）
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

在/ros_ws/src/pkg_03_service/pkg_03_service下新建service_01_cli.py
```python
import rclpy
from rclpy.node import Node
from example_interfaces.srv import AddTwoInts
import sys

class Client(Node):
    def __init__(self,node_name):
        super().__init__(node_name)
        self.cli = self.create_client(AddTwoInts,'add_two_ints')
        while not self.cli.wait_for_service(timeout_sec=1):
            self.get_logger().info('等待服务启动...')
        self.req = AddTwoInts.Request()

    def call_service(self,a,b):
        self.req.a = a
        self.req.b = b
        self.future = self.cli.call_async(self.req) 
        #rclpy.spin_until_future_complete(self, self.future)   
        #return self.future.result()                           
        while rclpy.ok():
            rclpy.spin_once(self)          
            if self.future.done():         
                if self.future.result() is not None:
                    return self.future.result()
                else:
                    return None

def main():
    rclpy.init()
    node = Client('service_client')
    a = int(sys.argv[1])
    b = int(sys.argv[2])
    response = node.call_service(a, b)
    node.get_logger().info(f'{a} + {b} = {response.sum}')
    rclpy.shutdown()
```

![[Pasted image 20260817192344.png]]

## 配置依赖

setup.py
```python
    entry_points={
        'console_scripts': [
            'service_01_srv=pkg_03_service.service_01_srv:main',
            'service_01_cli=pkg_03_service.service_01_cli:main',
        ],
    },
```
![[Pasted image 20260817185100.png]]

package.xml
```xml
  <depend>rclpy</depend>
  <depend>example_interfaces</depend>
```
![[Pasted image 20260817181842.png]]

## 编译运行

```bash
colcon build
ros2 run pkg_03_service service_01_cli 3 6
```
![[Pasted image 20260817185443.png]]
当服务端未启动时，客户端处于等待中
新开终端，`ros2 run pkg_03_service service_01_srv`启动服务端
![[Pasted image 20260817191804.png]]


