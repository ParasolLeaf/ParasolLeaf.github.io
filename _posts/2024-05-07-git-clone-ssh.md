---
title: Git clone 与 SSH 配置中的一些报错与解决记录
date: 2024-05-07 15:22:00 +0800
categories: [环境配置, Git]
tags: [Git, SSH, 开发工具]
author: wyt
---

> 本文是一次**使用 Git 过程中零散问题的学习记录汇总**，主要集中在：
- `git clone` 过程中常见的报错
- HTTPS 与 SSH 拉取方式的差异
- Windows 与 Ubuntu 环境下 SSH 配置的不同点
- `git lfs install` 相关的初始化问题

内容并非系统教程，而是以问题出现 → 排查 → 解决的顺序整理，方便后续快速回溯。


## 一、使用 HTTPS 方式 git clone 的 HTTP/2 报错

### 1. 报错现象

在使用 HTTPS 方式拉取仓库时，出现如下错误信息：
`fatal: unable to access 'XXX': HTTP/2 stream 1 was not closed cleanly before end of the underlying stream`

### 2. 问题原因记录

HTTP/2 与 HTTP/1.1 的一个核心区别在于：

- HTTP/2 基于 SPDY
- 目标是 **只使用一个连接来提升性能**

在某些网络环境或代理配置下，Git 的 HTTP/2 实现可能会导致连接异常，从而触发该错误。

### 3. 解决方式记录

有两种可行方案：

**方案一：改用 SSH 方式 clone**
直接使用 `git@github.com:xxx/xxx.git` 的形式进行拉取，绕过 HTTP 协议层问题。

**方案二：在 Git 中禁用 HTTP/2**
通过 Git 全局配置，将 HTTP 协议版本降级为 HTTP/1.1：
`git config --global http.version HTTP/1.1`

## 二、SSH 方式 git clone 时报 negotiation / 连接异常
### 1. 常见现象
在使用 SSH 拉取仓库时，终端提示类似：
`negotiation with <IP> port <port>`
或者表现为无法完成密钥协商。

### 2. 排查方向：.ssh 目录结构
在 Windows 环境下，该问题通常与用户目录下的 `.ssh` 文件夹配置有关。重点检查路径：`C:\Users\<username>\.ssh`。需要确认以下文件是否存在：
- `config`（无扩展名）
- `id_rsa`
- `id_rsa.pub`
- `known_hosts`（可自动生成）

### 3. config 文件缺失的处理方式
如果 `.ssh` 目录中没有 `config` 文件，可以通过以下方式创建：
- 新建一个 txt 文件
- 另存为时选择“所有文件类型”
- 文件名输入 `.config.`（再确认实际文件名为 `config`）

config 文件内容记录如下：
```text
Host *  
HostkeyAlgorithms +ssh-rsa
PubkeyAcceptedKeyTypes +ssh-rsa
```

### 4. SSH 公钥未配置的情况
如果 Git 提示 SSH 公钥未配置，需要进行以下步骤：
1. 在本地生成密钥对  
   使用命令 `ssh-keygen -t rsa -C "邮箱名"`
2. 在 `.ssh` 目录下生成：
   - `id_rsa`
   - `id_rsa.pub`
3. 打开 `id_rsa.pub`，复制 **包含 ssh-rsa 前缀的完整内容**
4. 在 GitHub / GitLab 中：
   - 进入个人账户 → Settings → SSH Keys
   - Add SSH Key
   - 粘贴公钥内容（无需填写 title）

### 5. 一种“兜底”解决方式记录
如果已经配置了 `config` 和 SSH key，但仍然无法 clone：
- 可以尝试删除已有公钥配置
- 直接执行 `git clone`
- 按提示输入 username 和 email
  
这种方式本质上是绕过了 SSH 配置未生效的问题，尤其在 `config` 文件实际命名不正确（如带点）时有效。


## 三、git lfs install 报错记录
### 1. 报错信息
在执行 `git lfs install` 时，出现错误：
`Error: Failed to call git rev-parse --git-dir: exit status 128`

### 2. 原因分析记录
该错误通常由两个原因之一导致：
- Git LFS 本身未正确安装
- 当前所在目录 **不是一个 Git 仓库**

### 3. 排查与解决步骤

**Step 1：确认 Git LFS 是否安装**
使用命令 `git lfs version` 查看是否能正常输出版本信息。

**Step 2：确认当前目录是否为 Git 仓库**
执行命令 `git rev-parse --git-dir`
- 如果返回 `.git`，说明当前目录是仓库
- 如果报错，说明当前目录不是 Git 仓库

**Step 3：初始化仓库**
若不是 Git 仓库，可先执行 `git init`，再执行 `git lfs install`

## 四、Ubuntu / Linux 环境下的 git clone 记录
### 1. 与 Windows 的主要区别
在 Ubuntu 环境中，SSH 配置路径为：`~/.ssh`，其结构与 Windows 基本一致。

### 2. 需要确认的文件
在 `~/.ssh` 目录下应存在：
- `config`
- `id_rsa`
- `id_rsa.pub`
- `known_hosts`
若不存在 `id_rsa.pub`，同样使用 `ssh-keygen` 生成。

### 3. 添加 SSH 公钥到 GitHub
在 Ubuntu 中，可以直接终端输入：`cat ~/.ssh/id_rsa.pub`
复制输出内容后，添加到 GitHub 账户：`Settings → SSH Keys`


## 小结
这次记录的 Git 问题，大多不是命令本身错误，而是：
- 协议选择不当（HTTP/2）
- SSH 配置文件细节问题（config / 文件名）
- 对 Git 仓库状态的前置条件理解不足
将这些零散问题记录下来，有助于在更换环境或新设备时快速恢复开发状态。
