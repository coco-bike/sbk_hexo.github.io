---
title: sql触发器
copyright: true
date: 2019-04-17 13:28:45
categories: sql
tags: [sql]
---

<blockquote class="blockquote-center">相信谎言的人必将在真理之前毁灭。 —— 赫尔巴特</blockquote>

<!-- more -->

## 1.华润项目用到的触发器

```SQL
USE [UCard_HuaRun]
GO
/****** Object:  Trigger [dbo].[Update]    Script Date: 2019/4/18 10:14:52 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
-- =============================================
-- Author:		宋必可
-- Create date: 2019-04-18
-- Description:	FlowState字段的更新，触发datastate的更新
-- =============================================
ALTER TRIGGER [dbo].[Update] 
   ON   [dbo].[UO_WeighRecord]
   AFTER UPDATE
AS
if UPDATE(FlowState) 
BEGIN
	 declare @WeighNO nvarchar(50),
			 @FlowState INT,
			 @DataState NVARCHAR(50),
			 @DataState1 NVARCHAR(50),
			 @DataState2 NVARCHAR(50),
			 @GoodsCode NVARCHAR(50),
			 @GoodsID INT,
			 @GoodName NVARCHAR(50),
			 @Photos INT,
			 @CStepFlag int
			 
			 
	 SELECT @WeighNO=WeighNO,@FlowState=FlowState,@GoodsID=GoodsID,@CStepFlag=CStepFlag FROM deleted;
	 SELECT @Photos=COUNT(*) FROM dbo.UO_WeighPhotos WHERE WeighNO=@WeighNO;
	 SELECT @GoodsCode=GoodsCode FROM dbo.UO_Goods WHERE ID=@GoodsID;
	 --SELECT @FlowDetail=COUNT(*) FROM dbo.UO_WeighFlowDetail WHERE WeighNO=@WeighNO;
	 
	 --图片判断
	 IF(@GoodsCode='H01' OR @GoodsCode='H02')
		IF(@Photos=10)
		    SET	@DataState1='正常'
		ELSE
		    SET	@DataState1='照片不完整'
	 ELSE 
		IF(@Photos=8)
		    SET	@DataState1='正常'
		ELSE 
			SET	@DataState1='照片不完整'
	 
	 	 
	 --流程判断
	 IF(@CStepFlag=5)
		SET @DataState2='正常'
	 ELSE
		SET @DataState2='流程未完成'
			
	--总判断
    IF(@DataState1='正常'AND @DataState2='正常')
	     SET @DataState='正常';
	ELSE IF(@DataState1='照片不完整' AND @DataState2='流程未完成')
		 SET @DataState='流程未完成';
	ELSE IF(@DataState1='照片不完整' AND @DataState2='正常')
		 SET @DataState='照片不完整';
	ELSE IF(@DataState1='正常' AND @DataState2='流程未完成')
		 SET @DataState='流程未完成';

		
	--更新
	 update UO_WeighRecord set DataState=@DataState where WeighNO=@WeighNO 
END

END
```

## 2.触发器基本知识点

SQL Server为每个触发器都创建了两个专用表：Inserted表和Deleted表，这两个表由系统来维护，它们存在于内存中而不是在数据库中。这两个表的结构总是与被该触发器作用的表的结构相同。触发器执行 完成后，与该触发器相关的这两个表也被删除。

    Deleted表存放由于执行Delete或Update语句，而要从表中删除的所有行。
    Inserted表存放由于执行Insert或Update语句，而要向表中插入的所有行。


SQL Server2000提供了两种触发器：Instead of 和After 触发器。这两种触发器的差别在于他们被激活的时间。


    Instead of触发器用于替代引起触发器执行的T-SQL语句。除表之外，Instead of 触发器也可以用于视图，用来扩展视图可以支持的更新操作。


    After触发器在一个Insert,Update或Deleted语句之后执行，进行约束检查等动作都在After触发器被激活之前发生。After触发器只能用于表。