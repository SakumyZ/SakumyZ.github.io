---
layout: post
tile: JavaScript排他
date: 2018-10-14
description: 排他样式设计
tag: 博客
---

### 所谓排他

我们在设计网页的时候，经常会使用到排他的功能，所谓的排他简单的来说就是指排除其他样式的干扰，显示当前的样式，通俗点来说就是，我点击一个东西，现在只应该是这个东西高亮，其他都不应该被高亮，这就是一个排他。



### 如何排他

明白了排他是什么了之后，实现其实很简单，主要是分为`两步`



+ 去除全部样式

+ 设定当前样式

我们假定我们需要设定的样式为`current`,则我们首先要先去掉一系列的样式

```javascript
for(var i = 0; i < object.length; i++) {
    //添加点击事件
    object[i].addEventListener("click", function() {
    //清除所有样式
        for(var j = 0; j < object.length; j++) {
            object[i].removeAttribute("class");
        }
	//设置当前样式为current
	this.className = "current";
},false);
```

这就是最简单的排他，该例子完成的是我点击某个对象，某个对象就高亮。