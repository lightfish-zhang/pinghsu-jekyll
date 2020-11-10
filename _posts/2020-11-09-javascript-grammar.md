---
layout: post
title: JavaScript语法归纳
date: 2020-11-10 09:24:00 +0800
category: Code
thumbnail: https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/kobu-agency-ipARHaxETRk-unsplash.jpg
icon: web
summary: 在这里记录JavaScript自己常用的语法
tag: [JavaScript,语法]

---

* content
{:toc}


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

```
<a href="javascript:function_name()"></a>
```

例：

```javascript
<a href="javascript:do_del(<?php echo $row['id']; ?>)">删除</a>
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

