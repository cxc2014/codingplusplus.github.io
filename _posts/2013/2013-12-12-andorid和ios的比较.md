---
layout: post
title: Android和ios的一些比较
categories:
- Programming
tags:
- ios
---

一个xib文件里面可以放多个UIViewController，而每个UIViewController可以成为主UIViewController的一个property，这样就省去了主动创建的麻烦。
android里面的做法是在FragmentActivity的xml描述里面包含Fragment。

ios的方式对开发者更加友好，它可以实现一个.m文件对应多个xib，而且不用写代码，直接使用可视化的方式查看，.m文件只管纯粹的逻辑，是mvc里面的c，xib文件负责外观，是mvc里面的v。
android就麻烦一些，要达到同样地效果，必须要写代码，得由FragmentActivity传值给Fragment，fragment再setContentView，完全没有ios在xib里面拖拖拽拽方便和直观。
