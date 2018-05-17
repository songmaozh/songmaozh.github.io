---
title: css小记录
date: 2017-01-18 16:12:07
tags:
---
div设置背景图片问题：
必须设置高度，否则高度为0，背景图片显示不出来
background-size：100%，不生效，不能控制图片的按比例缩放
background: url("../images/part_1.png") no-repeat center top;

img元素的边框问题：
若未设置src或src文件找不到，则会出现灰色边框
background-size: 100%;设置后可按比例缩放，无需设置高度
若强制设置高度，则可能变形。
原因：CSS不能控制图片的大小，当然也不能在css中指定src
