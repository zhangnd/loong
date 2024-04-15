# nvm管理node版本

## 概述

nvm（Node Version Manager）是 node.js 的版本管理器，可以让我们轻松地在不同的 node.js 版本之间进行切换。

## nvm-windows

下载地址：https://github.com/coreybutler/nvm-windows/releases

![](https://img.zhangniandong.com/2024/20231211091636.png)

卸载计算机已安装的 node.js

选择 `num-setup.exe` 下载安装

打开命令行，执行 `nvm -v` 可查看版本，即安装成功

执行 `nvm ls available` 查看可用的 node.js 版本

执行 `nvm ls` 查看已安装的 node.js 版本

执行 `nvm install [version]` 安装指定的 node.js 版本

执行 `nvm uninstall [version]` 卸载指定的 node.js 版本

执行 `nvm use [version]` 切换指定的 node.js 版本
