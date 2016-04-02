---
title: SQL中Group By的常见场景
date: 2016-03-17 15:16:59
tags: mysql
description: “Group By”从字面意义上理解就是根据“By”指定的规则对数据进行分组，所谓的分组就是将一个“数据集”划分成若干个“小区域”，然后针对若干个“小区域”进行数据处理。

---

## 概述

“Group By”从字面意义上理解就是根据“By”指定的规则对数据进行分组，所谓的分组就是将一个“数据集”划分成若干个“小区域”，然后针对若干个“小区域”进行数据处理。


## 示例

**原始表**

| 类别 | 数量 | 摘要 |
|:---:|:---:|:---:|
| a | 5 | a1 |
| a | 3 | a2 |
| b | 15 | b1 |
| b | 30 | b2 |
| a | 15 | a3 |
| c | 20 | c2 |
| c | 25 | c1 |
| a | 3 | a2 |

### group by实例

**简单group by**
```
select 类别, sum(数量) as 数量之和
from A
group by 类别
```

| 类别 | 数量之和 | 
|:---:|:---:|
| a | 50 |
| b | 70 |
| c | 30 |

** Group By 和 Order By **

```
select 类别, sum(数量) AS 数量之和
from A
group by 类别
order by sum(数量) desc
```

| 类别 | 数量之和 | 
|:---:|:---:|
| a | 50 |
| b | 70 |
| c | 30 |


** Group By中Select指定的字段限制 **

在select指定的字段要么就要包含在Group By语句的后面，作为分组的依据；要么就要被包含在聚合函数中。

```
-- 下面的sql语句是错误的：
select 类别, sum(数量) as 数量之和, 摘要
from A
group by 类别
order by 类别 desc
```

** 聚合函数 **

| 函数 | 作用 |
|:---|:---|
| sum(列名) | 求和 |
| max(列名) | 最大值 |
| min(列名) | 最小值 |
| avg(列名) | 平均值 |
| count(列名) | 除null外的记录数 |
| count(*) | 统计记录数包含null |
| count(distinct 列名) | 除null和重复数据后记录数 |

```
--- 求各组平均值
select 类别, avg(数量) AS 平均值 from A group by 类别;

--- 求各组记录数目
select 类别, count(*) AS 记录数 from A group by 类别;

--- 求记录总数
select 类别, sum(数量) AS 数量之和 from A group by 类别;

```


** Having与Where的区别 **

where 子句的作用是在对查询结果进行分组前，将不符合where条件的行去掉，即在分组之前过滤数据，where条件中不能包含聚组函数，使用where条件过滤出特定的行。

having 子句的作用是筛选满足条件的组，即在分组之后过滤数据，条件中经常包含聚组函数，使用having 条件过滤出特定的组，也可以使用多个分组标准进行分组。

```
--- where条件不能包含聚合函数
select 类别, sum(数量) as 数量之和 from A
group by 类别
having sum(数量) > 18

--- Having和Where的联合使用方法
select 类别, SUM(数量)from A
where 数量 > 8
group by 类别
having SUM(数量) > 10

```


** Case ** 

```
--- |:--位置--:|:--分类id--:|:--cms配置id--:|:--点击量--:|:--点击uv--:|:--展现量--:|:--展现uv--:|
select va.area, va.classId, va.cmsId, va.pv, va.uv, vb.pv, vb.uv
from (
    select hk_urlfields['area'] as area,
    hk_urlfields['classId'] as classId,
    hk_urlfields['cmsId'] as cmsId,
    count(1) as pv, 
    count(distinct baiduid) as uv from reduced_iknow_wap_lighttpd
     where 
           dt = '{@date}'
           and hk_urlfields['pid'] = '102'
           and hk_urlfields['page'] = 'question'
           and hk_urlfields['action'] = 'click'
           and hk_urlfields['area'] in ('top_banner', 'text_banner','middle_promote','bom_fix_banner')
     group by hk_urlfields['area'], hk_urlfields['classId'], hk_urlfields['cmsId']
) va join (
    select ts.area as area, ts.classId as classId, ts.cmsId as cmsId, count(1) as pv, count(distinct ts.bid) as uv
    from (
        select transform(hk.cmsKey, hk.classId, hk.bid)
            using '${hiveconf:HDFS_PHP} decodeCmsKey.php'
             as (area string, cmsId string, classId string, bid string)
            from (
                select hk_urlfields['cmsKey'] as cmsKey,
                        hk_urlfields['classId'] as classId,
                        baiduid as bid
                from reduced_iknow_wap_lighttpd
                where 
                dt = '{@date}'
                and hk_urlfields['pid'] = '102'
                and hk_urlfields['page'] = 'question'
                and hk_urlfields['action'] = 'pv'     
                and hk_urlfields['cmsKey'] is not NULL
             ---- group by hk_urlfields['cmsKey'], hk_urlfields['classId'], baiduid
            ) hk
    ) ts group by ts.area, ts.classId, ts.cmsId
) vb on(va.area = vb.area and va.classId = vb.classId and va.cmsId = vb.cmsId)

```


** 参考资料 **
[http://www.cnblogs.com/rainman/archive/2013/05/01/3053703.html](http://www.cnblogs.com/rainman/archive/2013/05/01/3053703.html)



