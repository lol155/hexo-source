---
title: aws-vpn
categories: 工具
tags:
  - vpn
  - aws
toc: true
date: 2018-01-03 00:20:16
scaffolds:
---

# 1. aws服务器的创建及vpn的安装
https://www.jianshu.com/p/b0d460efca4e

# 2. 配置 IPsec/L2TP VPN 客户端
https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/docs/clients-zh.md#windows
## 2.1. 注册表修改
https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/docs/clients-zh.md#windows-错误-809 

windows10注册表修改,然后需要重启才能连上vpn 
```
REG ADD HKLM\SYSTEM\CurrentControlSet\Services\PolicyAgent /v AssumeUDPEncapsulationContextOnSendRule /t REG_DWORD /d 0x2 /f
```
