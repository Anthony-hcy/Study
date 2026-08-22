---
createTime: 2024-06-07
---
# Windows下的VS Code、OpenCV及C++的安装和配置

## 前言
本文包括基本的<font color=red>VS Code安装</font>、<font color=red>C++环境配置</font>以及<font color=red>OpenCV配置</font>全过程，以及如何解决在配置过程中会遇到的问题。

## 一、资源的下载和安装
### 1、VS Code
#### （1）VS Code下载
官网下载：[Visual Studio Code](https://code.visualstudio.com/#alt-downloads)

![[VS Code-1.png]]

#### （2）VS Code安装
双击.exe文件开始安装，同意此协议，点击下一步：

![[VS Code-2.png]]

选择安装路径，点击下一步：

![[VS Code-3.png]]

点击下一步：

![[VS Code-4.png]]

根据图中提示勾选，点击下一步：

![[VS Code-5.png]]

点击安装：

![[VS Code-6.png]]

安装扩展：

![[VS Code-7.png]]
![[VS Code-8.png]]

### 2、MinGW-w64
官网下载：[MinGW-w64离线包下载地址](https://sourceforge.net/projects/mingw-w64/files/)

![[VS Code-9.png]]

### 3、CMake
官网下载：[CMake](https://cmake.org/download/)

![[VS Code-10.png]]

### 4、OpenCV
官网下载：[OpenCV](https://opencv.org/releases/)

![[VS Code-11.png]]

## 二、环境配置
### 1、VS Code
安装完后，可以在环境变量的<font color=red>用户变量</font>的Path里查看到VS Code，说明配置完成

![[VS Code-12.png]]

### 2、MinGW-w64
解压MinGW-w64压缩包，将该文件夹的bin路径添加到环境变量的<font color=red>系统变量</font>中：

![[VS Code-13.png]]

Win+R，cmd调出控制台，检查MinGW-w64是否安装成功，若成功则如下图所示：

![[VS Code-14.png]]

### 3、CMake
将CMake安装包解压，文件夹如图所示：

![[VS Code-15.png]]

将该文件夹下的bin文件路径添加到环境变量的<font color=red>系统变量</font>中：

![[VS Code-16.png]]

Win+R，cmd调出控制台，检查CMake是否安装成功，若成功则如下图所示：

![[VS Code-17.png]]

### 4、OpenCV
安装完OpenCV后，在 build\x64 路径下新建一个文件夹（可自起，这里是 MinGW ）

![[VS Code-18.png]]

### 5、生成MakeFiles
进入D:\cmake-3.29.4-windows-x86_64\bin :

![[VS Code-19.png]]

打开cmake-gui，选择OpenCV的源文件路径和MakeFiles保存路径（即之前新建的MinGW）：

![[VS Code-20.png]]

点击Configure，弹窗配置如下，点击Next：

![[VS Code-21.png]]

选择前面安装的D:/mingw64/bin文件夹下的gcc.exe和g++.exe，点击Finsh:

![[VS Code-22.png]]


--><font color=red>耐心等待中。。。</font>

执行过程中消息框会出现一堆红色信息，最后显示Configure done，是正常的。</br>显示Configure done后，勾选BUILD_ opencv_world、WITH_ OPENGL和BUILD EXAMPLES，不勾选WITH_IPP、WITH_MSMF和ENABLE_PRECOMPILED_HEADERS (如果有的话)，CPU_ DISPATCH选空。再次点击Configure

--><font color=red>耐心等待中。。。</font></br>这次执行完后仍有错误如下：

![[VS Code-23.png]]

由于网络问题（最好用梯子），仍然会有文件没有成功下载，这个时候需要手动下载它们。
在自创建的MinGW下的CMakeDownloadLog.txt文件中列出了所有丢失文件的下载链接，比如：
>
>https://raw.githubusercontent.com/opencv/opencv_3rdparty/4d348507d156ec797a88a887cfa7f9129a35afac/ffmpeg/opencv_videoio_ffmpeg.dll

一个个访问这些链接，下载后放到OpenCV源文件里.cache的相应子文件夹中替代原缓存文件（下载的文件重命名为相应地缓存文件名并删除原缓存文件）。

这样从头到尾下载CMakeDownloadLog.txt中列出的所有丢失文件，之后，再次点击Configure，出现configure down之后查看CMakeDownloadLog.txt文件，成功了显示下图：
![[VS Code-24.png]]


之后点击Generate，显示Generate Done。

![[VS Code-25.png]]

### 6、编译OpenCV
<font color=red>使用 cmake 时要求安装 python3（版本没有限制，比2高就行），并且python 必须配好了环境变量。</font></br>cmd到MakeFiles所在文件夹（[如何cmd](https://blog.csdn.net/weixin_42809924/article/details/104929103)），执行如下命令：

>mingw32-make -j 8

![[VS Code-26.png]]

如果报错可查阅[编译报错解答](https://blog.huihut.com/2018/07/31/CompiledOpenCVWithMinGW64/)，如果编译成功则执行如下命令完成装载：

>mingw32-make install

![[VS Code-27.png]]

将D:\opencv\build\x64\MinGW\bin加到环境变量的<font color=red>系统变量</font>中：​​​​​​​
![[VS Code-28.png]]


### 7、VS Code配置
#### （1）c_cpp_properties.json
~~~json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "D:/opencv/opencv/build/x64/MinGW/install/include",
                "D:/opencv/opencv/build/x64/MinGW/install/include/opencv2"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "windows-gcc-x64",
            "compilerPath": "D:/VScode/mingw64/bin/gcc.exe"
        }
    ],
    "version": 4
}
~~~

#### （2）launch.json
~~~json
{
 
    "version": "0.2.0",
    "configurations": [
        {
            "name": "opencv4.9.0 debuge",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": true,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "miDebuggerPath": "D:/VScode/mingw64/bin/gdb.exe",
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": false
                }
            ],
            "preLaunchTask": "opencv4.9.0 compile task"
        }
    ]
}
~~~

#### （3）tasks.json
~~~json
{
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++.exe 生成活动文件",
            "command": "D:\\VScode\\mingw64\\bin\\g++.exe",
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                //"${file}",
                //"${fileDirname}\\*.cpp",
                "*.cpp",
 
                //取消注释后运行慢，但不报错
                // "-I", "D:/opencv/opencv/build/include",
                // "-L", "D:/opencv/opencv/build/x64/MinGW/bin",
                // "-l", "libopencv_world490",
                // "-l", "opencv_videoio_ffmpeg490_64",
 
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe"
            ],
            "options": {
                "cwd": "${fileDirname}"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "detail": "调试器生成的任务。"
        }
    ],
    "version": "2.0.0"
}
~~~

