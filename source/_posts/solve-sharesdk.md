---
title: Hexo 集成分享功能
date: 2018-05-03 20:50:40
tags: Web
---

今天打算扩充一下自己博客，给它加一个分享的功能。在 NexT 主题中默认集成了 baidushare，只要在主题配置文件 _config.yml 中打开 baidushare 相应的开关即可，但是遇到了 HTTPS 请求 HTTP 资源的问题。在各种资料的冲击下，我决定试试 ShareSDK。这是 ShareSDK 的官方文档连接：[sharesdk-for-web快速集成](http://wiki.mob.com/sharesdk-for-web%E5%BF%AB%E9%80%9F%E9%9B%86%E6%88%90/)

1. 创建文件

在 next/layout/_partials/share 文件夹下新建一个 sharesdk.swig 文件，上文说的 baidushare.swig 就在该文件夹下。然后将官网的示例代码块复制粘贴进去，然后将 “你的appkey” 改成 {{ theme.shareSDKappkey }}。最终代码如下所示：

```
<!--MOB SHARE BEGIN-->
<div class="-mob-share-ui-button -mob-share-open">分享</div>
<div class="-mob-share-ui" style="display: none">
    <ul class="-mob-share-list">
        <li class="-mob-share-weibo"><p>新浪微博</p></li>
        <li class="-mob-share-qzone"><p>QQ空间</p></li>
        <li class="-mob-share-qq"><p>QQ好友</p></li>
        <li class="-mob-share-douban"><p>豆瓣</p></li>
        <li class="-mob-share-facebook"><p>Facebook</p></li>
        <li class="-mob-share-twitter"><p>Twitter</p></li>
    </ul>
    <div class="-mob-share-close">取消</div>
</div>
<div class="-mob-share-ui-bg"></div>
<script id="-mob-share" src="http://f1.webshare.mob.com/code/mob-share.js?appkey={{ theme.shareSDKappkey }}"></script>
<!--MOB SHARE END-->
```

2. 添加可选配置

在 next/layout/post.swig 文件中添加如下代码：

```
{% elseif theme.sharesdk %}
        {% include '_partials/share/sharesdk.swig' %}
```

整体代码如下所示：

```
<div class="post-spread">
      {% if theme.jiathis %}
        {% include '_partials/share/jiathis.swig' %}
      {% elseif theme.baidushare %}
        {% include '_partials/share/baidushare.swig' %}
      {% elseif theme.add_this_id %}
        {% include '_partials/share/add-this.swig' %}
      {% elseif theme.duoshuo_shortname and theme.duoshuo_share %}
        {% include '_partials/share/duoshuo_share.swig' %}
      {% elseif theme.sharesdk %}
        {% include '_partials/share/sharesdk.swig' %}
      {% endif %}
</div>
```

3. 修改 _config.yml

在 next/_config.yml 文件中添加如下配置，appkey 填写成自己注册获取的 appkey 即可：

```
sharesdk: true
shareSDKappkey: appkey
```

4. 注意

这样配置下来部署到 GitHub 上访问的时候可以在 Console 看到 HTTP 请求的错误，网上提供了很多解决方案，比如将请求的 js 文件直接保存到本地以相对路径的方式引用 [Hexo+Github搭建个人博客(三)——百度分享集成](https://www.jianshu.com/p/276d10de413e)。但是都不太靠谱，而且操作也很麻烦。其实只需要将 http 改成 https 即可，同样也是可以请求到 js 文件的：

```
<script id="-mob-share" src="https://f1.webshare.mob.com/code/mob-share.js?appkey={{ theme.shareSDKappkey }}"></script>
```

