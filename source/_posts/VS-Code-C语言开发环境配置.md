---
title: VS Code C语言开发环境配置
tags:
  - C语言
categories:
  - 学习
  - C语言
abbrlink: bfe43f2e
date: 2022-07-01 10:43:12
---



## 安装VS code

VS code：https://code.visualstudio.com

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103908.png)

下载完成后双击安装



![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103906.png)







安装完成后，打开VS Code，在安装插件栏搜索chinese，找到简体中文版本，点击install，安装结束后重启应用完成汉化



![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103914.png)





## 安装用于C/C++的插件



插件栏搜索Code Runner，点击安装



![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103918.png)

安装完成后在扩展设置中勾选`Ignore Selection`、`Run In Terminal`

![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103923.png)



![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103926.png)



插件栏搜索C/C++,点击安装



![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103930.png)



## 安装编译器MinGW-W64 GCC



MinGW-W64 GCC：https://sourceforge.net/projects/mingw-w64



![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103935.png)



![9](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103938.png)

下载完成后解压文件



![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103941.png)





在D盘新建文件夹（英文命名就行，中文总会有莫名其妙的问题），之后将解压的文件移动到刚刚新建的文件夹中



![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103947.png)





##  配置环境变量

打开mingw64，点开bin，将bin所在地址复制下来

![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103951.png)



打开设置，依次点击`系统`--`关于`--`高级系统设置`--`环境变量` 在`Administrator的用户变量`中编辑`Path`变量，点击空白处，将复制好的地址粘贴进去，点击确定





![13](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103955.png)

同样的步骤，在文件夹中找到inlude，点开，复制地址，粘贴到Path中



![14](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701103959.png)



依次点击确定关闭窗口

![15](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701104003.png)





完成后按下win+r，输入cmd，点击确定





![16](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701104008.png)

弹出黑框后输入

```
gcc -v -E -x c++ -
```


若显示结果如图 则配置成功



![17](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701104013.png)







## 配置三个文件 c_cpp_propertise.json、launch.json、tasks.json



先在电脑上新建一个文件夹（像我就是在我的电脑上新建一个叫`C--`的文件夹），打开它

![18](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701104018.png)



之后新建三个文件，名字是`c_cpp_propertise.json`、 `launch.json`、 `tasks.json`，如图



![19](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701104021.png)

接下来把相应内容写进去

c_cpp_propertise.json

> 在这个代码中，我们需要把地址改成自己的mingw 64 bin的地址

```json
{   
    "configurations": [       
     {          
         "name": "Win32",       
         "includePath": [
         "${workspaceRoot}",               
         "D:/C/mingw64/include/**",               
         "D:/C/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++",
         "D:/C/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/x86_64-w64-mingw32",
         "D:/C/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/backward",
         "D:/C/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include",
         "D:/C/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include-fixed",
         "D:/C/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/../../../../x86_64-w64-mingw32/include"
    ],      
    "defines": [
        "_DEBUG",               
        "UNICODE",
        "__GNUC__=6",               
        "__cdecl=__attribute__((__cdecl__))"            
        ],           
    "intelliSenseMode": "msvc-x64",
    "browse": {              
        "limitSymbolsToIncludedHeaders": true,
        "databaseFilename": "",
        "path": [                   
            "${workspaceRoot}",                   
            "D:/C/mingw64/include/**",                   
            "D:/C/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++",
            "D:/C/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/x86_64-w64-mingw32",
            "D:/C/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/backward",
            "D:/C/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include",
            "D:/C/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/include-fixed",
            "D:/C/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/8.1.0/../../../../x86_64-w64-mingw32/include"
           ]        
        }       
     }   
  ],   
   "version": 4
}
```





![20](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701104027.png)



launch.json

> 在这个代码中，我们需要把地址改成自己的mingw 64 bin的地址

```json
{  
    "version": "0.2.0",
    "configurations": [     
        {           
            "name": "(gdb) Launch", // 配置名称，将会在启动配置的下拉菜单中显示            
            "type":"cppdbg", // 配置类型，这里只能为cppdbg           
            "request": "launch", //请求配置类型，可以为launch（启动）或attach（附加）          
            "program":"${workspaceFolder}/${fileBasenameNoExtension}.exe", // 将要进行调试的程序的路径
            "args": [], // 程序调试时传递给程序的命令行参数，一般设为空即可            
            "stopAtEntry":false, // 设为true时程序将暂停在程序入口处，一般设置为false            
            "cwd":"${workspaceFolder}", // 调试程序时的工作目录，一般为${workspaceRoot}即代码所在目录workspaceRoot已被弃用，现改为workspaceFolder            
            "environment": [],  
            "externalConsole": true, // 调试时是否显示控制台窗口，一般设置为true显示控制台           
            "MIMode": "gdb",            
            "miDebuggerPath":"D:/C/mingw64/bin/gdb.exe", // miDebugger的路径，注意这里要与MinGw的路径对应  
            "preLaunchTask": "gcc", // 调试会话开始前执行的任务，一般为编译程序，c++为g++, c为gcc      
            "setupCommands": [     
                {                   
                    "description":     "Enable pretty-printing for gdb",                   
                    "text": "-enable-pretty-printing",                   
                    "ignoreFailures": false  
                  }        
               ]     
            }  
         ]
      }
```







![21](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701104033.png)









tasks.json

```json
{   
     "version": "2.0.0",   
     "command": "gcc", // 注意对应  
     "args":["-g","${file}","-o","${fileBasenameNoExtension}.exe"],    // 编译命令参数
     "problemMatcher": {       
         "owner": "cpp",
         "fileLocation":["relative","${workspaceFolder}"],        
         "pattern": {
         "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
         "file": 1,         
         "line": 2,         
         "column": 3,           
         "severity": 4,            
         "message": 5       
         }  
      }
   }
```







## 重启与调试

重启vscode，在文件夹C--上新建一个以.c结尾的文件
这里我们简单编写一个C语言的程序，设置断点并调试





![22](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220701104037.png)







完成
