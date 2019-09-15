---
title: flutter开发环境安装
categories: flutter
tags:
  - flutter
toc: true
date: 2019-09-15 21:41:36
scaffolds:
---

## 1. 开发环境安装参考视频
https://www.imooc.com/video/18526
## 2. 遇到问题
### 2.1. 安装了MuMu模拟器
#### 2.1.1. 添加环境变量
* 进入Android sdk安装环境目录
* 将 platform-tools 和 emulator 添加到环境变量中
![2019-09-15-21-39-28-2019915213929](http://blogimage.signalfire2017.com/blog/2019-09-15-21-39-28-2019915213929.png?imageMogr2/thumbnail/!100p)
#### 2.1.2. 打开模拟器 
cmd中执行
```
adb connect 127.0.0.1:7555
```
#### 2.1.3. 错误1
```
Failed to setup Skia Gr context
```
flutter Failed to setup Skia Gr context导致白屏
添加 --enable-software-rendering 参数运行
```
flutter run --enable-software-rendering
```
vscode debug需要添加配置文件
```
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Flutter",
            "request": "launch",
            "type": "dart",
            "args": ["--enable-software-rendering", "-d", "all"]
            
        }
    ]
}
![图片](https://img2018.cnblogs.com/blog/82061/201902/82061-20190201084328506-453598747.png)

```
#### 2.1.4. 注意模拟器中usb调试开启 调试程序设置