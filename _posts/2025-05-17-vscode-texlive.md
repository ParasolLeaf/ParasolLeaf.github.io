---
title: Windows 下 TeX Live + VS Code 的 LaTeX 写作环境部署
date: 2025-05-17 22:54:00 +0800
categories: [环境配置, VSCode]
tags: [LaTeX, TeX Live, VSCode, 开发工具]
author: wyt
---

> 本文记录在Windows 系统下部署TeX Live + VS Code（LaTeX Workshop）的完整流程，主要面向论文写作、科研排版等使用场景。

---

## 一、本机安装 TeX Live
### 1. 下载 TeX Live 镜像文件
首先前往 TeX Live 官方下载页面：<https://tug.org/texlive/acquire-iso.html>
在页面中可以通过mirror list选择国内镜像源。
为了加快下载速度，推荐使用清华大学镜像：<https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/>

在该页面中下载对应版本的 `.iso` 安装镜像即可。

### 2. 启动安装程序
下载完成后，按以下流程操作：
- 找到下载好的 `.iso` 文件  
- 右键 → 打开方式 → Windows 资源管理器  
- 进入镜像目录，找到 `install-tl-windows`  
- 右键 → 以管理员身份运行

随后会进入 TeX Live 的图形化安装界面。

### 3. 安装选项说明

在安装过程中可以进行如下设置：
- 根据需要修改安装路径
- 建议取消勾选 Texworks 前端

由于后续主要使用 VS Code 进行 LaTeX 编辑，Texworks 并非必需。

### 4. 验证 TeX Live 是否安装成功

安装完成后，打开 `cmd`，输入：`xelatex -v`
如果能够正确显示 XeLaTeX 的版本信息，则说明 TeX Live 已成功安装。

## 二、VS Code 中配置 LaTeX 环境

### 1. 安装 LaTeX Workshop 插件

打开 VS Code，在扩展市场（Extensions）中搜索并安装插件：`LaTeX Workshop`。该插件提供 LaTeX 的编译、预览、自动补全和正反向同步等功能。

### 2. 打开 VS Code 设置文件（JSON）

进入设置界面的方式如下：

- 使用快捷键 `Ctrl + ,`
- 或点击左下角齿轮图标 → 设置

在设置页面右上角，点击 **打开设置（JSON）**，进入 `settings.json`。

### 3. LaTeX Workshop 主要配置说明

在 `settings.json` 中加入 LaTeX 相关配置，核心思路包括：

- 启用右键菜单：`latex-workshop.showContextMenu: true`
- 启用宏包级别的自动补全：`latex-workshop.intellisense.package.enabled: true`
- 关闭错误和警告弹窗提示，减少干扰
- 设置编译输出目录为 `./build`
- 配置常用编译工具：`xelatex`、`pdflatex`、`latexmk`、`bibtex`
- 配置多种编译链（recipes），例如  
  `xelatex → bibtex → xelatex → xelatex`
- 指定清理的辅助文件类型，如 `.aux`、`.log`、`.toc` 等
- 设置是否自动编译（如设为 `never`）
- 配置 PDF 反向同步方式（如双击定位源码）

这些配置可保证在写作过程中，参考文献、交叉引用和同步定位等功能稳定可用。


## 三、测试 LaTeX 编译流程

### 1. 获取测试项目

为了验证环境是否完整可用，可以从 GitHub 克隆一个示例仓库：`https://github.com/Ali-loner/Ali-loner.github.io.git`

### 2. 在 VS Code 中进行编译测试

操作步骤如下：

1. 使用 VS Code 打开仓库中的 `.tex` 文件  
2. 左侧栏切换到 `TEX` 视图  
3. 点击构建 LaTeX 项目
4. 选择编译方案：`xelatex + bibtex + xelatex`

如果能够正常生成 PDF 文件，说明：

- TeX Live 工作正常  
- VS Code 与 LaTeX Workshop 配置成功  
- 编译链和参考文献流程可用  

## 小结

至此，Windows 下的 LaTeX 写作环境已经完整部署完成，包括：

- TeX Live 本地发行版  
- VS Code + LaTeX Workshop 编辑与编译环境  
- 基于 XeLaTeX 的论文级排版流程  

该环境适合课程作业、毕业论文以及科研论文写作，后续也可以在此基础上扩展 `latexmk` 自动化构建或 Git 版本管理写作流程。
