---
layout:     post
title:      Github Pages 个人主页的配置
subtitle:   关于本地调试与数学公式支持
date:       2018-08-01
author:     Yifan
header-img: img/post/bg-coffee.jpeg
catalog: true
tags:
    - Github
    - Jekyll
    - Web Development
---

> 感谢[Huxpro](https://github.com/huxpro)提供的博客模板
> 感谢[BY](http://qiubaiying.top/2017/02/06/%E5%BF%AB%E9%80%9F%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)的详细指导
> 
> 本文的环境为 Windows 10

# 前置步骤

本文搭建的博客主要利用了 Github 提供的 Github Pages 服务，因此事先需要注册一个 Github 账号，并完成一系列配置。这一部分 BY 前辈写的比较详细了，可以直接参考[他的博客](http://qiubaiying.top/2017/02/06/%E5%BF%AB%E9%80%9F%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)中“快速开始”部分。

# 本地调试

本文主要介绍下在当前时间节点下（2018-07）怎么在 Windows 10 平台上搭建自己的本地调试环境。

## 安装 Ruby 与 Jekyll

由于网页的框架 Jekyll 基于 Ruby 来运行，为了能在本地调试我们首先需要安装 Ruby。

之前文章中的 Ruby 版本比较老了，我们没有必要为了迎合版本要求选择一个特别老的版本，尤其是老版本还需要再单独安装 Ruby Devkit 。我们可以到[网站上](https://rubyinstaller.org/downloads/)直接安装 "Ruby+Devkit 2.4.4-2 (x64)"。安装 Ruby 后直接打开命令行输入 gem install jekyll 即可安装 Jekyll 框架。

如果在国内的网络状况比较差的话，可以考虑更换 Ruby 的源。需要注意的是淘宝的源已经不再维护了。

```
# 删除默认的官方源
gem sources -r https://rubygems.org/
# 添加国内源
gem sources -a https://gems.ruby-china.org/
```

## 调试

在真正本地调试之前，我们需要将项目从 Github 克隆到本地。这一部分的初始配置可以参考廖雪峰老师的博客中[关于远程仓库的章节](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374385852170d9c7adf13c30429b9660d0eb689dd43a000)。

### clone 项目到本地

完成以上配置后，输入一行代码即可将项目从 Github 克隆到本地。

```
git clone git@github.com:XXX/XXX.github.io.git
```
### 在本地运行服务器

对自己的博客完成了相应的修改后，可以直接运行一个本地的服务器查看网页效果。运行过程中可能会提示你缺少某些包，根据错误信息直接 gem install 相应的包就行了。

```
cd .../XXX.github.io
# 在项目文件夹内运行服务器
jekyll s
```

另外，用初始模板本地调试时，会产生一些不影响运行的小错误，我的仓库中已经进行了相应的修正。成因和修正方法如下：

- 执行 jekyll s 指令运行服务器时报 "Liquid Warning: Liquid syntax error :unexpected character { in "tag\[1\].size" > {{site.featured_condition_size}}..."

> 这是 Jekyll 的语法问题，在使用模板中的标签库时不需要再加双大括号表示变量。直接打开出错的 html 文件将双大括号去掉就行了。详情可见[ StackOverflow 上的讨论](https://stackoverflow.com/questions/39688902/liquid-warning-liquid-syntax-error-unexpected-character-when-i-jekyll-serve)。

- 点击文章列表时跳转到 404 页面。

> 这可能是 Windows 平台上的问题，主要的产生原因是 Jekyll 没能解析中文文件名。解决方式是修改 Ruby 的配置文件，具体可以参考[这篇博客](https://blog.csdn.net/yinaoxiong/article/details/54025482)。


### 提交相应的修改到 Github

在打开 http://127.0.0.1:4000 查看效果并进行修改后，可以直接在命令行提交相应的修改到 Github 上。

```
git add .
git commit -m "修改备注"
git push origin master
```

# 使 Github Pages 支持数学公式

这一点主要参考了[这篇博客](https://wanguolin.github.io/mathmatics_rending/)。

简单来说就是在 _includes\head.html 中加入：

```
<script type="text/javascript"
    src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
```

效果如下：

$$ \sum_{i=0}^3 \frac{1}{(1+r)^{15+i}} $$

注：在 Chrome 中使用时该脚本可能会被标记为 "unsafe script" ，需要点击地址栏右侧的红叉允许运行。


# Star

若您觉得本文有帮助，请给我的 [github仓库](https://github.com/Freefighter/Freefighter.github.io) 点个 **star** 吧！

谢谢~