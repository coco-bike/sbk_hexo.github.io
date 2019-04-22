---
title: 关于element-ui的table组件表头和内容部分错位的问题
copyright: true
date: 2019-04-22 14:40:02
categories: 前端
tags: [element-ui]
---


<blockquote class="blockquote-center">一件事实是一条没有性别的真理。 —— 纪伯伦</blockquote>

<!-- more -->

### element-ui的table组件表头和内容部分错位的问题解决方案

``` css

body .el-table th.gutter {
  display: table-cell !important;
}

body .el-table colgroup.gutter {
  display: table-cell !important;
}

```

上述代码放在css公共类或者App.vue等主页面里面