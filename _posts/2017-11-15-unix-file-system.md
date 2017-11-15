---
layout:     post
title:      Unix file system
subtitle:   正则表达式的工作原理和应用实例
date:       2017-11-15
author:     zz
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - unix
    - filesystem 
    - tech
    - engineering 
---

## Unix - Filesystem Hierarchy Standard
```
- bin
- sbin
- home
- tmp
- lib
- usr  -> usr/bin  usr/sbin  usr/lib usr/local
- var
- etc
- dev
```
总结：
- /：存放系统程序，也就是At&t开发的Unix程序
- /usr：存放Unix系统商（比如IBM和HP）开发的程序
- /usr/local：存放用户自己安装的程序
- /opt：在某些系统，用于存放第三方厂商开发的程序，所以取名为option，意为"选装"。
