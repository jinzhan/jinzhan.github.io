---
title: Web前端面试题(2017年) 
date: 2017-03-20 23:35:09
---

## 前端基础知识

1. 请写下display属性有哪些属性，每个属性的意义
2. 写出常用的CSS选择符，至少5种，并说出他们的优先级
3. 两栏布局，左侧固定右侧自适应，写出所有可以实现的方案
4. 使用css3做一个旋转的动画效果
5. css如何清除浮动
6. 如何实现响应式布局
7. 删除数组a中值为m的项
8. js中如何定义一个只读变量、属性
9. 前端有哪些方法可以实现本地的数据储存，有什么区别
10. 怎么用canvas加载一张图片
11. 一个ul下有多个li，点击任意一个alert出其index
12. 使用正则表达式，验证表单中输入的是一个url（尽可能精确），如：
	
	```
	提示，网站链接形式：
	- http://www.baidu.com
	- https://know.baidu.com/question/33546162633632336435650200/401301
	- https://www.baidu.com/s?wd=%E7%99%BE%E5%BA%A6%E6%B4%BE&rsv_spt=1&rsv_iqid=0x9741e0c70008a553&issp=1&f=8&rsv_bp=0&rsv_idx=2&ie=utf-8&tn=baiduhome_pg&rsv_enter=1&rsv_sug3=9&rsv_sug1=9&rsv_sug7=100&rsv_sug2=0&inputT=1629&rsv_sug4=1630&rsv_sug=1
	```
13.  ((a,...b) => a*b.length)(1, 2, 3, 4, 5)返回值是多少;
14. 写一个方法flatten，将多维数组转化为一维数组：

	```
	let a1 = [[0, 1], [2, 3], [4, 5]];
	let a2 = [0, [1, [2, [3, [4, [5]]]]]];
	flatten(a1) // [0, 1, 2, 3, 4, 5]
	flatten(a2) // [0, 1, 2, 3, 4, 5]

	```

15. 写出返回值
	```
	let a = function (x, y, z) { return x + 10 * y + 100 * z;};
	let b = a.bind(null, 1, 2);
	b(3, 4, 5); // 求返回值
	```

16. 用js实现冒泡排序

17. 九宫格实现最短路径

	| 10 | 9 | 8 |
	|----|----|----|
	| 7 | 5 | 9 |
	| 6 | 3 | 2 |
	
18. 写出Vue生成的HTML结构

```
// html
<div id="app">
  <hello-world :level=level>
   <div slot="h1">Hello Foo</div>
   <div slot="h2">Hello Bar</div>
  </hello-world>
</div>

// js
Vue.component('hello-world', {
  render: function (createElement) {
    return createElement(
      'h' + this.level,
      [
        this.$slots['h' + this.level],
        'Hello World'
      ]

    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
});

new Vue({
  el:'#app',
  data: {
    level: 2
  },
})
```

## 系统知识

1.  为什么 0.1 + 0.2 === 0.3 为 false 
1. 怎么画出0.5px的边框，有哪些方案
2. 请解释进程和线程的区别
3. 请解释下js中的事件轮询的机制
4. 请说说你对Vue或React中虚拟dom的理解
5. 浏览器如何判断访问本地缓存还是请求服务器
6. 请至少说出一个你实现的组件，组件功能/参数/调用方式


## 开放问答

### 最能代表技术的案例？

- 前后端怎么配合的？
- 负责了哪个部分
- 其中的技术难点在哪儿
- 怎么实现的

### 有哪些技术产出？
- 有开源的项目么
- 有做过技术分享么（组内）


### 为什么会选择离开当前的团队呢？

### 你当下最关注的技术是什么？

### 未来的职业规划



<div style="display:none">
## 自问自答，哈哈哈哈
11.

```
// 1
$('ul').click('li', function(){
    console.log($(this).index());
});

// 2
var li = document.getElementsByTagName('li');
var i = 0;
var len = li.length;
for(;i<len;i++){
  !function(i){
    li[i].onclick(function(){alert(i)});
  }(i);
}

```

14.
```
var flatten = function(args){
    return args.reduce((prev, current, index, arr) => {
    	return prev.concat(Array.isArray(current) ? flatten(current) : current);
    }, []);
};

```


15.
```
var bubbling = function(arr){
	var len = arr.length;
	var i;
	while(len--) {
		for(i = 0;i<len;i++) {
			if(arr[i] > arr[i+1]) {
				arr[i] ^= arr[i+1];
				arr[i+1] ^= arr[i];
				arr[i] ^= arr[i+1];
			}
		}
	}
	return arr;
};
```
</div>