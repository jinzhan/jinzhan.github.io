---
title: 使用js制作六边形边框
date: 2016-02-15 16:58:02
description: 该网站的logo是用svg制作而成，svg的优点是矢量图形可伸缩，这里我使用js生成一个正六边形，增加一些动画效果，让它更加生动。
tags:
- javascript
---

<div class="hexagon-container" style="width: 300px;height: 300px;margin: 0 auto;"></div>

<div style="text-align:center;font-size: 12px;padding: 20px;">最终效果</div>

<script src="http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>

<script type="text/javascript" src="http://rawgit.com/jinzhan/31458be6c083ea3cd8f6/raw/098a446e4c7cf44ca0874fd69cb4c342545f4fce/hexagon.js"></script>

<script type="text/javascript">
	$(function(){
		new Hexagon('.hexagon-container', {
	        width: 300,
	        height: 300,
	        transitionDuration: 0.5
	    });
	});
</script>


## 背景

灵感来自于这个网站LOGO图案：http://www.colorhexa.com/: 
![colorhexa logo](http://www.colorhexa.com/static/i/logo.min.svg)

## 关于clip-path
 该网站的logo是用svg制作而成，svg的优点是矢量图形可伸缩，这里我想使用js生成一个正六边形，增加一些动画效果，让它更加生动。生成不规则的图形一定要用svg和cavnas吗？其实通过css3提供的clip-path，就能胜任这个效果，基本思路：
 1. 这个正六边形的边，恰好是由30个等边三角形构成；
 2. 假设三角形的边长为`n`；则六边形的高度为：`n*6`，宽度为`n*sin60*6`；
 3. 整体用百分比，以便自适应宽高；
 

### clip-path Syntax
 ```
/* Keyword values */
clip-path: none;
clip-path: fill-box;
clip-path: stroke-box;
clip-path: view-box;

/* Image values */
clip-path: url(resources.svg#c1);
clip-path: linear-gradient(blue, transparent);

/* Geometry values */
clip-path: inset(100px 50px);
clip-path: circle(50px at 0 100px);
clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);

/* Global values */
clip-path: inherit;
clip-path: initial;
clip-path: unset;
```

## 完整代码
[https://gist.github.com/jinzhan/31458be6c083ea3cd8f6](https://gist.github.com/jinzhan/31458be6c083ea3cd8f6)

## 兼容性问题
在该图案上添加了CSS动画的transition后，在chrome下面表现良好，但是在Safari及移动端失效。经追查，CSS3的transition中的clip-path并不能被safari很好的支持了。这个就只能通过js来实现动画效果了。
