---
title: Deepin配置使用ibus输入引擎
date: 2019-07-16
image: D736D06C-7B39-4813-A96D-027E2A5F8D2D.png
tags:
  - Linux
categories:
  - Linux
  - Deepin
  - ibus
---

### 1、安装

`sudo apt install -y ibus ibus-gtk ibus-gtk3 ibus-qt4`

### 2、使用 im-config 切换输入法引擎

![](810DAEA8-E19F-4186-A9BE-0BA8FC6792A4.png)

直接一路确定、yes

![](2C8C3069-1C1F-430E-9C47-71D1399D6601.png)

到这一步选择 ibus，再一路确定。就搞定了

### 常见问题：

旧版本 im-config 因为存在两个 ibus 配置文件，所以 ibus 读取不到。可到`/usr/share/im-config/data/`目录下删除 23_ibus 的两个文件。再次启动 im-config 就可以了
