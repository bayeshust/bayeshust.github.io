---
layout: post
title: "Windows更新后自动重启"
description: "Windows更新后自动重启"
rategory: Jekyll
tags: [jekyll, rankfile]
---

Windows更新后最多延迟三天就自动重启，今天吃亏了，系统留的十五分钟来不及保存工作。于是乎使用组策略

## 组策略

Win+R
gpedit.msc
计算机配置
管理模板
系统组件
Windows更新
对于已登录的用户，计划的自动更新安装不执行重新启动
已启用

## 修改注册表
Win+R
regedit
1. Launch the Registry Editor by hitting Windows + R, entering regedit into the dialog box and hitting Enter.
2. Navigate to HKEY_LOCAL_MACHINE/SOFTWARE/Policies/Microsoft/Windows.
3. Create a new registry key called WindowsUpdate under Windows if one does not already exist. You can create a new registry key by right clicking on the Windows key, selecting New then Key and renaming the folder which appears to WindowsUpdate.
4. Create a new registry key named AU under WindowsUpdate.
5. Create a DWORD Value named NoAutoRebootWithLoggedOnUsers in the AU Key. To create a new DWORD value, right click in the right window pane and select New then DWORD Value. Then rename the value as needed.
6. Set NoAutoRebootWithLoggedOnUsers to 1. You can set the value by double clicking on the DWORD name and entering the appropriate number in the dialog box which appears.
7. Close the Registry Editor and Reboot.