[![Aloxaf](https://pica.zhimg.com/v2-173441e1abe70ebab1c490a643149b1f_l.jpg?source=1940ef5c)](https://www.zhihu.com/people/aloxaf)

[Aloxaf](https://www.zhihu.com/people/aloxaf)

不描述，留给你们想象的空间

337 人赞同了该回答

距第一次翻开 C Primer Plus 已经有六七年了, 书里讲了什么都已经记不太清了.   
然而里面有一句话我印象特别深刻, 直到今天都记得特别清楚:

**自由的代价是永远的警惕!**

  

Rust 是否限制了程序员的能力发挥? 是, 限制了.   
灵活, 速度和安全, 一个语言最多同时拥有其中的两样. Rust 选择了速度和安全, 只能牺牲灵活了.

因为:

1. 程序员水平不一, 让他们随意发挥不知道能写出什么代码来. 你可能觉得这种写法没有问题, 但是编译器: 我不要你觉得, 我要我觉得.

迭代 vector 的同时修改 vector 导致[迭代器](https://www.zhihu.com/search?q=%E8%BF%AD%E4%BB%A3%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A967096043%7D)失效的坑, 不知道有多少 C++ 程序员踩过? 然而这种代码在 Rust 里根本通过不了编译. 还有什么 use after free, double free, dangling pointer...blabla 之类的坑在 safe Rust 里你想踩都没有机会踩.   
因为(臭名昭著的)(划掉) Borrow Checker 限制了你要么拥有一个[可变引用](https://www.zhihu.com/search?q=%E5%8F%AF%E5%8F%98%E5%BC%95%E7%94%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A967096043%7D), 要么拥有若干个不可变引用; [所有权机制](https://www.zhihu.com/search?q=%E6%89%80%E6%9C%89%E6%9D%83%E6%9C%BA%E5%88%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A967096043%7D)防止你在不知不觉中创建拷贝以及使用已经失效了变量; 生命周期检查保证了引用的有效性...

题主好奇的话, 我建议直接阅读 TRPL: [Rust 程序设计语言 简体中文版](https://link.zhihu.com/?target=https%3A//kaisery.github.io/trpl-zh-cn/title-page.html). 听别人说不如直接自己上手学.

2. Rust 目前表达能力还不足, 有时确实会出现"这个地方真的没有问题然而编译器就是觉得有问题"的情况.

比如(臭名昭著的)(再次划掉) Borrow Checker, 早期非常蠢. 现在有了 NLL 以后情况好很多了, 然而还是会碰到不能覆盖的情况: 下面这段明明没啥问题的代码 (来自 [Polonius - Nicholas Matsakis](https://link.zhihu.com/?target=https%3A//nikomatsakis.github.io/rust-belt-rust-2019)) 就无法通过检查.

```rust
fn get_or_insert(
    map: &mut HashMap<u32, String>,
) -> &String {
    match map.get(&22) {
        Some(v) => v,
        None => {
            map.insert(22, String::from("hi"));
            &map[&22]
        }
    }
}
```

又比如经典的"双指针修改链表倒数第 n 个节点"问题: [双指针遍历](https://www.zhihu.com/search?q=%E5%8F%8C%E6%8C%87%E9%92%88%E9%81%8D%E5%8E%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A967096043%7D), 那必然是要两个不可变引用才行. 然而遍历完了我需要使用慢指针来修改节点, 那慢指针得是可变引用才行——矛盾产生了, 你在目前的 safe Rust 中没法写出这样的代码来.

怎么办呢? 只能上 [unsafe](https://www.zhihu.com/search?q=unsafe&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A967096043%7D) 了......

  

总而言之, Rust 确实限制了程序员的能力, 然而大部分限制都是合理的. 对与我这种一方面想要极致速度, 一方面又觉得自己水平太差, 做不到"注意一点"就能避开 C/C++ 中的坑的平庸程序员, Rust 简直太棒了 (而且想作死用 unsafe 就行了 (误

[编辑于 2020-01-09 21:46](https://www.zhihu.com/question/364980266/answer/967096043)