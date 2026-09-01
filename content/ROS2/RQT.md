安装插件
```
sudo apt install ros-$ROS_DISTRO-rqt-tf-tree -y
```
启动模块化可视化工具
```
rqt
```
![[Pasted image 20260901135640.png]]

发布静态TF
```
ros2 run pkg_09_tf static_tf_bro
```
![[Pasted image 20260901135929.png]]

继续发布动态TF
```
ros2 run pkg_09_tf dynamic_tf_bro
```
![[Pasted image 20260901140100.png]]

