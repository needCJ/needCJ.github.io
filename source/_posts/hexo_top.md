---
title: Hexo增加置顶功能
date: 2023-09-07 09:08:09
categories: Hexo
tags: [hexo,top]
---

默认的Hexo 排序是根据文章更新时间做倒序排序，但是我们有一些文章希望具有置顶功能，那么在不增加插件的前提下，就需要我们手动的去修改排序逻辑。本文只针对Home和Archive标签下的文章排序做定制处理，具体代码如下。

<!-- more -->
### 1. 找到node_modules/hexo-generator-index/lib/generator.js这个文件

在 const pagination = require('hexo-pagination'); 后面添加如下代码
```
  module.exports = function(locals) {
    const config = this.config;
    const posts = locals.posts.sort(config.index_generator.order_by);
    posts.data = posts.data.sort(function(a, b) {
      if(a.top && b.top) {
      // 两篇文章top都有定义
          if(a.top == b.top) return b.date - a.date;
           // 若top值一样则按照文章日期降序排
          else return  a.top - b.top ;
           // 否则按照top值升序排
      }
      else if(a.top && !b.top) {
      // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
          return -1;
      }
      else if(!a.top && b.top) {
          return 1;
      }
      else return b.date - a.date; // 都没定义按照文章日期降序排
  });

 // posts.data.sort((a, b) => (b.sticky || 0) - (a.sticky || 0));

```


### 2. 找到 /node_modules/hexo-generator-archive/lib/generator.js 同样添加代码
```
  const allPosts = locals.posts.sort(config.archive_generator.order_by || '-date');
  allPosts.data = allPosts.data.sort(function(a, b) {
    if(a.top && b.top) {
        if(a.top == b.top) return b.date - a.date;
        else return  a.top - b.top;
    }
    else if(a.top && !b.top) {
        return -1;
    }
    else if(!a.top && b.top) {
        return 1;
    }
    else return b.date - a.date;
  });

```

### 3.重新编译 hexo clean & g
