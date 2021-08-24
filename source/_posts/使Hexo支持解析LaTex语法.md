---
title: 使Hexo支持解析LaTex语法
date: 2021-8-23
mathjax: true
tags: 
    心得
excerpt: 由于在写博客的过程中，不可避免的需要使用数学公式，LaTeX可以让我们在博客里使用这些数学符号，它有着特定的语法。但是今天发现Hexo搭建的博客是不支持LaTex，需要自行安装插件解决。下面为解决LaTex语法支持的过程。
---



由于在写博客的过程中，不可避免的需要使用数学公式，[LaTeX](https://www.latex-project.org/) 可以让我们在博客里使用这些数学符号，它有着特定的语法。但是今天发现Hexo搭建的博客是不支持LaTex，需要自行安装插件解决。下面为解决LaTex语法支持的过程。


## **安装插件**

首先需要安装 [hexo-math](https://github.com/hexojs/hexo-math) 插件，该插件（plugin）可支持使用 [MathJax](https://www.mathjax.org/) 或 [KaTeX](https://katex.org/) 来实现 LaTeX 排版系统，进而在网页上渲染出数学表达式。

```shell
# 首先进入hexo博客根目录 
$ cd /blog
# 安装 hexo ； --save 参数会让 npm 在安装 hexo-math 之后自动将它写入 package.json 文件里，以便之后多电脑同步时使用
$ npm install hexo-math --save
```

注意，如果本身已经安装有`hexo-math`，需要先卸载`hexo-math`，卸载命令：

```shell
$ npm uninstall hexo-math --save
```

卸载默认 `markdown` 渲染引擎 `hexo-renderer-marked`；若不卸载，会和新的引擎发生冲突（conflict）

```shell
$ npm uninstall hexo-renderer-marked --save
```

安装新引擎 `hexo-renderer-kramed` 

```shell
$ npm install hexo-renderer-kramed --save
```



## **解决语义冲突**

由于LaTex与Markdown语法存在冲突，因此还需要对`kramed`默认的语法规则进行修改，否则排版仍然会有问题。

打开`~/blog/node_modules\kramed\lib\rules\inline.js`文件。

将文件中的第11行的`escape`变量的值更改为：

```js
escape: /^\\([`*\[\]()#$+\-.!_>])/,
```

将第20行的`em`变量的值更改为：

```js
em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```

在`blog`所在的根目录下面的`_config.yml`文件中添加：

```yaml
# MathJax
math:
  engine: 'mathjax'
  mathjax:
    src: https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-MML-AM_CHTML

```

最后，清除缓存，重新生成。

```shell
$ hexo clean
$ hexo g
$ hexo s
```

## **查看效果**

矩阵LaTex代码：

```latex
$$
A = \begin{bmatrix}
        a_{11}    & a_{12}    & ...    & a_{1n}\\
        a_{21}    & a_{22}    & ...    & a_{2n}\\
        a_{31}    & a_{22}    & ...    & a_{3n}\\
        \vdots    & \vdots    & \ddots & \vdots\\
        a_{n1}    & a_{n2}    & ... & a_{nn}\\
    \end{bmatrix} , b = \begin{bmatrix}
        b_{1}  \\
        b_{2}  \\
        b_{3}  \\
        \vdots \\
        b_{n}  \\
    \end{bmatrix}
$$

```



矩阵表示：

$$ A = \begin{bmatrix}        a_{11}    & a_{12}    & ...    & a_{1n}\\        a_{21}    & a_{22}    & ...    & a_{2n}\\        a_{31}    & a_{22}    & ...    & a_{3n}\\        \vdots    & \vdots    & \ddots & \vdots\\        a_{n1}    & a_{n2}    & ... & a_{nn}\\    \end{bmatrix} , b = \begin{bmatrix}        b_{1}  \\        b_{2}  \\        b_{3}  \\        \vdots \\        b_{n}  \\    \end{bmatrix} $$



贝叶斯公式LaTex代码：

```latex
 P(A_i \mid B) = \frac{P(B\mid A)P(A_i)}{\sum_{j=1}^{n}P(A_j)P(B \mid A_j)} 
```

贝叶斯公式：

$$ P(A_i \mid B) = \frac{P(B\mid A)P(A_i)}{\sum_{j=1}^{n}P(A_j)P(B \mid A_j)} $$

