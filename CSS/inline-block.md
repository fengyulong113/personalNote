# display:inline-block的问题
两个display:inline-block的元素放在一起会产生一段空白
原因：
元素被当成行内元素排版的时候，元素之间的空白符（空格、回车换行等）都会被浏览器处理，根据CSS中white-space属性的处理方式（默认是normal，合并多余空白），原来HTML代码中的回车换行被转成一个空白符，在字体不为0的情况下，空白符占据一定宽度，所以inline-block的元素之间就出现了空隙。
解决办法：
1. 为子元素设置float:left
2. 父元素中设置font-size: 0，在子元素上重置正确的font-size
3.  将子元素标签的结束符和下一个标签的开始符写在同一行或把所有子标签写在同一行
