# 多看电子书CSS模板

众所周知多看的EPUB格式十分惊艳，尤其是在图片和注释方面。但是多看的epub制作工具却只有windows版本。因此我对多看EPUB内的css和xhtml进行了分析，总结出了一份css使之可以在sigil中使用，这样就可以在Linux等其他操作系统中制作出像多看这么惊艳的EPUB了。

*注：多看epub格式需要在支持的软件上才会显示。（原多看阅读现改名为小米阅读）*

# About this repository

- 库中文件说明
  - 多看EPUB原CSS文件：[duokanstyle.css](https://github.com/Eninix/sigil-template/blob/master/duokanstyle.css)
  - 必要的CSS代码总结：[duokancpt.css](https://github.com/Eninix/sigil-template/blob/master/duokancpt.css)
  - 自制的sigil用模板：[dk-template.css](https://github.com/Eninix/sigil-template/blob/master/dk-template.css)

- 如何下载
  - jsdelivr (适合国内访问)
    - [dk-template.css](https://cdn.jsdelivr.net/gh/Eninix/sigil-template/dk-template.css)
  - github (直接从GitHubRaw下载)
    - [dk-template.css](https://raw.githubusercontent.com/Eninix/sigil-template/master/dk-template.css)

**使用时可以配合本README的后半部分分析**  
**制作的EPUB在多看阅读上才能展现全部效果**



# DuokanEpub's css source code analysis

## 多看特有格式

此处只列举常用的。（这些格式只在多看中有用）

- duokan-text-indent: 0em;
	- 为多看特有的首行缩进
	- 0em 首行不缩进
	- 2em 首行缩进2字符

- class="duokan-image-single"
	- 容器中有且只有只有一个图片

## 对Epub中后缀为.opf的元数据文件的编辑

***这是非常关键的一步！！！***  
***这是非常关键的一步！！！***  
***这是非常关键的一步！！！***

如下例子所示，在properties属性中添加一些特殊的东西。

+ duokan-page-fullscreen //全屏插图页
+ duokan-page-fitwindow //上下居中插图页
+ duokan-page-fullscreen-spread-left //延伸全屏插图页

```html
<spine>
    <itemref idref="cover.xhtml" properties="duokan-page-fullscreen"/>
    <itemref idref="title.xhtml"/>
    <itemref idref="message.xhtml"/>
    <itemref idref="summary.xhtml"/>
    <itemref idref="illus-0.xhtml" properties="duokan-page-fullscreen"/>
    <itemref idref="illus-1.xhtml" properties="duokan-page-fitwindow"/>
    <itemref idref="contents.xhtml"/>
    <itemref idref="Chapter001.xhtml"/>
    <itemref idref="Chapter002.xhtml"/>
    <itemref idref="Chapter003.xhtml"/>
  </spine>
```

## 图片

```css
/* 图片-无图说 */
img.duokan-image {
	width: 100%;
}
/* 图片-有图说 */
img.duokan-image-note {
	width: 100%;
	font-size: 16px;
	margin-bottom: 0.5em;
}
```

### 全屏插图页

只需要把图片正常插入即可，其代码如下。

```html
<html xmlns="http://www.w3.org/1999/xhtml">
<head><title></title><link href="../Styles/stylesheet.css" rel="stylesheet" type="text/css" /></head>
<body>
  <p><img alt="" src="../Images/Cover.jpg" /></p> 
</body>
</html> 
```



### 随文图

直接插入即可，其代码如下。（单独一段落的图片）

```html
<img src="../Images/HARUHI_01_059.jpg" class="duokan-image" alt="" /><br />
```



### 浮层图

直接插入即可，其代码如下。（在段落文本之中的图片）

```html
<img class="duokan-image" src="../Images/HARUHI_01_117.jpg" alt="" />
```


### 并列图

```html
  <table cellspacing="0" class="duokan-gallery-image">
    <tbody>
      <tr>
        <td class="duokan-gallery-cell" width="49.8726%">
          <div class="duokan-image-single duokan-in-cell"><img class="duokan-image" src="../Images/HARUHI_01_033.jpg" alt="" /></div>
        </td>

        <td class="duokan-gallery-cell" width="50.1274%">
          <div class="duokan-image-single duokan-in-cell"><img class="duokan-image" src="../Images/HARUHI_01_059.jpg" alt="" /></div>
        </td>
      </tr>
    </tbody>
  </table>
```

- 使用HTML的表格元素实现

相关的css如下：

```css
/* 并列图表格样式 */
table.duokan-gallery-image {
	font-size: 1em;
	width: 100%;
	margin: 1em auto 1em auto;
	border-spacing: 0.3em 0.3em;
}
/* 并列图单元格样式 */
td.duokan-gallery-cell {
	text-indent: 0em;
	duokan-text-indent: 0em;
}
/* 格内图样式 */
div.duokan-in-cell {
	width: 100%;
}
```


## 图说

```html
<p class="duokan-note">
    <img class="duokan-image-note" src="../Images/HARUHI_01_117.jpg" alt="" />
    我是图说！！！！！
</p>
```

- **class**为" duokan-footnote " 。
- 有图说的
- 图说即为用p标签包裹起来的文本。

相关的css如下：

```css
/* 图说楷体居左顶格 */
p.duokan-note {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 14px;
	text-indent: 0em;
	duokan-text-indent: 0em;
	text-align: justify;
}
/* 图说楷体居中 */
p.duokan-note-center {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 14px;
	text-align: center;
}
/* 图说楷体居右 */
p.duokan-note-right {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 14px;
	text-align: right;
}
```

## 注释

多看的注释是可以在阅读器中直接点开的，这也是一个很强大的功能，其代码如下。

```html
<img class="duokan-footnote" src="../Images/note_original.png" alt="这里是一切的开端，也是地球的终焉！" id="note_1" />
```

- 注释是以**图片**的形式插入 。
- **class**为" duokan-footnote " 。
- **注释内容**在**alt**中 。
- **id字段**只做标识作用
- 当然对于不支持的阅读器，这就是一张图片。

相关的css如下：

```css
/* 多看注 */
img.duokan-footnote {
	width: 0.8em;
}
```




## 多看样式表原代码（附）

```css
/* 西文字体 */
span.duokan-western {
	font-family: "DK-SERIF", "Palatino";
}
/* 符号字体 */
span.duokan-symbol {
	font-family: "DK-SYMBOL", "Symbol";
}
/* 文字不换行（古诗） */
span.duokan-text-nowrap {
	white-space: nowrap;
}
/* 段首大字2行高 */
span.duokan-dropcaps-two {
	float: left;
	font-size: 2em;
	duokan-drop-caps-style: duokan-two-line-caps;
	margin-bottom: -0.25em;
}
/* 段首大字3行高 */
span.duokan-dropcaps-three {
	float: left;
	font-size: 3em;
	duokan-drop-caps-style: duokan-three-line-caps;
	margin-bottom: -0.15em;
}
/* 代码中文样式 */
span.duokan-code-cn {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
}
/* 超链接 */
a.duokan-hyperlink {
	color: blue;
	font-style: italic;
	text-decoration: underline;
}
/* 多看注 */
img.duokan-footnote {
	width: 0.8em;
}
/* 默认有序列表 */
ol.duokan-olist {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 16px;
}
/* 无序号有序列表 */
ol.duokan-olist-noindex {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 16px;
	list-style-type: none;
}
/* 默认无序列表 */
ul.duokan-ulist {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 16px;
	list-style-type: disc;
}
/* 无序号无序列表 */
ul.duokan-ulist-noindex {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 16px;
	list-style-type: none;
}
/* 二级列表 */
ul.list-sub2 {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 16px;
	list-style-type: circle;
	margin-left: 1em;
	margin-top: 1em;
}
/* 三级列表 */
ul.list-sub3 {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 16px;
	list-style-type: square;
	margin-left: 2em;
	margin-top: 1em;
}
/* 图片左绕排50%宽度 */
div.duokan-float-left {
	width: 50%;
	float: left;
	margin-right: 0.5em;
	margin-bottom: 0.5em;
	text-align: center;
}
/* 图片左绕排30%宽度-细高图 */
div.duokan-float-left30 {
	width: 30%;
	float: left;
	margin-right: 0.5em;
	margin-bottom: 0.5em;
	text-align: center;
}
/* 图片右绕排50%宽度 */
div.duokan-float-right {
	width: 50%;
	float: right;
	margin-left: 0.5em;
	margin-bottom: 0.5em;
	text-align: center;
}
/* 图片右绕排30%宽度-细高图 */
div.duokan-float-right30 {
	width: 30%;
	float: right;
	margin-left: 0.5em;
	margin-bottom: 0.5em;
	text-align: center;
}
/* 图片居中80%宽度 */
div.duokan-center {
	width: 80%;
	margin: 1em auto 1em auto;
	text-align: center;
}
/* 图片居中60%宽度 */
div.duokan-center60 {
	width: 60%;
	margin: 1em auto 1em auto;
	text-align: center;
}
/* 图片居中30%宽度 */
div.duokan-center30 {
	width: 30%;
	margin: 1em auto 1em auto;
	text-align: center;
}
/* 图片-无图说 */
img.duokan-image {
	width: 100%;
}
/* 图片-有图说 */
img.duokan-image-note {
	width: 100%;
	font-size: 16px;
	margin-bottom: 0.5em;
}
/* 图说楷体居左顶格 */
p.duokan-note {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 14px;
	text-indent: 0em;
	duokan-text-indent: 0em;
	text-align: justify;
}
/* 图说楷体居中 */
p.duokan-note-center {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 14px;
	text-align: center;
}
/* 图说楷体居右 */
p.duokan-note-right {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 14px;
	text-align: right;
}
/* 表格 */
table.duokan-tablebody {
	font-family: "DK-HEITI", "方正兰亭黑简体", "黑体";
	font-weight: normal;
	font-size: 14px;
	margin-top: 1em;
	margin-bottom: 2em;
	margin-left: auto;
	margin-right: auto;
}
/* 并列图表格样式 */
table.duokan-gallery-image {
	font-size: 1em;
	width: 100%;
	margin: 1em auto 1em auto;
	border-spacing: 0.3em 0.3em;
}
/* 并列图单元格样式 */
td.duokan-gallery-cell {
	text-indent: 0em;
	duokan-text-indent: 0em;
}
/* 格内图样式 */
div.duokan-in-cell {
	width: 100%;
}
/* 代码样式 */
pre.duokan-code {
	font-family: "DK-CODE", "Inconsolata";
	font-size: 14px;
}
/* 文段内-部分代码字体 */
span.duokan-nowestern-code-span {
	font-family: "DK-CODE", "Inconsolata";
	color: #91531d;
}
/* 文段内-部分代码字体-加粗 */
span.duokan-nowestern-code-span-b {
	font-family: "DK-CODE", "Inconsolata";
	color: #91531d;
	font-weight: bold;
}
/* 文段内-部分代码字体-斜体 */
span.duokan-nowestern-code-span-i {
	font-family: "DK-CODE", "Inconsolata";
	color: #91531d;
	font-style: italic;
}
/* 文段内-部分代码字体-粗体斜体 */
span.duokan-nowestern-code-span-b-i {
	font-family: "DK-CODE", "Inconsolata";
	color: #91531d;
	font-weight: bold;
	font-style: italic;
}
/* 代码块-部分加粗 */
span.code-b {
	font-weight: bold;
	color: #91531d;
}
/* 代码块-部分斜体加粗 */
span.code-italic-b {
	font-style: italic;
	font-weight: bold;
	color: #91531d;
}
/* 代码块-部分注释色 */
span.note {
	color: #008000;
}
/* 代码块-部分注释色加粗 */
span.note-b {
	color: #008000;
	font-weight: bold;
}
/* 版权信息页标题 */
h4.duokan-copyright-title {
	font-size: 24px;
	font-family: "DK-XIAOBIAOSONG", "方正小标宋简体";
	font-weight: normal;
	text-align: center;
}
/* 版权信息页正文 */
p.duokan-copyright-bodytext {
	font-family: "DK-SONGTI", "方正宋三简体","方正书宋","宋体";
	font-size: 16px;
	text-indent: 2em;
	duokan-text-indent: 2em;
}
/* 拼音样式 */
ruby {
	ruby-align: center;
	margin-right: 0.5em;
}
/* 拼音尾字样式 */
ruby.duokan-rubylast {
	margin-right: 0em;
}
/* 拼音文字样式 */
ruby > rt {
	font-family: "DK-SYMBOL", "Symbol";
	font-size: 0.5em;
}
/* 一级标题小标宋居中 */
h1.auxiliary-title1 {
	font-family: "DK-XIAOBIAOSONG", "方正小标宋简体";
	font-size: 28px;
	text-align: center;
	color: #91531d;
	font-weight: normal;
	margin-top: 2.5em;
	margin-bottom: 2.5em;
}
/* 二级标题 */
h2.chapter-title2 {
	font-family: "DK-XIAOBIAOSONG", "方正小标宋简体";
	font-size: 26px;
	text-indent: 0em;
	duokan-text-indent: 0em;
	color: #91531d;
	font-weight: normal;
	margin-top: 2em;
	margin-bottom: 1.8em;
}
/* 三级标题 */
h3.chapter-title3 {
	font-family: "DK-XIAOBIAOSONG", "方正小标宋简体";
	font-size: 22px;
	text-indent: 0em;
	duokan-text-indent: 0em;
	color: #91531d;
	font-weight: normal;
	margin-top: 2em;
	margin-bottom: 1.8em;
}
/* 内文四级标题 */
h4.chapter-title4 {
	font-size: 20px;
	font-family: "DK-HEITI", "方正兰亭黑简体", "黑体";
	font-weight: normal;
	duokan-text-indent: 0em;
	text-indent: 0em;
	margin-top: 2em;
	margin-bottom: 1.8em;
	color: #91531d;
}
/* 内文五级标题 */
h5.chapter-title5 {
	font-size: 18px;
	font-family: "DK-HEITI", "方正兰亭黑简体", "黑体";
	font-weight: normal;
	duokan-text-indent: 0em;
	text-indent: 0em;
	border-left: solid 5px #d9d9d9;
	padding-left: 0.7em;
	margin-top: 2em;
	margin-bottom: 1.8em;
	color: #91531d;
}
/* 内文六级标题 */
h6.chapter-title6 {
	font-size: 16px;
	font-family: "DK-HEITI", "方正兰亭黑简体", "黑体";
	font-weight: normal;
	duokan-text-indent: 0em;
	text-indent: 0em;
	margin-top: 2em;
	margin-bottom: 1.8em;
	margin-left: 2em;
	color: #91531d;
}
/* 章前语区域 */
div.chapter-pre-div {
	font-size: 16px;
	padding: 1em;
	margin: 3em 0em 1em 0em;
	border-left: solid 1px #e0e0e0;
}
/* 章前语楷体 */
p.chapter-pre-text {
	font-family: "DK-KAITI", "方正楷体","华文楷体","楷体";
	font-size: 16px;
	text-indent: 2em;
	duokan-text-indent: 2em;
}
/* 正文宋体 */
p.bodycontent-text {
	font-family: "DK-SONGTI", "方正宋三简体", "方正书宋", "宋体";
	font-size: 16px;
	duokan-text-indent: 2em;
	text-indent: 2em;
}
/* 正文宋体-强制左对齐 */
p.bodycontent-text-left {
	font-family: "DK-SONGTI", "方正宋三简体", "方正书宋", "宋体";
	font-size: 16px;
	text-align: left;
	duokan-text-indent: 2em;
	text-indent: 2em;
}
/* 正文宋体-强制左对齐顶格 */
p.bodycontent-text-left0 {
	font-family: "DK-SONGTI", "方正宋三简体", "方正书宋", "宋体";
	font-size: 16px;
	text-align: left;
	duokan-text-indent: 0em;
	text-indent: 0em;
}
/* 引文黑体标题居中 */
p.reference-title {
	font-family: "DK-HEITI", "方正兰亭黑简体", "黑体";
	font-size: 16px;
	text-align: center;
}
/* 引文楷体 */
p.reference-text {
	font-family: "DK-KAITI", "方正楷体","华文楷体","楷体";
	font-size: 16px;
	text-indent: 2em;
	duokan-text-indent: 2em;
}
/* 引文楷体顶格 */
p.letter-head {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 16px;
	duokan-text-indent: 0em;
	text-indent: 0em;
}
/* 署名楷体居右 */
p.signature {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 16px;
	text-align: right;
}
/* 诗集编号小标宋居中 */
h4.num-tilte {
	font-family: "DK-XIAOBIAOSONG", "方正小标宋简体";
	font-weight: normal;
	font-size: 18px;
	margin-bottom: 5em;
	color: #91531d;
	text-align: center;
	margin-top: 2em;
}
/* 部分斜体 */
span.xieti {
	font-style: italic;
}
/* 部分斜体加粗 */
span.italic-b {
	font-style: italic;
	font-weight: bold;
}
/* 部分加粗 */
span.bold {
	font-weight: bold;
}
/* 部分楷体 */
span.kaiti {
	font-family: "DK-KAITI","方正楷体","华文楷体","楷体"; 
}
/* 部分楷体加粗 */
span.kaiti-jiacu {
	font-family: "DK-KAITI","方正楷体","华文楷体","楷体"; 
	font-weight: bold;
}
/* 部分黑体加粗 */
span.heiti {
	font-family: "DK-DENGXIAN";
	font-weight: bold;
}
/* 楷体自然段 */
p.kaiti-duan {
	font-family: "DK-KAITI", "方正楷体","华文楷体","楷体";
	font-size: 16px;
	text-indent: 2em;
	duokan-text-indent: 2em;
}
/* 楷体加粗自然段 */
p.kaiti-duan-jiacu {
	font-family: "DK-KAITI", "方正楷体","华文楷体","楷体";
	font-size: 16px;
	text-indent: 2em;
	duokan-text-indent: 2em;
	font-weight: bold;
}
/* 黑体加粗自然段 */
p.heiti-duan {
	font-family: "DK-DENGXIAN";
	font-weight: bold;
	font-size: 16px;
	text-indent: 2em;
	duokan-text-indent: 2em;
}
/* 下划线 */
span.xiahuaxian {
	text-decoration: underline;
}
/* 音频 */
audio.yinpin {
	height: 1.2em;
	vertical-align: duokan-middle-line;
}
/* 视频 */
video.shipin {
	width: 100%;
	margin: 1em auto 1em auto;
	text-align: center;
}
/* 1字高图标 */
img.formula-1em {
	height: 1em;
	vertical-align: duokan-middle-line;
}
/* 1.5字高图标 */
img.formula-1-5em {
	height: 1.5em;
	vertical-align: duokan-middle-line;
}
/* 2字高图标 */
img.formula-2em {
	height: 2em;
	vertical-align: duokan-middle-line;
}
/* 2.5字高图标 */
img.formula-2-5em {
	height: 2.5em;
	vertical-align: duokan-middle-line;
}
/* 填充的点着重号 */
span.emphasis-filled-dot {
	text-emphasis: filled dot;
}
/* 上标字体 */
span.math-super {
	font-size: 0.7em;
	vertical-align: super;
}
/* 下标字体 */
span.math-sub {
	font-size: 0.7em;
	vertical-align: sub;
}
/* 上标倾斜字体 */
span.math-super-italic {
	font-size: 0.7em;
	font-style: italic;
	vertical-align: super;
}
/* 下标倾斜字体 */
span.math-sub-italic {
	font-size: 0.7em;
	font-style: italic;
	vertical-align: sub;
}
/* 图片-有图注 */
img.duokan-image2 {
	width: 100%;
	font-size: 16px;
	margin-bottom: 0.5em;
}
/* 图注居中 */
p.duokan-note2 {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 14px;
	text-align: center;
}
/* 图注居右 */
p.duokan-note3 {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 14px;
	text-align: right;
}
/* 扉页章标题居右 */
h2.feiye-zi {
	font-family: "DK-HEITI", "方正兰亭黑简体", "黑体";
	text-align: right;
	font-size: 16px;
	color: #2F4F4F;
	margin-top: 30%;
	padding-right: 0.5em;
	border-right: solid 2px #2F4F4F;
	padding-bottom: 0.3em;
	font-weight: normal;
}
/* 扉页文字居右 */
p.feiye-zi-2 {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	text-align: right;
	font-size: 15px;
	color: #2F4F4F;
	margin-top: 5%;
	padding-bottom: 0.1em;
}
/* 章标题居左带细线 */
h2.bodycontent-title-2 {
	color: #1f4a92;
	font-size: 22px;
	font-family: "DK-XIAOBIAOSONG", "方正小标宋简体";
	font-weight: normal;
	border-bottom: solid 1px #1f4a92;
	padding: 0.2em 0em 0.5em 0em;
	duokan-text-indent: 0em;
	text-indent: 0em;
}
/* 章标题居左带粗线 */
h2.bodycontent-prefix-text-top {
	font-family: "DK-XIAOBIAOSONG", "方正小标宋简体";
	font-size: 22px;
	color: #91531d;
	font-weight: normal;
	margin-top: 2%;
	duokan-text-indent: 0em;
	text-indent: 0em;
	padding-bottom: 1em;
	border-bottom: solid 4px #e8c696;
}
/* 章标题居右 */
h2.preface-title-two {
	font-size: 22px;
	font-family: "DK-XIAOBIAOSONG", "方正小标宋简体";
	text-align: right;
	font-weight: normal;
	color: #cf181a;
	margin-bottom: 3em;
}
/* 章标题居中带虚线 */
h2.chapter-title-center {
	font-family: "DK-XIAOBIAOSONG", "方正小标宋简体";
	font-weight: normal;
	font-size: 22px;
	color: #478686;
	text-align: center;
	border-bottom: dashed 1px #478686;
	margin-bottom: 2em;
	padding: 0.3em 0em 0.3em 0em;
}
/* 二级标题 */
h3.title2 {
	font-family: "DK-HEITI", "方正兰亭黑简体", "黑体";
	font-size: 18px;
	duokan-text-indent: 0em;
	text-indent: 0em;
	font-weight: normal;
	color: #91531d;
	margin-top: 2.2em;
	margin-bottom: 1.4em;
}
/* 三级标题 */
h4.title3 {
	font-family: "DK-HEITI", "方正兰亭黑简体", "黑体";
	font-size: 16px;
	duokan-text-indent: 0em;
	text-indent: 0em;
	font-weight: normal;
	color: #91531d;
	margin-left: 2em;
	margin-top: 2.2em;
	margin-bottom: 1.4em;
}
/* 正文等线体 */
p.bodycontent-text-ds {
	font-family: "DK-DENGXIAN";
	font-size: 16px;
	duokan-text-indent: 2em;
	text-indent: 2em;
}
/* 正文黑体 */
p.bodycontent-text-ht {
	font-family: "DK-HEITI", "方正兰亭黑简体", "黑体";
	font-size: 16px;
	duokan-text-indent: 2em;
	text-indent: 2em;
}
/* 引文 */
p.bodycontent-reference-text {
	font-family: "DK-KAITI", "方正楷体", "华文楷体", "楷体";
	font-size: 16px;
	duokan-text-indent: 2em;
	text-indent: 2em;
}
/* 文字区域直角块紫色填充 */
div.body-quotation-background {
	background-color:#ecf1f8;
	font-size: 16px;
	padding: 0.8em 0.5em 0.8em 0.5em;
	margin-left: 0.5em;
	margin-right:0.5em;
}
/* 文字区域圆角块蓝灰色填充 */
div.body-quotation-background2 {
	background-color:#D6E0E8;
	font-size: 16px;
	padding: 0.8em 0.5em 0.8em 0.5em;
	margin-left: 0.5em;
	margin-right:0.5em;
	border-radius: 0.5em 0.5em 0.5em 0.5em;
}
/* 文字区域圆角块灰绿色填充 */
div.body-quotation-background3 {
	background-color: #E2EDE3;
	font-size: 16px;
	padding: 0.8em 0.5em 0.8em 0.5em;
	margin-left: 0.5em;
	margin-right:0.5em;
	border-radius: 0.5em 0.5em 0.5em 0.5em;
}
```

