---
title: supervisor在deepin安装、使用与卸载
date: 2017-11-14 11:00:00
tags: [supervisor,ubuntu,deepin]
category: linux
---
# 简介
Supervisor是一个python开发的进程管理工具，你可以使用它对  UNIX-like 操作系统的多个进程进行控制和监控。

<!--more-->
# 安装
```shell
sudo apt-get install supervisor
```
# 使用
## 1.配置
创建一个配置文件在/etc/supervisor/conf.d 目录下，文件命名必须以.conf结尾。例如: idea-worker.conf
内容大致如下
```
[program:idea-worker]
command=/root/IntelliJIDEALicenseServer_linux_amd64 //启动命令
autostart=true // 在 supervisord 启动的时候也自动启动，如果设置为false 重启服务器后还需要手动执行命令进行运行
autorestart=true // 程序异常退出后自动重启 设置为false supervisord 将不会在程序关闭后重新运行、设置
stderr_logfile=/var/log/idea-worker.err.log // 错误日志
stdout_logfile=/var/log/idea-worker.out.log // 所有日志
```


**autostart** 
 在 supervisord 启动的时候也自动启动
 false：重启服务器后还需要手动执行命令进行运行
 true： 重启服务器后自动运行

**autorestart**
程序异常退出后自动重启
false：supervisord 将不会在程序关闭后重新运行
true：supervisord 将会在程序关闭后重新运行
unexpected：supervisord 将在程序报异常的时候进行重启

**stderr_logfile、stdout_logfile**
主要定义程序运行日志位置，指定的目录必须在启动程序前存在，supervisord不会创建目录，只会创建对应的文件


文档列出很多可以进行配置的选项，可以查阅官方手册 [configuration](http://supervisord.org/configuration.html#program-x-section-settings)

## 2.reload并应用配置
```shell
supervisorctl reread
supervisorctl update
```

## 3.管理程序
```shell
supervisorctl 
```
执行后进入supervisorctl界面，就可以对程序进行执行任务了
```shell
start idea-worker
stop idea-worker
restart idea-worker
status
quit
```
## 4.使用web界面管理程序
supervisor 提供了通过浏览器来管理程序的方法。
开启方法：
```shell
vi /etc/supervisor/supervisord.conf
```
添加这几行文字，
```
[inet_http_server]         ; inet (TCP) server disabled by default
port=0.0.0.0:9001          ; (ip_address:port specifier, *:port for ;all iface)
username=user              ; (default is no username (open server))
password=123               ; (default is no password (open server))
```
打开 http://ip:9001  输入用户名密码，即可进入程序管理web界面
![webui](/img/supervistor_deepin_install/web_ui.png)


**默认外网可以进行访问，请自行修改账号密码**
### 参考资料: 
* [How To Install and Manage Supervisor on Ubuntu and Debian VPS](https://www.digitalocean.com/community/tutorials/how-to-install-and-manage-supervisor-on-ubuntu-and-debian-vps)
* [使用 supervisor 管理进程](http://liyangliang.me/posts/2015/06/using-supervisor/)
* [supervisor 安装配置使用](https://laravel-china.org/topics/2126/supervisor-installation-configuration-use)

# 卸载
```shell
sudo apt purge supervisor
```

执行命令后

supervisor 正常被卸载
supervisord 并没有被正常卸载

输入supervisord 命令后报错
```python
Traceback (most recent call last):
  File "/usr/local/bin/supervisord", line 6, in <module>
    from pkg_resources import load_entry_point
  File "/usr/lib/python2.7/dist-packages/pkg_resources/__init__.py", line 3019, in <module>
    @_call_aside
  File "/usr/lib/python2.7/dist-packages/pkg_resources/__init__.py", line 3003, in _call_aside
    f(*args, **kwargs)
  File "/usr/lib/python2.7/dist-packages/pkg_resources/__init__.py", line 3032, in _initialize_master_working_set
    working_set = WorkingSet._build_master()
  File "/usr/lib/python2.7/dist-packages/pkg_resources/__init__.py", line 655, in _build_master
    ws.require(__requires__)
  File "/usr/lib/python2.7/dist-packages/pkg_resources/__init__.py", line 963, in require
    needed = self.resolve(parse_requirements(requirements))
  File "/usr/lib/python2.7/dist-packages/pkg_resources/__init__.py", line 849, in resolve
    raise DistributionNotFound(req, requirers)
pkg_resources.DistributionNotFound: The 'supervisor==3.2.0' distribution was not found and is required by the application
```

执行 
```shell
whereis supervisord
```
查找supervisord在哪
```shell
root@jdu4e00u53f7:~# whereis supervisord
supervisord: /usr/local/bin/supervisord
```

执行
```
rm -rf /usr/local/bin/supervisord
```卸载成功

