# 高质量的 CSS
1. 怪异模式。
    - IE对于盒模型的解析：在标准模式中，网页元素的宽度是由 padding , border, width 三者的宽度相加决定的。在怪异模式中，width 本身就包括了 padding 和 border 的宽度。此外，标准模式下块级元素的经典居中的方法也无法在怪异模式中正常工作。

2. 如何组织 CSS
    - 可将网站的所有样式，按照职能分成三大类：base、common 和 page
    - 使用 ` .blockCenter{margin-left:auto;margin-right:auto;}`来让块级元素居中的时候，直接使用它是不足以让块级元素居中的。还需要设定宽度（width）。
    - `.clearfix:after{content:".";display:block;height:0;clear:both;visibility:hidden;} .clearfix{display:inline-block} *html clearfix{height:1%} .Clearfix{display:block;} `使用 .clearfix 类可以用于在父容器直接清除子元素浮动。通常情况下，为了让浮动元素的父容器能够根据浮动元素的高度而自适应高度，有三种做法：
        + 让父级元素同时浮动起来，例如：`<div class="fl"><div class="fl"></div></div>`; **这种方法会让父容器也浮动起来，影响父级元素后面元素的布局。**
        + 让浮动元素后面紧跟一个用于清除浮动的空标签，例如：`<div><div class="fl"></div><div class="cb"></div></div>`; **这种方法增加了一个空标签，会破坏语义化。**
        + 给父元素挂一个特殊的 class ，直接从父容器清除浮动元素的浮动：`<div class="clearfix"><div class="fl"></div></div>`; **无副作用，推荐使用。**
    - zoom 类,用来触发 IE 浏览器的 hasLayout 。

3. 模块化 CSS
    - 拆分模块的第一个技巧：模块与模块之间尽量不要包含相同的部分，如果有相同的部分，应将他们提取出来，拆分成一个独立的模块。
    - 模块应在保证数量尽可能少的原则下，做到尽可能简单，以提高重用性。
    - CSS 命名:驼峰命名法和划线命名法。驼峰命名法可以用来区分不同的单词，划线用来表明从属关系。在团队写作中为了避免冲突，可以在每个人的 CSS 命名前加上名字的前缀。
    - 多用组合，少用继承。
    - 上下margin 的处理：相邻的 margin-top 和 margin-bottom 会产生重合。所以最好统一使用 margin-top 或者 margin-bottom 。

4. 低权重原则——避免滥用子选择器
    - 当不同选择符的样式设置冲突时，会采用权重高的选择符设置的样式。权重的规则：HTML 标签的权重是 1，class 的权重是 10，id 的权重是 100，例如 p 的权重是 1，“ div em ”的权重是 1+1=2，“ strong .demo ”的权重是 10+1=11，“ #test .red ”的权重是 100+10=110。如果 CSS 选择符权重相同，那么样式会遵循就近原则，哪个选择符最后定义，就采用哪个选择符的样式。为了保证样式易被覆盖，提高可维护性，CSS 选择符需保证权重尽可能低。

5. CSS sprite

    代码清单：

    ```
    <style type="text/css">
    .nav li{float: left;display:inline;margin-right: 10px;font-family: 黑体;}
    .nav a{float: left;width: 139px;height: 31px;line-height: 31px;font-size: 24px;color: #fff;text-decoration: none;text-align: center;background: url(img3.gif);}
    .nav a:hover{background-position: 0,-31px;}
    </style>
    <ul class="nav">
        <li><a href="#">联系我们</a></li>
        <li><a href="#">产品答疑</a></li>
        <li><a href="#">广告服务</a></li>
    </ul>
    ```

    - 使用图片翻转技术将多张图片合并为一张，然后使用 background-position 属性来展示我们需要的部分。
    - CSS sprite 能合并的只能是用于背景的图片，对于`<img src=""/>`设置的图片，是不能合并到CSS sprite 大图中的。
    - 对于横向和纵向都平铺的图片，也不能使用 CSS sprite；如果是横向平铺的，只能将所有横向平铺的图合并成一张大图，只能竖直排列，不能水平排列。如果是纵向平铺的，只能所有纵向平铺的图合成一张大图，只能水平排列，不能竖直排列。
    - 好处是减少 HTTP 请求数。

