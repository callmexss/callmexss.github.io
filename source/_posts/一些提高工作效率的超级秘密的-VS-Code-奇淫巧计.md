---
title: 一些提高工作效率的超级秘密的 VS Code 奇淫巧计「翻译」
tags:
  - vscode
  - ARTS
categories:
  - 翻译
  - 器
date: 2019-09-08 23:20:13
categorises:
---


# 前言

拖延了很久的 ARTS 打卡终于准备开始了。A 是指 Algorithm，即每周练习一道算法题；R 是指 Review，阅读并点评至少一篇英文技术文章；T 是指 Tip，学习一个新的技术技巧；S 是 Share，分享一篇有观点和思考的技术文章。

不知不觉已经脱离 IDE 很久了，因为找工作期间都是写一些代码量比较小的算法题目和程序片段，主力开发软件已经成了 Vim 和 VS Code。工欲善其事必先利其器，所以我的第一篇 Review 选择了一篇有关 VS Code 的文章。

# 译文

> 原文[Here are some super secret VS Code hacks to boost your productivity](https://medium.com/free-code-camp/here-are-some-super-secret-vs-code-hacks-to-boost-your-productivity-20d30197ac76)

不论是作为一名爱好者、专业者、或者甚至一个月只写代码的开发者，您都必须知道，对于一个在工作时愿意投入最大效率时间的人来说，拥有智能和锋利的开发工具是至关重要的。

我已经筹措整理了一些技巧、诀窍和扩展，并经过重重筛选，只保留了对现代 web 开发者来说最少和最有用的一部分。正如我们所知道的，Javascript 的生态系统非常庞大，而且还在不断壮大。基于这个原因，我会尽量做到不偏不倚。

接下来的 Visual Studio Code 技巧将会让您在完成所有的编码阶段后能够以如下的姿态走出去：

<iframe src="https://giphy.com/embed/26gJyIscAHtBNcc00" width="480" height="266" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/ufc-mma-ufc-205-26gJyIscAHtBNcc00">via GIPHY</a></p>

## 让它变得美丽友好

> If it really looks good and friendly, you’ll love the time spent with it.
> 如果它真的看起来不错和友好，你会爱上和它相处的时间的。

### [Material Theme](https://marketplace.visualstudio.com/items?itemName=Equinusocio.vsc-material-theme) & [Icons](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)

这是 VS Code 主题的核心。我认为 material theme 是一款编辑器中最接近使用纸笔书写的东西了（尤其是使用 **no contrast variant** 主题后）。从集成工具到文本编辑器，您的编辑器看起来几乎是扁平、无缝的。

想象一下史诗级别的主题配上史诗级别的图标。[Material Theme Icons](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme) 是替换默认 VS Code 的令人惊叹的选择。其设计的大型图标目录与主题融为一体，使之更加美观。这将帮助您轻松地在资源管理器中找到您的文件。

{% asset_img vsc-material-theme.jpg material theme %}

### 居中布局的禅模式

您也许已经知道了禅模式（Zen Mode）视图，也称为无干扰视图（对于那些来自 Sublime Text 的用户来说），它会移除所有事物（除了代码），来让您和您的编辑器有真正亲密的接触。您知道您可以使其居中布局并帮助你像在 PDF 阅读器中一样阅读您的代码吗？这将真正帮助您专注于一个函数或者研究别人的代码。

```text
Zen Mode: [View > Appearance > Toggle Zen Mode]
Center Layout: [View > Appearance > Toggle Centered Layout]
```

{% asset_img zen-mode-with-centered-layout.jpg material theme %}