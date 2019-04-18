---
title: sql游标循环执行update语句
copyright: true
date: 2019-04-18 14:06:36
categories: sql
tags: [sql]
---


<blockquote class="blockquote-center">时间是一切财富中最宝贵的财富。 —— 德奥弗拉斯多</blockquote>

<!-- more -->

### 更新一组数据，sql触发器只能触发第一位数据，无法触发其他，一个update的语句对应一个触发器，所有使用游标循环执行update语句


```SQL
DECLARE My_Cursor CURSOR --定义游标
FOR (SELECT *FROM [UCard_HuaRun].[dbo].UO_WeighRecord where CStepFlag=5) --查出需要的集合放到游标中
OPEN My_Cursor; --打开游标
FETCH NEXT FROM My_Cursor ; --读取第一行数据
WHILE @@FETCH_STATUS = 0
    BEGIN
        --UPDATE dbo.MemberAccount SET UserName = UserName + 'A' WHERE CURRENT OF My_Cursor; --更新
        --DELETE FROM dbo.MemberAccount WHERE CURRENT OF My_Cursor; --删除
        
        update UCard_HuaRun.dbo.UO_WeighRecord set FlowState=1 WHERE CURRENT OF My_Cursor
        
        FETCH NEXT FROM My_Cursor; --读取下一行数据
    END
CLOSE My_Cursor; --关闭游标
DEALLOCATE My_Cursor; --释放游标
GO
```