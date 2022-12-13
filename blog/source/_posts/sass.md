---
title: sass学习
date: 2022-12-12 10:36:15
tags: 学习
categories: 前端
typora-copy-images-to: upload
top: 98
---

### 特色功能

***

- 完全兼容css3
- 在css基础上增加变量、嵌套、混合等功能
- 通过函数进行颜色值与属性值的运算
- 提供控制指令等高级功能
- 自定义输出格式

<!--more-->

### 语法格式

***

Sass有两种语法格式。首先是Scss，这种格式仅在css3语法的基础上尽心拓展，所有css3语法在scss中都是通用的，同时加入Sass的特色功能。此外，scss也支持大多数css hacks写法以及浏览器前缀写法。这种格式以,scss作为拓展名。另一种是最早的sass语法格式，被称为缩进格式通常简称“Sass”，是一种简化格式。它使用“缩进”代替花括号表示属性属于某个选择器，用“换行”代替“分号”分隔属性，很多人认为这样做比SCSS更容易阅读，书写也更快速。缩进格式也可以使用Sass的全部功能，只是与SCSS相比个别地方采取了不同的表达方式。这种格式以.sass作为拓展名。

任何一种格式可以直接导入（@import）到另一种格式中使用，或者通过sass-convert命令行工具转换成另一种格式

```scss
# Convert Sass to SCSS
$ sass-convert style.sass style.scss

# Convert SCSS to Sass
$ sass-convert style.scss style.sass
```

### 使用Sass

***

Sass可以通过以下三种方式使用：作为命令行工具，作为独立的Ruby模块，或者作为Rack-enabled框架的插件。无论哪种方式都需要先安装sass gem(windows系统需要安装Ruby)

```scss
gem install sass
//在命令中运行Sass
sass input .scss output .css
//监视单个Sass文件，每次修改并保存时自动编译
sass --watch input .scss:output .css
//监视整个文件夹
sass --wwatch app/sass:public/stylesheets
//在Ruby中使用Sass也非常容易，Sass gem安装完毕后运行require "sass"然后按照下面的方法使用Sass::Engine
engine = Sass::Engine.new("#main {background-color:#0000ff}",:syntax => :scss)
engine.render #=> "#main { background-color: #0000ff;}\n" 
```

**缓存**

Sass自动缓存编译后的模板与partials，这样做能够显著提升重新编译的速度，尤其在处理由@import导入多个子文件的大型项目时

单独使用Sass，缓存内容保存在.sass-cache文件夹中。在Rails和Merb项目中缓存文件保存在tmp/sass-cache文件夹中（可通过:cache_location修改路径）。禁用缓存可将:cache设为false

判断语法格式

Sass命令行工具根据文件的拓展名判断所使用的语法格式，没有文件名sass命令默认编译.sass文件，添加 --scss选项或者使用scss命令编译SCSS命令编译SCSS文件

编码格式

在Ruby1.9及以上环境中运行Sass时，Sass对文件的编码格式比较敏感，首先会根据CSS spec判断样式文件的编码格式，如果失败则检测Ruby string encoding。也就是说，Sass首先检查Unicode byte order mark，然后是@charset声明，最后是Ruby string encoding，加入都没有检测到，默认使用UTF-8编码。

与css相同，使用@charset可以声明特定的编码格式。在样式文件的起始位置插入@charset "encoding-name"，Sass将会按照给出的编码格式编译文件。注意所使用的编码格式必须可转换为Unicode字符集

### css功能扩展

***

**嵌套规则**

Sass允许将一套css样式嵌套进另一套样式中，内层的样式将它外层的选择器作为父选择器，

```scss
#main p {
  color: #00ff00;
  width: 97%;

  .redbox {
    background-color: #ff0000;
    color: #000000;
  }
}
转译为
#main p {
  color: #00ff00;
  width: 97%; }
  #main p .redbox {
    background-color: #ff0000;
    color: #000000; 
  }
```

**父选择器&**

在嵌套css规则时，有时也需要直接使用嵌套外层的父选择器，例如，当给某个元素设定hover样式时，或者当body元素有某个classname时，可以用&代表嵌套规则外层的父选择器

```scss
a {
  font-weight: bold;
  text-decoration: none;
  &:hover { text-decoration: underline; }
  body.firefox & { font-weight: normal; }
}
//转译为
a {
  font-weight: bold;
  text-decoration: none; }
  a:hover {
    text-decoration: underline; }
  body.firefox a {
    font-weight: normal; 
   }
```

