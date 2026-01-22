---
title: Java、JDK、JRE 环境配置教程
date: 2024-07-11 01:44:00 +0800
categories: [环境配置, Java]
tags: [Java, 配置]
author: wyt
description: Windows下配置Java开发环境(JDK+JRE)
---

> 本文简要说明如何在 Windows 系统上手动配置 Java 开发环境（JDK 和 JRE），并设置环境变量以进行命令行验证。

## 1. 下载 JDK 安装包

前往 Oracle 官网下载 JDK 8 安装包：

- 官网链接：[https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
- 安装包：`jdk-8u202-windows-x64.exe`

## 2. 安装路径建议

- 创建安装目录，例如：`D:\user\java`
- 安装 JDK 至：`D:\user\java\jdk-1.8` ← `JAVA_HOME`
- 安装 JRE 至：`D:\user\java\jre-1.8`

## 3. 配置环境变量

打开「系统属性 → 高级 → 环境变量」，设置如下变量：

- 新建系统变量：
    ```plaintext
    变量名：JAVA_HOME
    变量值：D:\user\java\jdk-1.8
    ```

- 编辑Path变量，追加下述两项：
    ```plaintext
    %JAVA_HOME%\bin
    %JAVA_HOME%\jre\bin
    ```

- 新建系统变量：
    ```plaintext
    变量名：ClassPath
    变量值：.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;
    ```

## 4. 重启验证
重启电脑确保环境变量更新，新开cmd在其中输入以下命令进行环境验证：
```shell
echo %JAVA_HOME%
java
java -version
javac
```
若均有版本信息输出，则说明安装成功。

## 5. 参考链接
- <https://blog.csdn.net/xhmico/article/details/122390181>