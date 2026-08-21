---
title: "Python PyCharm 安装使用 | 菜鸟教程"
source: "https://www.runoob.com/python3/python-pycharm-usage.html"
author:
published:
created: 2026-08-05
description: "Python PyCharm 安装使用  PyCharm 是 JetBrains 出品的 Python 集成开发环境（IDE），拥有智能代码补全和图形化调试功能，支持 Windows /macOS/ Linux 全平台。。  PyCharm 是一套写代码、跑程序、排错、管理项目、协作一体化工具，是目前行业最主流的 Python 开发工具。 本章节我们将介绍 Python 和 PyCharm 的安装，并创建运行你的第一个 Python 程.."
tags:
  - "clippings"
---
## Python3.xPython PyCharm 安装使用

PyCharm 是 JetBrains 出品的 Python 集成开发环境（IDE），拥有智能代码补全和图形化调试功能，支持 Windows /macOS/ Linux 全平台。。

PyCharm 是一套写代码、跑程序、排错、管理项目、协作一体化工具，是目前行业最主流的 Python 开发工具。

本章节我们将介绍 Python 和 PyCharm 的安装，并创建运行你的第一个 Python 程序。

| 阶段 | 内容 |
| --- | --- |
| 1、安装 Python | 下载、安装、验证 Python 解释器 |
| 2、安装 PyCharm | 下载并安装 PyCharm Community 版 |
| 3、创建项目 | 新建项目并创建 Python 文件 |
| 4、编写运行代码 | 编写 hello.py 并运行 |

---

## 安装 Python

Python 是运行 Python 程序所必需的解释器，PyCharm 依赖它来执行代码。

安装 Python 分为三步：下载安装包、执行安装、验证安装结果。

### 下载 Python

