---
layout: post
title: StringUtils.join
keywords: 
category: java
author: 晴天
description: 
---

把List中元素加“，”拼接，以前都是这么写的，那天同事说用StringUtils

```
StringBuilder sb = new StringBuilder();
for(int i = 0;i<list2.size();i++){
	sb.append(list2.get(i) + ",");
}
sb.subSequence(0,sb.length()-1);
```

```
String str = StringUtils.join(list,",");
```

这里看一下源码

```
   public static String join(final Iterator<?> iterator, final String separator) {

        // handle null, zero and one elements before building a buffer
        if (iterator == null) {
            return null;
        }
        if (!iterator.hasNext()) {
            return EMPTY;
        }
        final Object first = iterator.next();
        if (!iterator.hasNext()) {
            @SuppressWarnings( "deprecation" ) // ObjectUtils.toString(Object) has been deprecated in 3.2
            final String result = ObjectUtils.toString(first);
            return result;
        }

        // two or more elements
        final StringBuilder buf = new StringBuilder(256); // Java default is 16, probably too small
        if (first != null) {
            buf.append(first);
        }

        while (iterator.hasNext()) {
            if (separator != null) {
                buf.append(separator);
            }
            final Object obj = iterator.next();
            if (obj != null) {
                buf.append(obj);
            }
        }
        return buf.toString();
    }
```

如果只有一个元素，则拼接并返回。如果大于一个元素，则先把第一个元素拼接，然后开始迭代。每次迭代先拼接指定的分隔符，后拼接元素。迭代完返回。

再加一个，在拼接的时候我最初是用StringBuffer，但是此处并没有线程安全问题，所以改为StringBuilder。

静态方法如果没有使用静态变量，则没有线程安全问题。静态方法内声明的变量，每个线程调用时，都会新创建一份，而不会共用一个存储单元。比如这里的tmp,每个线程都会创建自己的一份，因此不会有线程安全问题。
注意:静态变量，由于是在类加载时占用一个存储区，每个线程都是共用这个存储区的，所以如果在静态方法里使用了静态变量，这就会有线程安全问题。

