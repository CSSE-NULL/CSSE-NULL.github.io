---
title: 基于Hexo和Github Pages搭建个人博客
date: 2018-10-12 22:49:32
tags: hexo
---

## 准备工作
[安装Git](http://git-scm.com/download/)
[安装Node.js](http://www.runoob.com/nodejs/nodejs-install-setup.html)

## Hexo安装及使用

### 安装并初始化博客

```js
npm install hexo-cli -g
hexo init blog
cd blog
npm install
```

### 本地测试

生成文件并启动本地预览：

```js
hexo g # 或者hexo generate
hexo s # 或者hexo server
```

此时，可以打开浏览器输入`http://localhost:4000/` 查看。

## 部署Hexo到Github Pages

### 准备工作

[注册Github账号](https://github.com/)
[通过ssh关联Github](https://help.github.com/articles/connecting-to-github-with-ssh/)

### 关联Github仓库

- 创建Github Pages仓库

创建名为 **你的用户名.github.io** 的仓库，注意，这里名称是使用Github Pages的必要条件，不能随意更改。

- 修改博客配置文件

打开博客主目录下的`_config.xml`文件，修改如下：

```js
deploy:
  type: git
  repo: git@github.com:用户名/用户名.github.io.git
  branch: master
```

注意：冒号后一定要是**英文半角空格**，不能遗漏或写成中文空格。

- 安装配置扩展

```js
npm install hexo-deployer-git --save
```

- 进行部署

```js
hexo d
```

这一步会将`hexo g`生成的静态页面文件提交到git仓库，然后通过Github Pages就能展示出来了。至此，你的博客已经是可以正常使用了。

## 写文章并进行部署

- 新建页面

利用前面提到的`hexo new page xxx`命令，就可以生成新页面了，这个页面指的是标题栏上的分类页面，比如关于我，Wiki等。

- 新建文章

利用`hexo new xxx`生成新文章

- 生成静态文件并部署

你写的文章需要经过hexo转化成html,css,js等静态文件，才能在页面上有效地展示，所以需要先执行`hexo g`，然后执行`hexo d`进行部署。当然也可以用前面提到的组合命令`hexo g -d`.

## 高级配置

### 更换主题

Hexo主题很多，官方网址为：https://hexo.io/themes/, 看到喜欢的主题，就点进相应的github， 然后按照其安装指导操作就行。这里以typing主题为例，需要执行的步骤为：

1. 安装

```js
cd your-blog
git clone https://github.com/geekplux/hexo-theme-typing themes/typing
```

2. 启用

更改主目录下的`_config.xml`，把其中的`theme`改为`typing`就可以用了。

3. 自定义主题

进入`themes/typing`,修改`_config.xml`进行自定义，注意这里不是主目录下的配置文件。

### 绑定域名

- 准备工作

在[腾讯云](https://cloud.tencent.com/)，[阿里云](https://www.alibabacloud.com/?lang=en)等平台购买一个域名。

- 添加域名解析

在域名解析页面，选择记录类型为**CNAME**，记录值为你的博客网址，以我的配置为例：

![domain.jpg](hexo_blog/Hexo-domain-1.jpg)

配置成功后，输入你的网址就可以看到：
![domain2.jpg](hexo_blog/Hexo-domain-2.png)
这表示你的域名已经连上Github，但是没有找到你的博客，你还差最后一步就完成了。

- 配置博客网址映射到你的域名

在你的**source**文件夹下，添加文件CNAME（注意一定不要直接放在主文件夹下，否则无法用hexo上传，也不要直接在github里仓库主页创建，那样是放在首页，也会被覆盖的），内容为你的域名，这里不包含**http://,www**。再次以我的配置为例：

![domain3.jpg](hexo_blog/Hexo-domain-3.jpg)

文件添加完毕后，等个几秒钟，再次输入你的域名后，便会转到你的博客。

### 添加sitemap和feed

- 安装对应的插件

```js
npm install hexo-generator-feed
npm install hexo-generator-sitemap
```

- 修改配置文件

打开主目录下的 **_config.xml**,添加对应的配置：

```js
//#Feed Atom
feed:
  type: atom
  path: atom.xml
  limit: 20
//#sitemap
sitemap:
  path: sitemap.xml
```

更多配置请阅读[hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed)和[hexo-generator-sitemap](https://github.com/hexojs/hexo-generator-sitemap)的详细文档。

- 在Google和Baidu上提交你的Sitemap

我们使用Sitemap，就是为了告诉搜索引擎自己有那些东西可以爬，从而使自己的搜索权重更大。关于提交部分，请参考：https://www.jianshu.com/p/9c2d6db2f855 

- 拓展

如果你想下载更多插件，可以去官方网址搜索：https://hexo.io/plugins/

### 添加评论

- Disqus

由于Hexo天然集成了Disqus,所以这个是最方便的。主要步骤如下：

1. [注册disqus账号](https://disqus.com/)
2. 在 **_config.xml**中添加内容：

```js
#填写你自己的 shortname
disqus_shortname: xxxxx
```

就可以使用了。

- Gitment

在多说关闭后，貌似这个的呼声比较高，不过在评论的时候，每一次新建文章都需要手动初始化，而且需要Github用户授权认证，这个权限还挺高的，我折腾了半天，还是觉得不太舒服，所以舍弃了这个方案，不过大家有兴趣的可以尝试一下，放个参考链接：https://imsun.net/posts/gitment-introduction/

### 支持Latex数学公式

1. 卸载Marked渲染器(或其他渲染器)，安装Markdown it Plus渲染器

```js
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-markdown-it-plus --save
```

1. 安装`katex`:

```js
npm install markdown-it-katex --save
```

1. 在`_config.xml`中添加如下配置：

```js
markdown_it_plus:
  highlight: true
  html: true
  xhtmlOut: true
  breaks: true
  langPrefix:
  linkify: true
  typographer:
  quotes: “”‘’
  plugins:
    - plugin:
        name: markdown-it-katex
        enable: true
    - plugin:
        name: markdown-it-mark
        enable: false
```

1. 在博客html的head中加载Katex的CSS样式：

在路径/themes/typing/layout/_partial下找到head.ejs文件，将以下语句写入文件:

```html
<link href="https://cdn.bootcss.com/KaTeX/0.7.1/katex.min.css" rel="stylesheet">
```

这样就完成了基于Katex引擎的Latex配置工作，可以添加数学公式了。

### 上传图片

我这里采用了一种比较简单的方式：
首先，打开配置文件`_config.xml`进行编辑：

```js
post_asset_folder: true
```

把这一项原来的false改为true就好，这一步执行完成之后，再次新建文章时，会同时在文章目录下生成一个同名文件夹，用来存放一些资源，这里就是我们的图片啦。使用的时候只需要：

```js
![](folder_name/pic_name.jpg)
```

比如你新建了个文章名为`test.md`，就会看到同时生成了一个`test`文件夹，将你的图片，比如`test.jpg`，放入其中，就可以执行下列语句引用：

```js
![](test/test.jpg)
```

其他图片操作类似，在完成写作之后，就需要上传了。但是文件里相对路径怎么变成链接呢？这个可以依靠插件，直接下载：

```js
npm install hexo-asset-image --save
```

安装完成之后，你就可以放心大胆地进行图片上传操作了。
**注意：** 千万要保证你的markdown文档名和相应的文件名是一致的，这个其实默认生成的时候就一致，提醒只是防止你强行更改导致不一致，那样的话会导致图片无法放到public相应目录中，从而无法显示。

当然，不装插件也行，有个比较简单的方法，直接插入以下命令：

```js
{% asset_img test.jpg 你的描述  %}
```

这样也是可以导入的，同样要注意上面提到的问题。

### 更多相关操作

- 基本操作命令

```js
hexo generate (hexo g) 生成静态文件，会在当前目录下生成一个新的叫做public的文件夹
hexo server (hexo s) 启动本地web服务，用于博客的预览
hexo deploy (hexo d) 部署播客到远端
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
```

- 命令简写

```js
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

- 组合命令

```js
hexo d -g #生成部署
hexo s -g #生成预览
```

## 其他参考文章
[手把手教你使用Hexo + Github Pages搭建个人独立博客](https://linghucong.js.org/2016/04/15/2016-04-15-hexo-github-pages-blog/)
[个性化博客——域名绑定](https://www.jianshu.com/p/cea41e5c9b2a)
[hexo+github搭建博客上传图片问题](https://www.jianshu.com/p/5f75a74b0f5e)
[如何在HEXO中渲染Latex数学公式](http://dawnote.net/2017/12/01/%E5%A6%82%E4%BD%95%E5%9C%A8HEXO%E4%B8%AD%E6%B8%B2%E6%9F%93Latex%E6%95%B0%E5%AD%A6%E5%85%AC%E5%BC%8F/)
[教你怎么让你的hexo博客在搜索引擎中排第一](https://juejin.im/post/590b451a0ce46300588c43a0)