打开浏览器，访问 Python 官方网站的下载页面： [https://www.python.org/downloads/](https://www.python.org/downloads/) 。

网站通常会自动检测你的操作系统（Windows / macOS / Linux），点击页面上的黄色按钮即可下载最新版本。

![](https://www.runoob.com/wp-content/uploads/2026/07/runoob_1784086091695.png)

> 建议选择 Python 3.10 及以上版本，目前主流教程和第三方库对新版本支持较好，Python 2.x 已于 2020 年停止维护，不要再使用。

### 安装 Python

以下以 Windows 系统为例，macOS 和 Linux 用户参见对应说明。

**Windows 安装步骤：**

| 步骤 | 操作 | 说明 |
| --- | --- | --- |
| 1 | 双击安装包 | 运行下载好的.exe 文件，例如 `python-3.12.x-amd64.exe` |
| 2 | 勾选 PATH 选项 | 关键一步：在安装界面底部勾选 **Add python.exe to PATH** ，否则命令行可能找不到 Python  ![](https://www.runoob.com/wp-content/uploads/2026/07/runoob_1784086295025.png) |
| 3 | 点击 Install Now | 等待安装进度条完成即可 |

**macOS 用户：**

可以直接双击.pkg 安装包按提示完成安装，也可以通过 Homebrew 包管理器安装：

```
brew install python3
```

### 验证安装

打开命令行工具（Windows 下使用命令提示符或 PowerShell，macOS 下使用终端），输入以下命令：

```
python --version
```

如果显示类似 Python 3.12.0 的版本号，说明安装成功。

> 部分系统（尤其是 macOS/Linux）中系统自带的 python 命令指向 Python 2，此时需要输入 `python3 --version` 来检查新安装的 Python 3。

---

## 安装 PyCharm

PyCharm 由 JetBrains 公司开发，是目前最流行的 Python IDE 之一。

它分为两个版本，初学者选择免费的社区版即可：

| 版本 | 费用 | 适用人群 | 主要功能 |
| --- | --- | --- | --- |
| Professional（专业版） | 付费 | 专业开发者 | 支持 Web 开发、数据库工具、科学计算等全套功能 |
| Community（社区版） | 免费 | 初学者 / 学生 | 纯 Python 开发的核心功能，对初学者完全够用 |

### 下载 PyCharm

访问 JetBrains 官网的 PyCharm 下载页面： [https://www.jetbrains.com/pycharm/download/](https://www.jetbrains.com/pycharm/download/) 。

在页面中找到 Download 按钮，点击对应操作系统的下载按钮。

![](https://www.runoob.com/wp-content/uploads/2026/07/runoob_1784096161404.png)

### 安装 PyCharm

不同操作系统的安装方式如下：

| 操作系统 | 安装方式 |
| --- | --- |
| Windows | 双击.exe 安装包，一路点击「下一步」即可。建议勾选「创建桌面快捷方式」 |
| macOS | 双击.dmg 文件，将 PyCharm 图标拖入 Applications 文件夹 |
| Linux | 可解压.tar.gz 后运行 `bin/pycharm.sh` ，或通过 Snap 安装 |

**Linux Snap 安装命令：**

```
sudo snap install pycharm-community --classic
```

安装完成后打开 PyCharm，首次启动会引导你选择 UI 主题和快捷键方案，保持默认设置或按个人喜好选择即可。

### 安装 PyCharm

#### Windows 系统

1. 双击下载的安装文件（`.exe` ）。
2. 在安装向导中，选择安装路径，建议使用默认路径。
3. 勾选 创建桌面快捷方式（Creat Desktop Shortcut）以便快速启动 PyCharm。
4. 点击 安装（Install） 开始安装。
5. 安装完成后，点击 完成（Finish） 退出安装向导。

![](https://www.runoob.com/wp-content/uploads/2025/04/1_1E4ttYj8NfVAKwOxMrVFaQ.png)

### macOS 系统

1. 打开下载的 `.dmg` 文件。
2. 将 PyCharm 图标拖拽到 "Applications" 文件夹中。
	![](https://www.runoob.com/wp-content/uploads/2025/04/b40352d2-3083-45e3-9a90-a489770c9c6d.png)
3. 在 "Applications" 文件夹中找到 PyCharm，双击启动。
	![](https://www.runoob.com/wp-content/uploads/2025/04/1c1657b8-6eb2-4abc-a765-2208b0f0d22e.png)

### Linux 系统

1. 解压下载的 `.tar.gz` 文件。
	```
	tar -xzf pycharm-*.tar.gz
	```
2. 进入解压后的目录，找到 `bin` 文件夹。
	```
	cd pycharm-*/bin
	```
3. 运行 `pycharm.sh` 启动 PyCharm。
	```
	./pycharm.sh
	```

---

## 配置 PyCharm

首次启动 PyCharm 时，会提示你进行一些初始配置，我们可以设置语言并同意协议：

![](https://www.runoob.com/wp-content/uploads/2025/04/c1e028a0-2b6d-47f7-b7eb-b472e55b9be8.png)

![](https://www.runoob.com/wp-content/uploads/2025/04/768e08bb-eddb-4281-8420-5f525aa712d8.png)

启动后，可以进行个人喜好的配置：

![](https://www.runoob.com/wp-content/uploads/2025/04/60f5db7a-2591-4f6c-9402-93ab57272d10.png)

**选择主题** ：你可以选择 "Darcula"（深色）或 "Light"（浅色）主题。

![](https://www.runoob.com/wp-content/uploads/2025/04/751a8637-00f2-4a8a-826b-b6e29495eaab.png)

**配置 Python 解释器** ：如果你已经安装了 Python，PyCharm 会自动检测并配置解释器。如果没有安装，你可以手动添加。

![](https://www.runoob.com/wp-content/uploads/2025/04/c6a7fc7a-b3a9-4b65-96c5-2b3680c2e8ae.png)

**安装插件** ：PyCharm 会推荐一些常用插件，你可以根据需要选择安装。

![](https://www.runoob.com/wp-content/uploads/2025/04/c0a8a33b-ecbd-4d8a-97f0-e4f3e5fd50f7.png)

---

## 创建第一个项目

PyCharm 以项目（Project）为单位管理代码，一个项目对应一个文件夹，其中可以包含多个 Python 文件。

### 新建项目

打开 PyCharm 后，点击欢迎界面的 New Project（新建项目）按钮。

![](https://www.runoob.com/wp-content/uploads/2026/07/newrunoob_1784096869537.png)

在弹出的创建窗口中需要确认两项设置：

| 设置项 | 说明 | 示例 |
| --- | --- | --- |
| Location（项目位置） | 项目文件夹的保存路径 | `D:\PythonProjects\HelloWorld` |
| Python Interpreter（解释器） | Python 解释器的路径，PyCharm 通常会自动检测 | 如未自动识别，手动选择 `python.exe` 或 `python3` |

![](https://www.runoob.com/wp-content/uploads/2026/07/newrunoob2_1784096869537.png)

确认无误后，点击右下角的 Create（创建）按钮。

> 项目路径中不要包含中文或空格，否则可能导致一些第三方工具运行异常。推荐使用全英文路径。

### 新建 Python 文件

项目创建完成后，在 PyCharm 主界面中按以下步骤创建 Python 文件：

| 步骤 | 操作 |
| --- | --- |
| 1 | 在左侧 Project（项目）面板中，右键点击项目根目录 |
| 2 | 选择 **New → Python File** |
| 3 | 输入文件名（如 `hello_runoob` ），回车确认 |

![](https://www.runoob.com/wp-content/uploads/2026/07/runoob1_1784097029391.png)

![](https://www.runoob.com/wp-content/uploads/2026/07/runoob2_1784097029391.png)

PyCharm 会自动生成 `hello_runoob.py` 文件，并在右侧编辑区打开供你编写代码。

---

## 编写并运行第一个程序

文件创建好后，编写代码并运行它，看看 Python 程序的执行效果。

### 编写代码

在 `hello_runoob.py` 文件中输入以下代码：

## 实例

\# 文件：hello\_runoob.py  
\# 第一个 Python 程序：问候用户并与其互动  
  
\# print() 函数用于在屏幕上输出文字  
print("Hello, World!")  
  
\# input() 函数等待用户从键盘输入内容，返回值为字符串  
\# 括号中的文字是显示给用户的提示信息  
name = input("请输入你的名字：")  
  
\# f-string（格式化字符串）可以在字符串中直接嵌入变量  
\# 花括号 {} 中的变量会被替换为变量的值  
print(f"你好，{name}！欢迎学习 Python。")

![](https://www.runoob.com/wp-content/uploads/2026/07/runoob3_1784097029391.png)

这段代码做了三件事：

| 代码 | 作用 |
| --- | --- |
| `print("Hello, RUNOOB!")` | 打印一句固定问候语到屏幕上 |
| `input("请输入你的名字：")` | 在屏幕上显示提示文字，等待用户输入名字，按回车确认后把输入内容存入变量 `name` |
| `print(f"你好，{name}！...")` | 使用 f-string 将变量 `name` 的值嵌入到问候语中，生成个性化输出 |

### 运行程序

PyCharm 提供了多种运行方式，任选其一即可：

| 运行方式 | 操作 |
| --- | --- |
| 右键菜单 | 在编辑区任意位置右键，选择 **Run 'hello'** |
| 运行按钮 | 点击代码编辑区右上角的绿色三角形按钮 |
| 快捷键 | Windows/Linux 按 `Shift + F10` ，macOS 按 `Ctrl + R` |

![](https://www.runoob.com/wp-content/uploads/2026/07/runoob4_1784097029391.png)

运行后，PyCharm 底部会弹出 Run（运行）窗口，显示程序输出：

```
Hello, RUNOOB!
请输入你的名字：runoob
你好，runoob！欢迎学习 Python。

进程已结束，退出代码为 0
```

在运行窗口的输入区域键入你的名字并回车，即可看到个性化的问候语。

> 如果运行时提示 "No Python interpreter configured"，说明 PyCharm 没有找到 Python 解释器。前往 `File → Settings → Project → Python Interpreter` 手动指定 Python 安装路径即可解决。

---

## 常见问题排查

初次使用 Python 和 PyCharm 时，以下是最常遇到的四个问题及其解决方案。

| 问题现象 | 可能原因 | 解决办法 |
| --- | --- | --- |
| 命令行输入 `python` 提示「不是内部或外部命令」 | 安装时未勾选 Add to PATH | 重新运行安装程序并勾选该选项，或手动将 Python 路径添加到系统环境变量 |
| PyCharm 提示找不到解释器 | PyCharm 未自动检测到 Python | 在 `File → Settings → Project → Python Interpreter` 中手动指定 Python 可执行文件的路径 |
| 运行代码没有反应或报错 | 代码缩进错误或文件未保存 | 检查代码缩进是否一致（Python 严格要求缩进），确认文件已保存（未保存时文件名旁有圆点标记） |
| 中文输出乱码 | 文件编码不是 UTF-8 | 在 `File → Settings → Editor → File Encodings` 中将编码设为 UTF-8 |