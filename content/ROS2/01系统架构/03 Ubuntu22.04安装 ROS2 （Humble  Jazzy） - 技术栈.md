---
title: "Ubuntu 22.04/24.04 安装 ROS2 完整教程（Humble / Jazzy） - 技术栈"
source: "https://jishuzhan.net/article/2037153277050159106"
author:
published:
created: 2026-08-05
description: "最后更新：2026年2月 适用系统：Ubuntu 22.04 LTS（Jammy） / Ubuntu 24.04 LTS（Noble） 适用架构：x86_64 / arm64（包括树莓派）"
tags:
  - "clippings"
---
最后更新：2026年2月

适用系统：Ubuntu 22.04 LTS（Jammy） / Ubuntu 24.04 LTS（Noble）

适用架构：x86\_64 / arm64（包括树莓派）

---

### 📌 一、版本选择原则（必读）

| 你的Ubuntu版本 | 必须安装的ROS2版本 | 支持截止 | 推荐度 |
| --- | --- | --- | --- |
| 22.04 LTS | ROS2 Humble Hawksbill | 2027年5月 | ⭐⭐⭐ 稳定成熟 |
| 24.04 LTS | ROS2 Jazzy Jalisco | 2029年5月 | ⭐⭐⭐⭐ 新项目首选 |

❗ 核心原则：Ubuntu版本与ROS2版本严格绑定，22.04不能装Jazzy，24.04不能装Humble。

❗ 架构说明：x86\_64（普通PC）和arm64（树莓派4B/5、NVIDIA Jetson）均支持上述版本。

---

#### 🚀 方案一：一键脚本安装（5分钟极速版）

推荐人群：新手、希望零坑位、网络环境一般、需要快速验证者

工具来源：鱼香ROS（国内开源社区维护，已稳定运行5年+）

##### 1️⃣ 第一步：确认系统版本

复制代码

```
lsb_release -a
```

预期输出：包含 Ubuntu 22.04.5 LTS 或 Ubuntu 24.04.2 LTS

##### 2️⃣ 第二步：执行一键安装脚本

复制代码

```
wget http://fishros.com/install -O fishros && . fishros
```

##### 3️⃣ 第三步：交互式配置（严格按顺序）

⚠️ 第一次运行脚本：

- 输入数字：5 → 选择【一键配置系统源】
- 输入数字：2 → 选择【更换系统源并清理第三方源】（推荐中科大）
- 脚本自动执行，等待完成

⚠️ 第二次运行脚本（重新执行上述命令）：

复制代码

```
wget http://fishros.com/install -O fishros && . fishros
```
- 输入数字：1 → 选择【添加ROS/ROS2官方源】
- 脚本自动执行，等待完成

⚠️ 第三次运行脚本（再次执行）：

复制代码

```
wget http://fishros.com/install -O fishros && . fishros
```
- 输入数字：1 → 选择【不更换源安装】（因前两步已完成换源）
- 选择镜像源：推荐输入 1（中科大）
- 选择ROS2版本：
	- Ubuntu 22.04 → 输入 humble
		- Ubuntu 24.04 → 输入 jazzy
- 选择安装类型：输入 desktop（桌面完整版，包含RViz、Turtlesim等）

⏳ 等待约3-8分钟（取决于网速），无报错即安装完成。

##### 4️⃣ 第四步：验证安装（小乌龟测试）

打开终端A（Ctrl+Alt+T）：

复制代码

```
ros2 run turtlesim turtlesim_node
```

打开终端B：

复制代码

```
ros2 run turtlesim turtle_teleop_key
```

预期效果：出现蓝色窗口和小乌龟，键盘↑↓←→可控制移动 → ✅ 安装成功

