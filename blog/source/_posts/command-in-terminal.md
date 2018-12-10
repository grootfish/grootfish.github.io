---
title: 终端命令和快捷键
tags: [工具]
comments: true    // 是否开启评论
date: 2018-12-10 15:28:14
description: terminal
---

## 前言

笔者使用的是 Mac 电脑，终端使用的是 Iterm + zsh，本文只是对常用命令的整理，方便自己记忆，没有参考意义

## 常用命令

### 查看

- `ls`显示在存放在相应文件系统下的所有主要目录。

  ```bash
  ls /applications
  ls -a #显示所有隐藏的文件
  ```

- `cat` 查看文件内容
- `ps` 命令用于查看系统的进程状态

  ```bash
  ps -a
  ps -x
  # USER 所属用户
  # PID 进程ID
  # TTY 所属终端
  # VSZ 虚拟内存使用量(KB)
  # RSS 物理内存使用量(KB)
  # STAT 进程状态 R:运行  S:中断  D:不可中断 Z:僵死 T:停止
  ```

- `who` 命令用于查看当前用户信息
- `history` 用于显示运行过的命令，默认是记录最近的 1000 条命令
- `ifconfig` 命令用于查看网卡配置和网络状态信息

### 操作

- `cd` 用来跳转（或“更改”）到一个目录的

  ```bash
        cd Gym\ Playlist # 空格需要转译
  ```

- `mv`将文件从一个文件夹转移到另一个文件夹
- `mkdir` 创建目录或文件夹
- `rmdir` 删除目录或文件夹
- `rm` 删除文件
- `touch` 创建新的、空的文件
- `vi/vim` 编辑文件

### 权限

- sudo 命令用于以 root 管理员权限运行某命令

  ```bash
  sudo poweroff # 使用 root 权限执行 poweroff 命令
  su - # 切换到 root 账户
  sudo !! #以 root 权限运行上一条命令
  ```

- poweroff 用于关闭系统,reboot 用于重启系统

  ```bash
  poweroff #关闭系统

  reboot #重启系统·
  ```

### 输出

- echo 命令用于将字符串和变量输出到屏幕上。

  ```bash
  echo hi # 输出字符串
  echo $SHELL # 输出变量
  ```

- cal 命令用于输出一个日历

  ```bash
  cal # 输出这个月日历
  cal 2018 # 输出 2018 年日历
  cal 6 2018 # 输出 2018 年 6 月日历
  ```

- date 命令用于显示或设置系统的时间

  ```bash
  date # 显示当前日期
  date -s "20180428 7:15:00" # 设置指定日期
  date "+%j" # 显示当天是当年的第几天
  date "+%Y-%m-%d %H:%M:%S" # 以另一种格式显示当前日期
  ```

---

## iterm 快捷键

### 标签

- 新建标签：command + t
- 关闭标签：command + w
- 切换标签：command + 数字 1/2 command + 左右方向键
- 切换全屏：command + enter
- 查找：command + f

### 分屏

- 垂直分屏：command + d
- 水平分屏：command + shift + d
- 切换屏幕：command + option + 方向键 command + [ 或 command + ]
- 查看历史命令：command + ;
- 查看剪贴板历史：command + shift + h

### 其他

- 清除当前行：ctrl + u
- 到行首：ctrl + a
- 到行尾：ctrl + e
- 前进后退：ctrl + f/b (相当于左右方向键)
- 上一条命令：ctrl + p
- 搜索命令历史：ctrl + r
- 删除当前光标的字符：ctrl + d
- 删除光标之前的字符：ctrl + h
- 删除光标之前的单词：ctrl + w
- 删除到文本末尾：ctrl + k
- 交换光标处文本：ctrl + t
- 清屏 1：command + r
- 清屏 2：ctrl + l

