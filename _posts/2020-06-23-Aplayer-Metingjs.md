---
layout: post
title: 利用基于Aplayer插件封装好的Metingjs，为博客首页添加音乐播放器
date: 2020-06-23 22:40:10 +0800
category: Web
thumbnail: https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/daniel-schludi-l8cvrt3Hpec-unsplash.jpg
icon: design
summary: 这个音乐插件在跳转网页后就会刷新，所以我只放在了首页，并且将首页都设置为在新标签页中打开。后续我会尝试解决这个问题，使得其可以在跳转页面后连续播放。
tag: [服务器,网站,音乐播放器,QQ音乐]
---

* content
{:toc}

# 1.前言

个人博客使用Halo搭建：https://halo.run/

Aplayer官网文档：https://aplayer.js.org/#/zh-Hans/

Metingjs官网文档：https://github.com/metowolf/MetingJS

# 2.配置（简单示例）
## 1.在header中插入以下内容
```html
<!--音乐播放器-->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.css">
<script src="https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.js"></script>
```
![image.png](http://qiening.top/upload/2020/06/image-aad57a004b4944a1942e4bf01f662144.png)

## 2.在footer中插入以下内容
```html
<!--音乐播放器-->
<script src="https://cdn.jsdelivr.net/npm/meting@2.0.1/dist/Meting.min.js"></script>
```
![image.png](http://qiening.top/upload/2020/06/image-c944d87e86444320a175c652e669fbb4.png)

## 3.在想要展现音乐插件的位置插入以下内容,这里就单放在了首页。

![image.png](http://qiening.top/upload/2020/06/image-f004538a0d694e6d9a303102d887af8c.png)

```html
<!--首页音乐播放器-->
<meting-js server="tencent" type="playlist" id="3441886932" fixed="true" autoplay="true" order="random"></meting-js>
```
效果如下（可点击预览）：[宁的小栈](https://ning-qie.github.io/)
![image.png](http://qiening.top/upload/2020/06/image-4de40ebc0d8a4d0cb864c4fd176fb4aa.png)

# 3.用法说明

## 1.最基本的语法介绍
```html
<meting-js server="tencent" type="playlist" id="3441886932"></meting-js>
```

>·server指音乐平台,支持平台：netease, tencent, kugou, xiami, baidu
>
>·type类型，支持类型：song, playlist, album, search, artist
>
>·id指歌曲或专辑或列外链id，支持：song id / playlist id / album id / search keyword
>
>·可参考Metingjs文档：https://github.com/metowolf/MetingJS#option

### 效果
<meting-js server="tencent" type="playlist" id="3441886932"></meting-js>

## 2.其他参数

|名称   |默认值  |描述   |
|-----|-----|-----|
|container|document.querySelector('.aplayer')|播放器容器元素|
|fixed	|false	|开启吸底模式, <a href="https://aplayer.js.org/#/home?id=fixed-mode" target="_blank"><font color=blue>详情</font></a>|
|mini	|false	|开启迷你模式, <a href="https://aplayer.js.org/#/home?id=mini-mode" target="_blank"><font color=blue>详情</font></a>|
|autoplay	|false	|音频自动播放|
|theme	|'#b7daff'	|主题色|
|loop	|'all'	|音频循环播放, 可选值: 'all', 'one', 'none'|
|order	|'list'	|音频循环顺序, 可选值: 'list', 'random'|
|preload	|'auto' |预加载，可选值: 'none', 'metadata', 'auto'|
|volume	|0.7	|默认音量，请注意播放器会记忆用户设置，<br>用户手动设置音量后默认音量即失效|
|audio	|-	|音频信息, 应该是一个对象或对象数组|
|audio.name	|-	|音频名称|
|audio.artist	|-	|音频艺术家|
|audio.url	|-	|音频链接|
|audio.cover	|-	|音频封面|
|audio.lrc	|-	|<a href="https://aplayer.js.org/#/home?id=lrc" target="_blank"><font color=blue>详情</font></a>|
|audio.theme	|-	|切换到此音频时的主题色，<br>比上面的 theme 优先级高|
|audio.type	|'auto'	|可选值: 'auto', 'hls', 'normal'<br> 或其他自定义类型, <a href="https://aplayer.js.org/#/home?id=mse-support" target="_blank"><font color=blue>详情</font></a>|
|customAudioType	|-	|自定义类型，<a href="https://aplayer.js.org/#/home?id=mse-support" target="_blank"><font color=blue>详情</font></a>|
|mutex	|true	|互斥，阻止多个播放器同时播放，<br>当前播放器播放时暂停其他播放器|
|lrcType	|0	|<a href="https://aplayer.js.org/#/home?id=lrc" target="_blank"><font color=blue>详情</font></a>|
|listFolded	|false	|列表默认折叠|
|listMaxHeight	|-	|列表最大高度|
|storageName	|'aplayer-setting'	|存储播放器设置的 localStorage key|

## 3.常用参数示例

#### 1.mini="true"
迷你播放器
```html
<meting-js server="tencent" type="playlist" id="3441886932" mini="true"></meting-js>
```

<meting-js server="tencent" type="playlist" id="3441886932" mini="true"></meting-js>

#### 2.list-folded="true"
列表是否应该首先折叠
```html
<meting-js server="tencent" type="playlist" id="3441886932" list-folded="true"></meting-js>
```

<meting-js server="tencent" type="playlist" id="3441886932" list-folded="true"></meting-js>

#### 3.fixed="true"
开启吸底模式，见网页左下角
```html
<meting-js server="tencent" type="playlist" id="3441886932" fixed="true"></meting-js>
```

<meting-js server="tencent" type="playlist" id="3441886932" fixed="true"></meting-js>

# 4.歌曲相关信息获取

以我的QQ音乐❤“我喜欢”歌单为例

![image.png](http://qiening.top/upload/2020/06/image-c99c83b4ff8645b39f2bc02979773a60.png)

点击进入后，查看其url，得到<font color=#20B2AA>"playlist" id="3441886932"</font>
![image.png](http://qiening.top/upload/2020/06/image-8c449ec34fdb4c4f9e8f11fbbbf1ac28.png)

故
>server="tencent" 
>type="playlist" 
>id="3441886932"

# 其他

这个音乐插件在跳转网页后就会刷新，所以我只放在了首页，并且将首页都设置为在新标签页中打开。

后续我会尝试解决这个问题，使得其可以在跳转页面后连续播放。