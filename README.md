 # Css学习笔记

## 精通Css
### 基础知识
-  除非确定元素是页面上唯一的存在才使用id为元素命名，否则使用通用的class命名。
-  类不应该被滥用，应该视情况而定，除非特殊情况，应该使用**层叠**标识某些元素。如果发现添加了许多类，那么可能意味着HTML文档结构有问题。
- div元素并非没有语义，实际代表着**部分**(division)，可以将文档分割为几个有意义的区域，为了将不必要的标记减到最少，应该只有在没有现有元素能够实现区域分割的情况下使用div元素，例如以下代码是非必要的。

``` html
<div class="nav">
 <ul>
    <li>Home</li>
    <li>Contact</li>
  </ul>
</div>
```

-  DOCTYPE声明用于浏览器解析DTD(文档类型定义）、HTML文档有效性验证以及浏览器模式的设定(标准模式和混杂模式)等。

### 为样式找到应用目标

- :link和:visited称为链接伪类，:hover、:active和:focus称为动态伪类，理论上可以应用于任何元素。可以试试:visited:hover的效果。
- 后代选择器选择一个元素的所有后代，而子选择器值选择元素的直接后代。
- 层叠和特殊性可以参考《css权威指南》。
- 特殊性中需要说明的是如果css规则没有起到作用，那很有可能是出现了特殊性冲突，可以在选择器中添加一个它的父元素id(如果页面中的所有样式都是以class进行设计，那么要增加特殊性其实只要添加id就能做到，当然也并不是滥用id，比id更高的就是内联样式和!important了，但是这里最好也别滥用!important)。  
- 使用@import导入样式和link的链接样式的区别：兼容性问题(链接样式无兼容性问题)，导入样式的浏览器下载时间比链接样式慢等。当然如果要在css样式表中引入外部的样式表，就需要使用@import了。

#### 设计css代码的结构

 一般性样式
 - 主体样式（body标记的应该由站点上所有元素继承的样式）
 - reset样式
 - 链接
 - 标题
 - 其他元素
 
辅助样式
 - 表单
 - 通知和错误
 - 一致的条目
 
页面结构
 - 标题、页脚和导航
 - 布局
 - 其他页面结构元素
 
页面组件 
 - 各个页面
 
覆盖

> tip: 其实可见这样的代码结构从上至下处理的样式越来越特殊，这样有助于维护样式表，而防止非预期的特殊性覆盖问题。

建立统一风格的注释则有助于快速寻找特性样式以及规划css代码

``` css
/* @group general styles
---------------------------------------------*/

/* @group helper styles
---------------------------------------------*/

/* @group page structure
---------------------------------------------*/

/* @group page components
---------------------------------------------*/

/* @group overrides
---------------------------------------------*/
```

除了@group这种结构注释，也可以增加自我提示注释，例如

``` css
/* @colordef #434343; dark gray */
/* @todo 以后需要进行修改、修复或复查 */
/* @bugfix 特定浏览器遇到的问题等 */
/* @workaround 并不完善的权宜之计 */
```

>tip: 这些关键字称为gotcha，都是[CSSDoc项目](http://cssdoc.net) 的一部分，目的是为样式表开发一套标准的注释语法。  
