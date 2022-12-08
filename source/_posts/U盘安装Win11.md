---
title: U盘安装Win11
date: 2022-12-08 10:33:32
tags: win11
---

## 跳过TPM检测

进入Win11安装界面

`Shift+F10` 打开CMD,输入 `regedit` 打开注册表管理器

找到`HKEY_LOCAL_MACHINE` > `SYSTEM` > `Setup` 并新建项 `LabConfig`

在新建的项内新建两个32位的值BypassTPMCheck和BypassSecureBootCheck
键值都填00000001

关闭注册表编辑器和CMD，即可继续安装

## 关闭连接网络

`Shift+F10` 打开CMD,输入 `taskmgr` 打开任务管理器

结束任务 `强制网络连接流` 和 `网络连接流`

