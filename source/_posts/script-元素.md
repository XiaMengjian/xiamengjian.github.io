---
title: script 元素
date: 2019-03-23 16:01:39
tags:
---

# script元素
外部脚本:
```
<script type='text/javascript' src='xxx'></script>
```

嵌入脚本:
```
<script type='text/javascript'>
    function(){
        alert('你好Xia')
    }
</script>
```

## 外部脚本支持的属性
* defer  文档在完全解析和显示出来的时候再去执行
* async

#### 两者的区别
1. 延时脚本是按照顺序执行的
2. 异步脚本不能保证执行顺序