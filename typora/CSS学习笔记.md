# CSS学习笔记

## position定位

1.  static默认值
2.  relative
3.  absolute

### absolut（最常用）

1.  会脱离文档流，提升层级
2.  该元素会变成块元素，其宽高被内容撑开
3.  相对于其包含块定位
4.  等式：left+margin left/right+border left/right+width+right=包含块的内容宽度
5.  垂直方向也有这样一个等式，继而可以做到在包含块内的垂直居中
6.  等式里如果是auto的自动调整优先级：width/height>left/right/margin

包含快是：根元素或者开启了定位（非static）的最近的祖先块元素。

### fixed

1.  类似于绝对定位
2.  相对于浏览器视口（不是根元素或者祖先块元素）
3.  不随着页面滚动而移动，像小广告

sticky

1.  很新的属性，chrome56才开始支持
2.  类似相对定位
3.  可以在元素到达某个位置的时候固定，例如会留在窗口顶部的导航条

## 层级

对开启了定位的元素，可以通过z-index属性设置一个整数设置层级，值越大越高。默认情况下html中最后的元素更高级。

子元素会继承后代元素的层级。

## 字体

相关属性

1.  color
2.  font-size
3.  em一个当前元素的文字字符的宽度
4.  rem一个html根元素的文字字符的宽度
5.  font-family

字体样式

1.  serif衬线字体
2.  san-serif非衬线字体
3.  monospace等宽字体

font-family属性可以设置多个字体或样式，使用逗号隔开，优先使用靠前的。

### 网络字体

```css
@font-face{
font-family:my-font;
src:url('http://www.exmaple.com/mydont.ttf') format("truetype");
}
```

加载网络字体并可以使用通过名字使用该字体。其中的format可以不写明。

### 图标字体

使用font awesome图标字体库。下载并解压。

1.  将css和webfont文件夹加入到项目里，必须保持这两个文件夹是同一级目录。
2.  引入all.css
3.  使用类名使用图标字体
4.  例如class=“fas fa-bell”，也可能是“发布fa-bell”
5.  也可以使用字符编码使用

### 行高

行高会在字符上下平均分配，所以使用行高和元素高度一样高可以做到上下居中。行间距使用行高确定。

### 简写

直接使用font属性可以设置各种font相关的属性，简写。

```css
/*
font:斜体 加粗 字号（必须）/行高倍数 字体名（必须）
*/
font:16px/24 'myfont'
```