#### （4）settings.json
~~~json
{
    "code-runner.executorMap": {
 
        "javascript": "node",
        "java": "cd $dir && javac $fileName && java $fileNameWithoutExt",
        "c": "cd $dir && gcc $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
        "zig": "zig run",
        "cpp": "cd $dir && g++ *.cpp -o $fileNameWithoutExt -I D:/opencv/opencv/build/include -L D:/opencv/opencv/build/x64/MinGW/bin  -l libopencv_world490 -l opencv_videoio_ffmpeg490_64 && $dir$fileNameWithoutExt",
        "objective-c": "cd $dir && gcc -framework Cocoa $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
        "php": "php",
        "python": "python -u",
        "perl": "perl",
        "perl6": "perl6",
        "ruby": "ruby",
        "go": "go run",
        "lua": "lua",
        "groovy": "groovy",
        "powershell": "powershell -ExecutionPolicy ByPass -File",
        "bat": "cmd /c",
        "shellscript": "bash",
        "fsharp": "fsi",
        "csharp": "scriptcs",
        "vbscript": "cscript //Nologo",
        "typescript": "ts-node",
        "coffeescript": "coffee",
        "scala": "scala",
        "swift": "swift",
        "julia": "julia",
        "crystal": "crystal",
        "ocaml": "ocaml",
        "r": "Rscript",
        "applescript": "osascript",
        "clojure": "lein exec",
        "haxe": "haxe --cwd $dirWithoutTrailingSlash --run $fileNameWithoutExt",
        "rust": "cd $dir && rustc $fileName && $dir$fileNameWithoutExt",
        "racket": "racket",
        "scheme": "csi -script",
        "ahk": "autohotkey",
        "autoit": "autoit3",
        "dart": "dart",
        "pascal": "cd $dir && fpc $fileName && $dir$fileNameWithoutExt",
        "d": "cd $dir && dmd $fileName && $dir$fileNameWithoutExt",
        "haskell": "runghc",
        "nim": "nim compile --verbosity:0 --hints:off --run",
        "lisp": "sbcl --script",
        "kit": "kitc --run",
        "v": "v run",
        "sass": "sass --style expanded",
        "scss": "scss --style expanded",
        "less": "cd $dir && lessc $fileName $fileNameWithoutExt.css",
        "FortranFreeForm": "cd $dir && gfortran $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
        "fortran-modern": "cd $dir && gfortran $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
        "fortran_fixed-form": "cd $dir && gfortran $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
        "fortran": "cd $dir && gfortran $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
        "sml": "cd $dir && sml $fileName",
        "mojo": "mojo run",
        "erlang": "escript",
        "spwn": "spwn build",
        "pkl": "cd $dir && pkl eval -f yaml $fileName -o $fileNameWithoutExt.yaml",
        "gleam": "gleam run -m $fileNameWithoutExt"
    }
}
~~~

