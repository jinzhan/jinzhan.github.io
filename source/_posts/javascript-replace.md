---
title: javascript前端模板
date: 2015-12-26 18:26:39
description: 强大的replace函数
tags:
- replace
- javascript

---

## ECMASript 6模板语法

字符串模板

```
let name = 'steinitz';
console.log(`My name is ${name}`); // My name is steinitz


var a = 5;
var b = 8;
console.log(`${a} x ${b} = ${a*b}`); // 5 x 8 = 40
```
标签模板：

```
var a = 5;
var b = 10;
tag`Hello ${ a + b } world ${ a * b }`;

```
上面代码中，模板字符串前面有一个标识名tag，它是一个函数。整个表达式的返回值，就是tag函数处理模板字符串后的返回值。

tag函数所有参数的实际值如下。

- 第1个参数：['Hello ', ' world ']
- 第2个参数: 15
- 第3个参数：50

tag函数实际上以下面的形式调用：`tag(['Hello ', ' world '], 15, 50)`;

```
// 一个例子，标签模板的用法极为灵活
var a = 5;
var b = 10;
tag`Hello ${ a + b } world ${ a * b }`;

function tag(arr, i1, i2) {
	console.log(arr);
	console.log(i1*i2);
	return 'Yes!';
}

```


## 构造前端模板
强大的replace函数

```
function replacer(match, p1, p2, p3, offset, string) {
  // p1 is nondigits, p2 digits, and p3 non-alphanumerics
  return [p1, p2, p3].join(' - ');
}

// will return: 'abc - 12345 - #$*%':
var newString = 'abc12345#$*%'.replace(/([^\d]*)(\d*)([^\w]*)/, replacer);

```

稍微修改下：

```
var html = function(tmpl, data) {
    return tmpl.replace(/\$\{(\w+)\}/g, function(word, key) {
       return data[key];
    });
};

// example
var tmpl = 'My name is ${name}, I am working in ${place}';
console.log(html(tmpl, {
	name: 'Steinitz',
	place: 'Beijing'
})); 

// My name is Steinitz, I am working in Beijing
```