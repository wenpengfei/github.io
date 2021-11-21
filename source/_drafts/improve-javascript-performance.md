---
title: 提升 JavaScript 性能的 12 个小技巧
date: 2019-07-08 14:30:43
tags:
---

> 原文地址：[12 Tips for Improving JavaScript Performance](https://nodesource.com/blog/improve-javascript-performance)
> 同步：{% post_link improve-javascript-performance 夜色镇歌的个人博客 %}

构建一个 APP 或者 Web 页面最重要的一点就是性能，没人希望程序崩溃，或者不停地 loading，用户可以容忍的加载时间都很短，据 [Kissmetrics](https://www.nngroup.com/articles/how-long-do-users-stay-on-web-pages/) 统计，47% 的用户希望等待时间不超过 2 秒，当加载时间超过 3 秒时，40% 的用户会选择关闭或者离开。

考虑到这数字，创建一个 Web App 的时候应该认真考虑性能问题，以下是有效提高性能的 14 个方法。

## 1.浏览器缓存

有两种方法，第一种就是使用 JavaScript 的 Cache API，第二种是使用 HTTP 协议的缓存。

某些经常访问的变量我们就可以把它缓存起来，这样对性能会有很大的提升。

## 2.建立多个环境测试

为了有效的评估在程序中的改进，必须建立一组定义良好的环境，以便测试代码的性能。

尝试对所有 JavaScript 引擎的所有版本进行性能测试和优化在实践中是不可行的，但是在单一环境中进行测试并不是一个好习惯，因为这只能提供部分结果，因此建立多个环境并测试非常重要。

## 3.移除无用的代码

移除无用的代码，不仅可以减小 js 文件的体积，还会缩短浏览器分析和编译代码所需的时间。为此，必须考虑以下几点：如果你检测到一个 function 已经不会再使用到，可以把引用到这个方法的代码都删掉，这样网站的加载速度会更快，用户也会有更好的体验。移除不必要的 npm 包，比如原生已经实现的一些方法就没必要引用一个很大的包去增加代码的体积。

## 4.避免占用太多内存




