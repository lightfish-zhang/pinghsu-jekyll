---

layout: post
title: Valine——为静态网页添加评论组件
date: 2020-11-05 23:50:00 +0800
category: Web
thumbnail: /ning_file/thumbnail/mhmd-sedky-4u_NwZCnwuY-unsplash.jpg
icon: web
summary: 在测试了Gittalk、Gitment等多款产品后，意外发现的惊喜！
tag: [valine,web,github.io]
---



* content
{:toc}
# 1.准备：注册，获取APP ID和APP KEY

请先 <a href='https://leancloud.cn/dashboard/login.html#/signin' target='_blank'>登录</a>或<a href='https://leancloud.cn/dashboard/login.html#/signup' target='_blank'>注册</a>`LeanCloud`, 进入<a href='https://leancloud.cn/dashboard/applist.html#/apps' target='_blank'>控制台</a>后点击左下角<a href='https://leancloud.cn/dashboard/applist.html#/newapp' target='_blank'>创建应用</a>：

![image.png](https://i.loli.net/2019/06/21/5d0c995c86fac81746.jpg)

应用创建好以后，进入刚刚创建的应用，选择左下角的`设置`>`应用Key`，然后就能看到你的`APP ID`和`APP Key`了：

![img](https://i.loli.net/2019/06/21/5d0c997a60baa24436.jpg)



#  2.复制HTML 片段到需要的地方

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



#  3.评论数据管理

由于Valine 是无后端评论系统，所以也就没有开发评论数据管理功能。请自行登录`Leancloud应用`管理。

具体步骤：`登录`>`选择你创建的应用`>`存储`>选择Class `Comment`，然后就可以尽情的发挥你的权利啦(～￣▽￣)～



# 4. 安全域名

为了你的数据安全，请设置自己的`安全域名`：

![设置安全域名](https://i.loli.net/2019/06/21/5d0c995bddd4f99219.jpg)



# 5.头像配置

Valine 目前使用的是<a href='http://cn.gravatar.com/'>Gravatar</a>作为评论列表头像。

请自行登录或注册<a href='http://cn.gravatar.com/'>Gravatar</a>，然后修改自己的头像。

评论的时候，留下在<a href='http://cn.gravatar.com/'>Gravatar</a>注册时所使用的邮箱即可。



---

> 参考资料：
>
> 1.<a href='https://blog.csdn.net/weixin_30708329/article/details/96852440'>Jekyll博客添加Valine评论</a>
>
> 2.<a href='https://valine.js.org/'>Valine官网文档</a>
>
> 3.<a href='https://lovelijunyi.gitee.io/posts/e52c.html'>Valine评论系统详解</a>