**占位符选择器%foo**

Sass额外提供了一种特殊类型的选择器：占位符选择器。与常用的id与class选择器写法相似，只是#或.替换成了%。必须通过@extend指令调用。

### 注释/* */与//

***

Sass支持标准的css多行注释/* */，以及单行注释//，前者会被完整输出到编译后的css文件中，而后者则不会

```scss
/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */
body { color: black; }

// These comments are only one line long each.
// They won't appear in the CSS output,
// since they use the single-line comment syntax.
a { color: green; }

//转译为

/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */
body {
  color: black; }
  
  
a {
  color: green; 
  }
```

### SassScript

在css属性的基础上Sass提供了一些名为SassScript的新功能。SassScript可作用于任何属性，允许属性使用变量、算数运算等额外功能。

通过interpolation，SassScript甚至可以生成选择器或属性名，这一点对编写mixin有很大帮助

Interractive Shell可以在命令行中测试SassScript的功能。在命令行中输入sass -i，然后输入想要测试的SassScript查看输出结果

```scss
$ sass -i
>> "Hello, Sassy World!"
"Hello, Sassy World!"
>> 1px + 1px + 1px
3px
>> #777 + #777
#eeeeee
>> #777 + #888
white
```

**变量$**

SassScript最普遍的用法就是变量，变量以美元符号开头，赋值方法与css属性的写法一样

```scss
$width: 5em

#main {
    width:$width;// 5em
}
```

变量支持块级作用域，嵌套规则内定义的变量只能在嵌套规则内使用（局部变量），不在嵌套规则内定义的变量则可在任何地方使用（全局变量）。将局部变量转换为全局变量可以添加!global

```scss
#main {
  $width: 5em !global;
  width: $width;
}

#sidebar {
  width: $width;
}

转译为

#main {
  width: 5em;
}

#sidebar {
  width: 5em;
}
```

**数据类型**

SassScript支持6种主要的数据类型，数字、字符串、颜色、布尔型、空值、数组、maps

**字符串**

SassScript支持css的两种字符串类型：有引号字符串，如“”和‘’；与无引号字符串，在编译css文件时不会改变其类型。只有一种情况除外，使用#{}（interpolation）时，有引号字符串将被编译为无引号字符串，这样便于在mixin中引用选择器名

```scss
@mixin firefox-message($selector) {
  body.firefox #{$selector}:before {
    content: "Hi, Firefox users!";
  }
}
@include firefox-message(".header");
//转译为
body.firefox .header:before {
  content: "Hi, Firefox users!"; 
  }
```

**数组**

数组指Sass如何处理cs中margin：10px 15px 0 0这样通过空格或者逗号分隔的一系列的值。独立的值也被视为数组

数组本身没有太多功能，Sass list functions赋予了数组更多的新功能；nth函数可以直接访问数组中的某一项，join函数可以将多个数组连接在一起，append函数可以在数组中添加新值；而@each指令能够遍历数组中的每一项

**运算**

所有的数据类型都支持相等运算==或!=，此外，每种数据也有其各自支持的运算方式SassScript支持数字的加减乘除、取整等运算（+，-，*，/,%），如果必要会在不同单位见转换值

插值语句#{}

通过#{}插值语句可以在选择器或属性名中使用变量

```scss
$name: foo;
$attr: border;
p.#{$name} {
  #{$attr}-color: blue;
}
//转译为
p.foo {
  border-color: blue;
  }
```

### @-Rules与指令

Sass支持所有的css3 @-Rules，以及Sass特有的指令

**@import**

Sass在当前地址，或Rack，Rails，Merb的Sass文件地址寻找Sass文件，如果需要设定其他地址，可以用：load_paths选项，或者在命令中输入--load-path命令

通常，@import寻找Sass文件并将其导入，但在以下情况下，@important寻找Sass文件并将其导入，但在以下情况下，@import仅作为普通的css语句，不会导入任何Sass文件，文件拓展名是.css，文件名以http://开头，文件名是url(),@import包含media queries

如果不在上诉情况内，文件的拓展名是.scss或.sass，则导入成功

**@media**

Sass中@media指令与css中用法一样，只是增加了一点额外的功能：允许其在css规则中嵌套。如果@media嵌套在css规则内，编译时，@media将被编译到文件的最外层，包含嵌套的父选择器。这个功能让@media用起来更方便，不需要重复使用选择器，也不会打乱css的书写流程

