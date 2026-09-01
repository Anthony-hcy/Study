![[Pasted image 20260901200358.png]]

## 创建first_robot.urdf

创建功能包
```
ros2 pkg create learning_urdf --build-type ament_cmake --license Apache-2.0
```
![[Pasted image 20260901191632.png]]

新建/home/hcy/ros_ws/src/learning_urdf/urdf/first_robot.urdf
```xml
<?xml version="1.0"?>
<robot name="first_robot">
    <!-- 机器人的身体部分 -->
    <link name="base_link">
        <!-- 部件的外观描述 -->
        <visual>
            <!-- 沿着自己几何中心的偏移和旋转 -->
            <origin xyz="0.0 0.0 0.0" rpy="0.0 0.0 0.0"/>
            <!-- 几何形状 -->
            <geometry>
                <!-- 圆柱体 radius 半径 0.10m  高度 0.12m-->
                <cylinder radius="0.10" length="0.12"/>
            </geometry>
            <!-- 材质颜色 -->
            <material name="white">
                <color rgba="1.0 1.0 1.0 0.5"/>
            </material>
        </visual>
    </link>
    <!-- 机器人的IMU部件，惯性测量传感器 -->
    <link name="imu_link">
        <!-- 部件的外观描述 -->
        <visual>
            <!-- 沿着自己几何中心的偏移和旋转 -->
            <origin xyz="0.0 0.0 0.0" rpy="0.0 0.0 0.0"/>
            <!-- 几何形状 -->
            <geometry>
                <!-- 正方体-->
                <box size="0.02 0.02 0.02"/>
            </geometry>
            <!-- 材质颜色 -->
            <material name="black">
                <color rgba="0.0 0.0 0.0 0.5"/>
            </material>
        </visual>
    </link>
    <!-- 机器人的关节，用于组合机器人的部件 -->
    <joint name="imu_joint" type="fixed">
        <parent link="base_link"/>
        <child link="imu_link"/>
        <origin xyz="0.0 0.0 0.03" rpy="0.0 0.0 0.0"/>
    </joint>
</robot>
```
![[Pasted image 20260901191850.png]]

查看URDF模型结构
```
urdf_to_graphviz first_robot.urdf
```
> [!warning]
> hcy@hcy-pc:~/ros_ws$ urdf_to_graphviz first_robot.urdf
>WARNING: OUTPUT not given. This type of usage is deprecated!Usage: urdf_to_graphviz input.xml [OUTPUT]  Will create either $ROBOT_NAME.gv & $ROBOT_NAME.pdf in CWD  or OUTPUT.gv & OUTPUT.pdf.
>Error:   Error document empty.
>         at line 100 in ./urdf_parser/src/model.cpp
>ERROR: Model Parsing the xml failed
>![[Pasted image 20260901192134.png]]

原因是路径打开错误，应该在.urdf所在路径打开终端
![[Pasted image 20260901192255.png]]
得到first_robot.pdf
![[Pasted image 20260901192419.png]]

## 模型可视化效果
终端`rviz2`启动三维可视化显示平台
点击`Add`，选择`RobotModel`
![[Pasted image 20260901192808.png]]
选择打开刚创建的.urdf
![[Pasted image 20260901193120.png]]
> [!warning]
> 出现报错：![[Pasted image 20260901193335.png]]

修改`Fixed Frame`为`base_link`解决一部分报错
![[Pasted image 20260901193520.png]]

需要采用Launch来启动

### 采用自带Launch文件
安装urdf_tutorial
```
sudo apt install ros-${ROS_DISTRO}-urdf-tutorial
```
![[Pasted image 20260901194049.png]]

终端运行
```
ros2 launch urdf_tutorial display.launch.py model:=/home/hcy/ros_ws/src/learning_urdf/urdf/first_robot.urdf
```
![[Pasted image 20260901194129.png]]

### 采用自建Launch文件
在~/ros_ws/src/learning_urdf/launch/下创建一个display_robot.launch.py
```python

```

编译运行
```
colcon build --packages-select learning_urdf
ros2 launch learning_urdf display_robot.launch.py
```
> [!warning]
> 出现错误
>![[Pasted image 20260901195137.png]]

原因是未配置依赖
在CMakeLists.txt添加
```
install(
  DIRECTORY launch urdf
  DESTINATION share/${PROJECT_NAME}
)
```
![[Pasted image 20260901195258.png]]
再次编译运行
![[Pasted image 20260901195423.png]]
![[Pasted image 20260901195520.png]]