![](https://i-blog.csdnimg.cn/direct/ee6807f8a3b94fdab22415155b3088de.png)

---

#### 🔧 方案二：官方标准安装（完全可控）

推荐人群：开发者、需要纯净环境、网络稳定（海外/代理）、后期需深度定制者

---

##### 第1步：设置语言环境（UTF-8）

复制代码

```
sudo apt update
sudo apt install locales -y
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
locale
```

确认输出：LANG=en\_US.UTF-8 等环境变量正确

---

##### 第2步：启用Ubuntu Universe仓库

复制代码

```
sudo apt install software-properties-common -y
sudo add-apt-repository universe -y
```

---

##### 第3步：添加ROS2 GPG密钥及软件源

###### 3.1 安装curl并下载密钥

复制代码

```
sudo apt update
sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

###### 3.2 添加ROS2 apt仓库（架构自动识别）

复制代码

```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

---

##### 第4步：安装ROS2

###### 4.1 更新软件包索引

复制代码

```
sudo apt update
sudo apt upgrade -y
```

###### 4.2 根据Ubuntu版本执行对应命令

🔹 如果你的是 Ubuntu 22.04：

复制代码

```
sudo apt install ros-humble-desktop -y
```

可选精简版：sudo apt install ros-humble-ros-base -y（无GUI工具）

🔹 如果你的是 Ubuntu 24.04：

复制代码

```
sudo apt install ros-jazzy-desktop -y
```

可选精简版：sudo apt install ros-jazzy-ros-base -y（无GUI工具）

⏳ 安装大小：约1.5GB~2.5GB，耗时5-15分钟。

---

##### 第5步：安装开发工具与依赖管理

###### 5.1 安装ROS开发工具包

复制代码

```
sudo apt install ros-dev-tools -y
```

###### 5.2 安装colcon构建工具（编译工作空间必备）

复制代码

```
sudo apt install python3-colcon-common-extensions -y
```

###### 5.3 安装并初始化rosdep（依赖管理工具，可选但推荐）

复制代码

```
sudo apt install python3-rosdep -y
sudo rosdep init
rosdep update
```

⚠️ 常见报错：若sudo rosdep init提示"已存在"，则跳过；若提示网络错误，可尝试sudo rm /etc/ros/rosdep/sources.list.d/20-default.list后重试。

---

##### 第6步：配置环境变量（永久生效）

将ROS2环境加载脚本写入~/.bashrc，避免每次打开终端都要手动source。

🔹 Ubuntu 22.04 + Humble：

复制代码

```
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

🔹 Ubuntu 24.04 + Jazzy：

复制代码

```
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

验证环境：

复制代码

```
echo $ROS_DISTRO
```

预期输出：humble 或 jazzy

---

##### 第7步：验证安装（Talker-Listener测试）

打开终端A：

复制代码

```
ros2 run demo_nodes_cpp talker
```

预期输出： $INFO$ $xxx$ $talker$: Publishing: 'Hello World: 1' 持续滚动

打开终端B：

复制代码

```
ros2 run demo_nodes_py listener
```

预期输出： $INFO$ $xxx$ $listener$: I heard: $Hello World: 1$ 持续滚动

✅ 看到以上内容，恭喜！ROS2已完整、正确地安装在你的系统中。

![](https://i-blog.csdnimg.cn/direct/68e3e5c666c2470a8601d90c2fef2aa2.png)

---

### 🆘 三、常见问题急救包（FAQ）

| 问题现象 | 根本原因 | 解决方案 |
| --- | --- | --- |
| E: Unable to locate package ros-humble-desktop | ROS2 apt源未添加成功 | 重做第3步，或换清华源/中科大源 |
| rosdep: command not found | 未安装python3-rosdep | sudo apt install python3-rosdep -y |
| 每次打开终端都要手动source | 未写入.bashrc | 执行第6步 |
| 安装卡在99% / 依赖冲突 | 网络波动/源不稳定 | Ctrl+C中断，重新sudo apt install |
| ros2: command not found | 环境未加载 | 先执行source /opt/ros/{humble/jazzy}/setup.bash |
| 虚拟机无法运行turtlesim | OpenGL渲染问题 | 虚拟机设置启用3D加速，或安装export LIBGL\_ALWAYS\_SOFTWARE=1 |
| 树莓派编译内存不足 | 源码编译时RAM不够 | 使用二进制安装（本教程均为二进制），或增加swap |

---

### 📁 四、进阶：国内镜像源加速（可选）

如果你在执行sudo apt update时速度极慢，或方案一换源失败，可手动更换apt源为国内镜像。

Ubuntu 22.04（中科大源）：

复制代码

```
sudo sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list
sudo apt update
```

Ubuntu 24.04（中科大源）：

复制代码

```
sudo sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list
sudo apt update
```

ROS2软件源换国内源（清华）：

复制代码

```
sudo sed -i 's@packages.ros.org@mirrors.tuna.tsinghua.edu.cn/ros2@g' /etc/apt/sources.list.d/ros2.list
sudo apt update
```

---

### 📝 五、备注

本教程基于以下官方文档及社区实践编写：

- ROS2官方安装指南： [https://docs.ros.org](https://docs.ros.org/)
- 鱼香ROS一键安装： [http://fishros.com](http://fishros.com/)
- Ubuntu官方树莓派镜像： [https://ubuntu.com/download/raspberry-pi](https://ubuntu.com/download/raspberry-pi)