# 创建话题接口（.msg）
## 创建功能包

```bash
cd ~/ros_ws/src
ros2 pkg create pkg_04_interface --build-type ament_cmake --license Apache-2.0
cd pkg_04_interface
rm -rf include src
mkdir msg
cd msg
touch Learningmsg.msg
```

![[Pasted image 20260818124445.png]]

## 编写代码

在/ros_ws/src/pkg_04_interface/msg下编辑Learningmsg.msg
```msg
int32 counter
string message
```

![[Pasted image 20260818125422.png]]

## 配置依赖

CMakeLists.txt
```python
find_package(rosidl_default_generators REQUIRED)  
set(msg_files
  "msg/Learningmsg.msg"
)

rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
)
```
![[Pasted image 20260818130036.png]]

package.xml
```xml
  <buildtool_depend>rosidl_default_generators</buildtool_depend>
  <exec_depend>rosidl_default_runtime</exec_depend>
  <member_of_group>rosidl_interface_packages</member_of_group>
```
![[Pasted image 20260818130150.png]]

## 编译运行

```bash
colcon build
ros2 interface show pkg_04_interface/msg/Learningmsg
```
![[Pasted image 20260818130928.png]]
可以看到自定义的新接口已经正确生效

---

# 创建服务接口（.srv）

## 编写代码

在/ros_ws/src/pkg_04_interface/srv下编辑Learningsrv.srv
```srv
int64 a
int64 b
---
int64 sum
```
![[Pasted image 20260818131602.png]]


## 配置依赖

CMakeLists.txt
```python
find_package(ament_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)  
set(msg_files
  "msg/Learningmsg.msg"
)

set(srv_files
  "srv/Learningsrv.srv"
)

rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
  ${srv_files}
)
```
![[Pasted image 20260818131945.png]]

package.xml
```xml
  <buildtool_depend>rosidl_default_generators</buildtool_depend>
  <exec_depend>rosidl_default_runtime</exec_depend>
  <member_of_group>rosidl_interface_packages</member_of_group>
```
![[Pasted image 20260818130150.png]]


## 编译运行

```bash
colcon build
ros2 interface show pkg_04_interface/srv/Learningsrv
```
![[Pasted image 20260818132132.png]]
可以看到自定义的新接口已经正确生效

> [!attention]
>  ROS 2 中，主题名称和消息类型必须同时匹配才能进行通信。
> 主题名称：'topic'这种
> 消息类型：pkg_04_interface/msg/Learningmsg和example_interfaces/msg/String这种


