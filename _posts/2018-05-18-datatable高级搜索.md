---
layout: post
title: datatable高级搜索
keywords: 
category: datatable
author: 晴天
description: 
---


```
  $('#advanceSearch')
      .off('click').on('click', function search() {
      var inputs = $('.search-item-input');
      inputs.each(function each(i, ele) {
        var $ele = $(ele);
        var index = $ele.attr('data-column');
        var value;
        if ($ele.val()) {
          value = encodeURI($ele.val());
        } else {
          value = $ele.val()
        }
        // console.log($ele+"="+index+"=="+value)
        oTable.column(index).search(value);
      });
      oTable.draw();
    });
```

<p>今天做一个高级搜索，发现在给"data-column"属性赋值时，隐藏列的下标也要计算进去。</p>