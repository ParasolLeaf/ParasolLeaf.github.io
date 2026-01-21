---
title: VSCode 远程 SSH 连接配置与常见问题处理
date: 2023-11-01 20:38:00 +0800
categories: [环境配置, VSCode]
tags: [SSH, VSCode, 配置]
author: wyt
---

> 本文介绍在 VSCode 中通过 Remote SSH 插件连接远程服务器的基本操作方法，并详细记录了连接失败时的排查与修复经验。

---

## 1. VSCode 内远程连接步骤
1. 在 VSCode 中安装 **Remote - SSH** 插件；
2. 点击左下角绿色图标按钮；
3. 选择 `Connect to Host...`，输入连接格式：
    ```text
    <username>@<ip_address>
    ```
4. 输入远程主机的用户密码，连接成功后可以直接像在cmd一样操作，支持文件传输与终端命令。

## 2. 新建Remote连接
新建远程连接时，可以直接点击左侧`Remote Explorer`中的➕（+）号添加 New Remote，以避免直接点击齿轮图标（⚙）手动编辑config文件。
```bash
ssh <user>@<HostName> -p <port>
```
其中`<user>`为远程用户名，`<HostName>`服务器地址IP，`<port>`为SSH端口号（通常为22，但服务器可能自定义）

## 3. 配置文件路径与问题排查、
- 若 VSCode 无法保存 SSH config，可手动删除配置文件，位于`C:\Users\<你的用户名>\.ssh\config`
- 若连接失败，可能是C盘用户.ssh目录下known_hosts文件记录了旧的远程主机 key，可尝试删除该文件，位于`C:\Users\<你的用户名>\.ssh\known_hosts`

## 4. 常见错误示例与处理办法
当远程主机的 Key 更改后，VSCode SSH 可能会报如下错误：
```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
...
Host key verification failed.
```
遇到此问题：
1. 打开 known_hosts 文件
2. 找到与报错中提示的地址（如 [127.0.0.1]:20729）对应的行
3. 删除或修改该行中指纹（SHA256:xxx）的值，以匹配新主机的 Key

## 5. 无法连接的其他排查建议
- 检查远程主机用户主目录下 .ssh/config 文件是否配置正确端口
- 若配置正确但仍无法连接，可尝试修改文件权限：
    1. 右键 config 文件 → 属性 → 安全 → 编辑权限
    2. 将当前user用户的权限设置为 完全控制