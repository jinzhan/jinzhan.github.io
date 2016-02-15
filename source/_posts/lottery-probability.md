---
title: 一个简单的概率问题
date: 2015-09-27 09:32:42
tags: algorithms
description: 关于排列和组合的一些问题
---

## context
网上看到一道题目，5枚硬币随意抛，求3枚以上朝上的可能性

## 排列组合
**排列**
从n个不同的元素中取m(m≤n)个元素，按照一定的顺序排成一排，叫做从n个不同的元素中取m个元素的排列。
排列数：从n个不同的元素中取m(m≤n)个元素的所有排列的个数，叫做从n个不同元素中取出m个元素的排列数，记为Anm 
排列公式：A(n,m)=n!/(n-m)!，即：A(n,m)=n*(n-1)*.....(n-m+1)

**组合**
从n个不同的元素中，任取m(m≤n)个元素并成一组，叫做从n个不同的元素中取m个元素的组合。 
组合数：从n个不同的元素中取m(m≤n)个元素的所有组合的个数，叫做从n个不同元素中取出m个元素的组合数，记为Cnm 
组合公式：C(n,m)=A(n,m)/m!=n!/(m!*(n-m)!) 


## Code

```
/*
* @param {number} m
* @param {number} n
* @param {number} f
* @return {number}
*/
function posibility(m,n,f){
    f = +f || 5;
    var allPosibility = Math.pow(2,m);

    // n枚硬币朝上，组合：cmn = m!/n!*(m-n)!
    var mFactorial = 1;
    for(var i=m-n+1;i<=m;i++){
        mFactorial *= i;
    }

    for(var i=1;i<=n;i++){
        mFactorial /= i;
    }

    return +(mFactorial/allPosibility).toFixed(f);
}
```

**result**

答案是： posibility(5,3) + posibility(5,4) + posibility(5,5)

等价于：1 - (posibility(5,2) + posibility(5,1) + posibility(5,0))

