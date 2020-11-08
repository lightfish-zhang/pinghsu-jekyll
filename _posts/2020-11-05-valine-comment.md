---

layout: post
title: Valine——为静态网页添加评论组件
date: 2020-11-05 23:50:00 +0800
category: Web
thumbnail: https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/mhmd-sedky-4u_NwZCnwuY-unsplash.jpg
icon: lock
summary: 在测试了Gittalk、Gitment等多款产品后，意外发现的惊喜！
tag: [valine,web,github.io,javascript]
---



* content
{:toc}

# 基础配置

## 1.准备：注册，获取APP ID和APP KEY

请先 <a href='https://leancloud.cn/dashboard/login.html#/signin' target='_blank'>登录</a>或<a href='https://leancloud.cn/dashboard/login.html#/signup' target='_blank'>注册</a>`LeanCloud`, 进入<a href='https://leancloud.cn/dashboard/applist.html#/apps' target='_blank'>控制台</a>后点击左下角<a href='https://leancloud.cn/dashboard/applist.html#/newapp' target='_blank'>创建应用</a>：

![image.png](https://i.loli.net/2019/06/21/5d0c995c86fac81746.jpg)

应用创建好以后，进入刚刚创建的应用，选择左下角的`设置`>`应用Key`，然后就能看到你的`APP ID`和`APP Key`了：

![img](https://i.loli.net/2019/06/21/5d0c997a60baa24436.jpg)



##  2.复制HTML 片段到需要的地方

修改初始化对象中的`appId`和`appKey`的值为上面刚刚获取到的值即可(其他可以默认)。

```html
<head>
    ..
    <script src='//unpkg.com/valine/dist/Valine.min.js'></script>
    ...
</head>
<body>
    ...
    <div id="vcomments"></div>
    <script>
        new Valine({
            el: '#vcomments',
            appId: 'Your appId',
            appKey: 'Your appKey'
        })
    </script>
</body>
```



##  3.评论数据管理

由于Valine 是无后端评论系统，所以也就没有开发评论数据管理功能。请自行登录`Leancloud应用`管理。

具体步骤：`登录`>`选择你创建的应用`>`存储`>选择Class `Comment`，然后就可以尽情的发挥你的权利啦(～￣▽￣)～



## 4. 安全域名

为了你的数据安全，请设置自己的`安全域名`：

![设置安全域名](https://i.loli.net/2019/06/21/5d0c995bddd4f99219.jpg)



## 5.头像配置

Valine 目前使用的是<a href='http://cn.gravatar.com/' target="_blank">Gravatar</a>作为评论列表头像。

请自行登录或注册<a href='http://cn.gravatar.com/' target="_blank">Gravatar</a>，然后修改自己的头像。

评论的时候，留下在<a href='http://cn.gravatar.com/' target="_blank">Gravatar</a>注册时所使用的邮箱即可。

<br>
# 扩展功能

## 1.QQ头像

🔗<b style="font-size:20px;weight:bold;"> enableQQ</b>
- 类型: `Boolean`
- 默认值: `false`
- 必要性: `false`

是否启用`昵称框`自动获取`QQ昵称`和`QQ头像`, 默认关闭，需`博/网站主`主动启用

> ```
> v1.4.6+
> ```

![](https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/20201107214442.png)

![](https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/20201107214639.png)

> 成功显示
>

![](https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/20201107214837.png)

## 2.设置昵称或邮箱必填

🔗<b style="font-size:20px;weight:bold;"> requiredFields</b>

- 类型: `Array`
- 默认值: `[]`
- 必要性: `false`

设置`必填项`，默认`匿名`，可选值：

- `['nick']`
- `['nick','mail']`

> ```
> v1.4.6+
> ```

> 需要注意，设置nick后昵称需三个字节以上



## 3.修改评论输入框提示

通过查看网页元素，可以发现三个样式分别为`vnick vinput`、`vmail vinput`、`vlink vinput`的Input框和一个`veditor vinput`的textarea框，且均无id。



于是我们便可以通过class名称来定位到这些内容，然后修改其中的内容。

```js
//修改comment的昵称说明

function update_nick() {
    var inputs = document.getElementsByClassName("vnick vinput");//通常获取的是表单标签name
    document.getElementsByClassName("vnick vinput")[0].setAttribute('placeholder', '昵称/QQ（qq可以自动获取昵称头像！！！）');
    document.getElementsByClassName("vmail vinput")[0].setAttribute('placeholder', '邮箱（可不填）');
    document.getElementsByClassName("vlink vinput")[0].setAttribute('placeholder', '个人主页（可不填）');
    document.getElementsByClassName("veditor vinput")[0].setAttribute('placeholder', '来都来了，给我留个言吧[○･｀Д´･ ○]');
    //console.log(inputs);
}
setInterval(update_nick, 1000);
```

这就是修改前后的样式了：

![](https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/20201107214226.png)

![](https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/20201107214016.png)

<br>
<br>
<br>

---

> 参考资料：
>
> 1.<a href='https://blog.csdn.net/weixin_30708329/article/details/96852440' target="_blank">Jekyll博客添加Valine评论</a>
>
> 2.<a href='https://valine.js.org/' target="_blank">Valine官网文档</a>
>
> 3.<a href='https://lovelijunyi.gitee.io/posts/e52c.html' target="_blank">Valine评论系统详解</a>