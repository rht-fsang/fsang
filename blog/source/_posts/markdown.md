---
title: markdown语法笔记
date: 2022-09-01 09:33:32
tags: markdown
categories: 前端
top: 100
---

## 标题语法

要创建标题，需要在单词或短语前面添加井号（#）。# 的数量代表了标题的级别。例如添加三个#表示创建一个三级标题（h3）

<!--more-->
可选语法

还可以在文本下方添加任意数量的===号来标识一级标题，或者--号来标识二级标题

## 段落语法

要创建段落，请使用空白行将一行或多行文本进行分隔

## 换行语法

在一行的末尾添加两个或多个空格，然后按回车健，即可创建一个换行（）

## 强调语法

通过将文本设置为粗体或斜体来强调其重要性

### 粗体

**要加粗文本**，请在单词短语的前后各添加两个星号或下划线

### 斜体

*要用斜体显示文本，请在单词或短语前后添加一个星号或下划线。先斜体突出单词的中间部分，请在字母前后各添加一个星号，中间不要带空格。*

***要同时用粗体突出显示文本，请在单词或短语前后各添加三个星号或下划线。要加粗并用斜体显示单词或短语的中间部分，请在要突出显示的部分前后各加三个星号，中间不要带空格。***

## 引用语法

>要创建块引用，请在段落前添加一个>符号
>块引用可以包含多个段落
>为段落之间的空白行添加一个>符号
>>块引用可以嵌套。在嵌套的段落前添加一个>>符号
>#块引用可以包含其他Mackdown格式的元素。并非所有元素都可以使用，你需要进行实验以查看哪些元素有效
>-块引用可以包含其他Mackdown格式的元素。并非所有元素都可以使用，你需要进行实验以查看哪些元素有效
>-块引用可以包含其他Mackdown格式的元素。并非所有元素都可以使用，你需要进行实验以查看哪些元素有效
>-***块引用可以包含其他Mackdown格式的元素。并非所有元素都可以使用，你需要进行实验以查看哪些元素有效***
>
## 列表语法

### 有序列表

1. 要创建有序列表，请在每个列表项前添加数字并紧跟一个英文句点。数字不必按数学顺序排列，但是列表应当以数字 1 起始。
1. 要创建有序列表，请在每个列表项前添加数字并紧跟一个英文句点。数字不必按数学顺序排列，但是列表应当以数字 1 起始。
   1. 要创建有序列表，请在每个列表项前添加数字并紧跟一个英文句点。数字不必按数学顺序排列，但是列表应当以数字 1 起始。
   2. 要创建有序列表，请在每个列表项前添加数字并紧跟一个英文句点。数字不必按数学顺序排列，但是列表应当以数字 1 起始。
1. 要创建有序列表，请在每个列表项前添加数字并紧跟一个英文句点。数字不必按数学顺序排列，但是列表应当以数字 1 起始。

### 无序列表

- 要创建无序列表，请在每个列表项前面添加破折号（-）、星号（*）、或加号（+）。缩进一个或多个列表项可创建嵌套列表

- 要创建无序列表，请在每个列表项前面添加破折号（-）、星号（*）、或加号（+）。缩进一个或多个列表项可创建嵌套列表
  - 要创建无序列表，请在每个列表项前面添加破折号（-）、星号（*）、或加号（+）。缩进一个或多个列表项可创建嵌套列表

## 要在保留列表连续性的同时在列表中添加另一种元素，请将该元素缩进四个空格或一个制表符

- 要创建无序列表，请在每个列表项前面添加破折号（-）、星号（*）、或加号（+）。缩进一个或多个列表项可创建嵌套列表
- 要创建无序列表，请在每个列表项前面添加破折号（-）、星号（*）、或加号（+）。缩进一个或多个列表项可创建嵌套列表

- 代码块通常采用四个空格或一个制表符缩进。当它们被放在列表中时，请将它们缩进八个空格或两个制表符\

        <html>
            <head>
        <html/>
- 代码块通常采用四个空格或一个制表符缩进。当它们被放在列表中时，请将它们缩进八个空格或两个制表符

## 代码语法

