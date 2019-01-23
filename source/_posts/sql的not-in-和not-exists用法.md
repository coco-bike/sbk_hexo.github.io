---
title: sql的not in 和not exists用法
copyright: true
date: 2019-01-23 09:21:05
categories: sql
tags: [sql]
---
<blockquote class="blockquote-center">过放荡不羁的生活，容易得像顺水推舟，但是要结识良朋益友，却难如登天。 —— 巴尔扎克</blockquote>

<!-- more -->
```

select PlateNumber from UO_Vehicle where  not exists
(select PlateNumberfrom UO_Card where UO_Card.PlateNumber=UO_Vehicle.PlateNumber)


select PlateNumber from UO_Vehicle where PlateNumber not in
(select PlateNumber from UO_Card where UO_Card.PlateNumber=UO_Vehicle.PlateNumber)


```

在写查询时，尽量避免使用Not In，本文提供的Not Exists等价形式，将会减少很多麻烦