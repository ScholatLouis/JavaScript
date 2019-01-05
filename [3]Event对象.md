# [3]Event对象
1. Event对戏那个在event第一次出发的事后就被创建出来，并且一直伴随着事件在DOM结构中流转整个生命周期。event对象会被作为第一个参数传递给事件监听的回调函数。event对象具有以下的属性:  

> type(String)        -- 事件的名称  
  target(node)        -- 事件起源的DOM节点   
  currentTarget(node) -- 当前回调函数被触发的DOM节点  
  bubbles(boolean)    -- 指明该事件是否是一个冒泡事件(需要注意的是：不是所有的事件都是可以冒泡的)  
  cancelable(boolean) -- 指明这个事件的默认行为是否可以通过调用preventDefault()  來阻止，也就是只有当cancelable为true的时候，调用preventDefault方法才能生效  
  eventPhase(number)  -- 指明当前事件所处的阶段(phase)，none(0), capturing(1), target(2), bubbling(3)  
  timestamp(number)   -- 事件发生的事件  
  defaultPrevented(boolean) -- 指明当前事件对象都额preventDefault方法是否被调用过  
  preventDefault(function)  -- 这个方法将阻止浏览器中用户代理对当前事件的相关默认行为。  
  stopPropagation(function) -- 这个方法将阻止当前事件链上后面的元素的回调函数被触发，当前节点上针对此事件的其他回调函数依然会被触发。  
  stopImmediatePropagation(function) -- 这个方法将阻止当前事件链上所有回调函数被触发，也包括当前针对此事件已绑定的其他回调函数。     

[具体案例](http://jsfiddle.net/Louis_Tsang/vovnpLga/)  
[更改版本案例](http://jsfiddle.net/Louis_Tsang/vovnpLga/1/)  
