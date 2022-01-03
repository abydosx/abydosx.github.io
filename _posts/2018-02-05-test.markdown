---
layout: post
title: 基于 Xray-core 的 Xray VLESS TCP XTLS 一键脚本 转载
date: 2021-12-01 15:13:18 +0800
image:
tags: [Xray，代理]
categories: 网络
---

### 本文使用的一键脚本，引用自 GitHub mack-a 大佬，谢谢大佬的分享： [GitHub 地址](https://github.com/mack-a/v2ray-agent)

```
Project X GitHub:  点击跳转

Project X 官网： 点击跳转

Xray-core 介绍： 点击跳转

与 v2ray-core v4.32.1、v4.33.0 的一些 测试与对比（点击跳转）

关于 XTLS 的介绍，请看： 点我

关于 Xray 的白话文，请看： 点我
```

简单一句话：XRay 是 v2ray 的[超集](https://www.google.com/search?safe=active&sxsrf=ALeKk00DfdiOMElnKP6iIO25LOpK6Vgx4A%3A1606441165081&ei=zVjAX4O9BJOJoAT3mobgBQ&q=超集&oq=超集&gs_lcp=CgZwc3ktYWIQAzIECCMQJzICCAAyAggAMgIIADICCAAyAggAMgIIADICCAAyAggAMgIIADoICAAQsQMQgwE6BQgAELEDOgQIABBDOgoIABCxAxCDARBDOhAIABCxAxCDARCxAxCDARBDOg4IABCxAxCDARCxAxCDAToHCAAQsQMQDDoICAAQsQMQsQM6BAgAEAxQ-9aLAljDuIwCYOa7jAJoAHAAeACAAYkBiAHWB5IBAzAuOJgBAKABAaoBB2d3cy13aXrAAQE&sclient=psy-ab&ved=0ahUKEwjD1_zXy6HtAhWTBIgKHXeNAVwQ4dUDCA0&uact=5)、性能更优、速度更快、更新迭代快；

> 引用 rprx 大佬的描述：Xray 是 v2ray 的超集，含更好的整体性能和 XTLS 等一系列增强，且完全兼容 v2ray-core 的功能及配置 | “配置兼容，整体更好”

### 如何使用？具体速度怎么样？直接点击：[观看视频教程](https://youtu.be/0q4mGuy3ZN4)

初步使用下来，最大的感受是：

> –延迟低
>
> –Xray-core 占用内存低，对配置低的 VPS 也友好
>
> –科学上网速度快（这里是指，油管测速和 speedtest 网站测速快。**使用 gcp 台湾的最低配置的 VPS**，在 500 Mbps 电信宽带下，油管测速瞬时曾经到 18W+（注意是瞬时）；Speedtest.net 下载速度稳定在 500 Mbps 上下，上传稳定在 60 Mbps
>
> –测速图片（xtls-rprx-direct 模式）
>
> ![img](https://www.jamesdailylife.com/wp-content/uploads/2020/11/Xray-vless-4.jpg)
>
> ![img](https://www.jamesdailylife.com/wp-content/uploads/2020/11/Xray-vless-3.jpg)

## 前期准备工作：

```
1. 一个 VPS

2. 一个新域名（可以去 freenom 免费注册）

3. Cloudflare 解析域名

4. VPS 自行关闭防火墙，也就是防火墙放行 80,443 端口： 点击跳转，如何设置谷歌云防火墙规则

每日的凌晨 1 点 30 分，Nginx 会自动重启以配合证书的签发定时任务进行；
在此期间，节点无法正常连接，预计持续时间为若干秒至两分钟
```

## 指令（支持 Debian 9+ / Centos7/Ubuntu）

```
Root 用户： sudo -i

此脚本可能需要安装：

Centos ：  yum install -y wget curl

Debian ：  apt install wget curl -y

Ubuntu：   apt-get install wget -y

弹出脚本菜单： vasma
```

### # 一键安装脚本：

```
wget -P /root -N --no-check-certificate "https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh" && chmod 700 /root/install.sh && /root/install.sh
```

#### 2021-6-26 更新**：**

> #### **1. 请务必使用新域名和新建一个 VPS 操作系统、防火墙开放 443 和 80 端口，否则有可能会出现 “提示 TLS 安装失败 请检查 ACME 日志”**
>
> \2. 如果还出现 “TLS 安装失败” 问题：[请点击这里 1 ](https://github.com/mack-a/v2ray-agent/issues/201) | [请点击这里 2](https://github.com/mack-a/v2ray-agent/issues/206)
>
> #### 3. 使用纯净系统安装，如使用其他脚本安装过，请重新 build 系统再安装

#### 2020-12-8 更新：

> #### 最近 GCP 封禁异常流量，不建议谷歌云用户搭建翻墙程序。
>
> #### 如果你是非得要用谷歌云去搭建节点，确保搭建好的节点能用后。
>
> #### 请一定要、一定要、一定要隔天再使用节点，不然，很有可能因为短时间内流量太大，谷歌云账号被封禁。

### # 脚本的选项

![img](https://www.jamesdailylife.com/wp-content/uploads/2020/11/Xray-vless-7.jpg)

### # 安装前必看

#### 1.如果你使用的谷歌云，请新建一个 VPS，再使用相应的脚本。

#### 2.根据自身的情况，成功安装相应的协议后，可以开启使用 BBR plus+FQ 进行加速（开启后，可能对科学上网的速度有比较大的提升），方法如下

> A. 进入 ROOT 用户，再次输入并运行一键安装脚本，弹出对话框后，输入 “11”（安装 BBR ）；弹出新的对话框后，输入 “1”（安装【推荐原版 BBR+FQ】）
>
> ![img](https://www.jamesdailylife.com/wp-content/uploads/2020/11/bbr-picture-1.jpg)
>
> B. 此时会弹出新的对话框，核对你现在 VPS 的内核版本为 “BBR 加速内核”（如果非谷歌云 VPS，内核版本有所不同），如下图。核对完成后，输入 “11”（使用 BBR+FQ 加速）即可。
>
> ![img](https://www.jamesdailylife.com/wp-content/uploads/2020/11/bbr-picture-2.jpg)
>
> C. 此时会弹出 “BBR+FQ 修改成功，重启生效！”；在新的对话框里，输入 “reboot”，就会重启 VPS 并启用刚刚 BBR 加速选项；待 VPS 重启后，BBR 加速就完成了。
>
> ![img](https://www.jamesdailylife.com/wp-content/uploads/2020/11/bbr-picture-3.jpg)

\3. 使用非谷歌云 VPS，开启 BBR 加速的方法：[点击查看](https://www.jamesdailylife.com/bbr)

## 客户端—Windows | MacOS | 安卓手机 | 苹果手机 | Openwrt 软路由系统

### Qv2ray 客户端 | [点击观看 Qv2ray 客户端使用教程](https://youtu.be/yoEl16nfv-E)

#### 一定下载链接的对应的客户端，不然无法正常使用 Qv2ray 客户端

#### 一定下载链接的对应的客户端，不然无法正常使用 Qv2ray 客户端

#### 一定下载链接的对应的客户端，不然无法正常使用 Qv2ray 客户端

> [Windows 下载（64 位）](https://github.com/Qv2ray/Qv2ray/releases/download/v2.7.0-pre1/Qv2ray.v2.7.0-pre1.Windows-x64.7z)
>
> [Window 核心下载 (64 位)](https://github.com/XTLS/Xray-core/releases/download/v1.0.0/Xray-windows-64.zip)
>
> [MacOS 下载](https://github.com/Qv2ray/Qv2ray/releases/download/v2.7.0-pre1/Qv2ray.v2.7.0-pre1.macOS-x64.dmg)
>
> [MacOS 核心下载](https://github.com/XTLS/Xray-core/releases/download/v1.0.0/Xray-macos-64.zip)

#### Qv2ray 客户端 Xray VLESS TCP XTLS 节点的参考配置图

![img](https://www.jamesdailylife.com/wp-content/uploads/2020/11/xray-vless-2020.12.2.jpg)

> ### #如需使用以下协议，请更新相应的插件及插件核心，不更新可能会导致崩溃问题
>
> - Trojan 插件： https://github.com/Qv2ray/QvPlugin-Trojan/releases/tag/v3.0.0-pre3
> - SSR 插件： https://github.com/Qv2ray/QvPlugin-SSR/releases/tag/v3.0.0-pre3
> - NaiveProxy 插件： https://github.com/Qv2ray/QvPlugin-NaiveProxy/releases/tag/v3.0.0-pre3
> - Trojan-Go 插件: https://github.com/Qv2ray/QvPlugin-Trojan-Go/releases/tag/v3.0.0-pre3
> - SS 插件: https://github.com/Qv2ray/QvPlugin-SS/releases/tag/v3.0.0-pre3
> - Command: https://github.com/Qv2ray/QvPlugin-Command/releases/tag/v3.0.0-pre3
>
> ### **# TROJANGO 插件核心下载：**
>
> > [Windows（64 位）](https://github.com/p4gefau1t/trojan-go/releases/download/v0.8.2/trojan-go-windows-amd64.zip)
> >
> > [MacOS](https://github.com/p4gefau1t/trojan-go/releases/download/v0.8.2/trojan-go-darwin-amd64.zip)
> >
> > [Github 地址](https://github.com/p4gefau1t/trojan-go/releases)
>
> ### **# NaiveProxy 插件核心下载（插件路径设置方法，参照 Trojan-go 插件）:**
>
> > [Windows（64 位）](https://github.com/klzgrad/naiveproxy/releases/download/v86.0.4240.75-1/naiveproxy-v86.0.4240.75-1-win-x64.zip)
> >
> > [MacOS](https://github.com/klzgrad/naiveproxy/releases/download/v86.0.4240.75-1/naiveproxy-v86.0.4240.75-1-osx.tar.xz)
> >
> > [Github 地址](https://github.com/klzgrad/naiveproxy/releases)

###

### 安卓手机客户端：

> [V2rayNG](https://github.com/2dust/v2rayNG/releases/download/1.5.0/v2rayNG_1.5.0_arm64-v8a.apk) ( 支持 Vless XTLS )

### iOS 手机客户端：

```
最新版 Shadowrocket  (支持 Vless XTLS )

1.确认小火箭版本高于或者和下图一致（ 2.1.67 ）






2.按照下面界面配置
```

### **Openwrt 已支持/软路由系统**

x86 软路由 Openwrt 固件，含支持 Xray-core 的 passwall 插件：[点我下载](https://drive.google.com/file/d/1mAms5dMIFXvu9iNjRVu5ZihLSr8IQN9e/view)

原文地址：https://www.jamesdailylife.com/xray-core-xray-vless-tcp-xtls
