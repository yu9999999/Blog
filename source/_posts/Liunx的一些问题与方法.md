---
title: Liunx的一些问题与方法
date: 2023-10-19
tags:
- liunx
categories:
- liunx
aside: false
---


## 1. Error: EACCES: permission denied保存修改wsl中文件出错，因为没有权限

``` bash
$ sudo chmod -R 777 /文件目录
```

## 2. 删除liunx文件夹

``` bash
$ rm -rf /文件夹目录（慎用-rf，因为不能恢复）
$ rm ./* （如果进入该文件夹，该命令为删除文件夹下所有文件，不包括文件夹）
```

## 3. 删除linux系统
``` bash
$ wsl --list（查看wsl列表）
$ wsl --unregister Ubuntu-20.04（删除Ubuntu-20.04）
```

## 4. linux解压压缩文件

``` bash
unzip ./wenjian.zip
```