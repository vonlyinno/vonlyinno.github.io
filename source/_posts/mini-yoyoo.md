---
title: mini-yoyoo
date: 2018-08-17 17:17:02
tags: 小程序
categories: 总结
---

封培8天的时间，速成了小程序，只是入门，总结一下遇到的坑，后续完善。

### 1. 微信小程序没有会话的机制
  我们在做邮箱验证的过程中，是后台采用session存储验证码，但是调试过程中session一直为空，且每次请求所携带的sessionid不同。百度之后发现：在微信小程序开发中，由wx.request()发起的每次请求对于服务端来说都是不同的一次会话，微信小程序不会把session信息带回服务端，即对应服务端不同的session，由于项目中使用session保存用户信息所以导致后续请求相当于未登录的情况。

### 2. 微信上传文件
  微信是用api wx.uploadFile, 但我们开发过程中没有申请域名，也不支持https，而这个api是会发起https请求，因此文件一直上传不了。
  所幸我们要上传的是图片，最终是将图片转成base64位编码，将编码作为字符串上传。
  此处使用了[wx-image-uploader](https://github.com/xuweilu/wx-image-uploader)
  原理大概就是: 将获取到的图片绘制到canvas上，再通过微信的wx.canvasGetImageData,获取canvas数据信息，这里获取到的是buffer，利用upng.js进行编码，输出base64编码。

### 3. redirectTo & navigateTo
  在注册成功后判断登录态，如果登录过就重定向至主页，但是redirectTo在跳转时会有比较长时间的空白，是因为redirectTo会先关闭当前页面，再建立新的页面；而navigateTo则会保留当前页面。
