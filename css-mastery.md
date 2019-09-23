# CSS-Mastery

中文名：[《精通CSS》](https://book.douban.com/subject/30450258/)（第 3 版）

## 进度

- [x] 第 1 章 基础知识
- [x] 第 2 章 添加样式
- [x] 第 3 章 可见格式化模型
- [x] 第 4 章 网页排版
- [ ] 第 5 章 漂亮的盒子
- [ ] 第 6 章 内容布局
- [ ] 第 7 章 页面布局与网格
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

`<b>` 和 `<i>` 可以算是幸存的表现性标记了，它们在排版上会分别显示为粗体（bold）和斜体（italic）。这两个元素与 `<strong>` 和 `<em>` 的区别在于，它们没有任何强调自己所包含内容的意味。多数情况下，应该选择 `<strong>` 或 `<em>`，因为它们是用来强调及重点强调内容的语义正确的选择。

### 微格式

微格式是一组标准的命名约定和和标记模式，可用于表示特定的数据类型。微格式的命名约定是基于 vCard 和 iCalendar 等已有的数据格式制定的。比如下面的联系人信息就是以 hCard 格式标记的：

```html
<section class="h-card">
  <p><a class="u-url p-name" href="http://andybudd.com/">Andy Budd</a>
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

### CSS选择符

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
  <li>Services
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
h1, h2, h3, h4, h5, h6,
ul, ol, li, dl, p {
  margin: 0;
  padding: 0;
}
```

属性选择符基于元素是否有某个属性或者属性是否有某个值来选择元素。

```css
abbr[title] {
  border-bottom: 1px dotted #999;
}

input[type="submit"] {
  cursor: pointer;
}

/* 匹配以某些字符开头的属性值 */
a[href^="http:"] {}

/* 匹配以某些字符结尾的属性值 */
img[src$=".jpg"] {}

/* 匹配包含某些字符的属性值 */
a[href*="/about/"] {}

/* 匹配以空格分隔的字符串中的属性值 */
a[rel~=next] {}
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

| 选择符 | 特殊性 | 十进制特殊性 |
| --- | --- | --- |
| `style=""` | 1,0,0,0 | 1000 |
| `#wrapper #content {}` | 0,2,0,0 | 200 |
| `#content .datePosted {}` | 0,1,1,0 | 110 |
| `div#content {}` | 0,1,0,1 | 101 |
| `#content {}` | 0,1,0,0 | 100 |
| `p.comment .datePosted {}` | 0,0,2,1 | 21 |
| `p.comment {}` | 0,0,1,1 | 11 |
| `div p {}` | 0,0,0,2 | 2 |
| `p {}` | 0,0,0,1 | 1 |

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

`p`、`h1`、`article` 等元素属于块级元素，会显示为块级盒子，`strong`、`span`、`time` 等元素属于行内元素，会显示为行内盒子。

块级盒子会沿垂直方向堆叠，盒子在垂直方向上的间距由它们的上、下外边距决定。

行内盒子是沿文本流水平排列的，也会随文本换行而换行。它们之间的水平间距可以通过水平方向的内边距、边框和外边距来调节。但行内盒子的高度不受其垂直方向上的内边距、边框和外边距的影响。此外，给行内盒子明确设置高度和宽度也不会起作用。

由一行文本形成的水平盒子叫行盒子（line box），而行盒子的高度由所包含的行内盒子决定。修改行盒子大小的唯一途径就是修改行高（`line-height`），或者给它内部的行内盒子设置水平方向的边框、内边距或外边距。

垂直方向上的两个外边距相遇时，会折叠成一个外边距，折叠后外边距的高度等于两者中较大的那一个高度，这种机制叫做外边距折叠。

行内盒子、浮动盒子或绝对定位盒子的外边距不会折叠。

把一个元素的 `display` 属性设置为 `relative`，该元素仍然会待在原来的地方。无论是否位移，相对定位的元素仍然会在文档流中占用初始的空间。

绝对定位则会把元素拿出文档流，因此也就不会在占用原来的空间。

要阻止行盒子环绕在浮动盒子外面，需要给包含行盒子的元素应用 `clear` 属性。`clear` 属性的值有 `left`、`right`、`both` 和 `none`，用于指定盒子的哪一侧不应该紧挨着浮动盒子。很多人认为 `clear` 属性只是简单地删除几个用于抵消前面浮动元素的标记，事实却没有这么简单。清除一个元素时，浏览器会在这个元素上方添加足够大的外边距，从而将元素的上边沿垂直向下推移到浮动元素下方。因此，如果你给「已清除的」元素添加外边距，那么除非你的值超过浏览器自动添加的值，否则不会看到什么效果。

### 格式化上下文

当元素在页面上水平或垂直排布时，他们之间如何相互影响，CSS 有几套不同的规则，其中有一套规则叫做格式化上下文（formatting content）。前面我们已经介绍了行内格式化上下文（inline formatting context）的一些规则。比如，垂直外边距对于行内盒子没有影响。类似的，有的规则适用于块级盒子的叠放，比如前面介绍的外边距折叠。

此外，有些规则规定了页面必须自动包含突出的浮动元素（否则浮动元素中的内容可能会跑到可滚动区域之外），而且所有块级盒子的左边界默认与包含块的左边界对齐（如果文字顺序是从右向左，那么与包含块的右边界对齐）。这组规则就是块级格式化上下文（block formatting context）。

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
  content: " ";
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
.media-block, .media-body {
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
