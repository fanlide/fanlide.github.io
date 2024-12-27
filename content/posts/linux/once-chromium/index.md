---
title: 构造个开机即打开网页的 Linux
description: 从没有桌面环境的 Linux 开始
date: 2024-12-27T11:59:06+08:00
comments: true
draft: true
tags:
  - Chrome
  - Linux
categories:
  - Linux
---

## 系统 安装及配置

### 系统安装

在系统选择上，决定 Ubuntu Server， 版本是 24.04.1 LTS。系统安装上没有太多说的，有很多教程，也很简单

### 安装 xrog 和 i3-wm

```bash
sudo apt install xorg i3-wm
```

### 自动启动 Xorg 和窗口管理器

编辑 `.bashrc`文件，在文件的末尾添加以下行：

```bash
if [ -z "$DISPLAY" ] && [ "$(tty)" = "/dev/tty1" ]; then
    startx
fi
```

这会在你登录后，自动启动 Xorg 和窗口管理器。该脚本检查当前是否在 tty1 控制台（默认终端）上

### 配置自动登录

编辑 `/etc/systemd/system/getty.target.wants/getty@tty1.service` 文件，将 `ExecStart` 行修改为：

```bash
ExecStart=-/sbin/agetty --autologin <your_username> --noclear %I $TERM
```

其中：

- <your_username>：替换为你想自动登录的用户名。

## i3 配置

### 禁用 i3bar 状态栏

编辑 `~/.config/i3/config`，将如下行注释掉：

```bash
# bar {
#     status_command i3status
# }
```

### 隐藏鼠标和禁用熄屏锁屏

安装 `unclutter`：

```bash
sudo apt install unclutter
```

编辑 `~/.config/i3/config`，添加如下行：

```bash
exec --no-startup-id unclutter -root # 隐藏鼠标
exec --no-startup-id xset -dpms # 关闭屏幕自动关闭
exec --no-startup-id xset s off # 关闭屏幕保护
```

### 安装中文字体和 emoji 字体

```bash
sudo apt install fonts-noto-cjk fonts-noto-color-emoji
```

## 浏览器 安装及配置

### 安装浏览器

```bash
sudo add-apt-repository ppa:xtradeb/apps
sudo apt update
sudo apt install ungoogled-chromium
```

### 设置默认启动页面

编辑 `~/.config/i3/config`，添加如下行：

```bash
exec --no-startup-id ungoogled-chromium --kiosk --disable-extensions --disable-translate --app=<your_url>
```
