---
layout: post
title: 记录：网页美化Tips
date: 2020-11-13 19:37:00 +0800
updatetime: 2020/11/17 下午 09:51:15
category: Web
thumbnail: https://ning-blog-1304206373.cos.ap-nanjing.myqcloud.com/image/thumbnail/tim-mossholder-GOMhuCj-O9w-unsplash.jpg
icon: design
summary: 从各处扒来的东西，乱七八糟
tag: [web,HTML,css]
---

* content
{:toc}
### note标签样式

需要引入 `FontAwesome v4.7.0` 版本的 CSS 文件才会使图标生效。引入链接如下：

```html
<link type='text/css' rel="stylesheet" href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" media='all'/>
```

```css
/* note 公共样式 */
.note {
    position: relative;
    padding: 15px;
    margin-top: 10px;
    margin-bottom: 10px;
    border: initial;
    border-left: 3px solid #eee;
    background-color: #f9f9f9;
	border-radius: 3px;
	font-size: var(--content-font-size);
}

.note:not(.no-icon):before {
    position: absolute;
    font-family: FontAwesome;
    font-size: larger;
    top: 11px;
    left: 15px;
}

.note:not(.no-icon) {
    padding-left: 45px;
}

.note.info {
    background-color: #eef7fa;
	border-left-color: #428bca;
}

.note.info:not(.no-icon):before {
    content: "\f05a";
    color: #428bca;
}

.note.warning {
    background-color: #fdf8ea;
    border-left-color: #f0ad4e;
}

.note.warning:not(.no-icon):before {
    content: "\f06a";
    color: #f0ad4e;
}

.note.primary {
    background-color: #f5f0fa;
    border-left-color: #6f42c1;
}

.note.primary:not(.no-icon):before {
    content: "\f055";
    color: #6f42c1;
}

.note.danger {
    background-color: #fcf1f2;
    border-left-color: #d9534f;
}

.note.danger:not(.no-icon):before {
    content: "\f056";
    color: #d9534f;
}
```

在写 md 时使用方式如下，以 html 标签方式引入：

```html
<div class="note info">这里是 info 标签样式</div>

<div class="note info no-icon">这里是不带符号的 info 标签样式</div>

<div class="note primary">这里是 primary 标签样式</div>

<div class="note primary no-icon">这里是不带符号的 primary 标签样式</div>

<div class="note warning">这里是 warning 标签样式</div>

<div class="note warning no-icon">这里是不带符号的 warning 标签样式</div>

<div class="note danger">这里是 danger 标签样式</div>

<div class="note danger no-icon">这里是不带符号的 danger 标签样式</div>
```

<div class="note info">这里是 info 标签样式</div>

<div class="note info no-icon">这里是不带符号的 info 标签样式</div>

<div class="note primary">这里是 primary 标签样式</div>

<div class="note primary no-icon">这里是不带符号的 primary 标签样式</div>

<div class="note warning">这里是 warning 标签样式</div>

<div class="note warning no-icon">这里是不带符号的 warning 标签样式</div>

<div class="note danger">这里是 danger 标签样式</div>

<div class="note danger no-icon">这里是不带符号的 danger 标签样式</div>

<br>

### B站视频自适应屏幕大小

```html
<div style="position: relative; padding: 30% 45%;">
    <iframe style="position: absolute; width: 100%; height: 100%; left: 0; top: 0;"
        src="//player.bilibili.com/player.html?aid=***&bvid=***&cid=***&page=*&as_wide=*&high_quality=*&danmaku=*"
        scrolling="no" border="0" frameborder="no" framespacing="0"                               allowfullscreen="true">
    </iframe>
</div>
```

参数说明：

| key          | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| aid          | 之前 B 站使用的 AV 号                                        |
| bvid         | 目前的 BV 号                                                 |
| page         | 第几个视频, 起始下标为 1 (默认值也是为 1)就是 B 站视频, 选集里的, 第几个视频 |
| as_wide      | 是否宽屏 【1: 宽屏, 0: 小屏】                                |
| high_quality | 是否高清 【1: 高清(最高1080p) / 0: 最低视频质量(默认)】      |
| danmaku      | 是否开启弹幕 【1: 开启(默认), 0: 关闭】                      |

<div style="position: relative; padding: 30% 45%;">
    <iframe style="position: absolute; width: 100%; height: 100%; left: 0; top: 0;"
        src="//player.bilibili.com/player.html?bvid=BV1wJ411o7qD&as_wide=1&danmaku=1"
        scrolling="no" border="0" frameborder="no" framespacing="0"                               allowfullscreen="true">
    </iframe>
</div>


### 图片懒加载

图片懒加载可以在较长的页面中延迟加载图片，直到它们由于用户滚动而出现到视窗中。

傻瓜式的[懒加载插件](https://github.com/tuupola/lazyload)，它的名字就叫 lazyload，在博客需要使用图片懒加载的页面引入如下 js 文件：

```html
//CDN的lazyload插件
<script src="https://cdn.jsdelivr.net/npm/lazyload@2.0.0-rc.2/lazyload.js"></script>
```

然后在对应的图片 `<img>` 标签内做出如下修改，就是把图片真实的 `src` 路径放入 `data-src` 中，并给 `img` 标签增加 `class` 为 `lazyload` 的属性， `src` 处不填时，默认初始加载时填充一张灰色的背景图，填写 `src` 时默认就加载你给定的图片，然后加载完成后会用 `data-src` 中的值替换 `src` 中的值实现懒加载的效果。

```html
<img class="lazyload" data-src="img/example.jpg" width="765" height="574">
<img class="lazyload" src="img/example-thumb.jpg" data-src="img/example.jpg" width="765" height="574">
```

最后，在页面靠后位置的 `<script>` 中还要加载一下这个插件，方式如下：

```js
lazyload();
```

<br>

<br>

---

> 参考资料：
>
> 1.<a href='https://bestzuo.cn/posts/halo-beauty.html' target="_blank">halo 博客深度定制与美化教程</a>
>
> 2.<a href='https://bestzuo.cn/posts/pagespeed.html' target="_blank">使用 PageSpeed Insights 给博客提速</a>