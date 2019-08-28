---
title: 成都 GDG 2019 IO Extended
date: 2019-05-26
categories: 经历
---

# 成都 GDG 2019 IO Extended

时隔了大半年，博客停更这么久，终于又开始了，这半年为了准备华为杯，做了很多UI方面的学习，也接触了人工智能部分知识，昨天参加了成都 GDG 组织的一个活动，收获不小，记录一下（图片来源现场拍摄，画质请谅解）。

## 活动流程

活动一共三个阶段，从13:30一直持续到17:30，分别重点介绍了Flutter，Machine Learning 和 web方面的技术（不是很懂web →_→,听着很强的样子）

### IO Extended

第一部分，邀请了 GDG 的毕强，跟大家回顾今年的Google IO 大会，他首先提到的是大会主题的变更。

前几年的IO大会，都以技术来确定大会主题，而这一次的主题变为来帮助每一人

[![Theme](https://s2.ax1x.com/2019/05/26/VVlSpt.jpg)](https://s2.ax1x.com/2019/05/26/VVlSpt.jpg)

Google开始用前些年的技术，来解决人们生活中知识，成功，健康，幸福的问题，更加切实地的贴近生活，而不仅仅是学术技巧了。

之后重点就是Flutter了，现在的Flutter已经到了1.5版本了，它的框架如下

[![flutter_1](https://s2.ax1x.com/2019/05/26/VVlg3t.jpg)](https://s2.ax1x.com/2019/05/26/VVlg3t.jpg)

这样的框架，构建一个虚拟机去完成跨平台的工作，让自身的渲染引擎去完成绘制，达到这样一个跨平台的UI框架。

除此之外，Flutter有了一个平台通信的新功能，不在仅仅只有UI方面的能力，还具备客户端和宿主之间传递消息的能力，但是在国内因为种种原因，GMS服务不能用，提出一种解决方案，用国内相关的SDK，再利用平台通信实现相关功能。

接着就是我最喜欢的一个技术—热重载

[![img](https://s2.ax1x.com/2019/05/26/VV3AWn.jpg)](https://s2.ax1x.com/2019/05/26/VV3AWn.jpg)

修改参数，立即渲染，大大减少了调试时间。

最后，简单介绍了Flutter的未来，让我看到强大野心的未来

[![img](https://s2.ax1x.com/2019/05/26/VV366P.jpg)](https://s2.ax1x.com/2019/05/26/VV366P.jpg)

Flutter，不再单单局限在手机端，它还要囊括Web，PC甚至还有芯片，都将可以通过Flutter快速构建UI界面。

For Web，目标是：

 1.达到60FPS的图形渲染；

 2.和其他平台一致的行为和视觉效果；

 3.高效的开发工具，兼容现有开发者模式；

 4.支持所有现代游戏的核心web功能。

For DeskTop，有了一个[实验项目](https://github.com/Google/flutter-desktop-embedding),目前在MacOs端适配效果最好。

For Engine，通过一个示例验证了在树莓派上的可能性。

最后附上阿里巴巴做的帮助性App demo ：[Flutter Go](https://github.com/alibaba/flutter-go)。

### Machine Learning

成都GDG邀请的Google Cloud 的员工 Markku，讲解了关于AutoML的一些东西，大体来说就是，通过Google 云计算快速完成人工智能的模型搭建，通过分布式的云计算，大大提高处理效率。最后做了一个识别麻将的演示。

[![Markku](https://s2.ax1x.com/2019/05/26/VVJ76I.jpg)](https://s2.ax1x.com/2019/05/26/VVJ76I.jpg)

总之，TensorFlow是一个必须要涉及的层面。

### Web

这里，成都GDG邀请到了Web/JavaScript全栈工程师，开源软件作者，freeCodeCamp成都社区第4任负责人，水歌，介绍了很多JavaScript新特性，以及Chrome新的支持。

## 总结

这次集会，让我认识了一位Android工作者，叫他龚哥，与他的交谈，了解到Android前景的一些问题，同时认识自己的局限性，决定拓展自己知识面，同时对Flutter有极大的兴趣，准备正式开始学习，最后以龚哥的一句话作为结束：不要局限自己，不要遇到了自己不会的，才后悔，来不及了。。。