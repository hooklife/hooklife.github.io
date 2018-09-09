---
title: ubuntu中使用webpack,部分文件不能自动reload的解决方法
date: 2017-12-21 17:10:00
tags: [ubuntu,webpack]
category: 前端
---
在webstrome中开发,发现大部分文件是可以自动reload的,但是部分文件不能自动进行reload

<!--more-->

### 解决方法

命令行中执行命令

在你的系统中修改足够的 inotify watches,让webpack能正常监控文件变化 

```bash
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```

参考资料

https://stackoverflow.com/questions/26708205/webpack-watch-isnt-compiling-changed-files

https://webpack.github.io/docs/troubleshooting.html

