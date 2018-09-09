---
title: 简历
comments: false
---
# 个人信息
- 苗高鹏
- 1997.07.28
- 学历:中专
- 电话: 13019635349
- 邮箱: miaogaopeng123@gmail.com
- 博客: https://hooklife.me
- Github: https://github.com/hooklife
----
# 技能
## 通用
- 有非常丰富的面向对象、MVC、事件、异步、响应式的实践经验 
- 熟悉 HTTP 协议，有丰富的前后端分离开发经验，并且有丰富的接口对接、设计经验
- 对接过 支付宝、微信、连连支付等第三方接口
- 开发过大量微信程序
## PHP
- 熟悉 PHP5、PHP7 语法以及 PSR 标准
- 熟练掌握 Laravel，ThinkPHP 框架，并大概了解其原理可快速上手其他框架
- 对 swoole 有简单了解
- 对 PHP、PHP-FPM 原理有一定了解, 日常开发都很注意安全与提高性能的细节
## Linux 运维
- 能够熟练使用命令行完成各种日常任务。
- 熟悉计划任务、服务、进程等的管理，能够熟练搭建配置 PHP 环境
## MySQL
- 能针对 SQL、数据库进行性能调优
- 了解 MySQL 优化, 能根据业务选择合适的引擎、索引
- 能根据机器配置对 MySQL 参数进行调优
## Docker
- 熟练使用 Docker 作为日常开发工具，通过 docker-compose 能快速在本地/线上搭建各种开发/测试环境。
- 熟练编写 Dockerfile 能将各种服务镜像化便于开发、测试、部署。
## 前端
- 了解 es6 webpack vuejs gulp 等前端技术并能编写简单应用
- 能够编写语义化的 HTML、CSS，实现较为复杂的布局
- 熟悉 jQuery / Bootstrap 的使用与扩展
----
# 工作经历
### 沈阳云端科技有限公司(2014.6~2015.8)
	PHP程序员
	互联网/电子商务
	工作描述：担任技术主管，负责网站后台/api部分开发
### 沈阳中普互联网金融(2016.5~至今)
    PHP程序员
    互联网/互联网金融
    工作描述：负责公司P2P借贷项目API/微信部分编写
----
# 项目经历
## 1.赚个房API接口项目编写
- 使用laravel框架编写的 RESTful 接口，使用 JWT 进行权限控制
- 使用 redis 自旋锁解决高并发标的超卖问题
- 使用Laravel Event 进行异步处理耗时任务，使用 Laravel Scheduling 进行定时对账与同步各套系统数据
- 使用 openresty + ELK 对用户请求进行记录并友好展示，使用 elastalert + 钉钉机器人 对接口进行监控与报警
- 并通过日志/火焰图 对性能优化，通过 MySQL 慢查日志对 SQL 语句与数据库结构优化，并尝试使用 Swoole 优化性能
- 使用openresty 对网站进行限流，防止用户突然涌入导致系统出现问题
- 使用 golang 编写监控 MySQL binlog 进而进行对程序的缓存进行更新
- 使用 docker 进行部署,使用 confd 进行动态更新配置。
## 2.赚个房业绩系统编写
- 使用 golang 编写监控 MySQL binlog 的程序，并把数据库变化实时通知到 rabbitmq
- 使用 PHP 订阅 rabbitmq 对三个 Mysql 数据库的数据进行实时的同步/合并
- 使用 docker 进行部署。
## 3.通知中心编写
- 使用 golang + grpc 编写通知中心服务,主要用来发送短信/邮件/微信的通知，是赚个房项目微服务化的第一步。
## 4.华晨宝马投票项目编写
- 使用laravel框架编写，投票时使用mysql事务保证数据一致性，防止并发过高导致单账号多次投票问题
- 分析慢查询日志，对mysql慢查询进行索引、语句层面优化，对排行进行缓存优化
- 为防止并发过高,照成各种问题，使用 nginx 进行限流处理。
## 3. 赚个房微信项目编写
- 使用 Vue + mintui 编写的手机端SPA页面
- 使用 vuex 存储用户信息
- 使用 axios 调用 API 接口。
## 4.年会抽奖系统编写
- 使用laravel框架，预先生成中奖信息插入 redis，静态资源与页面全部扔到CDN上，抽奖时直接进行 redis 读取是否中奖
- 用户信息、日志全部使用redis做缓存，使用队列插入到mysql。单机能承受 1000+ 并发。

# 业余作品 (https://gihub.com/hooklife)
## 1.thinkphp5-wechat
	描述:微信 SDK for thinkphp5, 基于 overtrue/wechat
## 2.斗鱼火箭监视自动抢鱼丸
	描述:chrome扩展 自动监视斗鱼火箭，自动抢宝箱鱼丸，拥有上万（http://www.52pojie.cn/thread-471893-1-1.html）使用者
## 3.dongtu-function
	描述: 使用函数云计算调用 FFMPEG,自动生成动图, 并上传到 Upyun 对象存储. https://github.com/hooklife/dongtu-function 
## 4.pubg_recoil
	描述: 使用 golang 编写的 PUBG 自动压枪辅助