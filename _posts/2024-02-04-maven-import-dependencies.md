---
title: Maven 项目依赖导入完整流程
date: 2024-02-04 10:32:00 +0800
categories: [环境配置, Java]
tags: [Maven, 依赖管理, Java, pom.xml, 开发工具]
author: wyt
description: Maven项目中通过pom.xml文件导入依赖
---

> 本文介绍如何在 Maven 项目中通过 `pom.xml` 文件引入依赖包，包括版本声明、依赖声明、IDE 配置和常见问题处理。

## 1. 依赖声明结构概述

在 `pom.xml` 中使用依赖主要涉及两个核心部分：

- `<dependencyManagement>`：用于**统一声明依赖版本**；
- `<dependencies>`：用于**实际引入依赖**；

为了更好管理版本号，建议将版本信息写入 `<properties>` 标签，并以变量形式调用。

## 2. 示例：引入 `javax.servlet-api`

以下是完整的依赖声明过程。

### Step 1: 在 `<properties>` 中声明版本号
```xml
<properties>
  <javax.servlet-api.version>4.0.0</javax.servlet-api.version>
</properties>
```

### Step 2: 在 `<dependencyManagement>` 中声明依赖
```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>${javax.servlet-api.version}</version>
    </dependency>
  </dependencies>
</dependencyManagement>
```

### Step 3: 在 `<dependencies>` 中实际引入依赖
```xml
<dependencies>
  <dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <scope>provided</scope>
  </dependency>
</dependencies>
```
`<scope>` 为 provided 意味着该依赖只在编译时有效，部署时服务器会提供对应版本，避免冲突。

## 3. 使用IDE导入依赖
使用Maven Build按如下操作：
1. 打开 IDEA 右侧边框的 Maven 面板
2. 在 Lifecycle 下找到 clean
3. 右键点击 Run Maven Build, 完成后即可导入依赖
4. 若 settings.xml 中已配置本地仓库local repository，可在对应本地文件夹路径下看到依赖被成功下载

**注意**：Maven 导入依赖有一定延迟，执行完毕后建议重新进入 IDE确保依赖生效。

## 4. 手动添加依赖（解决依赖未生效问题）
当导入依赖后，可以在右侧边框的maven的`dependencies`下看到自己导入的依赖，但此时若在源码中依然无法使用依赖，说明导入失效，需要手工添加，即当在 `dependencies` 中已经声明，但依然无法在源码中使用相关 API，可按以下步骤手动添加：
1. 打开：File -> Project Structure -> Libraries
2. 点击左上角 + → 选择 From Maven
3. 输入关键词搜索dependency依赖（推荐使用 artifactId 名称,最好按照pom.xml中`<properties>`字段所加的artifactId搜索）
4. 选择版本并点击添加

添加成功后即可在项目中正常使用依赖的 API 接口。
