---
layout: post
title: "ios开发选择系列-2:加载图片的时候用imagNamed方法还是用imageWithContentsOfFile方法"
date: 2014-02-14 09:41:56 +0800
comments: true
categories: objc
tags: [ios开发选择系列,加载图片,原创]
---

这两图片都可以加载图片,各自的优缺点如下imageNamed:这种方式的话图片会被缓存,所以加载速度快,但是占内存多imageWithContentsOfFile:只有需要的时候才会从bundle中加载,所以加载速度较慢,但是节省内存所以可以很清楚的做出判断,如果图片经常要用到的话就用imageNamed,如果图片较大,用的次数相对较少就可以用imageWithContentsOfFile加载了.

### 原创文章,转载请注明出处,谢谢! ###