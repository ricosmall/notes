# CSS-Mastery

中文名：[《精通 CSS》](https://book.douban.com/subject/30450258/)（第 3 版）

## 进度

- [x] 第 1 章 基础知识
- [x] 第 2 章 添加样式
- [x] 第 3 章 可见格式化模型
- [x] 第 4 章 网页排版
- [x] 第 5 章 漂亮的盒子
- [x] 第 6 章 内容布局
- [x] 第 7 章 页面布局与网格
- [ ] 第 8 章 响应式 Web 设计与 CSS
- [ ] 第 9 章 表单与数据表
- [ ] 第 10 章 变换、过渡与动画
- [ ] 第 11 章 高级特效
- [ ] 第 12 章 品控与流程

## 第 1 章 基础知识

学习层叠样式表（CSS，cascading style sheets）最好的方法，就是不管三七二十一，直接上手写代码。

### HTML 简史

Tim Berners-Lee 在 1990 年发明了 HTML，当时是为了规范科研文档的格式。HTML 是一种简单的标记语言，为文本赋予了基本的结构和意义，比如标题、列表、定义等。这些文档通常没有什么装饰性的元素，可以方便地通过计算机来检索，而人类可以使用文本终端、Web 浏览器，或者必要时使用屏幕阅读器来阅读它们。

### CSS 的版本

CSS1 是在 1996 年底成为 W3C 推荐标准的，当时只包含字体、颜色和外边距等基本的属性。CSS2 在 1998 年成为推荐标准，增加了浮动和定位等高级特性，此外还有子选择符、相邻选择符和通用选择符等新选择符。

相比之下，CSS3 则采用了完全不同的模式。实际上不存在所谓的 CSS3 规范，因为 CSS3 指的是一系列级别独立的模块。如果规范模块是对之前 CSS 概念的改进，那就从 3 级（level 3）开始命名。如果不是改进，而是一种全新的技术，那就从 1 级（level 1）开始命名。

### 条件规则和检测脚本

如果希望根据浏览器是否支持某个 CSS 特性来提供完全不同的样式，那么可以选择 @supports 块。这个特殊的代码块称为条件规则，它会检测括号中的声明，并且仅在浏览器支持该声明的情况下，才会应用块中的规则：

```css
@supports (display: grid) {
  /* 在支持网格布局的浏览器中要应用的规则 */
}
```

条件规则的问题是其自身也很新，只能将它应用于新的浏览器中，因为旧版本浏览器不支持。

### 创建结构化、语义化富 HTML

语义化标记意味着在正确的地方使用正确的元素，从而得到有意义的文档。语义明确的元素有：

- `h1`、`h2` 等
- `p`、`ul`、`ol` 和 `dl`
- `strong` 和 `em`
- `blockquote` 和 `cite`
- `pre` 和 `code`
- `time`、`figcaption` 和 `caption`

HTML5 新增了一批结构化元素：

- `section`
- `header`
- `footer`
- `nav`
- `article`
- `aside`
- `main`

### 重新定义的表现性文本元素

`<body>` 和 `<nav>` 可以算是幸存的表现性标记了，它们在排版上会分别显示为粗体（bold）和斜体（italic）。这两个元素与 `<strong>` 和 `<em>` 的区别在于，它们没有任何强调自己所包含内容的意味。多数情况下，应该选择 `<strong>` 或 `<em>`，因为它们是用来强调及重点强调内容的语义正确的选择。

### 微格式

微格式是一组标准的命名约定和和标记模式，可用于表示特定的数据类型。微格式的命名约定是基于 vCard 和 iCalendar 等已有的数据格式制定的。比如下面的联系人信息就是以 hCard 格式标记的：

```html
<section class="h-card">
  <p>
    <a class="u-url p-name" href="http://andybudd.com/">Andy Budd</a>
    <span class="p-org">Clearleft Ltd</span>
    <a class="u-email" href="mailto:info@andybudd.com">info@andybudd.com</a>
  </p>

  <p class="p-adr">
    <span class="p-locality">Brighton</span>
    <span class="p-country-name">England</span>
  </p>
</section>
```

### 微数据

微数据是跟 HTML5 一起，作为给 HTML 添加结构化数据的另一种方式而推出的。它的目标与微格式非常接近，但在把微数据嵌入内容方面则有所不同。下面是微数据标记联系人信息：

```html
<section itemscope itemtype="http://schema.org/Person">
  <p><a itemprop="name" href="http://thatemil.com/">Emil Bojoklund</a></p>
  <p itemprop="affiliation" itemscope itemtype="http://schema.org/Organization">
    <span itemprop="name">inUse Experience AB</span>
    <a itemprop="email" href="mailto:emil@thatemil.com">emil@thatemil.com</a>
  </p>
  <p itemprop="address" itemscope itemtype="http://schema.org/PostalAddress">
    <span class="addressLocality">Malm?</span>,
    <span class="addressCountry">Sweden</span>
  </p>
</section>
```

## 第 2 章 添加样式

有效且结构良好的文档是添加样式的基础。

### CSS 选择符

类型选择符用于选择特定类型的元素。类型选择符有时候也被称为元素选择符。

```css
p {
  color: black;
}
```

后代选择符用于选择某个或某组元素的后代。

```css
blockquate p {
  padding-left: 2em;
}
```

要想更精确地选择目标元素，可以使用 ID 选择符（`#`）和类选择符（`.`）。

```css
#intro {
  font-weight: bold;
}

.date-posted {
  color: #ccc;
}
```

与后代选择符会选择一个元素的所有后代不同，子选择符（`>`）只选择一个元素的直接后代，也就是子元素。

```css
#nav > li {
  background: url(folder.png) no-repeat left top;
  padding-left: 20px;
}
```

```html
<ul id="nav">
  <li>Home</li>
  <li>
    Services
    <ul>
      <li>Design</li>
      <li>...</li>
    </ul>
  </li>
  <li>Contact Us</li>
</ul>
```

上面嵌套的 `<ul>` 下的 `<li>` 的样式不会受到影响。

相邻同辈选择符（`+`）可以选择位于某个元素后面，并与该元素拥有共同父元素的元素。

```css
h2 + p {
  font-size: 1.4em;
  font-weight: bold;
  color: #777;
}
```

一般同辈选择符（`~`）可以选择位于某个元素后面的所有该元素拥有共同父元素的元素。

```css
h2 ~ p {
  font-size: 1.4em;
  font-weight: bold;
  color: #777;
}
```

相邻同辈选择符和一般同辈选择符都不会选择前面的同辈元素。

通用选择符（`*`）可以匹配任何元素。

```css
* {
  margin: 0;
  padding: 0;
}
```

事实上，这样写可能带来很多意想不到的后果，特别是会影响 `<button>`、`<select>` 等表单元素。如果想重设样式，最好还是像下面这样明确指定元素：

```css
h1,
h2,
h3,
h4,
h5,
h6,
ul,
ol,
li,
dl,
p {
  margin: 0;
  padding: 0;
}
```

属性选择符基于元素是否有某个属性或者属性是否有某个值来选择元素。

```css
abbr[title] {
  border-bottom: 1px dotted #999;
}

input[type='submit'] {
  cursor: pointer;
}

/* 匹配以某些字符开头的属性值 */
a[href^='http:'] {
}

/* 匹配以某些字符结尾的属性值 */
img[src$='.jpg'] {
}

/* 匹配包含某些字符的属性值 */
a[href*='/about/'] {
}

/* 匹配以空格分隔的字符串中的属性值 */
a[rel~='next'] {
}
```

有时候我们想选择的页面区域不是通过元素来表示的，而我们也不想为此给页面添加额外的标记。CSS 为这种情况提供了一些特殊选择符，叫伪元素。

```css
.chapter::before {
  content: '“';
  font-size: 10em;
}

.chapter p::first-letter {
  float: left;
  font-size: 3em;
}
```

有时候我们想基于文档结构以外的情形来为页面添加样式，比如基于超连接或表单元素的状态。这时候就可以使用伪类选择符。

```css
a:visited {
  color: green;
}

.comment:target {
  background: #fffec4;
}

/* 结构化伪类 */
tr:nth-child(odd) {
  background: yellow;
}

/* 表单伪类 */
input:required {
  outline: 2px solid #000;
}
```

伪元素使用双冒号（`::`）语法，伪类使用单冒号（`:`）语法。

### 层叠

层叠机制的原理是为规则赋予不同的重要程度。最重要的是作者样式表，其次是用户样式表，最后是浏览器的默认样式表。在此基础上，规则再按选择符的特殊性排序。

为了给用户更高的优先权，CSS 允许用户使用 `!important` 覆盖任何规则。

```css
p {
  font-size: 1.5em !important;
  color: #666 !important;
}
```

### 特殊性

为了量化规则的特殊性，每种选择符都对应着一个数值。一条规则的特殊性就表示为其每个选择符的累加数值。但这里的累加计算使用的并非我们熟悉的十进制加法，而是基于位置累加，以保证 10 个以上类选择符累加的特殊性不会大于等于 1 个 ID 选择符的特殊性。这是为了避免 ID 这种高特殊性选择符被一堆低特殊性选择符（如类型选择符）的累加值所覆盖。如果某条规则中用到的选择符不足 10 个，为简单起见，也可以使用十进制来计算其特殊性。

任何选择符的特殊性都对应于如下 4 个级别，即 a、b、c、d：

- 行内样式，a 为 1；
- b 等于 ID 选择符的数目；
- c 等于类（class）选择符、伪类选择符及属性选择符的数目；
- d 等于类型（type）选择符和伪元素选择符的数目。

下面是特殊性计算举例：

| 选择符                     | 特殊性  | 十进制特殊性 |
| -------------------------- | ------- | ------------ |
| `style=""`                 | 1,0,0,0 | 1000         |
| `#wrapper #content {}`     | 0,2,0,0 | 200          |
| `#content .datePosted {}`  | 0,1,1,0 | 110          |
| `div#content {}`           | 0,1,0,1 | 101          |
| `#content {}`              | 0,1,0,0 | 100          |
| `p.comment .datePosted {}` | 0,0,2,1 | 21           |
| `p.comment {}`             | 0,0,1,1 | 11           |
| `div p {}`                 | 0,0,0,2 | 2            |
| `p {}`                     | 0,0,0,1 | 1            |

通用选择符（`*`）的特殊性为 0。

如果两条规则特殊性相等，则优先应用后定义的规则。

### 继承

有些属性，像颜色或字体大小，会被应用它们的元素的后代所继承。继承是很有用的机制，有了它就可以避免给一个元素的所有后代重复应用相同的样式。

继承的属性值没有任何特殊性，连 0 都说不上。通用选择符的特殊性为 0，但仍然优先于继承的属性。

## 第 3 章 可见格式化模型

浮动、定位和盒模型是学习 CSS 需要掌握的几个最重要的概念。

### 盒模型

页面中的所有元素都被看作一个矩形盒子，这个盒子包含元素的内容、内边距、边框和外边距。内边距（padding）是内容区周围的空间。给元素应用的背景会作用于元素内容和内边距。边框（border）会在内边距外侧增加一条框线，这条框线可以是实线、虚线或点划线。边框的外侧是外边距（margin），外边距是围绕在盒子可见部分之外的透明区域，用于在页面中控制元素之间的距离。

对元素盒子而言，内边距、边框和外边距不是必需的，因此它们的默认值都为 0。

默认情况下，元素盒子的 `width` 和 `height` 属性指的是内容盒子，也就是元素可渲染内容区的宽度和高度。这时候添加边框和内边距并不会影响内容盒子的大小，但会导致整个元素盒子变大。通过修改 `box-sizing` 属性可以改变计算盒子大小的方式。`box-sizing` 的默认值为 `content-box`，如果吧 `box-sizing` 修改为 `border-box`，那么 `width` 和 `height` 属性的值将会包含内边距和边框。

### 可见格式化模型

`p`、`h1`、`article` 等元素属于块级元素，会显示为**块级盒子**，`strong`、`span`、`time` 等元素属于行内元素，会显示为**行内盒子**。

块级盒子会沿垂直方向堆叠，盒子在垂直方向上的间距由它们的上、下外边距决定。

行内盒子是沿文本流水平排列的，也会随文本换行而换行。它们之间的水平间距可以通过水平方向的内边距、边框和外边距来调节。但行内盒子的高度不受其垂直方向上的内边距、边框和外边距的影响。此外，给行内盒子明确设置高度和宽度也不会起作用。

由一行文本形成的水平盒子叫**行盒子**（line box），而行盒子的高度由所包含的行内盒子决定。修改行盒子大小的唯一途径就是修改行高（`line-height`），或者给它内部的行内盒子设置水平方向的边框、内边距或外边距。

垂直方向上的两个外边距相遇时，会折叠成一个外边距，折叠后外边距的高度等于两者中较大的那一个高度，这种机制叫做外边距折叠。

行内盒子、浮动盒子或绝对定位盒子的外边距不会折叠。

把一个元素的 `display` 属性设置为 `relative`，该元素仍然会待在原来的地方。无论是否位移，相对定位的元素仍然会在文档流中占用初始的空间。

绝对定位则会把元素拿出文档流，因此也就不会在占用原来的空间。

要阻止行盒子环绕在浮动盒子外面，需要给包含行盒子的元素应用 `clear` 属性。`clear` 属性的值有 `left`、`right`、`both` 和 `none`，用于指定盒子的哪一侧不应该紧挨着浮动盒子。很多人认为 `clear` 属性只是简单地删除几个用于抵消前面浮动元素的标记，事实却没有这么简单。清除一个元素时，浏览器会在这个元素上方添加足够大的外边距，从而将元素的上边沿垂直向下推移到浮动元素下方。因此，如果你给「已清除的」元素添加外边距，那么除非你的值超过浏览器自动添加的值，否则不会看到什么效果。

### 格式化上下文

当元素在页面上水平或垂直排布时，他们之间如何相互影响，CSS 有几套不同的规则，其中有一套规则叫做**格式化上下文**（formatting content）。前面我们已经介绍了**行内格式化上下文**（inline formatting context）的一些规则。比如，垂直外边距对于行内盒子没有影响。类似的，有的规则适用于块级盒子的叠放，比如前面介绍的外边距折叠。

此外，有些规则规定了页面必须自动包含突出的浮动元素（否则浮动元素中的内容可能会跑到可滚动区域之外），而且所有块级盒子的左边界默认与包含块的左边界对齐（如果文字顺序是从右向左，那么与包含块的右边界对齐）。这组规则就是**块级格式化上下文**（block formatting context）。

还有些规则允许元素建立自己内部的块级格式化上下文，包括：

- `display` 属性值设置为 `inline-block` 或 `table-cell` 之类的元素，可以为内容创建类似块级的上下文；
- `float` 属性值不是 `none` 的元素；
- 绝对定位的元素；
- `overflow` 属性值不是 `visible` 的元素。

当一个元素具备了触发新块级格式化上下文的条件，并且挨着一个浮动元素时，它就会忽略自己的边界必须接触自己的包含快边界的规则。此时，这个元素会收缩到适当大小。不仅行盒子如此，所有盒子都是如此。

下面看一个使用常规方法清除浮动的示例：

```css
.media-block {
  background-color: gray;
  border: solid 1px black;
}
.media-fig {
  float: left;
  width: 30%;
}
.media-body {
  float: right;
  width: 65%;
}
.media-block::after {
  content: ' ';
  display: block;
  clear: both;
}
```

```html
<div class="media-block">
  <img class="media-fig" src="/img/pic.jpg" alt="The pic" />
  <div class="media-body">
    <h3>Title of this</h3>
    <p>Brief description of this</p>
  </div>
</div>
```

通过触发新的块级格式化上下文的方式，我们可以换种方式清除浮动：

```css
.media-block {
  background-color: gray;
  border: solid 1px black;
}
.media-fig {
  float: left;
  width: 30%;
  margin-right: 5%;
}
.media-block,
.media-body {
  overflow: auto;
}
```

这样就能实现我们的目标：

- 不用设置清除规则，就可以让 `.media-block` 包住浮动的图片，因为块级格式化上下文自动包含浮动；
- 顺带着，我们可以放弃给 `.media-body` 声明宽度和浮动。这是因为它会自动调整以适应浮动元素旁边的剩余空间，并确保挨着图片的一遍是直的。如果这里没有新的格式化上下文，而且文本比较多，那么位于浮动 `.media-fig` 下方的行盒子都会伸长，最终填满图片下方的空间。

尽量基于简单且可预测的行为来创建布局，这样可以降低代码复杂度，并提高布局稳健性。

## 第 4 章 网页排版

自印刷出版物诞生以来，排版就一直是平面设计的基础。

### CSS 的基本排版技术

当 `em` 用于计算盒模型的大小时，它不是基于继承的 `font-size`，而是基于元素自身计算的 `font-size`。

`font-size` 到底应该选多大，其实没有硬性要求。我们这里介绍一个适用于标题的大小的叫做「纯四度」（perfect fourth）的数学比例，即上一级标题会比下一级标题的字型大小大自身尺寸的 1/4，或者说是下一级标题字型大小的 1.3333... 倍。

```css
h1 {
  font-size: 2.315em; /* 37px */
}
h2 {
  font-size: 1.75em; /* 28px */
}
h3 {
  font-size: 1.314em; /* 21px */
}
```

此类比例关系对于初始阶段的网页设计至关重要。推荐大家试试 [Modular Scale](https://www.modularscale.com) 计算器，其中有很多预设比例关系可供参考。

![inline box](https://user-images.githubusercontent.com/18362949/65372704-f9318f00-dca5-11e9-9ad8-1250252e143d.png)

行内盒子中的内容区显示文本。内容区的高度由 `font-size` 的测量尺度，即上图中 `n` 那个 `1em` 见方的块，以及这个块与字形本身的关系来决定。

小写字母 `x` 的上边界决定了所谓的「x 高度」。

字形会被摆放在内容区中，每个字形都在垂直方向上不偏不倚，使得每个行内盒子的底边都默认对齐于靠近底部的共同水平线，这条线叫基线。内容区也不一定会限制住字形，比如某些字体中的小写字母「g」就会向下伸出内容区。

行高指的是行盒子的总高度。更通俗的叫法是行间距，排版术语叫铅空，就是排字员用来隔开字符行的铅块。

行盒子的整体行高减去 `font-size`，得到的值再平分成两份，也就是半铅空。

如果行盒子中包含多个行高不一的行内盒子，那么这个行盒子的最终高度至少等于其中最高的行内盒子。

```css
body {
  font-family: Georgia, Times, 'Times New Roman', serif;
  line-height: 1.5;
}
```

一般来说，行高取值在 1.2 ~ 1.5 范围内。关键是行与行之间既不能太密，也不能太疏。

这里给 `line-height` 设置类没有单位的值 1.5，意思就是当前 `font-size` 的 1.5 倍。`body` 的 `font-size` 为 16px，那么默认的 `line-height` 就是 24px。

也可以给 `line-height` 设置像素值、百分比值或 `em` 值，但要注意 `body` 元素的所有子元素都会继承 `line-height` 的计算值。换句话说，就算 `body` 用的是百分比值或 `em`，其子元素继承的都是计算后得到的像素值，但无单位的值就不会导致这个结果。因此，如果给 `lien-height` 设置没有单位的值，那么子元素继承的是一个系数，永远与自己的 `font-size` 成比例。

除了 `line-height`，行内盒子也会受到 `vertical-align` 属性的影响。它的默认值是 `baseline`，即子元素的基线与父元素的基线对齐。其他值还有 `sup`、`top`、`bottom`、`text-top`、`text-bottom` 和 `middle`。

如果行盒子中有一个元素使用 `vertical-align` 调整了位置，那么它可能会扩展行盒子的高度。

与行内文本相比，行内块和图片的垂直对齐行为稍有不同，因为它们不一定有自己的唯一基线。

我们使用 `font-weight` 属性来设置标题文本的粗细。关键字有以下几个：`normal`、`bold`、`bolder` 和 `lighter`，也可以直接给出数字值：100，200，300，……，900。默认值 `normal` 对应 400，`bold` 对应 700，这两个粗细值是最常用的。

### 版心宽度、律动和毛边

对阅读体验有着重大影响的因素：行长。用排版的行话说，就是版心宽度。过长或过短的文本行会打断人的眼球移动，导致读者无法连续阅读。

一行文本到底多长才合适，并没有什么终极答案。Robert Bringhurst 的经典图书 _The Elements of Typographic Style_ 提到，主题内容的文本行长通常是 45 ~ 75 个字符，平均值为 66 个字符。

### Web 字体

在下载 Web 字体的时候，浏览器有两种方式处理相应的文本内容。第一种方式是在字体下载完成前暂缓显示文本，术语叫 FOIT（flash of invisible text）。Safari、Chrome 和 IE 默认采用这种方式，问题是用户必须等待字体下载完成才能看到内容。如果用户的网络速度很慢，这个问题会非常明显。

第二种方式是在字体下载完成前，浏览器先用一种后备字体显示内容。这样可以避免因网速慢而引起的问题，但也会带来字体切换时的闪动问题。这个闪动有时候也被称为 FOUT（flash of unstyled text）。

## 第 5 章 漂亮的盒子

### 背景附着

背景会附着在指定元素的后面，如果你滚动页面，那么背景也会随着元素移动而移动。可以通过 `background-attachment` 属性改变这种行为。

```css
.profile-box {
  background-attachment: fixed;
}
```

随着页面滚动，该背景图片不会动，好像藏到了页面后面一样。

### 多重背景

Level 3 Backgrounds and Borders 规范现在支持一个元素设置多个背景图片。因此，每个背景属性也就有了相应的多值语法，多个值由逗号分隔。

```css
.multi-bg {
  background-image: url(img/spades.png), url(img/hearts.png), url(img/diamonds.png), url(img/clubs.png);
  background-position: left top, right top, left bottom, right bottom;
  background-repeat: no-repeat, no-repeat, no-repeat, no-repeat;
  background-color: pink;
}
```

多重背景按声明的先后次序自上而下堆叠，最先声明的在最上面，最后声明的在最下面。背景颜色层在所有背景图片下面。

### 可保持宽高比的容器

对于具有固定宽高比的位图，把高度设置为 `auto`，只改变宽度，或者把宽度设置为 `auto`，只改变高度，都是可以的。

但是如果没有固定宽高比的元素呢？如何是其在可伸缩的同时保持固定宽高比？

`iframe` 和 `object` 就属于这种情形，某些情况下的 SVG 内容也是。常见的例子是在页面中通过 `iframe` 嵌入一段视频：

```html
<iframe
  width="420"
  height="315"
  src="https://www.youtube.com/embed/dQw4w9WgXcQ"
  frameborder="0"
  allowfullscreen
></iframe>
```

如果像这样给它设置一个可伸缩的宽度：

```css
iframe {
  width: 100%; /* 或者其他任何比例 */
}
```

就会导致 `iframe` 宽度为 100%，而高度始终是 315 像素。因为视频本身也有宽高比，所以我们希望这里的高度也可以自适应。

此时无论把 `iframe` 的高度设置为 `auto` 还是删除 `height` 属性都不管用，因为 `iframe` 本身没有固定的宽高比。此外，这样做很可能导致 `iframe` 的高度变成 150 像素。为什么是 150 像素呢？CSS 规范指出，对于没有指定大小的可替代内容（如 `iframe`、`img`、`object`），最终的默认大小为 300 像素宽或 150 像素高。

要解决这个问题，需要借助一些巧妙的 CSS 技术。 首先，把 `iframe` 包在一个元素里：

```html
<div class="object-wrapper">
  <iframe
    width="420"
    height="315"
    src="https://www.youtube.com/embed/dQw4w9WgXcQ"
    frameborder="0"
    allowfullscreen
  ></iframe>
</div>
```

然后让这个包装元素的尺寸与要嵌入的对象具有相同的宽高比。简单计算一下，用原始的高度 315 像素除以原始宽度 420 像素，结果是 0.75。换句话说，高度是宽度的 75%。

接下来，将包装元素的高度设置为 0，但把 `padding-bottom` 设置为 75%：

```css
.object-wrapper {
  width: 100%;
  height: 0;
  padding-bottom: 75%;
}
```

之前介绍过，内边距和外边距如果使用百分比值来设置，那它们的实际值是基于包含块的宽度来计算的。这里的宽度是 100%（与包含块宽度相等），因此内边距就是包含块的 75%。于是我们就创建了一个具有宽高比的元素。

最后，在这个包装元素中绝对定位嵌入对象。尽管包装元素的高度是 0，仍然可以通过绝对定位把嵌入对象放到一个「可保持宽高比」的内边距盒子里：

```css
.object-wrapper {
  width: 100%;
  height: 0;
  padding-bottom: 75%;
  position: relative;
}

.object-wrapper iframe {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}
```

成功！这样就可以在页面中包含可伸缩的嵌入对象了。

## 第 6 章 内容布局

### 定位

定位并不适合总体布局，因为它会把元素拉出页面的正常流。关于定位的总结如下：

- 元素的初始定位方式为静态定位（`static`），意思是块元素垂直堆叠
- 可以把元素设置为相对定位（`relative`），然后可以相对于其原始位置控制该元素的偏移量，同时又不影响其周围的元素。与此同时，这也为该元素的后代元素创造了定位上下文。这一点也是相对定位真正的用处。
- 绝对定位（`absolute`）支持精确定位元素，相对于其最近的定位上下文：或者是其非静态定位的祖先元素，或者是 html 元素。绝对定位的元素会脱离页面流，然后再相对于其定位上下文进行定位。默认情况下，它们会被浏览器定位与之前静态定位时所处的位置，但不会影响周围的元素。然后，我们可以相对于定位上下文来改变它们的位置。
- 固定定位（`fixed`）与绝对定位基本类似，只不过定位上下文被自动设置为浏览器视口。

绝对定位非常适合创建弹出层、提示和对话框这类覆盖于其他内容之上的组件。

要用好定位，还有一个重点技术必须掌握，那就是 `z-index`，也就是堆叠元素的次序。静态定位（`static`）以外的元素会根据它们在代码树中的深度依次叠放，就像打扑克发牌一样，后发的牌会压在先发的牌上面。它们的次序可以通过 `z-index` 来调整。

除了 `z-index`，还有其他影响元素堆叠次序的因素。这里有一个概念，叫**堆叠上下文**。就像一盒扑克牌，每张牌本身也是一个上下文（牌盒），而牌只能相对当前的牌盒排定次序。有一个根堆叠上下文，所有 `z-index` 不是 `auto` 的定位元素都会在这个上下文中排序。随着其他上下文的建立，就会出现堆叠层级。

堆叠上下文是由特定属性和值创建的。比如，任何设定了 `position: absolute` 及值不是 `auto` 的 `z-index` 属性的元素，都会创建一个自己后代元素的堆叠上下文。

在一个堆叠上下文内部，无论 `z-index` 值多大或多小，都不会影响其他堆叠上下文，毕竟不能相对于别的堆叠上下文重新排序。

设置小于 1 的 `opacity` 值也可以触发新的堆叠上下文。

对于利用行内块创建水平布局而言，如果需要垂直对齐，有以下两点：

- 要让行内块沿上方对齐（很像浮动），设置：`vertical-align: top`；
- 要让两个元素的内容垂直对齐，先把它们都转换成行内块，再对它们应用 `vertical-align: middle`。

对于每个块都占据确切宽度的水平布局而言，空白是一个突出的问题。HTML 源代码中的换行符会被渲染成空白符，如果水平布局的几个行内块的宽度之和恰好等于包含元素宽度，那么这几个行内块一定会发生折行。解决这种问题的方法简单粗暴，就是把包含元素的 `font-size` 设置为 0（从而让每个空格的宽度为 0）。

### Flexbox

Flexbox，也就是 Flexible Box Layout 模块，是 CSS 提供的用于布局的一套新属性。这套属性包含针对容器（弹性容器，flex container）和针对其直接子元素（弹性项，flex item）的两类属性。Flexbox 可以控制弹性项的如下方面：

- 大小，基于内容及可用空间；
- 流动方向，水平还是垂直，正向还是反向；
- 两个轴向上的对齐与分布；
- 顺序，与源代码中的顺序无关。

Flexbox 可以针对页面中某一区域，控制其中元素的顺序、大小、分布及对齐。这个区域内的盒子可以沿两个方向排列：默认水平排列（成一行），也可以垂直排列（成一列）。这个排列方向称为**主轴**（main axis）。

与主轴垂直的方向称为**辅轴**（cross axis），区域内的盒子可以沿辅轴发生位移或伸缩。Flexbox 布局中最重要的尺寸就是主轴方向的尺寸：水平布局时的宽度或垂直布局时的高度。我们称主轴方向的这个尺寸为**主尺寸**（main size）。

如果不指定大小，Flex 容器内的项目就会自动收缩。也就是说，一行中的各项会收缩到各自的最小宽度，或者一列中的各项会收缩到各自的最小高度，以恰好可以容纳自身内容为限。

Flexbox 对子项的排列有多种方式。沿主轴的排列叫排布（justification），沿辅轴的排列则叫对齐（alignment）。

用于指定排布方式的属性就是 `justify-content`，其默认值是 `flex-start`，表示按照当前文本方向排布（也就是向左对齐）。`justify-content` 的其他几个关键字为：`flex-end`、`center`、`space-between`、`space-around`。

Flexbox 不允许通过以上这些关键字指定个别项的排布方式。然而，对 Flexbox 的子项指定值为 `auto` 的外边距在这里却有不同的含义。具体来说，如果指定某项一侧的外边距值为 `auto`，而且在容器里那一侧还有空间，那么该外边距就会扩展占据可用空间。利用这一点，可以创造让一项位于一侧，其他项位于另一侧的布局。

```css
.navbar li:first-child {
  margin-right: auto;
}
```

像上面这样，对第一项应用 `margin-right: auto` 可以吃掉所有剩余空间，把其他项推到右侧。

控制辅轴对齐的属性 `align-items`，其默认值是 `stretch`。也就是说，子项默认拉伸，以填满可用空间。其他的关键字还有 `flex-start`、`center`、`flex-end`、`baseline`。`baseline` 关键字可以将子项中文本的基线与容器基线对齐，效果与行内块的默认行为类似。如果子项大小不一，而你希望它们在辅轴上虽然位置不同，但本身对齐，那么就可以采用这种方法。

除了同时对齐所有项，还可以在辅轴上使用 `align-self` 指定个别项的对齐方式。

Flexbox 可以让我们轻松解决垂直对齐问题。在容器里面只有一个元素时，只要将容器设置为 `flex`，再将需要居中的元素的外边距设置为 `auto` 就行了。这是因为 Flexbox 中各项的自动外边距会扩展「填充」相应方向的空间。

```html
<div class="flex-container">
  <div class="flex-item">
    <h2>Not so lost in space</h2>
    <p>This item sits right in the middle of its container...</p>
  </div>
</div>
```

想要水平并且垂直居中 `.flex-item`，仅需要以下 CSS 代码，无论容器或其中元素有多大。

```css
html,
body {
  height: 100%;
}
.flex-container {
  height: 100%;
  display: flex;
}
.flex-item {
  margin: auto;
}
```

Flexbox 支持对元素大小的灵活控制。这一点既是实现精确内容布局的关键，也是迄今为止 Flexbox 中最复杂的环节。可伸缩的尺寸在实现之前，确实是很难预测的。

Flex 的意思是「可伸缩」，这体现在以下 3 个属性中：`flex-basis`、`flex-grow` 和 `flex-shrink`。这 3 个属性应用给每个可伸缩项，而不是容器。

- `flex-basis`：控制项目在主轴方向上、经过修正之前的「首选」大小（`width` 或 `height`）。可以是长度值（如 `18em`）、百分比（相对于容器的主轴而言），也可以是关键字 `auto`（默认值）。关键字 `auto` 的意思好像是把 `width` 或 `height` 设置为自动，但实际上并不是那么回事。这里 `auto` 值的意思是这个项目可以从对应的属性（`width` 或 `height`）那里获得主尺寸 -- 如果设置了相应属性的话。如果没有设置主尺寸，那么该项目就根据其内容确定大小，有点类似浮动元素或行内块。也可以设置 `content` 值，意思也是根据项目内容确定大小，但是会忽略通过 `width` 或 `height` 设置的主轴尺寸（与 `auto` 不同了）。

- `flex-grow`：一个弹性系数（flex factor）。在通过 `flex-basis` 为每一项设置了首选大小之后，如果还有剩余空间，该系数表示该如何处理。其值是一个数值，表示剩余空间的一个比值。默认值是 0，表示从 `flex-basis` 取得尺寸后就不再扩展。

- `flex-shrink`：也是一个弹性系数，与 `flex-grow` 类似，但作用相反。换句话说，如果空间不够了，该如何收缩？增加了 `flex-shrink` 这个因素之后，计算过程就更复杂了。默认值是 1，表示如果空间不够，所有项都会以自己的首选尺寸为基准等比例收缩。

要理解 `flex-basis` 与 `flex-grow` 以及 `flex-shrink` 的关系可并不容易。Flexbox 使用了相当复杂的算法来计算各伸缩项的大小。但是，如果我们将计算过程简化成以下两个步骤，那么理解起来就容易多了。

（1）检查 `flex-basis`，确定假想的主尺寸。

（2）确定实际的主尺寸。如果按照假想的主尺寸把各项排布好之后，容器内还有剩余空间，那么它们可以伸展。伸展多少由 `flex-grow` 系数决定。相应地，如果容器装不下那么多项，则根据 `flex-shrink` 系数决定各项如何收缩。

使用 `flex` 简写属性可以一次性设置 `flex-grow`、`flex-shrink` 和 `flex-basis` 属性：

```css
.navbar li {
  flex: 1 0 0%;
}
```

在多行布局中，我们可以相对于容器来对齐行或列。我们可以使用一个叫做 `align-content` 的属性，默认值为 `stretch`，意思是每一行都会拉伸自己以填充自己应占的容器高度。`align-content` 对容器中多行的作用，与 `justify-content` 对主轴内容排布的作用非常相似。`align-content` 的其他取值为：`flex-start`、`flex-end`、`center`、`space-between`、`space-around`。

使用 Flexbox 的 `order` 属性，可以完全摆脱项目在源代码中的顺序的约束。只要告诉浏览器这个项目排在第几就行了。默认情况下，每个项目的 `order` 值都为 0，意味着按照它们在源代码中的顺序出现。

通过 Flexbox 重拍次序只影响呈现的效果。按 Tab 键切换键盘焦点和屏幕阅读器并不会受 `order` 属性的影响。因此 HTML 代码还是要按照逻辑来写。

## 第 7 章 页面布局与网格

### 布局规划

规划阶段的关键在于从设计方案中找出重复的模式，并识别出一些本质的东西。

说到一个网站的整体布局，大家经常会想到网格系统。网格系统是设计师在切分布局时作为参照的一组行和列。行和列之间的空白叫做空距（gutter）。无论是设计师还是开发者，对他们说「一个元素占三列，左右各有一个空距」，谁都能明白。

按照最传统的说法，行和列指的是网格中的横条或竖条，分别撑满网格的宽度或高度。行与列相交的一个单元格，称为单元（unit）或模块（module）。多个单元按照某个比例可以构成更大的区块，比如三行两列。这些单元组合构成的区块，或水平或垂直，过去被称为区域（field）或范围（region）。

- 固定布局。指页面具有特定的宽度，比如 960 像素。固定布局已经流行很长时间了，因为这样设计师和开发者会轻松很多。但是，也有设计师质疑到底什么尺寸才是最好的：现在用户屏幕的主流宽度是 1024 像素，还是 1280 像素呢？

- 弹性布局。指布局元素的尺寸使用 em 单位。这样，即使用户缩放文本大小，布局的比例也不会变。再与最小和最大宽度结合使用，还能使页面更好地适应屏幕大小。虽然弹性布局在今天有点过时了，但其利用最大宽度限制 em 单位的思想是创建流动布局的关键。

- 流动布局。也称为「流式布局」，指页面元素会按比例缩放，但元素与元素之间的比率（有时候连元素之间的距离也）保持不变。这其实是 Web 的默认模式，即块级元素没有预置的宽度，其尺寸会随可用空间大小而变化。

### 创建灵活的页面布局

（1）包装元素

包装元素是页面布局中常用的一个盛放内容的元素，比如：

```html
<body>
  <div class="wrapper">
    <h1>My Page Content</h1>
  </div>
</body>
```

为什么不使用 body 作为包装元素呢，毕竟他就在那儿。这是因为很多时候我们需要的不仅仅是一个包装元素。比如，包装元素外面可能会有个宽度不同的网站级的导航条，或者几个跟屏幕一样宽的区块中分别包含一个居中的包装元素。

对于流动布局而言，使用百分比来设置一个稍微小于 100% 的宽度是很常见的。最大宽度则是相对与文本大小来设置，单位是 em。

```css
.wrapper {
  width: 95%;
  max-width: 76em;
  margin: 0 auto;
}
```

现在只要应用 wrapper 类就好了。

```html
<header class="masthead">
  <div class="wrapper">
    <h1>Important News</h1>
  </div>
</header>
<nav role="navigation" class="navbar">
  <div class="wrapper">
    <ul class="navlist">
      <li><a href="/">Home</a></li>
      <!-- ...还有更多 -->
    </ul>
  </div>
</nav>
<main class="wrapper">
  <!-- 这里是主体内容 -->
</main>
```

（2）行容器

我们想让行组件可以包含浮动元素。之前我们介绍过，只要创建一个块级格式上下文，就可以通过 overflow 属性来包含浮动元素。虽然对于较小的组件来说，使用 overflow 会比较容易实现包含，但这里使用的是一个设置了清除的伪元素。因为比较大的区块很可能会有定位内容被摆放到行容器之外，所以使用 overflow 可能对我们不利。

```css
.row::after {
  content: '';
  display: block;
  clear: both;
  height: 0;
}
```

（3）创建列

行容器的样式写好了，下面就该把行分成列了。此时最重要的是确定使用哪种水平布局的方法。上一章我们介绍过水平布局的几种方法，其中浮动是最常用的，也是浏览器支持最好的技术。因此，这里使用浮动创建列。对于从左到右书写的语言，默认的向左浮动应该是最佳选择。

考虑到将来可能会在不影响列宽度的前提下，直接给列容器添加边框和内边距，还应该把 box-sizing 属性设置为 border-box。

```css
.col {
  float: left;
  box-sizing: border-box;
}
```

下面又该确定如何设置列宽了。如果我们想通过可重用的类名来控制尺寸，就必须让标记与表现有个结合点。可一个这个结合点换个名字，不适用特定的宽度或者比率，让它更加普适。用音乐来比喻的话，可以创建一条规则，让行容器在正常情况下包含 4 个宽度相等的部分（quartet，四重奏）。

```css
.row-quartet > * {
  width: 25%;
}
```

```html
<div class="row row-quartet">
  <div class="col"></div>
  <div class="col"></div>
  <div class="col"></div>
  <div class="col"></div>
</div>
```

这样，.row-quartet 中的列如果想改变宽度，就可以应用覆盖宽度的一个类名，但这个类名并不与布局相关。

```html
<div class="row row-quartet">
  <div class="col my-special-column"></div>
  <div class="col"></div>
  <div class="col"></div>
</div>
```

```css
.my-special-column {
  width: 50%;
}
```

除了四重奏，当然还应该有三重奏：

```css
.row-quartet > * {
  width: 25%;
}
.row-trio > * {
  width: 33.3333%;
}
```

（4）流式空距

在流动布局中，空距可以是百分比，也可以是相对于字体大小的固定宽度。不管采用哪种方式，列元素两边的宽度都应该相等。

如果你想给列添加背景颜色或图片，而且希望背景和图片也保持间距，那就应该以外边距作为空距。这样，兼容不支持 box-sizing 的古老浏览器也是没有问题的。对于流动布局，应该使用百分比定义外边距。这是因为，如果没有 calc()，那么百分比和其他长度单位混合使用会让调试变得很麻烦，而且旧版本浏览器也不支持 calc()。

```css
.col {
  float: left;
  box-sizing: border-box;
  margin: 0 0.9% 1.375em;
}
```

### 二维布局：CSS Grid Layout

说到整体的页面布局，我们之前学习的任何技术都不是一个全面的解决方案，不能在二维空间里控制元素的顺序、位置和大小。

使用 Grid Layout 模块，可以抛开之前用到的很多辅助控制元素，从而大幅精简 HTML 标记。与此同时，这个模块也把基于元素本身来设置水平和垂直维度的负担，转移到了在页面中表示网格的一个包含元素上。

（1）网格布局的术语

![grid-layout](https://user-images.githubusercontent.com/18362949/66015904-fedd6f00-e506-11e9-97bc-2b3dc6c55728.png)

_网格容器及其组件_

下面来解释一下：

- 被设置为 display: grid 的元素叫网格容器（grid container），即图中的粗线框区域。
- 容器进一步被网格线（grid line）划分为不同的区域，叫网格单元（grid cell）。
- 网格线之间的水平或垂直路径叫网格轨道（grid track）。具体来说，水平方向的网格轨道叫网格行（grid row），垂直方向网格轨道叫网格列（grid column）。
- 由相邻网格单元组合起来的矩形区块叫网格区（grid area）。
- 网格容器的直接子元素叫网格项（grid item），网格项可以放在网格区内。

（2）定义行和列

创建网格需要告诉浏览器网格行与网格列的数量和行为。

```css
.wrapper {
  display: grid;
  grid-template-rows: 300px 300px;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}
```

上面的代码定义了一个 2 行 4 列的网格，行高 300 像素，4 列等宽。

这里用于表示列宽的单位是 fr，意思是可用空间中的部分（fraction of available space）。这个单位跟 Flexbox 中的扩展系数（flex-grow）非常相似，只不过这里有特定的单位符号，应该是为了避免跟其他没有单位的值发生冲突。可用空间就是网格轨道（通过明确指定的长度值或根据自己的内容）确定尺寸后的剩余空间。

每个 fr 单位在这里都表示网格可用空间的 1/4。假如再添加一个 fr，那么每个 fr 单位表示的就是可用空间的 1/5。

指定行和列的数量及大小时，可以混用不同的长度单位。比如，声明列时可以这样写：200px 20% 1fr 200px。这就是说，靠两边的两列宽度固定为 200 像素，左起第二列的宽度是总空间的 20%，而第三列则占据剩下的全部空间。换句话说，fr 单位的大小会在计算完其他长度值之后再确定，跟 Flexbox 一样。

```css
.grid-a {
  display: grid;
  grid-template-rows: auto auto auto;
  grid-template-columns: repeat(5, 1fr);
}
```

这里使用了网格布局模块提供的函数 repeat，可以用它为网格轨道指定重复的行或列声明，省去重复书写的麻烦。

因为在网格轨道在 DOM 中并没有特定的元素表示，所以不能通过 max-width 或 min-width 之类的属性来为它们指定大小。如果想在声明网格轨道时使用同样的功能，可以使用 minmax() 函数。

```css
.grid-a {
  display: grid;
  grid-template-rows: auto minmax(4em, 1fr) minmax(4em 1fr);
  grid-template-columns: repeat(5, 1fr);
}
```

使用 grid-template 属性可以把行和列声明都放在一行上，前面是行的定义，后面是列的定义，中间以斜杠（/）分隔。

```css
.grid-a {
  display: grid;
  grid-template: auto minmax(4em, 1fr) minmax(4em, 1fr) / repeat(5, 1fr);
}
```

（3）添加网格项

添加网格项要以其起止处的网格线作为参考。例如：子区块的标题区要占据左侧一整列。而添加到相应网格项的最麻烦的方式，就是同时指定两个维度上起止的网格线编号。

```css
.subsection-header {
  grid-row-start: 1;
  grid-column-start: 1;
  grid-row-end: 4;
  grid-column-end: 2;
}
```

也可以简化 grid-row 和 grid-column 属性，把行和列的起止网格线声明放在一行。起止网格线的编号以斜杠（/）分隔。

```css
.subsection-header {
  grid-row: 1/4;
  grid-column: 1/2;
}
```

假如只知道这个网格项应该跨所有行，但并不知道会有多少行，那么就需要一种方式来表示最后一行。Grid Layout 支持使用负值来反向表示行号。换句话说，-1 就是最后一个网格轨道的终止网格线的编号。另外，默认的跨度是一个网格单元，也就是说这里可以省略 grid-column 值的最后一部分。

```css
.subsection-header {
  grid-row: 1/-1;
  grid-column: 1;
}
```

最后，还有一个终极的 grid-area 属性，可以进一步简化网格项的声明。这个属性的值最多 4 个，由斜杠分隔。4 个值全给出的话，分别表示 grid-row-start、grid-column-start、grid-row-end 和 grid-column-end。

```css
.subsection-header {
  grid-area: 1/1/-1;
}
```

这段代码里省略了第 4 个参数，也就是表示列方向终止位置的值。实际上，两个方向上的终止参数都是可以省略的。省略的话，网格定位时生成的网格项在两个方向上会默认跨一个网格轨道。

添加完网格项后，它们会自动撑满相应的网格区。这里的高度自动扩展与 Flexbox 中的可伸缩项非常相似。这并非巧合。

Flexbox 和 Grid Layout 都是依据 CSS Box Alignment 规范确定其子项行为的。CSS Box Alignment 负责规范几种 CSS 上下文中元素的对齐与分布。

与 Flexbox 中的行一样，网格项的垂直对齐也是通过 align-items 和 align-self 来控制的。这两个属性的默认值都是 stretch，也就是让网格项在垂直方向上扩展以填满相应网格区。其他关键字值也跟 Flexbox 的行一样，只不过没有 flex- 前缀：start、end 和 center。

网格项与块级元素类似，会自动填充自己所在网格区的宽度，除非明确设置它的宽度。百分比值相对于网格项所在网格区（而非网格容器）的宽度来计算。

如果网格项没有在水平方向填满网格区，可以通过 justify-items 和 justify-self 属性指定它的左、中、右分布。

与 Flexbox 类似，align-self 和 justify-self 用于个别网格项。align-items 和 justify-items 则用于在网格容器上设置所有网格项的默认对齐。

在网格区没有占满的情况下可以对齐网格；同理，也可以在网格容器中对齐网格轨道。只要网格轨道的总和没有覆盖整个网格容器，就可以使用 align-content（垂直方向）和 justify-content（水平方向）来移动轨道。

在网格中创建空距的方法有很多。比如，给网格项声明外边距，利用网格轨道的不同对齐方式，或者创建空的网格轨道来充当空距。

如果你希望所有轨道间的空距都是一个固定的值，那么最简单的方法是使用如下的 grid-column-gap 和 grid-row-gap 属性。通过它们可以创建固定宽度的空距，就好像网格线有了宽度一样。这其实就相当于多栏布局中的 column-gap 或表格中的 border-spacing。

```css
.grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-column-gap: 1.5em;
  grid-row-gap: 1.5em;
}
```

（4）自动网格定位

Grid Layout 提供了一种自动定位（automatic placement）的机制。这种机制是 Grid Layout 中默认的，不会改变网格项的源代码次序。所有网格项自动从第一行第一个可用的网格单元开始，逐列填充。一行填满后，网格会自动开启一行并继续填充。

默认的自动定位算法是逐行地填充网格项，也可以设置为逐列填充，通过 grid-auto-flow 属性来控制这一顺序。

```css
.my-row-grid {
  grid-auto-flow: row;
}
.my-columnar-grid {
  grid-auto-flow: column;
}
```

这个默认定位算法很简单：从头开始，只跑一遍，逐个寻找要放置网格项的网格单元。如果网格项跨多个网格单元，那么网格中就会出现空洞。

如果改成稠密模式（默认为稀疏模式），自动定位算法会跑很多遍，每次都从头开始，尽力找到最前面的空位置。结果就是网格会更稠密。

```css
.grid {
  grid-auto-flow: row dense;
}
```

（5）网格模板区

CSS Grid Layout 的「命名模板区」（named template area）也许是其最不可思议的特性之一。通过这个特性，能够以可视化方式来指定如何排布项目。示例：

```html
<section class="subcategory">
  <div class="grid-b">
    <header class="subcategory-header"></header>
    <article class="story"></article>
    <article class="story"></article>
    <div class="ad ad1"></div>
    <div class="ad ad2"></div>
  </div>
</section>
```

然后使用 grid-template-areas 属性来声明网格布局：

```css
.grid-b {
  display: grid;
  grid-template-columns: 20% 1fr 1fr 1fr;
  grid-template-areas: "hd st1 . st2"
                       "hd st1 . st2";
}
```

grid-template-areas 属性的值是以空格分隔的字符串列表，每个字符串本身是空格分隔的自定义标识符，表示网格中的一行，其中每个标识符表示一列。标签符的名字随便起，只要不跟 CSS 的关键字冲突即可。

跨行或跨列相邻的同名网格单元构成所谓的命名网格区。命名网格区必须是矩形。用点表示的区域是匿名单元，没有名字。

为了把网格项放入网格中，我们仍然使用 grid-area 属性，但这次使用自定义的网格区名。

```css
.grid-b .subcategory-header {
  grid-area: hd;
}
.grid-b .story:nth-child(2) {
  grid-area: st1;
}
.grid-b .story:nth-child(3) {
  grid-area: st2;
}
```

## 第 8 章 响应式 Web 设计与 CSS

### 浏览器视口

视口就是浏览器显示网页的矩形区域。这个区域对布局的影响，用 CSS 的话来说，就是「有多少空间可用」。要恰当地使用视口进行响应式设计，需要理解视口的原理，以及如何操纵它。在桌面浏览器上，视口的概念很直观，就是通过 CSS 像素来合理利用视口中的空间。

这里有一点需要特别说明，那就是何为 CSS 像素。CSS 像素跟屏幕的物理像素不是一回事。CSS 中说的像素与屏幕物理像素之间存在一种灵活的对应关系。这个关系取决于硬件、操作系统和浏览器，以及用户是否缩放了页面。

来看一个具体的例子，iPhone 5 的物理像素宽度是 640 像素，但是在 CSS 中的视口宽度是 320 像素。这里明显有一个比例系数，就是每个 CSS 像素相当于 2x2 个物理像素。

「虚拟的」CSS 像素与实际的硬件像素之间的比例系数，范围从 1（1 个 CSS 像素 = 1 个物理像素）到 4（1 个 CSS 像素 = 4x4 个物理像素）不等，视设备屏幕的分辨率不同而不同。

（1）视口定义的差别

智能手机浏览器刚出现时，还没有多少网站专门针对小屏幕做过优化。于是，多数移动设备（包括平板电脑）上的浏览器都会硬性呈现桌面大小的视口，从而让未经优化的网站能够显示。通常的做法是模拟一个大约 1000 像素宽的视口，然后在其中显示缩小后的页面。这种视口称为「默认视口」，它是我们实现响应式设计的第一道「关卡」。

既然默认视口是一个模拟的视口，那就意味着还有一个与设备自身尺寸接近的视口。这个视口就称为「理想视口」。理想视口的大小因设备、操作系统和浏览器而异，但一般对手机而言，宽度大约在 300~500 CSS 像素之间；对平板电脑而言，宽度大约在 800~1400 CSS 像素之间。以 iPhone 5 为例，其理想视口的宽度就是 320 像素。

在响应式设计中，这才是我们设计要用的视口。

（2）配置视口

要让具有不同默认视口的设备都是用各自的理想视口，只要在页面的头部元素中添加一个小小的视口 meta 标签即可。这个标签如下：

```html
<meta name="viewport" content="width=device-width,initial-scale=1">
```

这行代码告诉浏览器，我们希望使用当前设备的理想尺寸（即 device-width）作为视口宽度的基准。这里同时还设置了 initial-scale=1，其作用是设置与理想视口匹配的缩放级别。这个配置也能帮我们避免 iOS 中一些奇怪的缩放行为。虽然在多数设备中，只要设置缩放级别，它们就会将视口宽度默认设置为 device-width，但为了确保跨设备和跨操作系统兼容，还是需要把这两项都设置上。

initial-scale 的值大于 1，表示要放大布局，实际会导致布局视口缩小，因为能显示的像素少了。

![dont-scale](https://user-images.githubusercontent.com/18362949/66022466-ab781a80-e520-11e9-874d-9b4c94d2025e.png)

_不要禁用缩放_

### 媒体类型与媒体查询

我们对约束布局的视口有了全面的理解，接下来讲怎么通过媒体查询让设计适配设备。

（1）媒体类型

根据设备能力来分离样式的能力，始于媒体类型。HTML 4.01 和 CSS 2.1 定义了媒体类型，用于针对特定的环境应用样式，包括屏幕显示、打印和电视等。

通过给 link 元素添加 media 属性，可以指定在哪些设备上应用相关样式，比如：

```html
<link rel="stylesheet" href="main.css" media="screen, print">
```

这段代码的意思是将相关的样式应用于（任意）屏幕显示和打印。如果不关心媒体类型，可以在这里使用 all 关键字，或者干脆不写 media 属性。逗号分隔的有效类型关键字列表，意味着只要其中一个匹配即可。如果一个都不匹配，则不应用该样式表。

除了在 HTML 中指定媒体类型，还可以在 CSS 文件中指定。最常见的方式就是使用 @media 语法，比如：

```css
@media print {
  /* 针对打印机的选择符和规则 */
  .smallprint {
    font-size: 11pt;
  }
}
```

（2）媒体查询

因为不仅要指定设备类型，还要指定设备的能力，所以 CSS3 的 Media Queries 规范应运而生。这个规范扩展了媒体类型，而且语法也是媒体类型加（包含在括号中指定媒体特性的）媒体条件。此外，在媒体选择语法中，也增加了新关键字，用于支持更复杂的逻辑。

在 link 元素中，媒体查询可以这样写：

```html
<link rel="stylesheet" href="main.css" media="screen and (min-width: 600px)">
```

这样就声明了 main.css 应用于屏幕媒体，而且媒体条件是视口至少 600 CSS 像素。

> 注意：在媒体查询并不匹配的情况下，很多浏览器仍然会下载 CSS 文件。因此，不要过度使用带媒体查询的 link 标签，否则可能导致下载过多不必要的数据，影响性能。

同样的声明可以再 CSS 文件中通过 @media 规则写成如下格式：

```css
@media screen and (min-width: 600px) {
  /* 规则 */
}
```

这里的 and 关键字负责把媒体类型与我们要测试的条件连接起来，因此可以同时测试多个条件：

```css
@media screen and (min-width: 600px) and (max-width: 800px) {}
```

使用 not 关键字可以对媒体查询取反。

```css
@media not screen {}
```

还有一个 only 关键字，其目的在于避免旧版本浏览器误解媒体查询。

正常情况下，不支持媒体查询的浏览器看到 screen and (min-width:...) 时，会认为它是语法错误的媒体类型声明，从而忽略它。但是有些旧版本浏览器可能会在读取完 screen 时停下来，认为它是一个有效的媒体类型，然后为所有屏幕媒体应用样式。

为此，Media Queries 规范特意引入了 only 关键字。这样，当前面提到的旧版本浏览器看到 only 时，就会跳过整个 @media 块，因为媒体类型中从未有过 only 这个关键字。所有支持媒体查询的浏览器则必须忽略掉 only 关键字，仿佛它不存在。

为防止旧版浏览器错误的应用样式，应该像下面这样声明只针对特定媒体类型的样式：

```css
@media only screen and (min-width: 30em) {}
```


