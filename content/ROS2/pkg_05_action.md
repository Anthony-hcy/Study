## 创建动作接口（.action）
### 编写代码

在/ros_ws/src/pkg_04_interface/action下编辑Learningaction.action
```action
int32 duration
---
bool success
---
int32 current_number
```
![[Pasted image 20260818161949.png]]


### 配置依赖

CMakeLists.txt
```python
# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)  
set(msg_files
  "msg/Learningmsg.msg"
)

set(srv_files
  "srv/Learningsrv.srv"
)

set(action_files
  "action/Learningaction.action"
)

rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
  ${srv_files}
  ${action_files} 
)
```
![[Pasted image 20260818162131.png]]

package.xml
```xml
  <buildtool_depend>rosidl_default_generators</buildtool_depend>
  <exec_depend>rosidl_default_runtime</exec_depend>
  <member_of_group>rosidl_interface_packages</member_of_group>
```
![[Pasted image 20260818130150.png]]

### 编译运行

```bash
colcon build
ros2 interface show pkg_04_interface/action/Learningaction
```
![[Pasted image 20260818162340.png]]
可以看到自定义的新接口已经正确生效

---

## 创建动作服务端（action server）
### 创建功能包并编译

```bash
cd ~/ros_ws/src
ros2 pkg create pkg_05_action --build-type ament_python --license Apache-2.0
cd ~/ros_ws
colcon build
```
![[Pasted image 20260818162624.png]]

### 编写代码

在/ros_ws/src/pkg_05_action/pkg_05_action下新建action_01_server.py
```python
import rclpy
from rclpy.node import Node
from pkg_04_interface.action import Learningaction
from rclpy.action import ActionServer
import time

class CountdownServer(Node):
    def __init__(self):
        super().__init__('action_server')
        self._action_server = ActionServer(self,Learningaction,'countdown',self.execute_callback)
        self.get_logger().info(f'动作服务端已启动，等待目标...')

    def execute_callback(self,goal_handle):
        duration = goal_handle.request.duration
        self.get_logger().info(f'收到倒计时目标：{duration} 秒')
        feedback = Learningaction.Feedback()
        result = Learningaction.Result()

        for i in range(duration, 0, -1):
            if goal_handle.is_cancel_requested:
                goal_handle.canceled()
                self.get_logger().warn('动作被取消')
                result.success = False
                return result
            feedback.current_number = i
            goal_handle.publish_feedback(feedback)
            self.get_logger().info(f'反馈：倒计时 {i}')
            time.sleep(1)
        goal_handle.succeed()
        result.success = True
        self.get_logger().info('倒计时完成！')
        return result

def main():
    rclpy.init()
    node = CountdownServer()
    rclpy.spin(node)
    rclpy.shutdown()
```
![[Pasted image 20260818165536.png]]

### 配置依赖

setup.py
```python
    entry_points={
        'console_scripts': [
            'action_01_server = pkg_05_action.action_01_server:main',
        ],
    },
```
![[Pasted image 20260818164004.png]]

package.xml
```xml
  <depend>rclpy</depend>
  <depend>pkg_04_interface</depend>
```
![[Pasted image 20260818164052.png]]

### 编译运行

```bash
colcon build
ros2 run pkg_05_action action_01_server
```
![[Pasted image 20260818164306.png]]

新开终端，
`ros2 node list`           查看当前运行的节点，
`ros2 action list`      查看当前运行的动作，
`ros2 action info <action_name> `  查看动作信息
`ros2 action send_goal <action_name> <action_type> <action_data>`发送服务请求
输入`ros2 action send_goal /countdown pkg_04_interface/action/Learningaction "{duration: 6}" --feedback`发送服务请求
![[Pasted image 20260818164842.png]]

---

## 创建动作客户端（action client）
### 编写代码

在/ros_ws/src/pkg_05_action/pkg_05_action下新建action_01_client.py
```python
import rclpy
from rclpy.node import Node
from rclpy.action import ActionClient
from pkg_04_interface.action import Learningaction

class CountdownClient(Node):
    def __init__(self):
        super().__init__('action_client')
        self.action_client = ActionClient(self, Learningaction, 'countdown')
        self.get_logger().info('动作客户端已初始化')

    def send_goal(self, duration):
        self.action_client.wait_for_server()
        self.get_logger().info(f'发送目标：倒计时 {duration} 秒')

        goal = Learningaction.Goal()
        goal.duration = duration
        send_goal_future = self.action_client.send_goal_async(goal, feedback_callback=self.feedback_callback)
        rclpy.spin_until_future_complete(self, send_goal_future)
        goal_handle = send_goal_future.result()

        if not goal_handle.accepted:
            self.get_logger().info('目标被拒绝')
            return
        self.get_logger().info('目标已被接受，等待执行结束...')
        result_future = goal_handle.get_result_async()
        rclpy.spin_until_future_complete(self, result_future)
        result = result_future.result().result

        if result.success:
            self.get_logger().info('结果：倒计时成功完成！')
        else:
            self.get_logger().warn('结果：倒计时未成功（或被取消）')

        rclpy.shutdown()

    def feedback_callback(self, feedback_msg):
        current = feedback_msg.feedback.current_number
        self.get_logger().info(f'收到反馈：当前倒计时 {current}')

def main():
    rclpy.init()
    node = CountdownClient()
    import sys
    duration = 5
    if len(sys.argv) > 1:
        duration = int(sys.argv[1])
    node.send_goal(duration)
```

![[Pasted image 20260818172309.png]]

### 配置依赖

setup.py
```python
    entry_points={
        'console_scripts': [
            'action_01_server = pkg_05_action.action_01_server:main',
            'action_01_client = pkg_05_action.action_01_client:main',
        ],
    },
```
![[Pasted image 20260818170935.png]]

package.xml
```xml
  <depend>rclpy</depend>
  <depend>pkg_04_interface</depend>
```
![[Pasted image 20260818164052.png]]

### 编译运行

```bash
colcon build
ros2 run pkg_05_action action_01_client
```
新开终端，`ros2 run pkg_05_action action_01_server`启动服务端
![[Pasted image 20260818172110.png]]

