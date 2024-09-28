[![大宽宽](https://pic1.zhimg.com/v2-0d277f6cffdf1141d9aaa7763b92b9d7_l.jpg?source=1940ef5c)](https://www.zhihu.com/people/xing-jiankuan)

[大宽宽](https://www.zhihu.com/people/xing-jiankuan)

程序员话题下的优秀答主

1,043 人赞同了该回答

简单来说，private并不是解决“安全”问题的。

安全是指不让代码被非法看到/访问。但是只要人能拿到代码，总会有办法去查看和改变代码。其他答案提到反射可以用SecurityManager来防止private被访问。但是从更高一层的角度，即便使用了SecurityManager，还是可以通过各种方式拿到java的[bytecode](https://www.zhihu.com/search?q=bytecode&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A518225224%7D)，并做任意修改。比如有[asm](https://www.zhihu.com/search?q=asm&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A518225224%7D)这样的lib，也有instrument api这种东西可以帮你。所以记得，如果你真有一段代码不允许被别人看/用，就不要把这段代码放到其他人可以碰到的地方，而是做一个server，通过接口允许有限制的访问。其他人想破解，只能破解你的服务器网关和跳板机器。

> 关于真正的安全性，可以参考[激活服务器](https://www.zhihu.com/search?q=%E6%BF%80%E6%B4%BB%E6%9C%8D%E5%8A%A1%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A518225224%7D)的工作原理

private想表达的不是“安全性”的意思，而是OOP的**封装**概念，是一种编译器可以帮助你的设计上的[hint](https://www.zhihu.com/search?q=hint&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A518225224%7D)。这就像是一家没人的店挂了个牌子“闲人免进”，但你真要进去还是有各种办法可以办到。所以private，以及所有其他的[access modifier](https://www.zhihu.com/search?q=access%20modifier&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A518225224%7D)都有一层隐含的含义：如果你按照遵守这套规则，开发者可以保证不问题（不考虑bug的情况下）；否则，后果自负。

比如，你在用spring的IoC的时候，你知道你要“注入”，不管它是不是private的，你知道“注入”是你自己控制的，是你设计好的效果。那么通过spring的IoC利用反射帮你注入一些private property是再正常不过的用法。

再比如，[单元测试](https://www.zhihu.com/search?q=%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A518225224%7D)，你就想测一个private方法。但是因为private的缘故就是测不了。于是你可以用反射绕开这个限制，开心的做测试。

> 虽说某些人坚持“不应该测试[private方法](https://www.zhihu.com/search?q=private%E6%96%B9%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A518225224%7D)，而应该通过测试其他方法间接测试private方法，但并没有形成广泛的共识。这里不对这个问题展开。

虽然能绕开，但绕开的代码很繁琐。久而久之就会厌倦。毕竟，**代码应该为你工作，而不是你为代码工作**。因此，我的经验是通常会用protected或者[default](https://www.zhihu.com/search?q=default&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A518225224%7D)来代替private。我曾设想runtime应该给一种运行模式，通过设定一个启动参数使其不管private这类的限制，这样做UT，做profiling等工作都会轻松许多。等到最后发布时，再用普通模式。但可惜现实当中并没有这种设定。

> 评论区提到了Android里的[VisibleForTesting](https://link.zhihu.com/?target=https%3A//developer.android.com/reference/android/support/annotation/VisibleForTesting)，可以实现我期望的功能。大赞！感谢 
> 
> [@尤华杰](https://www.zhihu.com/people/66d7543a6668569878f911c7152247af)

我之所以敢用protected/default来代替private是因为现实当中非private不可的情景非常少见。实际上，很多时候private带来的麻烦比起带来的好处要多，这是因为很多时候对OOP的误用造成的。OOP的误用造成了无谓的private，然后逼着你必须得绕开private。

其实private就是个约定而已。看看其他语言，比如python，它的“private“是一种很松散的约定，所有private的成员都用下划线开头，告诉调用者“不要随便调用我哦”，但是如果真调用了也就调用了。C++，通过指针就能绕开private。有人说，private会避免新手误用。但问题是，大家从出道开始，自己或者周围的同事朋友有谁曾经出过这个问题？IDE知道一个成员当前不能访问，就根本就不会提示。如果一个人已经开始通过[源代码](https://www.zhihu.com/search?q=%E6%BA%90%E4%BB%A3%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A518225224%7D)/反编译研究“我能不能调用这个私有方法了“，他还算是一个菜鸟吗？他会不知道这里的潜在风险吗？如果真的误用了，code review能过吗？测试能过吗？如果一个公司因为误用private成员，造成了重大的损失，那这个公司就活该倒闭算了，不要在世上丢人。

OOP是一种[编程思想](https://www.zhihu.com/search?q=%E7%BC%96%E7%A8%8B%E6%80%9D%E6%83%B3&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A518225224%7D)，是众多编程思想中的一种。**是开发者决定了一个问题应该用OOP合适，并且用了Java这样的语言来简化自己开发OOP代码时的工作。**如果抱着这种态度，就不会误用，因为private在开发者的心中。其他人也不太可能误用，如果他上过几天java培训。不要因为语言是OOP的就去套，把不适合的OOP的代码强用OOP的各种套路实现，然后给自己后续的维护扩展埋坑。

[编辑于 2018-11-16 11:08](https://www.zhihu.com/question/28161668/answer/518225224)