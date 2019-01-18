---
title: sql分组查询与子查询
copyright: true
date: 2019-01-18 15:56:02
categories: sql
tags: [sql]
---
sql分组查询与子查询

<!-- more -->
{% codeblock %}
select a.*, COUNT(b.WeighNO) as number from dbo.UO_Goods a 
left join UO_WeighRecord b on a.ID=b.GoodsID  
and b.FlowState=0 and b.IsDelete=0 
Group by a.CreateTime,a.Creator,a.GoodsCode,a.GoodsModel,
a.GoodsName,a.ID,a.LimtVCount,a.LimtVCount,a.Price,a.Remark,a.Warehouse


select *,(select COUNT(WeighNO) from UO_WeighRecord 
where UO_Goods.ID = GoodsID and FlowState=0 and IsDelete=0) As Number from UO_Goods
{% endcodeblock %}