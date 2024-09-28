WebAssembly ⼆进制⽂件依赖 Web 浏览器的 JavaScript 引擎来执⾏，需要独⽴的 WebAssembly 运⾏时才能在⾮ Web 浏览器中运⾏ WebAssembly 代码。美国佐治亚大学的论文[《How Far We’ve Come – A Characterization Study of Standalone WebAssembly Runtimes》](https://cobweb.cs.uga.edu/~wenwen/papers/iiswc2022.pdf)构建了一个标准的 WABench 的基准套件，对独立的 WebAssembly 运行时进行了全面的表征研究，包含性能、内存开销和架构特征。分析了`33 个独⽴ WebAssembly 运⾏时`的TOP5，发现这些独立运⾏时在运⾏ WebAssembly ⼆进制⽂件时平均会`降低 1.59 到 9.57 倍的性能`。

通常有两种执行 WebAssembly 代码的方法：`解释型和 JIT（SinglePass, Cranelift, LLVM）`。WebAssembly 独立运行时的标准：

-   该运行时是一个独立的 WebAssembly 运行时，支持使用 WASI 编译的 WebAssembly 二进制代码。
-   运行时足够成熟，可以运行广泛的 WebAssembly 应用程序。
-   运行时随着 WebAssembly 和 WASI 的发展而积极开发和维护。

论文研究了符合以上标准的 WebAssembly 独立运行时 TOP5：`Wasmtime（Rust，JIT）、WAVM（C/C++，JIT）、Wasmer（Rust，JIT）、Wasm3（C，解释型）、WAMR (C, 解释型)`。

## 论文研究的发现

![](media/screenshot-20230202-001657.jpg) ![](media/screenshot-20230202-003158.jpg)

-   与原生执行相⽐，所有五个独⽴的 WebAssembly 运⾏时在执⾏ WebAssembly ⼆进制⽂件时都会引⼊额外的性能开销。平均性能下降范围从`1.59× (Wasmer) 到9.57× (WAMR)`。
-   在三个 WebAssembly JIT 编译器中，Cranelift 和 LLVM 对于不同的基准程序集具有最佳性能。与 SinglePass 相⽐，Cranelift 的性能平均提速为`1.74×` ，⽽ LLVM 的提速为`1.43×`。
-   AOT 编译对 WAVM 的性能（`1.73×性能加速`）有很⼤影响，⽽对 Wasmtime （`1.02×加速`）和 Wasmer（`1.02×加速`）的影响⾮常有限。
-   WebAssembly 编译器优化可以为不同的 WebAssembly 运⾏时带来相当⼤的性能改进，性能加速`1.44× ‒ 3.57×`。
-   运⾏ WebAssembly ⼆进制⽂件时，独⽴ WebAssembly 运⾏时消耗的内存资源是原生执⾏的 `1.26x‒ 5.50x`。
-   WebAssembly 运⾏时不仅执⾏更多的机器指令，即平均为原生执⾏的 `2.03x‒ 14.61x`，⽽且还表现出⽐原生执⾏更⾼的每周期指令（Instructions per cycle, 即 IPC）值。
-   WebAssembly 运⾏时表现出⽐原生执⾏更多的分⽀预测未命中率，范围从`1.52× 到 12.64×`，但它们的分⽀预测未命中率通常`⾮常接近原生执⾏`。
-   与原生执⾏相⽐， WebAssembly 运⾏时的`平均缓存未命中率为1.39x⾄4.60x`，⽽它们的缓存未命中率通常相似。

下表是论文作者研究构建的 WABench 基准套件中包含的基准程序。

![](media/screenshot-20230202-000852.jpg)

## References

-   [WABench 基准套件](https://github.com/dunnock/wabench)
-   [Wasmtime](https://github.com/bytecodealliance)
-   [Wasmer](https://wasmer.io/)
-   [Wasm3 Labs](https://github.com/wasm3)
-   [WAMR](https://github.com/bytecodealliance/wasm-micro-runtime)
-   [WAVM](https://github.com/WAVM/WAVM)

[WebAssembly](https://unbug.github.io/tags.html#webassembly) [Rust](https://unbug.github.io/tags.html#rust)

## Releated

1.  ###### [一分钟读论文：《我们走了多远——WebAssembly 运行时的全面特征研究》](https://unbug.github.io/How-Far-We-ve-Come-A-Characterization-Study-of-Standalone-WebAssembly-Runtimes/)
    
    In [FrontEnd](https://unbug.github.io/categories.html#frontend)
2.  ###### [一分钟读论文：《评估消除 JS 死代码对移动网页性能的影响》](https://unbug.github.io/assessing-the-impact-of-JavaScript-dead-code-elimination-on-mobile-web-performance/)
    
    In [FrontEnd](https://unbug.github.io/categories.html#frontend), [Performance](https://unbug.github.io/categories.html#performance)
3.  ###### [一分钟读论文：《重新思考移动客⼾端的网页缓存》](https://unbug.github.io/Rethinking-Client-Side-Caching-for-the-Mobile-Web/)
    
    In [FrontEnd](https://unbug.github.io/categories.html#frontend), [Performance](https://unbug.github.io/categories.html#performance)
4.  ###### [一分钟读论文：《通过 JS 分类即时加速移动网页》](https://unbug.github.io/To-Block-or-Not-to-Block-Accelerating-Mobile-Web-Pages-On-The-Fly-Through-JavaScript-Classification/)
    
    In [Performance](https://unbug.github.io/categories.html#performance), [FrontEnd](https://unbug.github.io/categories.html#frontend)
5.  ###### [一分钟读论文：《StackOverflow 上 JS 代码片段规则违规的挖掘》](https://unbug.github.io/Mining-Rule-Violations-in-JavaScript-Code-Snippets/)
    
    In [FrontEnd](https://unbug.github.io/categories.html#frontend)
6.  ###### [一分钟读论文：《要不要上 TypeScript？GitHub 上 JS 和 TS 应用软件质量的系统比较》](https://unbug.github.io/On-A-Systematic-Comparison-of-the-Software-Quality-of-JavaScript-and-TypeScript-Applications-on-GitHub/)
    
    In [FrontEnd](https://unbug.github.io/categories.html#frontend)
7.  ###### [一分钟读论文：《关于（未）采用 JavaScript 前端框架的研究》](https://unbug.github.io/On-the-(Un-)Adoption-of-JavaScript-Front-end-Frameworks/)
    
    In [FrontEnd](https://unbug.github.io/categories.html#frontend), [Architecture](https://unbug.github.io/categories.html#architecture)
8.  ###### [一分钟读论文：《WebAssembly 与 JS 在移动设备上的能耗对比》](https://unbug.github.io/Comparing-the-Energy-Efficiency-of-WebAssembly-and-JavaScript-in-Web-Applications-on-Android-Mobile-Devices/)
    
    In [Performance](https://unbug.github.io/categories.html#performance)

![Unbug](media/Unbug.jpg)

##### Written by Unbug [Follow](https://twitter.com/unbug)

「一分钟读论文」把论文当成你的精神食粮，让学术重塑你的思维。转载请注明出处，有收获请赏🥤