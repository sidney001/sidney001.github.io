---
title: Hexo插件记录(长期更新)
date: 2016-11-12 18:31:52
categories:
	- hexo
tags:
	- hexo
	- blog
---
### 1.使用**本地图片**
 hexo 中使用本地图片是件非常让人纠结的事情，在 markdown 里的图片地址似乎永远无法和最后生成的网页保持一致。解决方案：
[hexo-asset-image](https://github.com/CodeFalling/hexo-asset-image)
#### 使用：
首先确认 `_config.yml` 中有 `post_asset_folder:true` 。
在 hexo 目录，执行
`npm install https://github.com/CodeFalling/hexo-asset-image --save`
假设在
```shell
MacGesture2-Publish
├── apppicker.jpg
├── logo.jpg
└── rules.jpg
MacGesture2-Publish.md
```
这样的目录结构（目录名和文章名一致），只要使用` ![logo](MacGesture2-Publish/logo.jpg)` 就可以插入图片。
<!--more-->