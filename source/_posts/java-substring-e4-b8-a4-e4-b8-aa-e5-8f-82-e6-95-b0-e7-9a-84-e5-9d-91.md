---
title: Java Substring 两个参数的坑
tags:
  - java
id: 2173
categories:
  - Java
date: 2014-06-07 23:04:53
---

搞了一天的elasticsearch，中间过程确是用大量时间处理java的几个字符串的问题。本来以为 java 里 String substring(int beginIndex, int endIndex) 的开始位置比0小时会自动为0，第二个参数大于总长度时会自动填长度，结果不是，为什么不这样做？

最后还是看源码吧：
` public String substring(int beginIndex, int endIndex) {
	if (beginIndex < 0) {
	    throw new StringIndexOutOfBoundsException(beginIndex);
	}
	if (endIndex > count) {
	    throw new StringIndexOutOfBoundsException(endIndex);
	}
	if (beginIndex > endIndex) {
	    throw new StringIndexOutOfBoundsException(endIndex - beginIndex);
	}
	return ((beginIndex == 0) && (endIndex == count)) ? this :
	    new String(offset + beginIndex, endIndex - beginIndex, value);
}
`