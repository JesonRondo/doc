# HTML Guide

#### 缩进

**使用两个空格**

不要使用 tab 或者混用 tab 和空格的方式作为缩进。

```html
    <ul>
      <li>Fantastic
      <li>Great
    </ul>
    .example {
      color: blue;
    }
```

#### 大小写

**只使用小写**

所有代码只使用小写：包括标签、选择器、属性及属性值（text/`CDATA` 例外，作为内容时例外）。

```html
    <!-- 不推荐 -->
    <A HREF="/">Home</A>

    <!-- 推荐 -->
    <img src="google.png" alt="Google">
```

#### 尾部空格

**删除多余的尾部空格**

尾部空格是多余的，可能会引起代码混乱。

```html
    <!-- 不推荐 -->
    <p>What?_

    <!-- 推荐 -->
    <p>Yes please.
```

#### 编码

**使用 UTF-8 (no BOM) 编码**

确保你的编辑器文档编码为 UTF-8，没有字节顺序标记。

在 HTML 中使用 `<meta charset="utf-8">` 置顶文档编码，在 CSS 中默认就是 UTF-8 编码，不需要特别指定。

（更多编码和指定方式的资料可以参见[Character Sets & Encodings in XHTML, HTML and CSS](http://www.w3.org/International/tutorials/tutorial-char-enc/en/all.html)）

#### 注释

**根据需要，给代码做注释**

用注释解释代码：它实现了什么功能，它的目的是什么，为什么这种方案更好？

（注释代码不是强制要求，视乎项目性质和复杂程度）

#### 待办事项

**使用 `TODO` 关键词标识待办事项**

只使用 `TODO` 关键词标识待办事项，而不用 `@@` 等其他格式。


#### 文档类型

**使用 HTML5**

HTML5 推荐所有 HTML 文档使用 `<!doctype html>`。

（由于缺少浏览器基础支持，优化空间比 HTML 小。推荐使用文档类型为 `text/html` 的 HTML，而非文档类型为 [`application/xhtml+xml`](http://hixie.ch/advocacy/xhtml) 的 XHTML。）

不要闭合空标签，例如：要 `<br>`，而非 `<br />`，尽管对于 HTML 两者都可以。

#### HTML 校验

**使用经校验的 HTML 代码**

使用经校验的 HTML 代码，否则很难达到性能上的提升。

使用 [W3C HTML validator](http://validator.w3.org/nu/) 等工具去校验代码。

HTML 代码有效性是重要的质量衡量标准，并可确保 HTML 代码可以正确使用。

```html
    <!-- 不推荐 -->
    <title>Test</title>
    <article>This is only a test.

    <!-- 推荐 -->
    <!doctype html>
    <meta charset="utf-8">
    <title>Test</title>
    <article>This is only a test.</article>
```

#### 语义化

**使用 HTML 要符合语义**

符合本义地使用元素（标签），比如在头部区域使用 heading 元素，使用 `p` 标签表示段落，使用 `a` 标签表示链接等。

语义化地使用 HTML 有助于网页的可访问性，复用性和提高代码效率。

```html
    <!-- 不推荐 -->
    <div onclick="goToRecommendations();">All recommendations</div>

    <!-- 推荐 -->
    <a href="recommendations/">All recommendations</a>
```

#### 多媒体备选内容

**为多媒体提供备选内容**

对于多媒体，如图像，视频，通过 `canvas` 读取的动画元素，确保提供备选方案。对于图像使用有意义的备选文案（ `alt` ）对于视频和音频使用有效的副本和文案说明。

提供备选内容在网页可访问性上是很重要的：给盲人用户以一些提示性的文字，用 `@alt` 告诉他这图像是关于什么的，给可能没理解视频或音频的内容的用户以提示。

（图像的 `alt` 属性会产生冗余，对于不是在 CSS 中引用的非内容图像，就不要使用 `alt` 描述了。）

```html
    <!-- 不推荐 -->
    <img src="spreadsheet.png">

    <!-- 推荐 -->
    <img src="spreadsheet.png" alt="Spreadsheet screenshot.">
```

#### 转义符

**不要使用转义符**

不需要使用类似 `&amp;mdash;` 、 `&amp;rdquo;` 和 `&amp;#x263a;`等的转义符，假定团队之间所用的文件和编辑器是同一编码（UTF-8）。

在 HTML 文档中具有特殊含义的字符（例如 `<` 和 `&` )为例外，还有 “不可见” 字符（例如 no-break 空格）。

```html
    <!-- 不推荐 -->
    The currency symbol for the Euro is &amp;ldquo;&amp;eur;&amp;rdquo;.

    <!-- 推荐 -->
    The currency symbol for the Euro is “€”.
```

#### 引用类型

**在 style 和 scitpt 标签中省略 `type` 属性**

不要在 style 和 scitpt 标签中省略 `type` 属性（除非标签中引用的不是 CSS 或者 JavaScript），

HTML5 默认使用 [`text/css`](http://www.whatwg.org/specs/web-apps/current-work/multipage/semantics.html#attr-style-type) 和 [`text/javascript`](http://www.whatwg.org/specs/web-apps/current-work/multipage/scripting-1.html#attr-script-type)，因此声明引用类型是不必要的，对于较老的浏览器也同样适用。

```html
    <!-- 不推荐 -->
    <link rel="stylesheet" href="//www.google.com/css/maia.css"
      type="text/css">

    <!-- 推荐 -->
    <link rel="stylesheet" href="//www.google.com/css/maia.css">

    <!-- 不推荐 -->
    <script src="//www.google.com/js/gweb/analytics/autotrack.js"
      type="text/javascript"></script>

    <!-- 推荐 -->
    <script src="//www.google.com/js/gweb/analytics/autotrack.js"></script>
```

#### 格式

**每个块元素、列表元素或表格元素都独占一行，每个子元素都相对于父元素进行缩进。**

> [http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml](http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml)
