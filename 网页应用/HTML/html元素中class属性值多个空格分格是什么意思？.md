# [html元素中class属性值多个空格分格是什么意思？](https://segmentfault.com/q/1010000000325835)

[![头像](media/头像-3.jpg)

**土豆一口吃一个**

5891918



](https://segmentfault.com/u/potato)

[发布于  
2013-10-27](https://segmentfault.com/q/1010000000325835/revision)

[![头像](media/头像-1.png)

**fenbox**

6.5k197778



](https://segmentfault.com/u/fenbox)

[更新于  
2013-10-27](https://segmentfault.com/q/1010000000325835/revision)

即指定多个class，这是bootstrap常干的事，比如 `<div class="alert alert-info">`

请问，这两个class之间的关系是什么，二者的优先级是怎样的？

我自己定义了一个class ，加在后面，但没起作用，当然，如果写到style里去是可以的。

[css](https://segmentfault.com/t/css)[html](https://segmentfault.com/t/html)

关注2收藏7

[回复](https://segmentfault.com/q/1010000000325835###)

阅读 58.3k

**4 个回答**

[得票](https://segmentfault.com/q/1010000000325835?sort=votes)[最新](https://segmentfault.com/q/1010000000325835?sort=newest)

9

[

**社区维基**

1

](https://segmentfault.com/a/1190000042203704)

[发布于  
2013-10-27](https://segmentfault.com/q/1010000000325835/a-1020000000325866/revision)

✓ 已被采纳

你说的没错，就是指定多个class的意思，在HTML的层面上说的话，这样指定的class是同级的。同级的class需要看CSS文件的先后次序，后加载的css会覆盖前面加载的css。写到style的话因为是最后解析的所以是最高的一个优先级。

[回复](https://segmentfault.com/q/1010000000325835###)

4

[![头像](media/头像-1.jpg)

**ShingChi**

810244



](https://segmentfault.com/u/shingchi)

[发布于  
2013-11-09](https://segmentfault.com/q/1010000000325835/a-1020000000333784/revision)

[更新于  
2013-11-09](https://segmentfault.com/q/1010000000325835/a-1020000000333784/revision)

前面的答案，都是合理的。但依我看，这么干侧重在于 **CSS 的模块化设计**。`.alert` 是基础公共层，`.alert-info` 是个表现扩展层。

像 @Aivier 所说的，它还有可能有 alert-warning，alert-success 等等，假如我们每个分开写的话，小页面没什么问题，但是它在一个大项目里，就显得很笨拙，增加了开发的时间成本。所以，人们为了提高代码的重用性，把类似的结构或功能等等的部件，划为一个模块。然后把它们的共性提炼出来，也就是这段代码前的 `.alert`，再分开写它们具体的表现样式，即 `.alert-info`。

[回复](https://segmentfault.com/q/1010000000325835###)

[![头像](media/头像.png)

**sm0king**

1



](https://segmentfault.com/u/sm0king)

[发布于  
2013-10-28](https://segmentfault.com/q/1010000000325835/a-1020000000326070/revision)

新手上路，请多包涵

多个class你可以理解成一对多的意思，说的是这一块有多个class样式。 css的优先级考虑的地方还算比较多，你这里使用style毫无疑问是优先级最高的，任何情况想style的优先级都是最高的。其次是ID和各种选择器和继承，其实单独一个class的优先级很低的。

[回复](https://segmentfault.com/q/1010000000325835###)

[**52lidan**](https://segmentfault.com/u/f2e)：

哥们儿，important可以覆盖掉style的。

[](https://segmentfault.com/q/1010000000325835###)[回复](https://segmentfault.com/q/1010000000325835###)2013-10-29

[![头像](media/头像-2.jpg)

**Aivier**

7



](https://segmentfault.com/u/aivier)

[发布于  
2013-11-08](https://segmentfault.com/q/1010000000325835/a-1020000000333588/revision)

[![头像](media/头像-2.png)

**公子**

36.6k105268



](https://segmentfault.com/u/lizheming)

[更新于  
2013-11-08](https://segmentfault.com/q/1010000000325835/a-1020000000333588/revision)

<div class="alert alert-info">

同时**指定**了**多个CSS样式**，这里面的alert-info还可以换成alert-warning，alert-success等，这样分开多个class可以**减少重复的代码**，alert中的样式只写一次即可，而不用alert-warning，alert-info中都重复一遍

你自己写的样式可以在分号前**增加 !important 来强制应用样式**