``要将单词或短语表示为代码，请将其包裹在反引号(`)中``

    ```bash
    <html>
    <head>
        echo helloword
    </head>
    </html>
    ```

***

## 分割线语法

要创建分割下，请在单独一行使用三个或多个星号（***）、（---）、（___），并且不能包含其他内容

## 链接语法

链接文本放在中括号内，链接地址放在后面的括号中，链接title可选
超链接Markdown语法代码：[超链接显示名](超链接地址"超链接title")

    ```
    这是一个链接 [Markdown语法](https://markdown.com.cn)。
    ```

### 渲染效果

这是一个链接[Markdown语法](https://baidu.com "demo")

### 给链接增加Title

链接title是当鼠标悬停在链接上时会出现的文字，这个title是可选的，它放在圆括号中链接地址后面，跟链接地址之间以空格分隔

### 网址和Email地址

使用尖括号可以很方便地把URL或者email地址变成可点击的链接

    ```
    <https://baidu.com>
    <1851353758@qq.com>
    ```

<https://baidu.com>
<1851353758@qq.com>

### 带格式化的链接

强调链接，在链接语法前后增加星号。要将链接表示为代码，请在方括号中添加反引号

    ```
    **[加粗链接](https://baidu.com)**
    *[斜体链接](https://baidu.com)*
    *[`代码链接`](https://baidu.com)*
    ```

**[加粗链接](https://baidu.com)**
*[斜体链接](https://baidu.com)*
*[`代码链接`](https://baidu.com)*

### 引用类型链接

引用样式链接是一种特殊的链接，它使URL在Markdown中更易于显示和阅读。参考样式链接分为两部分：与文本保持内联的部分以及存储在文件中其他位置的部分，以使文本易于阅读

## 图片语法

要添加图像，请使用感叹号（!）,然后再方括号增加替代文本，图片链接放在圆括号里，括号里的链接后可以增加一个可选的图片标题文本。
插入图片Markdown语法代码：`![图片alt](图片链接 "图片title")`

    ![这是图片](/assets/img/philly-magic-garden.jpg "Magic Gardens")
![这是图片](/assets/img/philly-magic-garden.jpg "Magic Gardens")

### 链接图片

给图片增加链接，请将图像的Markdown括在方括号中，然后将链接添加在圆括号中。

    [![沙漠中的岩石照片](/assets/img/shiprock "shiprock")](https://baidu,com)
 [![沙漠中的岩石照片](/assets/img/shiprock "shiprock")](https://baidu.com)

## 转译字符语法

 要显示原本用于格式化Markdown文档的字符，请在字符前面添加反斜杠字符\

    \*转义字符
\*转义字符

### 特殊字符自动转义

在html文件中，有两个字符需要特殊处理：<和&。符号用于起始标签，&符号则用于标记HTML实体，如果你只是想要使用这些符号，你必须要使用实体的形式，像是`&lt;`和`&amp;`。
`&`符号起始很容易让写作网页文件的人感到困扰，如果你要打[AT&T],你必须要写成[AT&amp;T],还得转换网址内的`&`符号，如果你要链接到：

    http://images.google.com/images?num=30&q=larry+bird
你必须要把网址转成：

    http://images.google.com/images?num=30&amp;q=larry+bird
才能放到链接标签的`href`属性里。不用说也知道这很容易忘记，这也可能是HTML标准检查所检查到的错误中，数量最多的。

Markdown允许你直接使用这些符号，它帮你自动转义字符。如果你使用`&`符号的作为HTML实体的一部分，那么它不会被转换，而在其他情况下，它则会被转换成`&amp;`。所以你如果要在文件插入一个著作权的符号，你可以这样写：

    &Copy
Markdown将不会对这段文字坐修改，但是如果你这样写

    AT&T
Markdown就会将它转为

    AT&amp;T
类似的状况也会发生在<符号上，因为Markdown支持行内HTML，如果你使用<符号作为HTML标签的分隔符，那Markdown也不会对它做任何转换，但是如果你是写：

    4<5
Markdown将会把它转换为：

    4&lt;5
需要特别注意的是，再Markdown的块级元素和内联元素，`<`和`&`两个符号都会被自动转换成HTML实体，这项特性让你可以很容易地用Markdown写HTML。（再HTML语法中，你要手动把所有的`<`和`&`都转换为HTML实体。）

## 内嵌HTML标签

对于MARkdown涵盖范围之外的标签，都可以直接在文件里面用HTML本身。如需使用HTML，不需要额外标注这是HTML或者Markdown，只需HTML标签添加到Markdown文本中即可。

### 行级内联标签

HTML的行级内联标签如`<span>`、`<cite>`、`<del>`不受限制，可以在Markdown的段落、列表或是标题里任意使用。依照个人习惯，甚至可以不用Markdown格式，而采用HTML标签来格式化。例如：如果比较喜欢HTML的`<a>`或`<img>`标签，可以直接使用这些标签，而不用Markdown提供的链接或是图片语法。当你需要更改元素的属性时（例如为文本指定颜色或更改图像的宽度）、使用HTML标签更方便些。
HTML行级内联标签和区块标签不同，在内联标签的范围内，Markdown的语法是可以解析的。

    this **word** is bold <em>word</em> is italic
this **word** is bold `<em>`word`</em>` is italic

### 区块标签

区块元素比如`<div>`、`<table>`、`<pre>`、`<p>`等标签，必须在前后加上空行，以便于内容区分。而且这些元素的开始与结尾标签，不可以用tab或是空白来缩进。Markdown会自动识别这区块元素，避免在区块标签前后加上没有必要的`<p>`标签
例如，在Markdown文件加上一段HTML表格：

    ```
    
    This is a regular paragraph.
    
    <table>
        <tr>
            <td>Foo</td>
        </tr>
    </table>
    
    This is another regular paragraph.
    ```

This is a regular paragraph.

    <table>
        <tr>
            <td>Foo</td>
        </tr>
    </table>

This is another regular paragraph.
请注意，Markdown语法在HTML区块标签中将不会被进行处理。例如，你无法在HTML区块内使用Markdown形式`*强调*`。

## 表格

要添加表格，请使用三个或多个连字符（---）创建每列的标题，并使用管道（|）分隔每列

    ```
    |标题1|标题2|
    |----|------|
    |header|大大撒旦|
    |dsadsa|dsdsa|
    ```

|标题1|标题2|
|:----|------:|
|header|大大撒旦|
|aa|dsa|

## 脚注

脚注使您可以添加注释和参考，而不会使文档正文混乱。当您创建脚注时，带有脚注的上标数字会出现在您添加脚注参考的位置。读者可以单击链接以跳出页面底部的脚注内容。

要创建脚注参考，请在方括号（[^1]）内添加插入符号和标识符。标识符可以是数字或单词，但不能包含空格或制表符。标识符仅将脚注参考与脚注本身相关联-在输出中，脚注俺顺序编号。

在括号内使用另一个插入符号和数字添加脚注，并用冒号和文本（[^1]：My footnote）.您不必在文档末尾添加脚注。您可以将他们放在除列表，块引号和表之类的其他元素之外的任何位置。

    ```
    Here's a simple footnote,[^1] and here's a longer one.[^bignote]
    
    [^1]: This is the first footnote.
    
    [^bignote]: Here's one with multiple paragraphs and code.
    
        Indent paragraphs to include them in the footnote.
    
        `{ my code }`
    
        Add as many paragraphs as you like.
    ```

Here's a simple footnote,[^1] and here's a longer one.[^bignote]

[^1]: This is the first footnote.

[^bignote]: Here's one with multiple paragraphs and code.

    Indent paragraphs to include them in the footnote.
    
    `{ my code }`
    
    Add as many paragraphs as you like.

## 标题编号

许多Markdown处理器支持标题的自定义ID，一些Markdown处理器会自动添加他们。添加自定义ID允许您直接链接到标题并使用css对其进行修改。要添加自定义标题ID，请在标题相同的行上用大括号括起该自定义ID。

### 标题一 { #first }

### 链接到标题ID（#headId）

通过创建带有数字符号（#）和自定义标题ID的`[标准链接](#headId)`

## 定义列表

一些Markdown处理器允许您创建术语及其对应定义的定义列表。要创建定义列表。要在第一行键入术语。在下一行，键入一个冒号，后面跟一个空格和定义。

    First Term
    : This is the definition of the first term.
    
    Second Term
    : This is one definition of the second term.
    : This is another definition of the second term.

 First Term
    : This is the definition of the first term.

    Second Term
    : This is one definition of the second term.
    : This is another definition of the second term.

## 删除线

    ~~通过在单词中心放置一条水平线来删除单词。结果看起来像这样。若要删除单词，请在单词前后时候使用前后使用两个~~
  ~~通过在单词中心放置一条水平线来删除单词。结果看起来像这样。若要删除单词，请在单词前后时候使用前后使用两个~~

## 任务列表语法

 任务列表使您可以创建带有复选框的项目列表。在支持列表的MArkdown应用程序中，复选程序中，复选框将显示在内容旁边。要创建任务列表，请在任务列表项之前添加破折号`-`和方括号`[]`，并在`[]`前面加上空格。要选择一个复选框，请在方括号`[x]`之间添加X。

    - [x] 选项一
    - [ ] 选项二
    - [ ] 选项三

- [x] 选项一
- [ ] 选项二
- [ ] 选项三

## 使用Emoji表情

有两种方法可以将表情符号添加到Markdown文件中：将标签符号复制并黏贴到Markdown格式的文本中，或者键入emoji shortcodes

### 复制和黏贴标签符号

在大多数情况下，可以简单地从Emojipendia等来源复制表情符号。从Markdown应用程序到处的HTML和PDF文件应显示表情符号。
Tip：如果使用静态网站生成器，请确保将HTML页面编码插入表情符号。这些以冒号开头和结尾，并包含表情符号的名称。

### 使用表情符号简码

一些Markdown应用程序允许通过键入表情符号代码来插入表情符号。这些以冒号开头和结尾，并包含表情符号的名称

    去露营了！ :tent: 很快回来。
    
    真好笑！ :joy:
去露营了！⛺很快回来。

真好笑！😂

## 自动网址链接

许多Markdown处理器会自动将URL转换为链接。这意味这输入<https://www.baidu.com/,即使没有使用方括号，Markdown>处理器也会自动将其转换为链接
<https://www.baidu.com/>

### 禁用自动URL链接

`http://www.example.com`
