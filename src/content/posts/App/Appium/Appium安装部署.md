---
title: Appium
pubDate: 2024-08-24
categories: ['APP']
description: ''

---

# Appium

## OverView

Appium 基本使用及控制真机及安卓模拟器





## 安装部署

[官网]([欢迎 - Appium Documentation](https://appium.io/docs/en/latest/))

您可以使用 `npm` 在全局范围内安装 Appium：

```
npm i -g appium
```

为了更新Appiums使用 `npm`:

```
npm update -g appium
```



## 驱动

如果没有驱动，无法使用Appium，驱动是允许Appium自动化特定平台的接口



### UiAutomator2驱动

[驱动文档](https://github.com/appium/appium-uiautomator2-driver)

安装 

```
appium driver install uiautomator2
```



```
pip install Appium-Python-Client
```



## 准备设备

- 如果使用模拟器，使用 Android Studio 创建并启动一个 Android 虚拟设备 (AVD)。 你可能需要下载你想要创建的模拟器的 API 级别的系统镜像。使用 Android Studio 中的 AVD 创建向导通常是完成所有这些操作的最简单方式。
- 如果使用真实设备，应该[为开发设置并启用 USB 调试](https://developer.android.com/studio/debug/dev-options)。
- 连接模拟器或设备后，你可以运行 `adb devices`（通过位于 `$ANDROID_HOME/platform-tools/adb` 的二进制文件）来验证你的设备是否显示为已连接

## 参考

[Android SDK Windows 安装及环境配置教程_android-sdk-windows-CSDN博客](https://blog.csdn.net/qq_62238325/article/details/130676856)



[appium执行过程中遇到的错误 - Walker~ - 博客园](https://www.cnblogs.com/walker20201219/p/14528263.html)



