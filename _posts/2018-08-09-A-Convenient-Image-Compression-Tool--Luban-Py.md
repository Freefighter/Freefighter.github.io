---
layout:     post
title:      A Convenient Image Compression Tool -- Luban-Py
subtitle:   Very Close to the Efficacy of WeChat
date:       2018-08-09
author:     Yifan
header-img: img/post-bg-compression.jpg
catalog: true
tags:
    - Python
    - Pillow
    - Image Compression
---

# Need for Convenient Compression

Recently I began to operate my own blog, and I found a high-quality image in my blog is attractive, however it will take users much time to load it. In that case I hoped to find a convenient tool to quickly compress my images while the local tool was scarcely available.

At that time, I happened to [Luban](https://github.com/Curzibn/Luban/), which is an image compressing tool for Android with efficacy very close to that of `WeChat` Moments. I read its source code and ported its algorithm to this project -- [Luban-Py](https://github.com/Freefighter/Luban-Py).

# Introduction to the Algorithm used

The algorithm itself is quite simple, while we have to thank Zheng Zibin for finding the proper parameters in the algorithm.

1. Determine whether the image needs to be compressed by its size.

2. If it does, compute the ratio of the width and the height, and resize the image by a certain factor.

3. Reduce the quality of the image to 60.

# Usage

- Drop one or more images to "Drop your images here.bat"

In this way, you can rewrite "luban.py" to meet your requirement. However the images have to be in the same folder with the .bat file.

![fig 1](/img/post/illus-compression-1.png)

- Drop one or more images to "compress.exe"

In this way, the images can be from anywhere.

![fig 2](/img/post/illus-compression-2.png)

> The compressed images will appear in the folder "target" under the same path with the original images.


# Star

If you think this tool may be helpful, please feel free to **star** my [Github Repository](https://github.com/Freefighter/Luban-Py)!

Thanks~