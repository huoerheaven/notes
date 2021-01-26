#### 一、是什么？
> **Sass**是成熟、稳定、强大的**CSS预处理器**，而SCSS是Sass3版本当中引入的新语法特性，完全兼容CSS3的同时继承了Sass强大的动态功能。
#### 二、特性概览
> CSS书写代码规模较大的Web应用时，容易造成选择器、层叠的复杂度过高，因此推荐通过SASS预处理器进行CSS的开发，SASS提供的变量、嵌套、混合、继承等特性，让CSS的书写更加有趣与程式化。
###### 1. 变量
> 变量用来存储需要在CSS中复用的信息，例如颜色和字体。SASS通过$符号去声明一个变量。
~~~
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
~~~
