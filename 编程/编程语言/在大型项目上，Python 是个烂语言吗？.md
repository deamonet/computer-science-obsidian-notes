最近在pongba的google groups看到一个较火的讨论：python是个烂语言[https://groups.google.com/forum/?fromgroups=#!topic/pongba/X7OpNT4BZLQ](https://link.zhihu.com/?target=https%3A//groups.google.com/forum/%3Ffromgroups%3D%23%2521topic/pongba/X7OpNT4BZLQ)。

比较客观的关于python的优点和缺点是什么呢。

这些看法：

“

可以很快的写点简单的东西出来。

但python代码一多开发效率就指数下降

一般的小项目， 代码超过 1000 行写 python 就已经是虐心了

代码超过 10w 以后你就别想用 python 开发了。

python 缺乏真正的开发工具

语法错误都在运行时

”

还算公正吗？

--------------------------------------------

我想问的不是哪种语言好哪种不好。而只是关于python自身的优势、劣势。

From <[https://www.zhihu.com/question/21017354/answer/2270327279](https://www.zhihu.com/question/21017354/answer/2270327279)>

作者：赵臣又

链接：https://www.zhihu.com/question/21017354/answer/2270327279

来源：知乎

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

python项目超过5万行，版本3.9.7，类型靠typing、assert和mypy保证，算中型项目吧。写python很容易放飞自我，必须靠一些best practice和principal才能保证代码质量，大项目用python确实有很多潜在危险。

但真正让我头疼的是性能问题，随着功能的增加，我遇到的情况和高赞回答类似：处处都慢，又没办法处处都优化。

我需要在大数据量上做数学计算，操作数据库，提供web服务，驱动selenium做网络爬虫，还需要动态的把mysql里存的python函数动态import到程序中执行（量化交易相关，需要执行交易策略函数，动态import可以解耦交易策略的发布和项目代码本身的发布）。整个项目运行在一台2核4g内存的云服务器上。

很难找到一个比python更好的支持以上所有需求的语言了。

java太耗内存，而且对大数据量的计算也没有优势，python有pandas用起来很爽。

c#其实是一个很有潜力的替代品，语法、生态、性能、开发效率，几乎没有短板。但是因为对c#不够熟悉，对微软的生态不够有信心，而且调研了一下好像c#生态中没有pandas的替代品，只能放弃。

c/c++很吃经验，很多方面都可能导致项目悲剧。搭建一个常见应用至少要涉及到[字符集](https://www.zhihu.com/search?q=%E5%AD%97%E7%AC%A6%E9%9B%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2270327279%7D)、db连接池、orm、api service、http请求、加密和ssl证书、email一堆基础功能，在java、python、php之类的语言上这些几乎都不会是问题，但c++就可能让人抓狂。

对我自己的项目做了profile，明确了下面几个部分最慢：

1.  selenium、requests，http网络请求慢，这块其实可以容忍，因为我把爬虫拆开成了一些cron job定时去跑，还用了multithreading来并行跑多个爬虫，所以速度不太重要。
2.  db模块慢，python orm用的是sqlalchemy，比拼接裸sql语句慢大概10-15倍。但我更不愿意维护裸sql。
3.  json serialization慢，所有数据都用marshmallow serialize为json string存db，读写db需要serialize和deserialize，就慢了。我考虑过给所有dataclass手写from_json，to_json api来提高速度，这类劳动既容易写bug，又费时费力。
4.  python本身天然比c/c++慢100-150倍。

目前第2、3、4点是我想优化的重点。

这两天重新看了看[rust 2021](https://www.zhihu.com/search?q=rust+2021&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2270327279%7D)，用rust重写一部分公共模块似乎是一个很有希望的方案。

—————更新—————

尝试了用rust重写一部分功能，pyo3编译为python module。pyo3还支持从rust调用python。rust的语言特性非常fashion，而且很多功能都可以找到第三方库来实现，似乎看到了直接用rust重写整个项目的希望。

—————更新2—————

经过了几天的各种尝试，不得不说，最终还是回到了c++重写部分模块的路上。有几点原因：

1.  对rust的积累没有c++那么深，从学校开始看各类经典c++书籍、stl和开源项目的源码，我对c++更如鱼得水。而rust的生命周期管理对心智负担还是太重，导致实际上rust开发进度比c++慢很多。
2.  cargo确实好用，但我对cmake工具链也足够熟悉，目前还没遇到无法解决的问题。关键最近找到了一个新的c++ orm库，因为之前放弃c++转向rust的主要原因是c++ orm劝退。
3.  rust很多第三方库在github、stackoverflow上的讨论不多。相信很多人像我一样，不打算深入源码中解决问题，如果google两天还无法解决问题就劝退了。
4.  c++比rust更加亲和python。比如rust enum无法直接转化为python enum，rust没有class继承机制、没有函数重载、默认参数机制，导致一部分rust api不适合转化为python api，进一步导致要多写很多wrapper代码。而我极度抗拒这类“重复劳动”。

—————更新3—————

已经用pybind11重写了几个公共的基础模块（环境变量、时间日期等），ubuntu 20/gcc 9.3开启c++17（刚好pybind11对optional有特定依赖），只能说真香。

本地unit test和regression test时明显速度变快，内存占用也明显降低。

目前正重写db模块，之前让我一度放弃c++转向rust的关键原因就是没有满意的c++ orm（rust有desiel）。试过odb、soci之后，发现了sqlpp11。

1.  我极度不适应odb的#pragma语法，耐着性子看了大半天文档也没找到怎么定义column name，不想浪费时间。
2.  soci的api很友好，但homebrew的默认安装不支持mysql backend，我用brew源码安装也没成功（github上提了issue说无法reproduce可能我本地环境有问题）。我又直接git clone soci源码下来也没编译成功。生产环境ubuntu上apt安装倒是没问题，但考虑到开发环境配置失败，就放弃了。
3.  最后找到了github上star数很高的sqlpp11，文档简陋但足够用，实现也相对简单，是个轻量级的库。head only免安装，api非常友好，对我足够用（我只要普通CRUD就行），关键本地终于跑起来了demo，就果断选择了sqlpp11。

再聊几句自己的c++开发心得吧：

1.  操作系统、编译器工具链选择新版本、新标准14/17，避免类似ubuntu 12.04、centos 6这种地狱模式。
2.  对一些经典软件和库（sqlite3、mysqlclient、curl、jemalloc等），一般brew/apt有默认安装包的就默认安装，简单省事。
3.  从11/14开始，很多新的c++库要么支持cmake add_subdirectory，要么直接head only，简化安装配置。直接git clone/submodule下载到third_party目录然后cmake包含到项目中一起编译。选择第三方库时尽量避开停止更新，既不能apt/brew安装，又不支持cmake/head only的第三方库，大概率是个坑。
4.  避免windows、unix/linux跨平台，这是一切罪恶的根源，要么windows、要么unix/linux。如果你确实需要界面，像我一样现学一套vue/quasar/nginx开发个网页app可能更效率。假如开发unix/linux的工作量是1，那么再写一套能跑在windows上的工作量大概是0.7-0.8（因为你有经验了写得更快），但当你妄图尝试搞个跨平台层把底层的一些api封装起来，把两个项目放到一个cmake里，期待着工作量降低到1.2-1.3（只要封装底层跨平台api即可），但实际上工作量反而会增长到3.0+。根据我的经验，光是在cmake里配一下boost就够让人秃头了。
5.  boost是很强大，但我只推荐那些即将/已经进入std标准的部分，作为-std参数开太低时的补充，比如optional、chrono之类，但我个人最喜欢的其实是preprocessor，像PP_STRINGIZE, PP_CAT, PP_SEQ_FOR_EACH什么的可以帮你少写很多代码，当半个template来用。而asio这类东西虽然很多博客的介绍看起来很牛逼，但你可能真正需要的是一个web framework，而非异步网络IO。
6.  可以信赖google绝大部分的开源库：absl、double conversion、leveldb之类。但和boost.asio类似，谨慎选择grpc这种大而全的框架，一旦你决定把整个app搬上去，就很难再甩掉了。一旦出现protobuf这种2和3大版本二进制数据格式不兼容的情况，意味着你会付出巨大的代价来upgrade整个系统（个人还是偏好json这种文本数据协议）。
7.  第6点已经提到了，谨慎选择二进制数据协议，一旦不兼容意味着你的数据都会面临很尴尬的处境：要么一辈子不升级，要么要写一整套东西来migration、verify数据，头不大么？个人还是偏好json这类文本数据协议。
8.  （对任何语言都）善用assert，除非你面对kpi的压力太大，否则总是让自己的app fast fail（包括production环境），有任何无法容忍的错误都直接assert false，这样可以帮你快速发现并解决错误。production环境去掉assert的最大坏处是如果真的出了bug，你可能根本无法在本地复现，也就没法解决。敢于production环境去掉assert的team，一定是构建了几乎1:1与真实世界相同的测试环境，其成本也会跟研发成本持平甚至超过。

接下来打算继续用c++重写一些基础模块：json serialization用nlohmann/json，http请求curl，发邮件vmime，交易数据计算用mpdec，来重构整个项目底层。

作者：赵臣又

链接：https://www.zhihu.com/question/21017354/answer/2270327279

来源：知乎

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。