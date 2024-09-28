[![HD Superman](media/HD_Superman.webp)](https://markdowner.net/user/29024678689374208)[HD Superman](https://markdowner.net/user/29024678689374208)

·

1 年前

举报

![](media/j04mqz-a4X4vg1.png)

## [](https://markdowner.net/article/235800197744746496#%E8%83%8C%E6%99%AF)背景

码道人的编辑器经过几次重构，最原始的编辑器是分屏模式，也就是国内外大部分技术社区的做法，一边编辑，一边渲染显示 markdown，最开始的 [LaTeX](https://en.wikipedia.org/wiki/LaTeX) 公式渲染方案采用 KaTex 在客户端实现。由于出现较多排版错乱问题，公式渲染的需求一度被砍掉。在不少用户强烈要求支持的情况下，我们打算在重构编辑器的同时重新考虑 LaTeX 公式渲染的实现。

梳理出来的需求主要有如下几个：

-   支持大部分 LaTeX 语法，公式显示正常、美观
-   支持在小程序端、app 端显示公式
-   支持暗黑模式，多种颜色配置
-   支持 inline 和 block 公式
-   公式渲染速度、访问速度、请求响应速度要快，不能有明显延迟

## [](https://markdowner.net/article/235800197744746496#%E6%8A%80%E6%9C%AF%E9%80%89%E5%9E%8B)技术选型

### [](https://markdowner.net/article/235800197744746496#%E6%B5%8F%E8%A7%88%E5%99%A8%E7%AB%AF%E5%AE%9E%E7%8E%B0%E8%BF%98%E6%98%AF%E6%9C%8D%E5%8A%A1%E7%AB%AF%E5%AE%9E%E7%8E%B0%EF%BC%9F)浏览器端实现还是服务端实现？

浏览器端的实现主要通过 KaTeX 或 MathJax 渲染库实时在前端将 LaTeX 公式转换为 SVG 或者 [MathML](https://en.wikipedia.org/wiki/MathML) 等 html 字符。

服务端实现主要通过将 LaTeX 公式转换为路径参数，服务端返回 SVG 图片的方式实现，比如使用下面的图片链接：

```
https://math.markdowner.net/math?color=%233b82f6&inline=E%20%3D%20mc%5E2
```

渲染成 img 标签：

![](media/math-4.svg)

我们可以看看支持 LaTeX 的平台都是怎么做的：

平台

渲染终端

渲染方案

维基百科

服务端渲染

MathJax

stackexchange

客户端渲染

MathJax

Quora

客户端渲染

MathJax

知乎

服务端渲染

MathJax

腾讯文档

客户端渲染

MathJax

CSDN

客户端渲染

KaTeX

掘金

客户端渲染

KaTeX

服务端渲染的优点

-   代码体积小：无需在客户端安装 KaTeX 或 MathJax 代码，减少代码包体积
-   多端兼容性好：在 APP 或者小程序场景下只需要 `<img>`标签即可
-   安全：LaTeX 公式都是静态图片显示，无 XSS 安全问题

服务端渲染的缺点：

-   渲染实时性较差：在网络延迟较大时，公式渲染速度较慢

#### [](https://markdowner.net/article/235800197744746496#%E4%B8%80%E4%B8%AA%E5%AE%89%E5%85%A8%E9%97%AE%E9%A2%98)一个安全问题

如果你用过 [Typora](https://typora.io/)，可以试下在公式块上写一段代码：

```latex
\href{javascript:alert('xss attack')}{MaliciousLink}
```

显示如下：

![](media/x5Ww2e-adWNn9P.png)

点击蓝色链接，跳出弹窗：

![](media/1jEvkA-wy1kROQ.png)

我们成功利用 MathJax 漏洞构造了一个 [XSS](https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%B6%B2%E7%AB%99%E6%8C%87%E4%BB%A4%E7%A2%BC) 攻击。这个漏洞已经披露过了，可以在[这里](https://snyk.io/vuln/SNYK-JS-MATHJAX-451470)看到。MathJax 小于 2.7.4 的版本都有这个问题。Typora 的 LaTeX 公式支持就是通过 MathJax 在本地端渲染实现的，可以看到在客户端渲染容易出现一些安全问题。

先前的方案就是客户端 KaTeX 渲染，为了兼容性、安全、代码包体积考虑，最终采用服务端渲染的方案，通过 [CDN](https://en.wikipedia.org/wiki/Content_delivery_network) + [Serverless](https://en.wikipedia.org/wiki/Serverless_computing) 加快响应速度，弥补缺点。

### [](https://markdowner.net/article/235800197744746496#%E9%80%89%E6%8B%A9-mathjax-%E8%BF%98%E6%98%AF-katex%EF%BC%9F)选择 MathJax 还是 KaTeX？

在线调试工具：[mathjax demo](https://www.mathjax.org/#demo)，[katex demo](https://katex.org/)

#### [](https://markdowner.net/article/235800197744746496#%E6%B8%B2%E6%9F%93%E9%80%9F%E5%BA%A6)渲染速度

详细对比可以看这里：[katex-mathjax-comparison](https://www.intmath.com/cg5/katex-mathjax-comparison.php)

指标

KaTex

MathJax2.7

MathJax3

Process Time

104ms

1679ms

38ms

DOMContentLoaded

104ms

7ms

2ms

Fonts loaded

195ms

1665ms

639ms

Page complete

1128ms

1729ms

1176ms

先前 MathJax 一直被吐槽的一个点就是公式渲染速度太慢，不如 KaTeX，但可以看到在 MathJax3 版本之后渲染速度已经有了明显的提示。

#### [](https://markdowner.net/article/235800197744746496#katex-%E6%B8%B2%E6%9F%93%E7%9F%A9%E9%98%B5)KaTeX 渲染矩阵

![](media/6Wy4MV-aWBXMaQ.png)

#### [](https://markdowner.net/article/235800197744746496#mathjax-%E6%B8%B2%E6%9F%93%E7%9F%A9%E9%98%B5)MathJax 渲染矩阵

![](media/dXQoyD-RQQDrkW.png)

MathJax 在 LaTeX 支持上比 KaTeX 丰富得多，而且更加美观，可以看上图对比，KaTeX 在渲染矩阵的时候有符号重叠。前面的表格可以看出大多数网站都是采用 MathJax 的方案，所以我们最终也选择MathJax 作为公式渲染引擎。

### [](https://markdowner.net/article/235800197744746496#%E6%9C%80%E7%BB%88%E6%8A%80%E6%9C%AF%E6%A0%88)最终技术栈

-   serverless：函数计算服务，降低服务端成本，简化服务部署
-   CDN：内容分发网络，提供 LaTex 图片缓存，同时降低网络延迟
-   nodejs：服务端运行环境
-   express.js：nodejs 下的 web 框架，实现 rest 接口
-   mathjax：提供 LaTex 代码转 svg 图片支持

### [](https://markdowner.net/article/235800197744746496#%E6%8A%80%E6%9C%AF%E4%BE%9B%E5%BA%94%E5%95%86)技术供应商

阿里云的 CDN 和函数计算服务，其中 CDN 是全球加速，函数计算服务挂载在中国大陆区域（避免你懂的网络问题）。实现原理图如下：

![](media/DAevdo-RgzPme5.png)

## [](https://markdowner.net/article/235800197744746496#%E4%BB%A3%E7%A0%81%E5%AE%9E%E7%8E%B0)代码实现

相关 npm 依赖：

```json
{
 "dependencies": {
    "body-parser": "^1.19.0",
    "corser": "^2.0.1",
    "express": "^4.17.1",
    "helmet": "^4.4.1",
    "mathjax-full": "^3.1.2"
  }
}
```

可以看到如果要有比较好的 LaTeX 语法支持，需要 [mathjax-full](https://www.npmjs.com/package/mathjax-full) 这个包，未解压的体积就达到 27.5 MB，如果放在前端代码中，就过于臃肿了。

由于代码相对简单，直接贴上来，总共两个文件：app.js 和 adaptor.js，由于阿里云 nodejs 模板不能很好支持 es6 语法，代码简单我们就不去配置支持了，不用 `import` 语法，直接用 `require` 导入包：

app.js 文件

```js
var express = require('express');
var helmet = require('helmet');
var bodyParser = require('body-parser');
var tex2svg = require('./adaptor');

var app = express();

app.use(helmet());
app.use(bodyParser.urlencoded({ extended: false }));

app.get("/math", async function (req, res, next) {
  const mode = (Object.keys(req.query).includes("from") || Object.keys(req.query).includes("block")) 
    ? "block"
    : Object.keys(req.query).includes("inline")
    ? "inline"
    : null;
  if (!mode) {
    return next();
  }
  const isInline = mode === "inline";
  const equation = isInline
    ? (req.query.inline)
    : (req.query.from || req.query.block);
  if (!equation || equation.match(/\.ico$/)) {
    return next();
  }

  const color = req.query.color;
  const alternateColor = req.query.alternateColor;
  if (
    (color && /[^a-zA-Z0-9#]/.test(color)) ||
    (alternateColor && /[^a-zA-Z0-9#]/.test(alternateColor))
  ) {
    return next();
  }

  const normalizedEquation = equation.replace(/\.(svg|png)$/, "");

  try {
    const svgString = tex2svg(
      normalizedEquation,
      isInline,
      color,
      alternateColor
    );
    // ont month
    res.setHeader("Cache-Control", "public, max-age=2592000");
    res.contentType("image/svg+xml");
    res.write(`<?xml version="1.0" standalone="no" ?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.0//EN" "http://www.w3.org/TR/2001/REC-SVG-20010904/DTD/svg10.dtd">
`);

    res.end(svgString);
  } catch (err) {
    res.status(500);
    res.write(
      '<svg xmlns="http://www.w3.org/2000/svg"><text x="0" y="15" font-size="15">'
    );
    res.write(err);
    res.end("</text></svg>");
  }
});

module.exports = app;
```

上述代码主要完成几个功能：

-   SVG 文件标准化
-   SVG 文件缓存 header 设置
-   LaTeX 公式提取和 SVG 字符串转换
-   提取路径参数颜色配置值

adaptor.js 文件

```js
var { mathjax } = require('mathjax-full/js/mathjax');
var { TeX } = require('mathjax-full/js/input/tex');
var { SVG } = require('mathjax-full/js/output/svg');
var { LiteAdaptor } = require('mathjax-full/js/adaptors/liteAdaptor');
var { RegisterHTMLHandler } = require('mathjax-full/js/handlers/html');
var { AllPackages } = require('mathjax-full/js/input/tex/AllPackages');

// MathJax bootstrap
const adaptor = new LiteAdaptor();
RegisterHTMLHandler(adaptor);

const html = mathjax.document('', {
  InputJax: new TeX({ packages: AllPackages }),
  OutputJax: new SVG({ fontCache: 'none' }),
});

function tex2svg(equation, isInline, color, alternateColor) {
  const svg = adaptor
    .innerHTML(html.convert(equation, { display: !isInline }))
    .replace(
      /(?<=<svg.+?>)/,
      `
<style>
  * {
    fill: ${color || "black"};
  }
  @media (prefers-color-scheme: dark) {
    * {
      fill: ${alternateColor || color || "black"};
    }
  }
</style>`
    );
  if (svg.includes('merror')) {
    return svg.replace(/<rect.+?><\/rect>/, '');
  }
  return svg;
}

module.exports = tex2svg;
```

这个文件主要实现 LaTeX 转 SVG 字符串，同时在 SVG 文件中注入颜色配置相关的样式，支持通过 alternateColor 参数配置暗黑模式下的公式颜色（@media (prefers-color-scheme: dark) 写法）。

## [](https://markdowner.net/article/235800197744746496#%E6%9C%80%E7%BB%88%E6%95%88%E6%9E%9C)最终效果

### [](https://markdowner.net/article/235800197744746496#%E6%94%AF%E6%8C%81inline-%E5%92%8C-block-%E5%85%AC%E5%BC%8F)支持inline 和 block 公式

内联公式：![math](media/math-4.svg)

公式块：

![math](media/math-2.svg)

### [](https://markdowner.net/article/235800197744746496#%E6%94%AF%E6%8C%81%E6%98%BE%E7%A4%BA%E9%A2%9C%E8%89%B2%E9%85%8D%E7%BD%AE)支持显示颜色配置

通过路径参数`color` 配置可以实现不同颜色的公式显示，可也通过 `alternateColor` 参数设置暗黑模式下的显示颜色，在操作系统切换暗黑模式时自动变化颜色。

![](media/math.svg)

![](media/math-3.svg)

![](media/math-5.svg)

![](media/math-1.svg)

### [](https://markdowner.net/article/235800197744746496#svg-%E5%9B%BE%E7%89%87%E7%BC%93%E5%AD%98%E6%8F%90%E9%AB%98%E5%93%8D%E5%BA%94%E9%80%9F%E5%BA%A6)SVG 图片缓存提高响应速度

一方面我们通过 http header 设置图片缓存，避免同一客户端重复发出请求，另一方面，我们通过 CDN 缓存 nodejs 返回的图片，加速 SVG 图片在不同区域的访问速度。

我们取10个简单公式和10个复杂公式做下测试，简单统计平均下响应时间：

简单公式

复杂公式

首次请求时间

518ms

514ms

缓存请求时间

29ms

30ms

可以看到复杂公式和简单公式在服务端处理的响应时间差距不大，基本都在 500ms 左右，经过 CDN 缓存后的请求缩短到 30ms 左右，提速较为明显。

## [](https://markdowner.net/article/235800197744746496#%E6%80%BB%E7%BB%93)总结

本文介绍了一种 LaTeX 公式服务端渲染的方法，分析了不同渲染方案的优缺点，通过 Serverless + CDN 简化服务部署，同时提高响应速度。

[express](https://markdowner.net/tag/express)[latex](https://markdowner.net/tag/latex)[nodejs](https://markdowner.net/tag/nodejs)[serverless](https://markdowner.net/tag/serverless)[mathjax](https://markdowner.net/tag/mathjax)

2.4 k 次查看