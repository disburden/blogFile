---
layout: post
title: "ios开发选择系列(5):用nil还是用Nil或者NULL"
date: 2014-02-14 09:50:00 +0800
comments: true
categories: objc
---

很简单,如果你是要判断(或者赋值)一个实例是否存在就用nil,如果你是要判断一个类是否存在就用Nil,如果你要判断一个基本数据类型,比如 
```
intint *pointerToInt = NULL;   
charchar *pointerToChar = NULL;   
struct TreeNode *rootNode = NULL; 
```
为空就用NULL

基本区别如下
nil:表示一个不存在的对象指针(指针不存在)
Nil:nil的表哥,一个指向0的指针(是个实实在在的指针)
NULL:是一个通用指针(泛型指针)
NSNull:这个是货真价实的对象,他是实际存在的,而不是0,由于他是个对象,所以你可以当做他是nil的封装(就好想NSNumber 是int那些类型的封装一样),更直接点立即就是0的封装


很多时候他们可以通用的,有区别的时候很少,举个栗子说明一下区别的地方
null常常用来表示一个空的对象,而nil则经常用做结束标志,比如在数组中经常用nil作为结尾,那么如果你有特殊需求的,确实需要在数组中插入一个空对象怎么办?放了nil数组就会结束了,解决办法就是把nil封装成一个对象放到数组中,前面提到了NSNull就是解决这个问题的.
