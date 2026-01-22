---
title: Windows 下使用 VS Code + MinGW + Ubuntu 虚拟机的开发环境配置记录
date: 2024-07-10 16:46:00 +0800
categories: [环境配置, VSCode]
tags: [VSCode, MinGW, Ubuntu, VirtualBox, SSH, 配置]
author: wyt
---

> 本文记录了在 Windows 系统下，配置MinGW（GCC）+ VS Code进行本地开发，以及使用 VirtualBox 安装 Ubuntu 并通过 SSH 进行远程开发与文件传输的完整流程。内容偏向实操与踩坑记录。

## 一、MinGW（GCC）安装与配置

### 1. 下载 MinGW-w64

前往 MinGW-w64 官方 SourceForge 页面：<https://sourceforge.net/projects/mingw-w64/>

**注意：**
-  不要下载 `mingw installer manager.exe`，只需要下载 **zip 压缩包**

推荐版本示例：
```text
x86_64-8.1.0-release-win32-seh-rt_v6-rev0.zip
```

### 2. 解压到本地目录
将压缩包解压到本地某路径，例如D盘（路径可自定义）：`D:\mingw64`

### 3. 配置系统环境变量
路径设置步骤：此电脑 → 右键 → 属性 → 高级系统设置 → 环境变量 → 系统变量 Path → 新增。添加环境变量：`D:\mingw64\bin`，只需要bin目录即可。设置后**重启电脑**，确保环境变量生效。

### 4. 验证 GCC 是否安装成功
在cmd中，输入`gcc -v`，若能正确输出版本信息，说明安装成功。

## 二、VS Code 本地编译与调试（Windows）
1. 编译环境说明
    VS Code 的 编译环境本质上仍然使用 MinGW 的 GCC。在 VSCode 中：使用 Terminal（终端）/ 手动执行 gcc 编译命令。示例：
    ``` bash
    cd <源文件所在目录>
    gcc main.cpp -o main
    ./main
    ```

2. 调试环境（简述）
调试可通过 VS Code 的 launch.json 配置完成，本文不展开，建议参考以下文章：<https://blog.csdn.net/qq_43331089/article/details/123221633>

## 三、VirtualBox 安装 Ubuntu
使用 VirtualBox 创建 Ubuntu 虚拟机，Ubuntu 安装包（.iso）建议提前放到虚拟硬盘所在目录。参考<https://blog.csdn.net/Inochigohan/article/details/119791518>

## 四、Ubuntu 与 Windows 主机的 SSH 远程连接
1. Ubuntu 中安装 OpenSSH
    在 Ubuntu 终端执行：
    ```bash
    sudo apt-get install openssh-server # 安装
    ps -aef | grep sshd # 确认 SSH 服务正在运行
    ssh localhost # 本机测试
    ```

2. 网络模式设置为「桥接」
    虚拟机构建时有NAT和桥接网络模式，其中NAT意味着虚拟机与主机不在同一网段，无法 SSH，桥接时主机和虚拟机处于同一局域网。Win和Ubuntu中分别可以通过`ipconfig`和`ifconfig`进行查看。可以通过`ping <ip_address>`测试连通性，例如ping Ubuntu中enp0s3 下的 inet，如 10.2.216.41；或是Win中WLAN 下的 IPv4 地址。

3. Windows 通过 SSH 连接 Ubuntu
    在cmd中执行`ssh <username>@<ip_address>`，输入密码或提前配置好rsa/ed25519的秘钥进行登录连接。

## 五、主机与虚拟机之间的文件传输（SCP）
1. Windows → Ubuntu
    ```bash
    scp -r ./local_folder username@ip:~/target_folder
    scp -r ./desktop/datalab-handout wuyt@10.2.216.41:~/Desktop # 示例
    ```
2. Ubuntu → Windows
    ```bash
    scp username@ip:~/folder/file ./local_path
    scp wuyt@10.2.216.41:~/Desktop/handout ./D:/vscode/ # 示例
    ```