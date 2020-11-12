---
layout: post
title: JavaScript语法归纳
date: 2020-11-10 09:24:00 +0800
category: Code
thumbnail: https://ning-blog-1304206373.cos.ap-nanjing.myqcloud.com/image/thumbnail/kobu-agency-ipARHaxETRk-unsplash.jpg
icon: web
summary: 在这里记录JavaScript自己常用的语法
tag: [JavaScript,语法]

---

* content
{:toc}
# 基础

#### 控制台打印信息

```JavaScript
console.log("Hello World!");
```



#### 函数

```javascript
function f_name(eml2,eml2){
    console.log("Hello World!");
}
```



#### 注释

```javascript
//注释掉一行
console.log("Hello World!");

/*注释注释符号之间内容*/
/*
console.log("Hello World!");
console.log("Hello World!");
console.log("Hello World!");
*/
```



#### if语句

```
if (condition1)
{
    当条件 1 为 true 时执行的代码
}
else if (condition2)
{
    当条件 2 为 true 时执行的代码
}
else
{
  当条件 1 和 条件 2 都不为 true 时执行的代码
}

```



#### 比较运算符

比较运算符在逻辑语句中使用，以测定变量或值是否相等。

| 运算符 | 描述                                               | 比较    | 返回值  |
| :----- | :------------------------------------------------- | :------ | :------ |
| ==     | 等于                                               | x==8    | *false* |
| ===    | 绝对等于（值和类型均相等）                         | x==="5" | *false* |
| !=     | 不等于                                             | x!=8    | *true*  |
| !==    | 不绝对等于（值和类型有一个不相等，或两个都不相等） | x!=="5" | *true*  |
| >      | 大于                                               | x>8     | *false* |
| <      | 小于                                               | x<8     | *true*  |
| >=     | 大于或等于                                         | x>=8    | *false* |
| <=     | 小于或等于                                         | x<=8    | *true*  |



#### for循环

```
for (语句 1; 语句 2; 语句 3)
{
    被执行的代码块
}
```

> 语句 1 （代码块）开始前执行
>
> 语句 2 定义运行循环（代码块）的条件
>
> 语句 3 在循环（代码块）已被执行之后执行

<br>

<br>



# 常用

#### 函数定时发生器

**①setInterval()**

```javascript
setInterval(function(){ 
    alert("Hello"); 
}, 3000);
```

`setInterval() `方法可按照指定的周期（以毫秒计）来调用函数或计算表达式，案例是每三秒执行一次。

>setInterval() 方法会不停地调用函数，直到 `clearInterval()`被调用或窗口被关闭。由 setInterval() 返回的 ID 值可用作 clearInterval() 方法的参数。
>
>**提示：** 1000 毫秒= 1 秒。
>
>**提示：** 如果你只想执行一次可以使用 `setTimeout()`方法。



**②setTimeout()**

```javascript
setTimeout(function(){ 
    alert("Hello"); 
}, 3000);
```

`setTimeout() `方法用于在指定的毫秒数后调用函数或计算表达式，案例是三秒后执行一次。



#### 提示，确认信息

```
window.confirm()
```

例：

```javascript
        function do_del(id) {
            if (window.confirm('确定要删除此条信息吗？')) {
                window.location.href = "message_del.php?id=" + id;
            }
        }
```



#### html内引用JavaScript函数

```html
<a href="javascript:function_name()"></a>
```

例：

```html
<a href="javascript:do_del(info)">删除</a>
```



#### 监听事件函数

```javascript
element.addEventListener(event, function, useCapture)
```

| 参数         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| *event*      | 必须。字符串，指定事件名。  **注意:** 不要使用 "on" 前缀。 例如，使用 "click" ,而不是使用 "onclick"。  **提示：** 所有 HTML DOM 事件，可以查看我们完整的<a href="https://www.runoob.com/jsref/dom-obj-event.html">HTML DOM Event 对象参考手册</a> |
| *function*   | 必须。指定要事件触发时执行的函数。  当事件对象会作为第一个参数传入函数。 事件对象的类型取决于特定的事件。例如， "click" 事件属于 MouseEvent(鼠标事件) 对象。 |
| *useCapture* | 可选。布尔值，指定事件是否在捕获或冒泡阶段执行。  可能值:true - 事件句柄在捕获阶段执行false- false- 默认。事件句柄在冒泡阶段执行 |

例：

```javascript
document.getElementById("myBtn").addEventListener("click", function(){
    document.getElementById("demo").innerHTML = "Hello World";
});
```

