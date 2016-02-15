---
title: linux下的常用命令
date: 2015-05-25 20:16:18
description: 大幅提高工作效率的的linux命令整理
tags:
- linux

---

## 文件处理

exec的语法


```
find . -name '*.js' -exec grep 'title' -rl {} \;
```

xargs: 给命令传递参数的过滤器

```
# 统计js的数量
find . -name '*.js' | wc -l

# 统计js文件的行数
find . -name '*.js' | xargs wc -l

# plus
find . -name '*.js' | xargs wc -l | sort -k2

cat url-list.txt | xargs wget –c

```

批量替换

```
# 反引号可以用$()
sed -i '' "s/common\/data/ipad\/data/g" `grep common\/data * --exclude-dir=\.svn -rl`

```


vi中的快速注释(其实就是正则)

```
假设是.php
3,5 s/^/#/g 注释第3-5行

3,5 s/^#//g 解除3-5行的注释

1,$ s/^/#/g 注释整个文档。

:%s/^/#/g 注释整个文档，此法更快。

```


awk神器

```
# 被修改的文件
svn st | grep -e 'template/|static/' | grep -e 'M|?' | awk '{print $2}'

# 获取文件前10行
awk 'FNR == 10 {print }' file.txt
# OR
awk 'NR == 10' file.txt

# OR
sed -n10p file.txt

# OR
tail -n+10 file.txt|head -1
```

## 系统命令

活动的进程

```
ps -ef 查看正在活动的进程
ps -ef |grep abc 查看含有"abc"的活动进程
ps -ef |grep -v abc 查看不含abc的活动进程

```

后台运行

```
nohup sh test.sh &>/dev/null &
```