6. CSS 常见问题
    1. CSS 的编码风格：多行式和一行式。多行式便于阅读，一行式有利于提高开发速度。
    2. id 和 class
        - 同一个页面，相同的 id 只能出现一次，不可重复，而 class 可以任意出现多次。
        - id 的 CSS 选择符权重为 100，而 class 为 10。
        - 原生 JS 提供 getElementbyId() 方法。
        - 一般来说，id 不能重用，使用 id 会限制网页扩展性，最好尽量使用 class，少用 id。
    3. CSS hack
        1. IE 条件注释法

            ```
            <!-- 针对 IE 引入一个 CSS 文件 -->
            <!-- [if IE] >
            <link rel="stylesheet" type="text/css" href="test.css"/>
            <! [endif] -->
            <!-- 针对特定版本的 IE 浏览器加载 CSS 文件 -->
            <!-- [ if IE 6]>
            <link rel="stylesheet" type="text/css" href="test.css">
            <![endif]  -->
            <!-- 针对某个范围以内的 IE 进行 hack，可以结合 lte，lt，gte，gt，! 关键字来进行注释。其中 lte 表示“小于等于”，lt 表示“小于”，gte 表示“大于等于”，gt 表示大于，! 表示不等于。-->
            <!-- [if ! IE7] >
            <link rel="stylesheet" type="text/css" href="test.css">
            <![endif] -->
            <!-- 这种方法不仅适用于引进 link 标签，同时适用于直接定义 CSS 以及 JS -->
            ```

        2. 选择符前缀法

            ```
            <!-- 在 CSS 选择符前加一些只有特定浏览器才能是别的前缀，从而让某些样式只对特定浏览器生效。例如“ *html ”只对 IE6 生效，“ *+html ”只对 IE7 生效。 -->
            <style type="text/css">
            .test {width:80px;}
            *html .test {width:60px;}
            *+html .test {width:70px;}
            </style>
            ```

        3. 样式符前缀法

            ```
            <!-- 在样式的属性名前面加前缀。例如“_”只在 IE6 生效，“*”在 IE6 和 IE7 都生效。 -->
            <style type="text/css">
            .test {width:80px;*width:70px;_width:60px;}
            </style>
            ```

    4. 解决超链接访问后 hover 样式不出现的问题
        - 这种问题常常是因为把 a 标签的四种状态的顺序放反了。关于 a 标签的四种状态的排序问题，有一个 love hate 原则，即：l**(link)**ov**(visited)**e h**(hover)**a**(active)**te。
    5. hasLayout
        - 使用“ zoom:1 ”可以触发 IE的hasLayout 属性。
    6. 块级元素和行内元素的区别
        - 常见块级元素：div、p、form、ul、ol、li 等；常见的行内元素有 span、strong、em 等。
        - 块级元素会独占一行，默认情况下，其宽度自动填满其父元素，行内元素不会独占一行，相邻的行内元素会排列在同一行内，直到一行排不下，才会换行。
        - 块级元素可以设置 width、height 等属性，行内元素则无效。**注意：块级元素即使设置了宽度，仍然独占一行。**
        - 块级元素可以设置 margin 和 padding 属性。行内元素水平方向的 padding-left、padding-right、margin-left、margin-right 都会产生边距效果。但是其竖直方向的 padding 和 margin 不会产生边距效果。
    7. display:inline-block 和 hasLayout
        - display:inline-block 可以让行内元素拥有块级元素的特点：可以设置长宽，可以设置 margin 和 padding 值，但他却不是独占一行。
    8. relative、absolute 和 float
        - position:relative、position:absolute 可以该改变元素在文档流中的位置，同时可以让元素激活 left、top、right、bottom 和 z-index 属性。
        - position:relative 会保留自己在 z-index:0 层的元素位置，不会对其他在 z-index:0 的元素造成影响。而position:absolute 会完全脱离文档流，不再在 z-index:0 层保留占位符，其 left、top、right、bottom 值是相对于自己最近的一个设置了 position:relative 或 position:absolute 的祖先元素的，如果祖先元素都没有设置 position:relative 或 position:absolute，那么就相对 body 元素。
        - float 也能改变文档流,不同的是，float 属性不会让元素“上浮”到另一个 z-index 层，他只能通过 float:left 和 float:right 来控制元素在同层里“左浮”和“右浮”，并且 float 会改变正常的文档流排列，影响到其他元素。
        - 不论是什么类型的元素，只要设置了 position:absolute、float:left 或 float:right 中的任意一个，都会让元素以 **display:inline-block** 的方式显示：可以设置长宽，默认宽度并不占满父元素。但是，**position:relative 却不会隐式改变 display 的类型**。
    9. 居中
        1. 水平居中
          1. 文本、图片等行内元素的水平居中
            ```
          <style type="text/CSS">
          .wrap{background: #000;width: 500px;height: 100px;margin-bottom: 10px;color: #fff;text-align: center}
          </style>
          <div class="wrap">hello world</div>
          <div class="wrap"><img src="#" /></div>
            ```
          2. 确定宽度的块级元素的水平居中通过设置 margin-left:auto 和 margin-right:auto 来实现的。
          3. 不确定宽度的块级元素的水平居中实现方式有三种。
            - 第一种方法是通过 table 来帮组不确定宽度的块级元素的水平居中，即使不设置 table 元素的宽度，仅仅设置 margin-left:atuo 和 margin-right:auto 就可以使 table 水平居中。这样的坏处是增加了无语义标签，加深了标签的嵌套层数。
            - 方法二通过将块级元素的 display 类型改为 inline，变成行内元素，然后用 text-align:center 来居中，好处是不用增加无语义标签，坏处是将块级元素变成了行内元素，而行内元素比起块级元素缺少一些功能，可能会带来一些限制。
            - 方法三通过给父元素设置 float，然后父元素设置 position:relative 和 left:50%,子元素设置 position:relative 和 left:-50% 来实现水平居中，他可以保留块级元素仍以 display:block 的形式显示，而且没有添加无语义标签。缺点是设置了 position:relative，带来一定的副作用。
        2. 竖直居中
          1. 父元素高度不确定的文本、图片、块级元素的竖直居中，是通过给父容器设置相同的上下边距实现的。
          2. 父元素高度确定的单行文本的竖直居中，通过给父元素设置 line-height 来实现的。
          3. 父元素高度确定的多行文本、图片、块级元素的竖直居中。 
