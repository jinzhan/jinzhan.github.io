---
title: 使用jquery来绘制气泡效果
date: 2014-09-23 13:44:15
tags:
description: 使用jquery来绘制气泡效果
---

<div id="y4-char" style="width: 200px;height: 200px;margin: 100px auto;position: relative;"></div>

<script src="http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>

<script type="text/javascript" src="https://rawgit.com/jinzhan/2471a61832609c828101/raw/c4a02ecf0ec430577892a211566925c7f3716a8b/bubble.js"></script>

<script type="text/javascript">
	//生成气泡
var makeBubble = function(){
    new Bubble("#y4-char",{
        a:0.015,
        b:0.2,
        c:100,
        color:"#f8e8e8"
    });

    setTimeout(function(){
        new Bubble("#y4-char",{
            a:0.01,
            b:0.2,
            c:100,
            color:"#ffee52"
        });
    },600);



    setTimeout(function(){
        new Bubble("#y4-char",{
            a:-0.01,
            b:0.2,
            c:150,
            color:"#fffacb"
        });
    },1200);

    setTimeout(function(){
        new Bubble("#y4-char",
            {
                a:0.015,
                b:0.2,
                c:100,
                color:"#288911"
            });
    },1800);

    setTimeout(function(){
        new Bubble("#y4-char",{

            a:-0.01,
            b:0.25,
            c:150,
            color:"#e7d2e6"
        });
    },2400);


   setTimeout(
   	function(){
   	new Bubble("#y4-char",{
        a:0.015,
        b:0.2,
        c:200,
        d: 50,
        color:"#f8e8e8"
    })	
   },200
    )

    setTimeout(function(){
        new Bubble("#y4-char",{
            a:0.01,
            b:0.2,
            c:300,
            d: 100,
            color:"#ffee52"
        });
    },400);



    setTimeout(function(){
        new Bubble("#y4-char",{
            a:-0.01,
            b:0.2,
            c:450,
            d: 30,
            color:"#fffacb"
        });
    },1000);

    setTimeout(function(){
        new Bubble("#y4-char",
            {
                a:0.015,
                b:0.2,
                c:-100,
                d: 40,
                color:"#288911"
            });
    },1500);

    setTimeout(function(){
        new Bubble("#y4-char",{

            a:-0.01,
            b:0.25,
            c:-150,
            d: 60,
            color:"#e7d2e6"
        });
    },2000);

    //递归调用
    setTimeout(makeBubble,6000);
};


makeBubble();
</script>


* 相关代码

<script src="https://gist.github.com/jinzhan/2471a61832609c828101.js"></script>

