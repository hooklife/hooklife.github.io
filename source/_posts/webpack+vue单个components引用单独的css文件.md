---
title: webpack+vue单个components引用单独的css文件
date: 2016-10-21 17:10:00
tags: [webpack,vue]
category: 前端
---
因为多个componets中css定义有冲突

所以每个componets必须要单独加载他自己的css
<!--more-->

直接 improt 的话 webpack 就会把所有的css样式生成一个大的css，由于当初编写css命名不是很好，很多classname都是重复的，然后页面就会各种错乱。

找了半天也没找到compoents 动态加载css的方法

但是找到了一个算是比较好的办法

在componets 中 使用这种形式去引用css

```css
<style scoped src="xxx.css"></style>
```

webpack打包的时候会在 css 中自动加上当前components的前缀 就能完美解决各个components css冲突的问题啦


参考资料（我本人的提问）：

https://segmentfault.com/q/1010000007093036

https://www.v2ex.com/t/311109