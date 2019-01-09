---
title: 如何使用hexo(新手向)
date: 2019-01-09 09:45:04
tags:
---
> 大误  这是一篇通用文档
这是一篇告诉女朋友怎么使用我给她搭建的博客的说明

hexo的搭建过程就不说明了, 关键是现在怎么使用.

### 拉取源码
> 建议使用SSH

![](https://ws2.sinaimg.cn/large/006tNc79gy1fz03zrlcpjj30o80diwgj.jpg)

` git clone git@github.com:pumpkinnan97/pumpkinnan97.github.io.git `
拉取下来了了之后, 切换到blog分支
` git checkout blog `
安装依赖
` npm i `

#### 添加SSH
> 要是我没有记错的话,你电脑之前肯定是配过SSH钥匙的.

那么
` cat ~/.ssh/id_rsa.pub `
**command + c**  
点击GitHub右上角进入 **Setting**  
然后点击 左方的SSH 
` New SSH key `
**command + v**
完毕即可

### hexo博客管理

hexo g : Generate static files. 就是生成静态资源,在发布之前需要进行
hexo s : Start the server.  这个的作用是在本地可以查看现在博客
hexo d : Deploy your website. 发布

使用` hexo new [your blog name]`
然后进入`source`中的`_posts`就可以看到新建的博客,在已经存在的内容下面加上你的博客内容就可以了
最后使用上面的`hexo g` `hexo d` 就完成了发布

### hexo 主题以及配置

>这一块都在**_config.yml**文件中,
`theme: next`选择主题,前提是先需要把主题下载到博客根目录的 themes 文件中

[主题](https://hexo.io/themes/)
[插件](https://hexo.io/plugins/)

就可以像QQ空间一样的愉快的玩耍了