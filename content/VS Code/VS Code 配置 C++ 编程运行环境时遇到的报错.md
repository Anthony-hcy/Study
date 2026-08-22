---
createTime: 2024-05-24
---
# VS Code 配置 C/C++ 编程运行环境时遇到的报错，如No such file or directory cc1.exe: fatal error: *.c: Invalid arg（小白）

关于如何在VS Code 配置 C/C++ 编程运行环境可以参考以下两篇（我是按这两个来的）：

[VS Code 配置 C/C++ 编程运行环境（保姆级教程）](https://blog.csdn.net/qq_42417071/article/details/137438374)

[VScode搭建C/C++开发环境](https://blog.csdn.net/Yikefore/article/details/130033638?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171651696216800182179601%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=171651696216800182179601&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-2-130033638-null-null.142%5Ev100%5Econtrol&utm_term=vscode%20%E6%90%AD%E5%BB%BA%20C%2FC%2B%2B%20%E7%BC%96%E8%AF%91%E7%8E%AF%E5%A2%83%E6%95%99%E7%A8%8B&spm=1018.2226.3001.4187)

主要解决一下我出现的报错，如下：

![[VS Code-29.png]]

![[VS Code-30.png]]

第一个问题在于，自己敲的.h文件引用时应该为 **# include "    .h"** 而不是用 **<    .h>** 之前并不知道这些；

第二个问题在于**tasks.json**的问题：

![[VS Code-31.png]]

如图所示的 
>"${file}"
>
>"*.c"
>
>"${workspaceFolder}\\*.c"
>
>"${fileDirname}\\*.c"  

这四个里选一个，
>"${fileDirname}\\${fileBasenameNoExtension}.exe",
>
>"${fileDirname}\\program.exe",  
>
>"${workspaceFolder}\\${workspaceRootFolderName}.exe",

这三个里选一个，
然后注意 __tasks.json__ 文件里“options”下的“cwd”应该为 __"${fileDirname}"__

修改成功后 __tasks.json__ 的代码：
~~~json
{
	"version": "2.0.0",
	"tasks": [
		{
			"type": "cppbuild",
			"label": "C/C++: gcc.exe 生成活动文件",
			"command": "D:/VScode/msys64/ucrt64/bin/gcc.exe",
			"args": [
				"-fdiagnostics-color=always",
				"-g",
				"*.c",
				"-o",
				"${workspaceFolder}\\${workspaceRootFolderName}.exe",
				""
			],
			"options": {
				"cwd":  "${fileDirname}"
			},
			"problemMatcher": [
				"$gcc"
			],
			"group": "build",
			"detail": "编译器: D:/VScode/msys64/ucrt64/bin/gcc.exe"
		}
	]
}
~~~

没有报错：

![[VS Code-32.png]]

以上便是我解决我遇到的报错的解决方法，感兴趣的小伙伴可以参考一下，至于其他可能出现的报错我目前还未遇到，可以留言或者网上查询一下，谢谢您的阅读！😘😘😘


# 2024-11-10更新如下:


>(1)c_cpp_properties.json

```json
{
  "configurations": [
      {
          "name": "Win32",
          "includePath": [
              "${workspaceFolder}/**"
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
```

>(2)launch.json

```json
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
```

>(3)settings.json

```json
{
  "code-runner.executorMap": {

      "javascript": "node",
      "java": "cd $dir && javac $fileName && java $fileNameWithoutExt",
      "c": "cd $dir && gcc *.c -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
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
```

>(4)tasks.json

```json
{
    "tasks": [
        {
            "type": "cppbuild",
            "label": ".C :g++.exe 生成活动文件",
            "command": "D:\\VScode\\mingw64\\bin\\g++.exe",
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                //"${file}",
                //"${fileDirname}\\*.cpp",
                "*.c",//或者"*.cpp",
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
```


原文链接： [__点击跳转__](https://blog.csdn.net/2301_76911910/article/details/139171662?spm=1001.2014.3001.5501)
