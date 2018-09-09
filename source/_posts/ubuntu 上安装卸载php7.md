---
title: ubuntu 上安装/卸载php7
date: 2016-10-17 17:29:00
tags: [php,ubuntu]
category: linux
---
## 添加PHP源
```shell
sudo apt-get install python-software-properties software-properties-common
sudo LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php
sudo apt-get update
```
## 安装PHP7和常用扩展
```shell
sudo apt-get install php7.0 php7.0-fpm php7.0-mysql -y
```
## 卸载php7
```shell
 sudo apt-get purge php7.0-common
```