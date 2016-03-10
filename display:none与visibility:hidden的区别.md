###display:none与visibility:hidden的区别
> 两者的区别主要有以下两点：  
>> 1) display:none不显示，而且不会在文档中占有位置；而visibility:hidden的效果也是不显示，但是会在文档中占有位置  
   2) 对于渲染完成的HTML，如果对某一个DOM节点加上display:none会引起浏览器的reflow操作；而如果加上visibility:hidden则只会引起浏览器的repaint操作。
