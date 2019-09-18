# 精通CSS（CSS-Mastery）

## 进度

- [x] 第 1 章 基础知识
- [ ] 第 2 章 添加样式
- [ ] 第 3 章 可见格式化模型
- [ ] 第 4 章 网页排版
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

微数据是跟 HTML5 一起，作为给 HTML 添加结构化数据的另一种方式而推出的。
