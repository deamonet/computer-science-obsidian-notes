  
992 人赞同了该回答

我用c++和[lua](https://www.zhihu.com/search?q=lua&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)做过游戏引擎和大型项目开发，js和ts也做过超大型项目，目前正在写rust原创大型项目，java/c#/go用过一些。评论区有人希望我也评价一下ython，因为对于python没有写过大型项目，没有深入了解就没有发言权，所以就不班门弄斧了。

1.用rust写程序，我可以一直写8小时，其间不去debug，然后如果能最后编译成功，debug的时间非常少。心智负担极小。找搭档很靠谱，因为不靠谱的混子连编译都过不了。review代码只需要考虑业务逻辑就行了。

2.用c++写程序，我可以一直写8小时，其间不去debug，然后不用太费力编译成功后，debug的时间非常多。心智负担较高。如果会用c++ 1x这些现代特性，才会稍好一些。找搭档要小心一些，不靠谱的混子能编译过，但会留下一堆内存安全和线程安全问题，review代码不是一般地累。

3.用javascript写程序，我必须在8小时内一边写一边调试，虽然写出能跑的程序极为简单，但大型项目的调试，心智负担极高，js的程序员上限不封顶，但下限极低，找靠谱的搭档要当心。review代码成本太高，所以一般不review了，鉴于国内的环境，反正都是赚快钱的，多雇些前端和测试，不停地996去磨出一个bug少的东西吧，屎山就屎山，不用管了。提到js这个屎一样的语言，就要看看类似的脚本语言，lua，论特性和js有些类似，在我看来lua简直是脚本语言的天花板，1.定位清晰，咱就是[脚本语言](https://www.zhihu.com/search?q=%E8%84%9A%E6%9C%AC%E8%AF%AD%E8%A8%80&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)，短小精悍，不干别的。2.设计优雅，没有那么多想不明白的设计。3.性能优秀，不用像js一样集天下之牛人搞出一个v8这样的玩意才基本解决了性能问题。

[4.java/c#/](https://link.zhihu.com/?target=http%3A//4.java/c%23/)go这类语言，因为有了gc，真的很适合编程新手，[强类型](https://www.zhihu.com/search?q=%E5%BC%BA%E7%B1%BB%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)约束，运行时检查，比c/c++/[rust](https://www.zhihu.com/search?q=rust&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)简单，又没有弱类型脚本语言带来的大规模项目难以为继的问题。事实上我认为上手这类强类型带gc的语言其实难度是小于javascript的。尤其是大型项目。但是，gc真的在某些软件领域是一个大问题，gc带来的性能损耗不可忽略，比如游戏引擎。但这不是主要的，gc的问题在于gc本身的过程不可控，程序员像黑盒一样只能被动接受gc。这样一来，c/c++/rust这类无过多运行时开销的语言仍然不可或缺，rust严格来说也算有gc，[raii](https://www.zhihu.com/search?q=raii&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)的自动释放本来也是，但本质在于内存是可控的，完全可以受你的摆布。gc的runtime体积也不容忽视，比如wasm，c和rust生成的wasm体积极小，有人非要用go去写wasm，您先看看这玩意编译完了多大。

5.c语言，反倒是简单的，因为对于c，把它当作汇编的高级翻译层，可能更合适，贴近硬件，没有高级货，算是一个通用的接近底层的中级语言。生成的二进制也极为简单，[abi](https://www.zhihu.com/search?q=abi&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)不复杂。这些特性注定也会占据这些生态位。我倒觉得，c语言能做的，rust肯定能做，但考虑到使用的场合，rust可能过重了，一是学习成本较高，而是接近底层的使用一定会搞[unsafe](https://www.zhihu.com/search?q=unsafe&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)，这样一来，不如c来得简单。

下面说rust

rust的缺点，学习曲线较高，前期写出一个能够成功编译的东西都有点难。但用熟了后，心智负担极小。C++的RAII，模板，静态分发，[元编程](https://www.zhihu.com/search?q=%E5%85%83%E7%BC%96%E7%A8%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)这些东西有多少人在真正在实际项目中大量用？因为写c++不用这些东西也能可以，很多人就当C++是一个有[虚函数表](https://www.zhihu.com/search?q=%E8%99%9A%E5%87%BD%E6%95%B0%E8%A1%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)的带对象的c语言在用。rust就是强制你去用这些新特性，去思考这些特性，所以我一直说，学了Rust，你的c++功力也会长劲不少，反过来，一个精通现代c++特性的人，上手Rust也不会难。顺便说一下，人总喜欢躲在自己的舒适区，当你用熟了一样语言后，很难容得下别的东西。在喷子喷之前，请你放下心态，静心驱动一下你那僵化懒惰的大脑，你也会发现rust的设计光辉。

Rust不是闭门造车的语言，设计者能看出来，是做过大量工程的人，rust是实践派，不是学院派，rust的创新，是所有权系统和生命周期，这个强大的能力带来了0开销的内存安全和线程安全。其它rust特性或多或少地借鉴了其他语言的优秀特性，[npm](https://www.zhihu.com/search?q=npm&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)的包管理，go的channel，Haskell的trait，还有强枚举，[闭包](https://www.zhihu.com/search?q=%E9%97%AD%E5%8C%85&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)，[智能指针](https://www.zhihu.com/search?q=%E6%99%BA%E8%83%BD%E6%8C%87%E9%92%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)的这些特性，并非rust原创，但rust确实把把这些优点全部吸收了进来，而没有做过度的设计，从这个角度来看，rust依然是站在前人的肩膀上。

-----------------------------------------------------------------------------------------------

补充一下评论区里[typescript](https://www.zhihu.com/search?q=typescript&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)的质疑，很多ts工程不敢全开严格模式，估计很多人用好多脚手架开箱后就可以用了，简历都写熟练使用TypeScript，可是聊下来发现声明复杂一点的变量只会any，简直就是来搞笑的！全篇any，你谈什么强类型？不开严格模式有很多理由，开发者怕麻烦，第三方库调用问题。关于变量的有和无的问题，c/c++用null指针搞，js发明了奇多无比的假值，undefined, null, false, etc，rust用强枚举优雅地解决了这个世纪难题。ts一开始就想着兼容js，也不可避免地引入了这些问题，就问你们敢不敢在项目中全开严格模式？

如果不敢开严格模式，那你用typescript干什么呢，和ide配合语法感知用？如果想简单，直接用js作为脚本语言来说更加简单的。当你的项目没有大到像做一个google doc这种体量的工程时，国内的大多程序员使用typescript基本体现不出其设计理念。我对typescript的评价，当年这东西推广的时候怕成为另一个coffee，dart之类的东西，主动选择了兼容js，成也兼容，败也兼容。兼容js意味着有很多半吊子程序员把这ts当一个better js来用，通篇的any看着好不舒服。

noImplicitAny不允许变量或函数参数具有隐式 any 类型

noImplicitThis不允许 this 上下文隐式定义

strictNullChecks不允许出现 null 或 undefined 的可能性

strictPropertyInitialization验证[构造函数](https://www.zhihu.com/search?q=%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2322776585%7D)内部初始化前后已定义的属性

strictBindCallApply对 bind, call, apply 更严格的类型检测

strictFunctionTypes对函数参数进行严格逆变比较

[编辑于 2022-10-21 09:35](https://www.zhihu.com/question/432640008/answer/2322776585)・IP 属地北京