默认账户：pi
默认密码：raspberry
必备：SD卡、读卡器、电源

# 一、烧录系统至SD卡
![|500](https://assets.raspberrypi.com/static/d90b668e881f3d1613a902d1a7eee4dd/fba41/windows.webp)
采用**Raspberry Pi Imager**烧录系统至SD卡
其中网络配置建议采用网络热点（比如名称：pi 密码：12345678 ）
账户配置建议默认即可
建议打开ssh
此处部分教程可参考
- [树莓派教程第一课 树莓派简介 十分钟玩转系列入门篇-哔哩哔哩](https://b23.tv/dfEHvKA)

# 二、查找树莓派ip地址
烧录完后将SD卡插入树莓派
通电等待一会儿后在电脑的热点信息即可查看树莓派的ip地址

# 三、SSH远程访问
此处需要工具[putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
在putty里输入树莓派ip通过ssh连接，输入账户名及密码即可通过命令行操控树莓派
部分教程可参考
- [使用 PuTTY 通过 SSH 登录到树莓派 \| 树莓派实验室](https://shumeipai.nxez.com/2024/04/22/use-putty-to-login-to-your-raspberry-pi-via-ssh.html)
- [树莓派教程05-无显示屏连接、网线远程连接-基础篇-每天十分钟带你学会树莓派\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV16U4y1879Q?t=564.9&p=5)

# 四、VNC远程访问
接着我们输入`sudo raspi-config`
打开VNC服务
此处部分教程可参考
- [每天十分钟带你学会树莓派 基础篇07 VN从连接，配置静态IP地址\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV16U4y1879Q?t=360.6&p=7)
- [远程访问 \| Raspberry Pi 树莓派 (官网26年04月更新)](https://pidoc.cn/docs/computers/remote-access/#%E8%BF%9E%E6%8E%A5%E5%88%B0-vnc-%E6%9C%8D%E5%8A%A1%E5%99%A8)

# 五、安装 Python的OpenCV库
1. 打开终端：在树莓派桌面上，点击顶部的黑色终端图标（Terminal）。
2. 更新软件源：在终端里输入以下命令并按回车（这会确保你下载的是最新版本）：
`sudo apt update`
3. 安装 OpenCV：更新完成后，输入以下命令并按回车：
`sudo apt install python3-opencv`
4. 确认安装：如果中途提示“是否继续 (Y/n)”，输入 `Y` 并按回车，然后等待进度条跑完。

# 六、树莓派传文件至电脑
在 Windows 电脑上按下 `Win + R` 键，输入 `cmd` 打开命令提示符
输入以下命令：
```
scp -r pi@192.168.137.145:/home/pi/Desktop/test C:\Users\34927\Desktop\test
```
(注意：把里面的 IP 地址和用户名替换成你真实的。-r 参数代表递归拷贝整个文件夹。)
敲击回车后，会提示你输入树莓派的密码（输入时屏幕不显示字符），填对后回车，就会看到文件传输完成

# 七、无线网卡
Windows端插上即用
下面讲述树莓派端

**1、确认树莓派是否从硬件层面识别了网卡**
在终端输入：
```
lsusb
```
在弹出的列表中，你应该能看到一行包含 `Realtek Semiconductor Corp. RTL8812AU 802.11a/b/g/n/ac 2T2R` 或类似字样的设备。这说明物理连接没问题。

**2、安装编译驱动所需的依赖工具**
（这一步需要保持网络畅通，也就是你的树莓派连着你笔记本热点的状态）
```
sudo apt update
sudo apt install -y dkms git bc build-essential libelf-dev
sudo apt install -y linux-headers-$(uname -r)
```

**3、下载并编译 RTL8812AU 驱动**
在你的 Windows 电脑浏览器里，直接访问这个地址（这是 aircrack-ng 库的源码压缩包直链）：
https://github.com/aircrack-ng/rtl8812au/archive/refs/heads/v5.6.4.2.zip
在Windows终端将文件传给树莓派
```
scp D:\rtl8812au-5.6.4.2.zip pi@树莓派的IP地址:/home/pi/
```
在树莓派终端解压编译
```
unzip rtl8812au-5.6.4.2.zip
cd rtl8812au-5.6.4.2
sudo make dkms_install
```
**4、重启、查询**
```
sudo reboot
iwconfig
```

# 八、启用静态IP

> [!note]
> 以下内容来自Gemini

**第一步：在 nmtui 中强行绑定静态 IP**
在树莓派终端输入：
```
sudo nmtui
```
进入蓝色配置界面后，新建专属配置：
1、按右方向键，选中界面右侧的` <添加>`，按回车。
2、在弹出的网络类型列表中，选中` Wi-Fi`，按回车。

**第二步：填写核心参数（关键！）**
进入详细的编辑界面后，请依次把参数填对：
- 配置名称 (Profile name)：随便起个名区分一下，比如输入 `pi-fast`
- 设备 (Device)：手动输入 `wlan1` （这一步极其重要，相当于硬绑定这块高速网卡）
- SSID：输入你的热点名称，也就是 `pi`
- 安全性 (Security)：选中右侧选项，把它改成 `WPA 及 WPA2 个人`
- 密码 (Password)：输入热点密码 `12345678`

**第三步：注入静态 IP**
继续往下翻，找到 IPv4 配置：
1、把 `<自动>` 改成 `<手动>`。
2、选中 `<手动>` 右边的 `<显示>` 按回车，展开详细设置：
- 地址 (Addresses)：输入 `192.168.137.100/24` （千万别漏了后面的 `/24`）
- 网关 (Gateway)：输入 `192.168.137.1`
- DNS 服务器：输入 `192.168.137.1`

3、填完之后，一路往下翻到界面最底部，选中 `<确定>` 按回车保存。

**第四步：激活高速通道**
保存后，按 `Esc` 键一路退回到黑底白字的命令行界面。然后输入这行命令，强制激活我们刚建好的配置文件：
```
sudo nmcli connection up pi-fast
```
只要它提示成功激活（或者你输入` ifconfig wlan1` 看到了 `.100` 的 IP），你就可以按照之前的原计划行动了。

# 九、树莓派开启热点
树莓派终端，建立热点
```
sudo nmcli device wifi hotspot ifname wlan1 ssid "Pi_5G_Vision" password "12345678" band a channel 36
```
Windows的WLAN2连接热点
之后树莓派IP即为`10.42.0.1`


# 其它
- [树莓派(Raspberry Pi)如何修改成中文](https://blog.csdn.net/meihualing/article/details/110677195)
> [!配置完以后启动]
>Windows的`WLAN`设置热点信息，即`名称：pi 密码：12345678 `，采用`2.4GHz频段`
>启动树莓派，会自动连接该热点，即可通过`IP：192.168.137.100`进行VNC远程连接
>进入树莓派后，在WIFI处打开树莓派热点`Pi_5G_Vision`
>Windows的WLAN2连接树莓派热点，即可通过`IP：10.42.0.1`进行VNC远程连接


```python
import cv2
import os

left_dir = "/home/pi/Desktop/test/left"
right_dir = "/home/pi/Desktop/test/right"
os.makedirs(left_dir, exist_ok=True)
os.makedirs(right_dir, exist_ok=True)

cap_left = cv2.VideoCapture(2, cv2.CAP_V4L2)
cap_right = cv2.VideoCapture(0, cv2.CAP_V4L2)

if not cap_left.isOpened() or not cap_right.isOpened():
    print("错误：无法同时打开摄像头。")
    exit()

# 强制指定底层数据流使用 MJPG 压缩格式传输
cap_left.set(cv2.CAP_PROP_FOURCC, cv2.VideoWriter_fourcc(*'MJPG'))
cap_right.set(cv2.CAP_PROP_FOURCC, cv2.VideoWriter_fourcc(*'MJPG'))

# 设置底层硬件最高分辨率
cap_left.set(cv2.CAP_PROP_FRAME_WIDTH, 4000)
cap_left.set(cv2.CAP_PROP_FRAME_HEIGHT, 3000)
cap_right.set(cv2.CAP_PROP_FRAME_WIDTH, 4000)
cap_right.set(cv2.CAP_PROP_FRAME_HEIGHT, 3000)

img_counter = 0

print("双目独立摄像头已启动 (单窗口拼接模式)...")
print(" -> 【注意】请务必用鼠标点击一下弹出的视频预览窗口，使其获得焦点！")
print(" -> 按 's' 键同步保存左右高清图像")
print(" -> 按键盘 'X' 或 'x' 键退出程序")

while True:
    ret_l, frame_l = cap_left.read()
    ret_r, frame_r = cap_right.read()
    
    if ret_l and ret_r:
        # 缩小预览画面
        disp_l = cv2.resize(frame_l, (800, 600))
        disp_r = cv2.resize(frame_r, (800, 600))
        
        # 【终极解决方案】：将左右两个预览画面横向无缝拼接成一张宽图
        # 形成一张 1600 x 600 的完整画面
        combined_disp = cv2.hconcat([disp_l, disp_r])
        
        # 只弹出一个独立的窗口显示拼接后的画面
        cv2.imshow('Stereo Camera Preview (Left | Right)', combined_disp)
        
        key = cv2.waitKey(1) & 0xFF
        
        if key == ord('x') or key == ord('X'):
            print("正在退出...")
            break
            
        elif key == ord('s') or key == ord('S'):
            left_filename = os.path.join(left_dir, f"left_{img_counter}.jpg")
            right_filename = os.path.join(right_dir, f"right_{img_counter}.jpg")
            
            # 注意：保存的依然是原始的、未被拼接的 4000x3000 高清大图
            cv2.imwrite(left_filename, frame_l, [int(cv2.IMWRITE_JPEG_QUALITY), 100])
            cv2.imwrite(right_filename, frame_r, [int(cv2.IMWRITE_JPEG_QUALITY), 100])
            
            print(f"[{img_counter}] 抓拍成功！")
            img_counter += 1
            
    else:
        print("警告：读取帧失败。如果仍报 timeout，请降低帧率或检查 USB 供电。")
        break

cap_left.release()
cap_right.release()
cv2.destroyAllWindows()
```
