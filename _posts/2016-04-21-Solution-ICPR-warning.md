---
layout: post
title:  Solution, ICPR-warning, page has the wrong paper size
date:   2016-04-21
categories: Solution
permalink: Solution-ICPR-warning
tags: Solution

# author
author: Minqi Mao
---

## ICPR上传文档时显示

There are compliance problems that prevent the file from being accepted for submission

--Page 1 has the wrong paper size (US Letter instead of the required A4)

--Page 2 has the wrong paper size (US Letter instead of the required A4)

...

--Page 1 has margin impositions

--Page 2 has margin impositions

...

![drawing](https://raw.githubusercontent.com/minqimao/minqimao.github.io/master/images/postsimage/2016/20160420201118.jpg)

模板(Template) IEEE Trans.

## 解决方案(Solution)

IEEE 模板的documentclass后面加上a4paper, 如下图

原始：

![drawing](https://raw.githubusercontent.com/minqimao/minqimao.github.io/master/images/postsimage/2016/20160421072734.jpg)

修改后：

![drawing](https://raw.githubusercontent.com/minqimao/minqimao.github.io/master/images/postsimage/2016/20160421072751.jpg)

最后提交paper显示

![drawing](https://raw.githubusercontent.com/minqimao/minqimao.github.io/master/images/postsimage/2016/20160420201036.jpg)