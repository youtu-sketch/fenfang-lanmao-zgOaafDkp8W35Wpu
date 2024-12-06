
## 先介绍下 DevNow


* [DevNow Github](https://github.com)
* [体验网站](https://github.com)


DevNow 是一个精简的开源技术博客项目模版，支持 Vercel 一键部署，支持评论、搜索等功能，欢迎大家体验。同时也支持 Follow、 RSS 订阅，欢迎大家订阅。


目前承载着：


* 我的Blog：一些相关的技术文章和个人生活记录。
* Weekly： 每周发布一些技术圈中热度比较高的内容，主要是技术相关的。
* Ph\-Daily： 热榜可能会穿插一些其他的话题和一些开源项目介绍等。
* DevNow：开源项目的一些迭代信息和介绍。


![](https://r2.laughingzhu.cn/3bcbae51ddcea2cdf738a11c2556a5cd-48f5c9.webp)


## 背景


很早之前就看到 `Astro 5.0 beta` 版本，但是没有跟进，想着等稳定版本发布在更新。随着 Astro 5\.0 在 12月4号 正式发布，今天正好不忙就把 DevNow 项目更新到了 `Astro v5.0` ，不过这个版本的更新不是很大，但是有很多很多的新特性，包括 Content Layer、Server Islands、Prerendering、Vite 6 等等，这些都是很值得大家关注的。具体的更新可以看以下文章：


* [Astro 5\.0 发布](https://github.com)
* [DevNow 更新内容](https://github.com):[悠兔机场](https://xinnongbo.com)


下边主要记录下升级过程中遇到的问题，以及解决的办法。


## 升级内容


### Content Layer


主要是通过新的 `Content Layer loader` 来实现加载本地资源文件，具体如下图：


![](https://r2.laughingzhu.cn/4c6fba0454222f893684141e9d795840-b77499.webp)


### 文章结构


* 用 id 替换掉 slug
* render 引入替换


![](https://r2.laughingzhu.cn/88a2371f7d6821c578876f5cec940a02-f11fd5.webp)


## Issues


### loader 加载本地文件


通过 `loader glob` 加载本地文件的时候，默认是通过 `entry id` 来生成文章的索引， `id` 和原先的 `slug` 的区别是 id 会带上文件名后缀，如:



```
// id: 2022-05-22.md
// slug: devnow

```

会导致原先的文章无法加载，搜索引擎已经收录的都是 `slug` 这种路由，这样替换成 `id` 会导致路由的变更，这里还是期望生成 `slug` 这样的路由。


`Astro` 中提供了两种方式：


* [legacy\-flags](https://github.com) : 通过 `legacy-flags` 来兼容原先的数据加载方案。
* 继续使用 `Content Layer loader` ，然后在 `loader` 中进行处理： 通过自定义 `generateId` 来实现自定义 id。解决方案是在社区找到的 [feat: add glob loader](https://github.com)


![](https://r2.laughingzhu.cn/e013d69507c70b5155758e7782520b36-ac8267.webp)


其他的设计一些小改动，具体的改动可以看 [DevNow 的升级 Commit](https://github.com) 中的详细信息。


剩下的后边在继续跟进，还有些新的 `Feature` 也会在后续迭代上。


