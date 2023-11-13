---
layout: ../../layouts/MarkdownPostLayout.astro
title: 如何学习官方文档
author: Astro 学习者
description: "读不懂官方文档，我想问他们是怎么学习的?"
image:
    url: "https://docs.astro.build/assets/rays.webp"
    alt: "Thumbnail of Astro rays."
pubDate: 2022-07-15
tags: ["astro", "learning in public", "setbacks", "community"]
---
# 读不懂官方文档，我想问他们是怎么学习的?

行业中许多刚入门技术人员阅读官方文档困难，正如许多国人学了几十年汉语后依旧读不懂诗歌、看不进去散文一样，原因在于他们没有认真思考过为什么会出现各种各样体裁的文章。

不同体裁的文章最明显的区别在于组织结构不同，你能很明显地感受到官方文档和博客文章的结构差异，却很可能没有认真想过到底什么是结构，哪些东西构成了结构，为什么要用这样的结构。

不懂结构，一方面是因为视野狭隘，只关注眼下的遇到的问题，而不去思考眼下遇到的问题在更大的场景中所处的位置和作用，另一方面是因为阅读量不够，知识结构不完整，不知道文章中各个部分的作用，有时候苦思冥想，不如不求甚解。正所谓学而不思则罔，思而不学则殆，学和思是不可偏废其一的。

文章的结构，如人的骨架一样，决定了文章的整体风貌。科技类的文章，比如软件框架的官方文档，结构往往都通过目录的方式醒目地提炼在了文章之外，可即便如此，许多人却不知道如何去挖掘目录文字之上的含义，也就是为什么这篇文档的目录是这样的。

挖掘目录文字之上的含义，不仅要读懂每个目录项，还要明白每个目录项对于整个文章的作用是怎样的。拿你问题中的链接来看，我们先看一级目录项，有三项，依次是引言、Servlet应用和响应式应用。

1. Introduction（引言）
2. Servlet Applications（Servlet应用）
3. Reactive Applications（响应式应用）

如果仔细阅读开篇第一个段落，

> Spring Security is a framework that provides authentication, authorization, and protection against common attacks. With first class support for both imperative and reactive applications, it is the de-facto standard for securing Spring-based applications.

发现一共两句话，第一句话说明了Spring Security框架提供了哪些功能，第二句话说明了这个框架能够用在什么样的应用中。认证、授权是安全保护机制的基本内容，学习过操作系统理论的人，至少清楚其大致意图和方法。命令式编程从前都是和[声明式编程](https://www.zhihu.com/search?q=声明式编程&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1505895971})放在一起做对比的，近些年开始流行[响应式编程](https://www.zhihu.com/search?q=响应式编程&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1505895971})的说法，熟悉UI编程的人对响应式的编程方式和架构应该有所了解。想到这里，至少明确了这个框架在两种不同的编程模式中应用也会有所差异，这也是为何文章的后两项分别是Servlet应用和响应式应用。作为使用者也就清楚了哪种应用是自己当下使用的，另外一种就不用看了。

我们再来看引言部分的二级目录项，依次包含了前置基础、Spring Security社区、Spring Security 5.4的新特性、开始使用Spring Security、功能特性、项目模块和依赖、代码样例。


1. Prerequisites（前置基础）
2. Spring Security Community（Spring Security社区）
3. What's New in Spring Security 5.4（Spring Security 5.4的新特性）
4. Getting Spring Security（开始使用Spring Security）
5. Features（功能特性）
6. Project Modules and Dependencies（项目模块和依赖）
7. Samples（代码样例）

<strong>要理解为什么一篇文档的目录是这样，而不是那样，就要明白一个最为朴素的道理，一切的文章，如同一切的软件一样，本质上也是一种产品，是要服务于特定群体，解决特定问题的。</strong>可以这样来思考，如果你是Spring Security的开发者，你会在这篇文档的引言中写什么呢？如果是我，我会先想想我的用户都有谁，我想应该有两类，一类是Spring Security框架的使用者，另一类是Spring Security框架的代码贡献者。

对于使用者，他们一定要清楚用这个框架的前提是什么，因此要有前置基础部分，也就是依赖于JVM和EJB Container或者Servlet Container。

在框架的使用中，他们必定会遇到诸多困难，如果是错误使用了软件，大概率能在Stackoverflow这样的社区找到答案，但如果是软件的漏洞，那么要有渠道去沟通，因此要有Spring Security社区。

软件产品的更新换代相比于物理实体的产品可以省去销毁旧产品的环节，可其他的环节一样都不能少，只不过其他的各个环节也都是抽象过程而已，因此要有版本控制，新版本中也有新的特性的加入和旧特性的移除。如果产品的老用户很多，那么把最新版本的特性单独放一个部分，可以提醒他们去做升级，如果觉得有必要的话。

对于新用户而言，他们最关心怎么用和能用什么的问题，因此需要包括开始使用Spring Security和功能特性部分。

安全在一个应用中并不处于中心位置，安全是为了保护应用中核心功能不受恶意攻击而存在的，因而安全模块的存在也必然要依附于其他的模块，因此要有项目模块和依赖。

此外，文档写得再多，不如代码来的精确，阅读官方的代码样例总是能更准确的知道每个功能特性到底应该怎么用。

对于框架的代码贡献者，除了关心使用者关心的所有内容之外，还很关心源代码从哪儿能获取到，提交Pull Request的标准是什么等等。

回过头来看，这篇文章要解决的问题是什么？简单说就是让新用户、老用户和代码贡献者，通过查看产品的功能、适用范围、使用方法、服务条款、维护信息后，能顺利地使用产品。虽然文章整体要解决的是这个问题，但由于服务用户类型不只一种、用户使用产品所处阶段也不单一，所以实际上解决的问题有很多。

按照这个思路，再来看一篇博客为用户提供了什么？博客的体裁是自由的，大多数的技术博客，其实都只是一个实验报告，并不具备思想性和说明性，而且很多质量其实也不高，缺乏实验过程中的各种前置条件、参数等等的详细记录。

为什么会觉得二手笔记十分详细呢，我想一方面是因为关注的点过小，注意力都在每个步骤的命令和配置细节上，并没有放开视野，思考真正解决的问题是什么；另一方面，反思总结得不够，没有抽象出使用不同库、中间件的基本模式和要素。

二手笔记有二手笔记的价值，官方文档有官方文档的价值，不必要把自己限制在一个地方，关键是能想明白，当下自己要解决的问题是什么，还缺什么造成问题得不到解决，是自己的知识空白，还是文档不够详细。

**能将困惑转换成问题，带着问题去搜索、阅读和找答案总是最高效的。**

[编辑于 2020-10-04 01:25](http://www.zhihu.com/question/423648872/answer/1505895971)