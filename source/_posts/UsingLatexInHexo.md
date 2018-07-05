---
title: 在Hexo使用Latex写公式
date: 2018-05-16 08:36:19
tags: [Hexo, Latex, Mathjax]
mathjax: true
---
如何在你的`Markdown`里加入优雅的数学公式？
<!-- more -->

### 在Hexo使用Latex写公式

最近在看《线性代数》，想用`Markdown`记一些笔记，要画不少公式，`Typera`完美兼容`Latex`， 一直用得很爽~~~直到我想把笔记移植到我的Hexo博客上时……

---

### Latex的安装

第一步就挂了……`copy`过来发现全是代码，根本没有转化成公式，猜到会有引擎这么个玩意儿，就去查怎么安装，推荐比较多的是用`Mathjax`引用`js`文件就可以的，这里给个官方的说明文档：[Mathjax](http://docs.mathjax.org/en/latest/index.html)

我知道你不会傻傻去看英文文档的，那就简单点，直接给重点吧：

```html
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}
});
</script>
<script type="text/javascript" async src="path-to-mathjax/MathJax.js?config=TeX-AMS_CHTML"></script>
```



`Config`里配置的是行内公式的格式，用`\(`和`\)`的话需要对`\`转义(___转义的坑有很多，后面会提到___)

不过这里有个问题`path-to-mathjax`这个路径是你的`Mathkax.js`文件的位置，然而你并没有这个文件，所以我偷个懒，直接用了别人的~  把这里的`src`改成`https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML`。

把这段代码拷到你的`theme`文件夹里公共的配置`js`的文件里，一般来说就是`casper`目录下的`post.ejs`或`head.ejs`文件里。

后来在文档里找到了需要转义的问题根源：

> Another source of difficulty is when MathJax is used in content management systems that have their own document processing commands that are interpreted before the HTML page is created. For example, many blogs and wikis use formats like [Markdown](http://docs.mathjax.org/en/latest/reference/glossary.html#term-markdown) to allow you to create the content of your pages. In Markdown, the underscore is used to indicate italics, and this usage will conflict with MathJax’s use of the underscore to indicate a subscript. Since Markdown is applied to the page first, it will convert your subscript markers into italics (inserting `<i>` or `<em>` tags into your mathematics, which will cause MathJax to ignore the math).

### 常用的公式

学一点记一点，以后再用到也就免得再去查了

#### 下标的书写

在纯`html`的任何`text`性质的文本中，可以直接这样写：

- `$ A_1 $`或`$ A_{12} $`

  展示效果：

  $ A\_1 $ 、$ A_{12} $

__注意__有时候，__`_`默认作为`Markdown`的斜体语法字符__，会出现这样的情况：

- `$ A_1A_{a_2} $`

  展示效果：

  $ A_1A_{a_2} $

  很明显出了问题，查看了一下网页源代码，才发现，`_`被`Markdown`解析成了`<em>`斜体标识

  ```Zsh
  $ A<em>1A</em>{a_2} $
  ```

  当出现`_{`的组合时，这个`_`之前若有未经转义的`_`，`Markdown`会将这两个`_`解析成斜体标识，对前面的`_`进行转义就可以了：

  `$ A\_1A_{a_2} $`

  不过遇到复杂的式子实在分不清要对哪一个进行转义，所以目前复杂的式子我是对所有`_`都进行转义的

#### 上标的书写

上标用`^`符号，不涉及转义，如：

- `$ n^2 $`或`$ n^{2a} $`

  展示效果：

  $ n^2 $、$ n^{2a} $

#### 比较符号的书写

常用的比较符号包括：大于、小于、等于、约等于、不等于、大于等于、小于等于，大于、小于、等于可以直接用键盘输入，其它的书写方式如下：

- `$ \thickapprox $`、 `$ \ne $` 、`$ \ge $`、`$ \le $`

  展示效果：

  $ \thickapprox $、 $ \ne $ 、$ \ge $、$ \le $

#### 常用函数的书写

- 累加（求和）函数：

  - `$ \sum $`、`$ \sum_{i=1}^n $`

    展示效果：

    $ \sum $、$ \sum_{i=1}^n $

    __`注意：`__如果想要展现成上下结构，需要写成单行模式：`$$ \sum_{i=1}^n $$`

    $$ \sum_{i=1}^n $$

    以下同理

- 累乘（求积）函数：

  - `$ \prod $`、`$ \prod_{i=1}^n $`

    展示效果：

    $ \prod $、$ \prod\_{i=1}^n $

- 极限：

  - `$ \lim_{x \rightarrow 0}\frac{\sin x}{x}=1 $`

    效果展示：

    $ \lim\_{x \rightarrow 0}\frac{\sin x}{x}=1 $

- 分数形式：

  - `$ \frac ab $`、`$ \frac {a+b}{c+d} $`

    效果展示：

    $ \frac ab $、$ \frac {a+b}{c+d} $

#### 特殊格式语法的书写

- 线性代数常用公式：矩阵、行列式

  - 矩阵：

    ```latex
     \left [ 
     \begin{matrix} 
     n\_{11} & n\_{12} \\\\ 
     n\_{21} & n\_{22} 
     \end{matrix} 
     \right ] 
    ```

    效果展示：

    $ \left [ \begin{matrix} n\_{11} & n\_{12} \\\\ n\_{21} & n\_{22} \end{matrix} \right ] $