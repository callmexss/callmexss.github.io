---
title: 一些能够提高工作效率的绝密 VS Code 奇淫巧计「翻译」
tags:
  - vscode
  - ARTS
categories:
  - [翻译]
  - [器]
date: 2019-09-08 23:20:13
categorises:
---


# 前言

拖延了很久的 ARTS 打卡终于准备开始了。A 是指 Algorithm，即每周练习一道算法题；R 是指 Review，阅读并点评至少一篇英文技术文章；T 是指 Tip，学习一个新的技术技巧；S 是 Share，分享一篇有观点和思考的技术文章。

不知不觉已经脱离 IDE 很久了，因为找工作期间都是写一些代码量比较小的算法题目和程序片段，主力开发软件已经成了 Vim 和 VS Code。工欲善其事必先利其器，所以我的第一篇 Review 选择了一篇有关 VS Code 的文章。

# 译文

> 原文 [Here are some super secret VS Code hacks to boost your productivity](https://medium.com/free-code-camp/here-are-some-super-secret-vs-code-hacks-to-boost-your-productivity-20d30197ac76)

不论是作为一名爱好者、专业者、或者甚至一个月只写一次代码的开发者，您都必须知道，对于一个在工作时愿意投入最大效率时间的人来说，拥有智能和锋利的开发工具是至关重要的。

我已经筹措整理了一些技巧、诀窍和扩展，并经过重重筛选，只保留了对现代 web 开发者来说最少和最有用的一部分。正如我们所知道的，Javascript 的生态系统非常庞大，而且还在不断壮大。基于这个原因，我会尽量做到不偏不倚。

接下来的 Visual Studio Code 技巧将会让您在完成所有的编码阶段后能够以如下的姿态走出去：

<iframe src="https://giphy.com/embed/26gJyIscAHtBNcc00" width="480" height="266" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/ufc-mma-ufc-205-26gJyIscAHtBNcc00">via GIPHY</a></p>

## 让它变得美丽友好

> If it really looks good and friendly, you’ll love the time spent with it.
> 如果它真的看起来不错和友好，你会爱上和它相处的时间的。

### [Material Theme](https://marketplace.visualstudio.com/items?itemName=Equinusocio.vsc-material-theme) & [Icons](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)

这是 VS Code 主题的核心。我认为 material theme 是一款编辑器中最接近使用纸笔书写的东西了（尤其是使用 **no contrast variant** 主题后）。从集成工具到文本编辑器，您的编辑器看起来几乎是扁平、无缝的。

想象一下史诗级别的主题配上史诗级别的图标。[Material Theme Icons](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme) 是替换默认 VS Code 图标的令人惊叹的选择。其设计的大型图标目录与主题融为一体，使之更加美观。这将帮助您轻松地在资源管理器中找到您的文件。

{% asset_img vsc-material-theme.jpg material theme %}

### 居中布局的禅模式

您也许已经知道了禅模式（Zen Mode）视图，也称为无干扰视图（对于那些来自 Sublime Text 的用户来说），它会移除所有事物（除了代码），来让您和您的编辑器有真正亲密的接触。您知道您可以使其居中布局并帮助你像在 PDF 阅读器中一样阅读您的代码吗？这将真正帮助您专注于一个函数或者研究别人的代码。

```text
Zen Mode: [View > Appearance > Toggle Zen Mode]
Center Layout: [View > Appearance > Toggle Centered Layout]
```

译者注：更简单的方法是 `ctrl+shift+p` 然后输入 zen mode 或者 centered 然后选择对应选项敲回车即可。

{% asset_img zen-mode-with-centered-layout.gif 居中布局的禅模式 %}

### 使用具有连写的字体

好的风格使阅读变得容易和舒服。您可以用支持连写的令人惊叹的字体使您的编辑器看起来更好。这里有 [6 款支持连写的最好的字体](https://www.slant.co/topics/5611/~monospace-programming-fonts-with-ligatures)（来源 [www.slant.co](http://www.slant.co/))。

{% asset_img coding-with-ligatures.gif 使用带连写的字体进行编码 %}

你可以试用 [Fira Code](https://github.com/tonsky/FiraCode)，它很棒并且是开源的。在安装这款字体后你应该这样修改 VS Code 的配置。

```json
"editor.fontFamily": "Fira Code",
"editor.fontLigatures": true
```

{% asset_img all-ligatures.png 使用带连写的字体进行编码 %}

着名的字体Operator Mono没有为连字提供原生支持，然而如果你是连字的狂热粉丝，你可以使用[这个库](https://github.com/kiliman/operator-mono-lig)来添加它们。

### [Rainbow Indent](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)

缩进风格。这个扩展为文本前面的缩进着色，每一步交替使用四种不同的颜色。

{% asset_img rainbow-indent.png %}

默认的缩进设置按照彩虹的配色为缩进着色。然而我通过定制自己的方案来跟随不同的灰度。如果您希望您的缩进风格和示例中的一致，把下面的片段复制粘贴到您的 `settings.json` 中。

```json
"indentRainbow.colors": [
"rgba(16,16,16,0.1)",
"rgba(16,16,16,0.2)",
"rgba(16,16,16,0.3)",
"rgba(16,16,16,0.4)",
"rgba(16,16,16,0.5)",
"rgba(16,16,16,0.6)",
"rgba(16,16,16,0.7)",
"rgba(16,16,16,0.8)",
"rgba(16,16,16,0.9)",
"rgba(16,16,16,1.0)"
],
```

### 定制标题栏

这是一个很好的视觉调整。我从 [Wes Bos](https://medium.com/u/86a55cd7983b?source=post_page-----20d30197ac76----------------------) 的一堂 [React & GraphQL](https://advancedreact.com/) 课程中复制了它。基本上，他通过给不同的项目配上不同的标题栏颜色来更容易地识别它们，并帮助观众区分它们。这在具有相同代码或文件名的应用中真的非常有用，例如，一个 react-native 的移动应用和一个 react web 应用。

{% asset_img title-bar-customization.png 定制标题栏 %}

您可以通过在每个您想要不同标题栏颜色的项目的 Workspace Settings 中编辑 Title Bar 来设置不同的标题栏颜色。

## 更快的编码

> The very essence of getting it done in time
> 及时完成任务的本质

### 标签换行

如果你不知道 [Emmet](https://emmet.io/)，那么你可能是喜欢在写代码时敲下每个字符的人。Emmet 使你能够通过输入缩写来获取相应的标签。这可以由选择一块代码并输入 Wrap with Abbreviation 命令来实现，我为这个操作绑定了一个快捷键 `shift + alt + .`。

参见下面的例子：

{% asset_img wrap-it-up-with-emmet.gif 使用 Emmit 包裹代码 %}

如果您想把所有这些都包裹成单独的行。那么您可以使用 *wrap with individual lines* 命令，然后在缩写后面插入一个 \*，例如 `div*`。

如果你想直接转到 Emmet，这里是一份[Emmet 的速查手册](https://docs.emmet.io/cheat-sheet/)。

### Balance Inwords 和 Balance Outwords

这个技巧来自我强烈推荐的 [https://vscodecandothat.com/](https://vscodecandothat.com/)。

你可以通过使用 Emmet 的 `balance inword` 命令和 `balance outword` 命令在 VS Code 中选择一个代码块。把这些命令绑定到快捷键时非常有用的，比如 `Ctrl + Shift + Up Arrow` 对应 Balance Outwords，`Ctrl + Shift + Down Arrow` 对应 Balance Inwords。

{% asset_img balance-inwords-and-outwords.gif Balance Inwords/Outwords %}

### [Turbo Console.log()](https://marketplace.visualstudio.com/items?itemName=ChakrounAnas.turbo-console-log)

没有人喜欢敲像 `console.log()` 这样长的输入。这着实时让人易怒的，大多数情况下你只是想快速的输出一些东西，查看一下它的值然后继续编码。如果我告诉您可以像 Lucky Luke 一样快的使用 `console.log` 会怎么样呢。

译者注：Lucky Luke — the man who shoots faster than his shadow，幸运星卢克，是一部由比利时漫画家莫里斯于1946年创作的西方连环画。

这个工作可以由 [Turbo Console Log](https://marketplace.visualstudio.com/items?itemName=ChakrounAnas.turbo-console-log) 扩展来完成。它能够在当前行的下一行记录任何变量,并遵循代码结构自动生成前缀。您还可以通过使用快捷键 `alt+shift+u/ alt+shift+c` 来注释或者取消注释由该插件添加的 `console.log`。

此外，还可以用 `alt+shift+d` 删除它们:

{% asset_img turbo-console-log.gif conso.log() 自动涡轮机 %}

### [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)

这是一个令人惊叹的扩展，它帮助您启动一个支持静态和动态页面实时重新加载的本地开发服务器。它对于大多数特性都有很好的支持：HTTPS，CORS，可定制的 IP 地址和端口。

[点击这里下载](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)

{% asset_img live-server.gif Live Server %}

如果搭配 [VS Code LiveShare](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare) 它甚至允许你分享你的 `localhost`。

### 多光标复制粘贴

使用 VS Code 的过程中，我的第一次“哇哦”献给了多光标编辑。很久之后，我发现这个特性的一个巧妙应用，您可以复制粘贴光标选中的内容，并且按照它们被复制的顺序粘贴他们。

看看下面的例子：

{% asset_image copy-and-paste-with-different-cursors.gif 多光标复制粘贴 %}

### 面包屑导航和大纲

面包屑导航展示当前的位置，并允许你快速的在文件和符号间跳转。通过 `View > Toggle Breadcrumbs` 命令或者 `breadcrumbs.enabled` 设定来启动它。

大纲视图是文件管理器树底部单独的部分。展开后，将显示当前活动编辑器的符号树。

大纲视图有不同的排序模式，可选光标跟踪。它还有一个输入框来过滤你输入的符号。错误和警告也会展示在大纲视图中，使你能够瞟一眼就找到错误所在。

{% asset_image breadcrumb-and-outline-relation.gif 面包屑导航和大纲的关系  %}

## 杂项

> Those little tweaks that change everything
> 那些改变一切的小调整

### 命令行里的 code

VS Code有一个强大的命令行界面，允许您控制如何启动编辑器。你可以打开文件，安装扩展，修改显示语言，通过命令行界面输出诊断信息。

{% asset_image code-cli.png code 的命令行接口 %}

想象一下您刚使用 `git clone <repo-url>` 克隆了一个仓库，然后您想替换当前正在使用的 VS Code 实例。`code . -r` 能够使你在不离开命令行的情况下就实现这个功能（[从这里了解更多](https://code.visualstudio.com/docs/editor/command-line)）。

### [polacode](https://github.com/octref/polacode)

> Polaroid for your code

您经常会遇到具有自定义字体和主题的吸引人的代码截图，如下所示。这是在带有 Polacode 扩展的 VS Code 中获得的。

{% asset_image polacode.png VS Code 的 Polacode 扩展 %}

我知道 [Carbon](https://carbon.now.sh/) 是一个好的并且更加可定制化的代替品。然而，Polacode 允许您留在代码编辑器中，并使用您可能购买的任何在 Carbon 中不可用的专有字体。

### [Quokka (JS/TS ScratchPad)](https://quokkajs.com/)

Quokka是JavaScript和TypeScript的快速原型开发平台。它在您键入代码时立即运行您的代码，并在代码编辑器中显示各种执行结果和控制台日志。

{% asset_image quokka.gif Quokka %}

Quokka的一个很棒的应用是，当您在为技术面试做准备时，您能够输出每个步骤，而不用在调试器中设置断点。

它还可以帮助您在实际使用诸如Lodash或MomentJS之类的库函数之前研究它们。它甚至可以用于异步调用。

### [WakaTime](https://wakatime.com/)

你的朋友认为你花了太多时间敲代码吗？记录并告诉他们每天 10 小时并不算“太多”。WakaTime 是一个有助于记录和存储有关您的编程活动的指标和分析的扩展。

您可以设定目标，查看您常用的编程语言，您甚至可以世界上的其他忍者做一个对比。

{% asset_image wakatime.png WakaTime Dashboard %}
{% asset_image wakatime-1.png WakaTime Leaderboards %}

### [VSCode Hacker Typer](https://marketplace.visualstudio.com/items?itemName=jevakallio.vscode-hacker-typer)

您是否曾经在人群前写过代码？你经常不顾一切地打字，一边打字一边聊天，这让你有点困惑。想象一下预先键入的代码，只有当您在 geektyper 中模拟类型时才会出现。

<iframe width="663" height="382" src="https://www.youtube.com/embed/ulnC-SDBDKE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

[Jani Eväkallio](https://medium.com/u/3e0af129613a?source=post_page-----20d30197ac76----------------------) 把这个插件带到了 VS Code。它将帮助您记录和重播宏(在编辑器中编写的代码)，使您在向观众打字时 100% 更加专注。

### Exclude folders

我在[一篇 stackoverflow 的帖子](https://stackoverflow.com/questions/33258543/how-can-i-exclude-a-directory-from-visual-studio-code-explore-tab/33277809#33277809)里学到这个技巧。这是一个快速调整，将 node modules 或其他文件夹从资源管理器树中排除，以帮助您只关注重要的内容。对于我来说，我非常讨厌在编辑器中打开乏味的node module文件夹，所以我决定隐藏它。

例如，要隐藏 node_modules，您可以执行以下操作：

1. **File > Preferences > Settings** (MAC 下 **Code > Preferences > Settings**)
2. 在设置中搜索 `files.exclude`
3. 点击 `Add Pattern` 然后输入 `**/node_modules`
4. 瞧！node_modules 从资源管理器树中消失了

### 你的建议

你可以随时评论一些关于VSCode的最秘密的提示，我很乐意将它们添加到列表中来帮助其他人。:)

我希望这些技巧能够真正提高 VS Code 的效率。如果您喜欢这篇文章，请鼓掌并分享，如果我错过了任何扩展，请评论。

# 尾声

以后学习前端的时候可以试一试里面提到的技巧。磕磕绊绊好几天才翻译完，借助了翻译工具，好吧其实它们呢才是主力。然后下一篇文章要读过再翻译，这一篇的读完的收获感觉不匹配投入的时间啊哭泣QAQ。

最后 VS Code 真好用呀。心痒痒的什么时候入一下 **typescript** 坑也自己动手写插件。