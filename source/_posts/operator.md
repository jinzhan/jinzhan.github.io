---
title: javascript中位运算符的应用
date: 2015-09-20 20:29:44
tags: operator
category: algorithms
---

## review
位运算符即按照内存中的位来操作数值，因此具有更好的性能。位运算符有：

- `~` 按位非(NOT)
- `&` 按位与(AND)
- `|` 按位或(OR)
- `XOR` 按位异或(XOR)
- `<<` 左移
- `>>` 右移
- `>>>` 无符号右移

位运算符属于最基层的操作，在数值逻辑的处理过程中，应该优先被考虑。

## practice

来看一个case：


**题1**

```
在一个整数数组中，除了1个数只出现1次，其他每个数都出现2次，找出这个数。

```

**解**

考虑到这篇文章的主题，聪明的你应该已经想到了，这里使用`异或`，能最优雅的解决该问题。

```
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
    var ret = nums[0];
    for(var i=1,len = nums.length;i<len;i++) {
    	ret ^= nums[i];
    }
    return ret;
};
```

用es6 再简洁一点

```
var singleNumber = function(nums) {
   return nums.reduce((pre, cur) => pre ^= cur);
};

```

[refer](https://leetcode.com/problems/single-number/)

**注意：二进制中负数是以二进制补码存储**
步骤如下：

1. 首先求该负数的绝对值码；
2. 求二进制反码，即将0替换为1，1替换为0；
3. 在得到的二进制码反码上加1；

如：-18

1. 绝对值码：0000 0000 0000 0000 0000 0000 0010 0010
2. 二进制反码：1111 1111 1111 1111 1111 1111 1101 1101
3. 加上1：1111 1111 1111 1111 1111 1111 1101 1110

下面，来个升级版：

**题2**

```
在一个整数数组中，除了2个数只出现1次，其他每个数都出现2次，找出这2个数。

```

**Analysis**

1. 同上，这个依然要用到异或；
2. 那么，如何将最终的结果(即：所求之数的异或结果）分解出来呢

**解**

```
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var singleNumber = function(nums) {
    // 第1步获得2个数的异或结果
    var num = nums[0];
    var len = nums.length;
    for(var i = 1;i<len;i++) {
        num ^= nums[i];
    }
    
    // 第2步是关键，num与-num按位与，得到num和-num都为1的位组成的数
    num &= -num;
    
    // 第3步，再遍历一次数组
    var a = 0;
    var b = 0;
    for(i = 0;i<len;i++) {
       num & nums[i] ? (a ^= nums[i]) : (b ^= nums[i]);
    }
    return [a, b]
};
```
[refer](https://leetcode.com/problems/single-number-iii/)

## plus
位操作的使用也能让代码变得更优雅：

- ~location.indexOf('baidu.com') 		// 只有-1按位非为0
- Math.random()*100 ^ 0    // 得到0~100(不含100)之间的随机数
- ~new Date   // 加时间戳
