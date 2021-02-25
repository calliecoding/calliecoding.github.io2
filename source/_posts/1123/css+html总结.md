---
title: css+html技巧总结
date: 2020-07-01 04:09:02
categories: 
- 前端
- css
tags: 
- 技巧 
---

> **文档使用指南：**
>
> md格式中：
>
> 1.看大纲视图的目录结构
>
> 2.跳转到想看的标题
>
> 
>
> 网页模式中：（需要目录插件）
>
> 1.看目录的结构
>
> 2.跳转到想看的标题

# css

##	知识库

> css常用知识库，方便了解css层叠样式表的书写绘制
>
> **主要内容：**来自css2，css3规范
>
> css2 w3c规范原版 [🔗](https://www.w3.org/TR/2011/REC-CSS2-20110607/#minitoc)
>
> css2.1 w3c规范原版 [🔗](https://www.w3.org/TR/CSS/#css)



1. 普通文档流
2. css中的层叠级别
3. css中的定位体系
   1. 普通文档流
   2. 浮动
   3. 绝对定位
4. visual formatting
   1. 普通流中的框(box)属于格式化上下文
      1. 块级格式化上下文( Block formatting contexts )( BFC )
      2. 行内格式化上下文( Inline formatting contexts ) ( IFC )
      3. 自适应格式化上下文( Flex Formatting Contexts )( FFC )
      4. 网格布局格式化上下文( GridLayout Formatting Contexts )( GFC )

##	技巧和经验总结

> 对css使用中常见问题的解决经验
>
> **主要内容**：解决问题的代码组合

###	1. 块级元素水平居中

**方法1: margin auto**

>   注意：元素需要有固定高度,margin auto才会生效
>
>   ```css
> div{
>     margin:50px auto 0;//上外边距50px,左右居中,下边距0
>     width：100px；
>     height：100px；
> }
>   ```
>

###	2. 如何用border画三角形

```css
span{
  /*一个正三角形*/
  width:0;
  height:0
  border:10px solid transparent;
  border-top:none;
  border-bottom-color:red;
}
```

###	3. 如何解决外边距合并的问题

**方法1：避免块级元素绘制起点重合**

> 将两个盒子的绘制起始线设置在不同的水平线上
>
> ```css
> 父容器：border：1px solid;（不能是0px）
> 父容器：padding：1px；（不能是0或auto，其他的任意数值都可以）
> ```

**方法2: 生成不同的BFC(块级格式上下文)**

> css规范中：属于同一个BFC的两个相邻Box的margin会发生重叠，
>
> 那么不同BFC的两个相邻Box的margin不会发生重叠
>
> 那么你就要知道哪些元素会生成BFC
>
> ```css
> 父容器或子容器： float：left；（不能是auto）
> 父容器或子容器：position：absolute；
> 子容器：display：inline-block;(或是inline-table)
> 相邻元素：任意一个添加float：left（不能是auto）
> 父容器：overflow: hidden;（或auto）
> ```

###	4. 如何解决行内/行内块块元素间解析空格的问题

**方法1:给父元素设置`font-size：0,`在子元素上重新设置正确的font-size**

> 原理：把空格的字体大小变成0
>
> 优点：当子元素是图片时，不需要重新设置文字大小
>
> 缺点：子元素如果需要字体的话，会需要重新在子元素添加fon-size属性
>
> ```html
> 
> 
> .parent {   
>     font-size: 0;
> }
> .child{
>     font-size: 16px;
> }
> 
> <div class="parent">
>     <span class="child">1</span><span class="child">2</span>
>     <span class="child">3</span>
> </div>
> ```

**方法2:使用浮动float，需要清除浮动**

> 原理：利用float元素的特性，把行内/行内块块元素变成浮动元素
>
> 缺点：float会导致父元素高度坍塌，需要清楚浮动
>
> ```html
> .parent {   
> 		overflow: hidden; /*清楚浮动的一种方法*/
>     background-color: #cef;
> }
> .child{
>     float: left;
> }
> 
> <div class="parent">
>     <span class="child">1</span><span class="child">2</span>
>     <span class="child">3</span>
> </div>
> ```

**方法3:子元素设置margin-left为负值**

> 缺点：元素之间间距的大小与上下文字体大小相关；并且同一大小的字体，元素之间的间距在不同浏览器下是不一样的，如：font-size:16px时，Chrome下元素之间的间距为8px,而Firefox下元素之间的间距为4px。所以不同浏览器下margin-right的负值是不一样的，因此这个方法不通用。

###	5. 如何添加多个背景图片

```css
background-image插入多张背景图片时,先插入的图片显示在上面

background-image:url(),url();
逗号之前是第一张.逗号之后对应第二张
背景的其他属性也是如此
```

###	6. 如何让单行文本在容器内垂直居中？

> 对浏览器来说,图片和文字都是文字
>
> 只需设置文本的行高等于容器的高度即可
>
> ```css
> .test{height:25px;line-height:25px;}
> ```

### 7.如何让文本/图片在容器内水平居中

```css
.test{text-align:center;}
```

###	8. 如何控制首个文字的距离

> 用text-indent比padding-left好,因为padding会影响盒子的大小
>
> ```css
> .test{text-indent:10px}
> ```

###	9. 字体必需是偶数,不然容易出错

###	10. 如何让单行文本溢出边界显示为省略号

> 首先需设置将文本强制在一行内显示，然后将溢出的文本通过overflow:hidden截断，并以text-overflow:ellipsis方式将截断的文本显示为省略号。
>
> ```css
> .test{
>   overflow:hidden;	/*超出隐藏*/
> 	white-space:nowrap;	/*不换行*/
> 	text-overflow:ellipsis; /*超出部分省略号显示*/
> }
> ```

###	11. 如何让多行文本省略的写法

> ```css
> .test{
>   display:-webkit-box;
>   overflow:hidden;
>   width: 100px;/*给元素固定宽度*/
>   -webkit-box-orient: vertical;/*设置元素是否水平或垂直放置其内容*/
>   -webkit-line-clamp: 2;/*文字显示2行*/
>   text-overflow:ellipsis;
> }
> ```
>
> 因使用了WebKit的CSS扩展属性，该方法适用于WebKit浏览器及移动端；
>
> 注：
>
> 1.-webkit-line-clamp用来限制在一个块元素显示的文本的行数。 为了实现该效果，它需要组合其他的WebKit属性。常见结合属性：
> 2.display: -webkit-box; 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 。
> 3.-webkit-box-orient 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。

###	12.如何解决基线对齐(行内元素)的问题

如清除图片下方出现几像素的空白间隙

**方法1:让vertical-align失效**

> 原理是`vertical-align`对块级元素无感
>
> ```css
> img{display:block}
> ```

**方法2:使用其他vertical-align值代替vertical-align:baseline**

> ```css
> img{
>     vertical-align:middle;
> }
> 
> /*或者*/
> img{
>     vertical-align:text-bottom;
> }
> ```

###	13. 如何解决幽灵空白节点(行内元素)

看12，是同一个问题

###	14. 行内块元素垂直居中三要素(inline,inline-block生效)

```css
box{
    line-height:100px;  /* 设置支柱的高度,用line-hieght代替height撑开盒子*/
    font-size:0;/*把支柱的大小变为0,让top,bottom,basline三线合一*/
}
#box img{
    vertical-align:middle
}
```

###	15. 使用浮动时，解决父级盒子高度坍塌问题

**方法1:父级使用after伪元素模拟元素来清除浮动(推荐!!企业规范常用）**

> ```css
> .clearfix:after{
> 	content:"";
> 	display:block;
> 	clear:both;
> }
> 
> <div class="parent clearfix">
>     <div>浮动元素1</div>
>     <div>浮动元素2</div>
>     <div>浮动元素3</div>
> </div>
> ```

**方法2:父级触发BFC环境**

> 原理：BFC特性中--“计算`BFC`的高度时，浮动元素的高度也参与计算”
>
> 优点:代码简单
>
> 缺点:`overflow:hidden`不能和定位一起使用,定位超出父级盒子会被隐藏
>
> ```css
> .parent{overflow:hidden;}
> ```

###	16. 如何让已知宽高的定位元素居中显示

**1.水平垂直居中**

```css
.test{
  position:absolute;
  top:50%;
  left:50%;
  margin-top:-50px; /* 50就是高度的一半 */
  margin-left:-70px ;/* 70就是宽度的一半 */
  width:100px; 
  height:140px;
}
```

**2.只水平居中**

```css
.test{
  position:absolute;
  left:50%;
  margin-left:-70px ;/* 70就是宽度的一半 */
  width:100px; 
  height:140px;
}
```

**3.只垂直居中**

```css
.test{
  position:absolute;
  top:50%;
  margin-top:-50px; /* 50就是高度的一半 */
  width:100px; 
  height:140px;
}
```

###	17. 如何让未知宽高的定位元素居中显示

**1.水平垂直居中**

```css
.test{
  position:absolute;
  top:0;
  bottom:0;
  left:0;
  right:0;
  margin:auto;
}

left:0;right:0;top:0;bottom:0;再加上margin：auto，会使.test居中
如果.test,没有宽高，则会继承父级的宽高。
```

**2.只水平居中**

```css
.test{
  position:absolute;
  left:0;
  right:0;
  margin:auto;
}
```

**3.只垂直居中**

```css
.test{
  position:absolute;
  top:0;
  bottom:0;
  margin:auto;
}
```

### 18.opacity与rgba

```
整个元素透明 opacity
仅背景透明	rgba
```

### 19.为什么img外面总要加div

```
Img外面最好套一层div，否则图片可能刚打开网页的时候加载不出来
img是替换元素，有src才会撑开高度
```





## 常见问题的产生原因

> 了解css常见问题的产生原因
>
> **主要内容**：研究问题背后的原理
>
> **标题格式**：xxx问题的存在原因

###	1. 外边距合并问题的存在原因

看笔记：css——05.1盒模型——外边距合并

笔记包含以下几点：

> 1. 什么是外边距合并
> 2. 什么时候会有外边距合并
> 3. 导致外边距合并的原因
> 4. 解决外边距合并的办法

###	2. 解析空格(行内元素)问题的存在原因

看笔记：css——07.1元素的分类——解析空格

笔记包含以下几点：

> 1. 产生原因
> 2. 解决办法

###	3. (行内元素)基线对齐问题的存在原因

看笔记：css——13.1vertial-calign——vertical-align与line-height

笔记包含以下几点：

> 1. 什么是基线
> 2. 什么元素有基线对齐的问题
> 3. 产生幽灵空白节点的原因
> 4. 解决办法



###	4. (行内元素)幽灵空白节点问题的存在原因

看上一条

###	5. 浮动时父级盒子高度坍塌的原因

看笔记：css——14.1浮动——float对元素的影响

笔记包含以下几点：

> 1. 浮动对
> 2. 解决高度坍塌问题

## 易混淆点辨析

> css使用中，对容易混淆的知识点进行区分
>
> **主要内容**：区分不同点
>
> **标题格式**：xxx和xxx的区别

###	1. background和img在实际使用中的区别



```css
建议：重要的需要优先加载的图片最好采用img。不重要的图片最好采用background。

1.写在css里面的图片是以背景图形式存在的，而写在html里的img是以标签形式存在的，在网页加载的过程中，以css背景图存在的图片会等到结构加载完成（网页的内容全部显示以后）才开始加载，而html里的img标签是网页结构（内容）的一部分会在加载结构的过程中加载。

2.通常是非内容的图片就写在css里面(用来修饰页面的元素)，如果是内容性的图片就写在html里面，打个比方，你要做一个有漂亮边框的相册。那么修饰边框的图片就写在css里面，相框里面的内容照片就写在html里面。

3.图片做为背景，在图片没加载的时候或者加载失败的时候，不会有个图片的占位标记，不会出现红叉。
```



## css优化方法

###	1. css代码书写顺序

> 使用推荐顺讯书写，避免浏览器回流重绘，损耗性能  [🔗](https://www.cnblogs.com/tyusBlog/p/9752952.html)
>
> ```css
> 1.布局定位属性
> 
> 	content/display/position(对应的方位值)/float/overflow
> 
> 2.自身属性
> 
> 	width/height/padding/margin/background/border
> 
> 3.文字属性
> 
> 	color/font/text-align/text-decoration/vertical-align
> 
> 4.其他/CSS3新增属性
> 
> 	cursor/border-radius/box-shadow/text-shadow
> ```

###	2. 尽量减少浏览器计算次数

设置css时，使用单一属性，避免复合属性



###	3.使用雪碧图/精灵图

url是向服务器请求,雪碧图能减少向后端的请求次数，提高加载效率

如有12个小图片,一般情况下要请求12张图片,但雪碧图只需要请求一次

雪碧图是通过<u>CSS Scprites技术</u>,将多个图片合成一张图片,这样就将请求合并为1个HTTP了,降低后台服务器的开销

##	常用布局

> 总结实际应用中，常用的几种布局方案，以及适用场景

```
1.盒模型的流体特性
2.浮动+盒模型的流体特性 实现三栏布局
3.定位+盒模型的流体特性 实现三栏布局
4.flex布局
```

两栏式布局

1.

左侧固定宽，右侧宽度自适应

左侧：利用浮动特性

右侧：利用块级元素的流体特性

三栏式布局



# html

## 正确使用标签

###	1. h标签在网页中的使用次数

```
h1 1次
h2 小于5次
h3 不限制
```



## html优化方法

###	1. 正确使用标签(标签语义化)

> 了解标签的含义，在合适的情况使用合适的标签

###	2. 简化标签

> 网页上的标签尽力少用,因为标签需要绘制
>
> 天猫双11期间,是精简版的网页,防止卡顿
>

# SEO

SEO（Search Engine Optimization）：汉译为[搜索引擎](https://baike.baidu.com/item/搜索引擎)优化

SEO的作用是：利用[搜索引擎](https://baike.baidu.com/item/搜索引擎/104812)的规则提高[网站](https://baike.baidu.com/item/网站/155722)在有关搜索引擎内的[自然排名](https://baike.baidu.com/item/自然排名/2092669)

## 前端的SEO优化

> 前端可以做到的SEO优化的办法