#### （5）测试代码
~~~cpp
#include <iostream>
#include <opencv2\highgui\highgui.hpp>
#include <opencv2\opencv.hpp>
using namespace std;
using namespace cv;

int main()
{
    string path = "C:/Users/WCJ/Desktop/0402/01/0.bmp";
    cv::Mat img = imread(path);
    imshow("img",img);
    waitKey(0);
    return 0;
}
~~~
## 三、问题及解决方法
主要是我在配置中遇到的一些问题，以及搜索到的解决方法，如果有遗漏的欢迎在评论区提出

### 1、下载问题
下载安装包和配置文件时最好挂上梯子，不然要么下载速度慢，要么下载失败

<font color=red>注意要下载安装Python3</font>

### 2、cmake编译Opencv出现ffmpeg_cmake手动下载后也无法使用问题
解决方法：[cmake编译Opencv出现ffmpeg_cmake手动下载后也无法使用问题](https://blog.csdn.net/martinkeith/article/details/108002333)

非常好！！！

### 3、如何在cmd（命令提示符）中打开指定文件夹路径
解决方法：

[总结：如何在cmd（命令提示符）中打开指定文件夹路径](https://blog.csdn.net/weixin_42809924/article/details/104929103)

Nice！！！

### 4、常规报错
[Windows下Mingw+OpenCV的编译步骤以及问题记录](https://blog.csdn.net/qq_40589021/article/details/129025062)

### 5、VScode报错：undefined reference to ......
解决方法：

#### （1）[VScode报错：undefined reference to ‘WinMain’ collect2.exe: error: ld returned 1 exit status](https://blog.csdn.net/weixin_43702899/article/details/106467562)

#### （2）[vscode下编译告警“undefined reference”？三步教你如何解决](https://blog.csdn.net/squall0984/article/details/107637986)
#### （3）Code Runner的问题
[VS code undefined reference to ‘xxx‘（容易被忽略的错误）](https://blog.csdn.net/weixin_45514968/article/details/119153141)

#### （4）头文件报错
[【解决】VSCode编写C++自定义头文件undefined reference异常问题](https://blog.csdn.net/qq_29750461/article/details/127972046)

真是离谱的报错。。。

### 6、无法在只读编辑器中编辑
解决方法：

[vscode提示“无法在只读编辑器中编辑”解决方法](https://blog.csdn.net/qq_41541179/article/details/109130412)

### 7、查看OpenCV版本
[Windows查看opencv版本](https://www.cnblogs.com/yocichen/p/11617202.html)

### 8、JSON文件的配置
看看这两篇：

[一文解决VS Code安装、C++环境配置、OpenCV配置](https://blog.csdn.net/weixin_43101257/article/details/124472866)

[关于 Windows 下使用 VSCode 搭建 OpenCV 环境的问题](https://blog.csdn.net/qq_41567540/article/details/115071559)

## 四、资料来源
### 1、[一文解决VS Code安装、C++环境配置、OpenCV配置](https://blog.csdn.net/weixin_43101257/article/details/124472866)
### 2、[关于 Windows 下使用 VSCode 搭建 OpenCV 环境的问题](https://blog.csdn.net/qq_41567540/article/details/115071559)

## 五、原文链接

[**点击跳转**](https://blog.csdn.net/2301_76911910/article/details/139509421?spm=1001.2014.3001.5501)
