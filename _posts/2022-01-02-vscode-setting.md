---
layout: post
title: VSCode 配置
date: 2022-1-1 11:23:21 +0800
image: posts/vscode.png
image-mode: 'background-size: 100% 100%;background-repeat: no-repeat'
tags: [vscode]
categories: 工具
---

​ 打开设置，可通过 UI 或 json 编辑：

- Windows/Linux - **File** > **Preferences** > **Settings**

- macOS - **Code** > **Preferences** > **Settings**

​ vscode 有两个配置模式，工作区 setting 将被优先使用

- User Settings - 全局配置，适用于所有的打开的实例
- Workspace Settings - 储存在工作区之下并仅适用于本工作区的配置

##### 一、通用配置

- terminal: 终端配置
- editor: 编译器相关设置
- files: 打开文件配置
- git: git 相关配置
- search: 搜索设置
- prettier：代码美化
- 可以通过安装插件进行更多语言、库的配置

##### 二、文件名匹配

- / 匹配除/之外的任意字符

* \*\* 匹配任意字符串
* ? 匹配任意单个字符
* [name] 匹配 name 字符
* [!name] 不匹配 name 字符
* [s1,s2,s3] 匹配给定的字符串
* [num1..num2] 匹配 num1 到 mun2 直接的整数

##### 三、常用插件

- ESLint: javascript 代码格式化
- Material Icon Theme: 文件图标
- TODO Highlight: 高亮显示 TODO
- Bracket Pair Colorizer: 括号彩色配对
- Code Spell Checker: 单词拼写检查
- 持续更新中...