```scss
.sidebar {
  width: 300px;
  @media screen and (orientation: landscape) {
    width: 500px;
  }
}
//转译为
.sidebar {
  width: 300px; }
  @media screen and (orientation: landscape) {
    .sidebar {
      width: 500px;
      }
    }
```

### 控制指令

**@if**

当@if的表达式返回值不是false或者null时，条件成立，输出{}内的代码

```scss
p {
  @if 1 + 1 == 2 { border: 1px solid; }
  @if 5 < 3 { border: 2px dotted; }
  @if null  { border: 3px double; }
}
//转译为
p {
  border: 1px solid;
  }
```

**@for**

@for指令可以限制的范围内重复输出格式，每次按要求对输出结果做出变动。这个指令包含两种格式：@for $var from  through ,区别在于through与to的含义：当使用through时，条件范围包含与的值，而使用to时条件范围只包含的值不包含的值。另外，$var可以是任何变量，比如$i,和必须是整数值

```scss
@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}
//转译为
.item-1 {
  width: 2em; 
  }
.item-2 {
  width: 4em; 
  }
.item-3 {
  width: 6em; 
  }
```

**@each**

@each指令的格式是$var in ,$var可以是任何变量名，比如$length或者$name，而是一连串的值，也就是值列表

@each将变量$var作用于值列表中的每一个项目，然后输出结果

```scss
@each $animal in puma, sea-slug, egret, salamander {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}
//转译为
.puma-icon {
  background-image: url('/images/puma.png'); }
.sea-slug-icon {
  background-image: url('/images/sea-slug.png'); }
.egret-icon {
  background-image: url('/images/egret.png'); }
.salamander-icon {
  background-image: url('/images/salamander.png'); }
```

**@while**

@while指令重复输出格式知道表达式返回结果为false。这样库实现比@for更复杂的循环，只是很少会用到

```scss
$i: 6;
@while $i > 0 {
  .item-#{$i} { width: 2em * $i; }
  $i: $i - 2;
}
//转译为
.item-6 {
  width: 12em; }

.item-4 {
  width: 8em; }

.item-2 {
  width: 4em; }
```

### 混合指令

混合指令用于定义可重复使用的样式，避免了使用无语意的class，比如.float-left。混合指令可以包含所有的css规则，绝大部分Sass规则，甚至通过参数功能引入变量，输出多样化的样式

### 函数指令

Sass支持自定义函数，并能在如何属性值或Sass Script中使用

```scss
$grid-width: 40px;
$gutter-width: 10px;

@function grid-width($n) {
  @return $n * $grid-width + ($n - 1) * $gutter-width;
}

#sidebar { width: grid-width(5); }
//转译为
#sidebar {
  width: 240px; }
```

### 输出格式

Sass默认的css输出格式很美观也能清晰反映文档结构，为满足其他需求Sass也提供了多种输出格式

Sass提供了四种输出格式，可以通过：style option选项设定，或者在命令行中使用 -- style选项

**:nested**

Nested样式是Sass默认的输出格式，能够清晰反映css与html的结构关系。选择器与属性等单独占用一行，缩进量与Sass文件中一致，每行的所尽量反映了其在嵌套规则内的层数。当阅读大型文件时，这种样式可以很容易地分析文件的主要结构

```scss
#main {
  color: #fff;
  background-color: #000; }
  #main p {
    width: 10em; }

.huge {
  font-size: 10em;
  font-weight: bold;
  text-decoration: underline; }
```

**:expanded**

Expanded输出更像是手写的样式，选择器、属性等占用一行，属性根据选择器缩进，而选择器不做任何缩进

```scss
#main {
  color: #fff;
  background-color: #000;
}
#main p {
  width: 10em;
}

.huge {
  font-size: 10em;
  font-weight: bold;
  text-decoration: underline;
}
```

**:compact**

compact输出方式比起上面两种占用的空间更少，每条css规则只占一行，包含其下的所有属性。嵌套过的选择器在输出时没有空行，不嵌套的选择器会输出空白行作为分隔符

```scss
#main { color: #fff; background-color: #000; }
#main p { width: 10em; }

.huge { font-size: 10em; font-weight: bold; text-decoration: underline; }
```

**:compressed**

compessed输出方式删除所有无意义的空格、空白行、以及注释，力求将文件体积压缩到最小，同时也会做出其他调整，比如会自动替换占用空间最小的颜色表达方式

```scss
#main{color:#fff;background-color:#000}#main p{width:10em}.huge{font-size:10em;font-weight:bold;text-decoration:underline}
```

