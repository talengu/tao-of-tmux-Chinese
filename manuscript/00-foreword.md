


# 前言 

我所有的朋友几乎都在使用tmux。我记得我们晚上出去喝酒，我们三个找一个桌子，掏出手机。这还是手机有“QWERTY”键的时代。

尽管放在家里的电脑开启睡眠模式或者关掉了，但我们经常访问的 IRC 频道中的用户名仍然存在于聊天室列表中。我们的黑色terminal 上五颜六色的东西。我们使用ConnectBot连接云服务器并通过运行 [`screen(1)`](https://en.wikipedia.org/wiki/GNU_Screen) 重新连接聊天窗口。在凌晨2点，我们的土耳其咖啡到了，`|away`状态消息被替换掉，我们开始工作。

有趣的是，即使我们知道对方的真实名字，我们有时也会用昵称相互打招呼。人际关系从线上到了线下。


似乎它是精心策划的，但我们每个人都陷入了生活的同一个潮起潮落。没有人告诉我们这样做，但一点一点地，我们逐步优化我们的个人和专业生活方式，以达到看似奇怪的目的地。

就像生活中的很多事情，当我随心所欲按导航走的时候，其实最后到达一个地方，这种常常会有发生。所以，当我写一本电脑软件的教育手册的时候，希望不是强塞给你 tmux，喜欢或者不喜欢，但告诉你tmux 是什么，一些常用的使用技巧。



## 关于本书

我通过完全免费的 [The Tao of tmux](https://tmuxp.git-pull.com/en/latest/about_tmux.html) 帮助过成百上千人学习 tmux，这份材料也属于 [tmuxp session manager](https://github.com/tony/tmuxp) 的一部分文档。
现在，我将它拓展了一下，完善图，例子等，使之成为了一本书。

你只是想使用tmux的话，有一份man技术手册  [manpage for tmux](http://man.openbsd.org/OpenBSD-current/man1/tmux.1)。然而 Manpages 很少解释清楚抽象概念，他们更多的是一种参考手册。在这本书中我总结了个人多年使用和教人使用tmux的经验。
在这本书里，我们将 tmux 按它的组成拆分，从 servers 到 panes。当然也会包括一些 terminal 的基础知识照顾到那些自学者。我参加过许多开源项目，设计了一些有效的 terminal 的 工作流程 （workflows）。

tmux 真的很有用。它成为了我日常的一部分，除了官方的文档资源，我也写了受欢迎的  tmux 的配置文件[configuration](https://github.com/tony/tmux-config)，和插件 [tmux session manager](https://tmuxp.git-pull.com)。

我此刻正用运行在tmux pane里面的vim，写此文档。
> I am writing this from vim running in a tmux pane, inside a window, in a session running on a tmux server, through a client.


对初学者的一句忠告：不要觉得你需要在一次就能掌握命令行和终端多路复用的概念。 您可以根据自己的需要或兴趣选择自己喜欢的tmux概念。 如果您还没有安装tmux，请查看本书的 [Installation section](99-installation.md)部分。

本书twitter [@TheTaoOfTmux](https://twitter.com/TheTaoOfTmux) 查看更新或者分享此文 [share on Twitter](https://twitter.com/intent/tweet?text=I%27m%20reading%20The%20Tao%20of%20tmux%20online%20at&url=https://leanpub.com/the-tao-of-tmux/read&hashtags=tmux&via=TheTaoOfTmux)!

## 代码等风格说明

强调符号如 `like this` ，这代表为代码部分。

以`$`开头的为 terminal 的命令（command）。`$ echo 'like this'`。在终端中输入时不需要输入dollar符号。想知道更多关于 dollar提示符的意义，可查看在  Super User 网站上的 [*What is the origin of the UNIX $ (dollar) prompt?*](https://superuser.com/questions/57575/what-is-the-origin-of-the-unix-dollar-prompt)。

在 tmux 中，快捷键（shortcuts）需要有个预按键 [prefix key](03-practical-usage.md) 和其他键来组合命令。如  `Prefix` + `d` 代表将从 session 中脱离（detach）。通过 tmux默认 `<Ctrl-b>`，用户也可以更改。这些内容在 *the prefix key*部分 和  [*configuration*](08-configuration.md) 章节。



## 本书主要内容

安装 [installation](http://man.openbsd.org/OpenBSD-current/man1/tmux.1)和技术细节部分在附录部分。许多书在前几章就介绍安装，但是背景写的比较少，可能会使刚开始学习的人迷惑。还有一些特殊的章节，如  [tmux on Windows 10](99-windows-bash.md)。我在文章加了一些截图，方便读者可视理解。

[*tmux 初识（Thinking in tmux）*](01-thinking-tmux.md) 概览一下 tmux 的功能和它如何配合GUI来操作。你会了解tmux大概东西，从而使得更加愉快的写代码。

[*Terminal Fundamentals*](02-terminal-fundamentals.md) shows the text-based environments you'll be dealing with. It's great for those new to tmux, but also presents technical background for developers, who learned the ropes through examples and osmosis. At the end of this section, you'll be more confident and secure using the essential components underpinning a modern terminal environment.

[*Practical usage*](03-practical-usage.md) covers common bread-and-butter uses for
you to use tmux immediately.

[*Server*](04-server.md) gives life to the unseen workhorse behind the scenes
powering tmux. You'll think of tmux differently and may be impressed a
client-server architecture could be presented to end users so seamlessly.

[*Sessions*](05-session.md) are the containers holding windows. You'll learn what
sessions are and how they help organize your workspace in the terminal. You'll
learn how to manipulate and rename and traverse sessions.

[*Windows*](06-window.md) are what you see when tmux is open in front of you.
You'll learn how to rename and move windows. 

[*Panes*](07-pane.md) are a terminal in a terminal. This is where you get to work and
do your magic! You'll learn how to create, delete, move between, and resize
panes.

[*Configuration*](08-configuration.md) discusses customization of tmux and sets the
foundation for how to think about `.tmux.conf` so you can customize your own.

[*Status bar and styling*](09-status-bar.md) is devoted to the customization
of the status line and colors in tmux. As a bonus, you'll even learn how to
display system information like CPU and memory usage via the status line.

[*Scripting tmux*](10-scripting.md) goes into command [aliases](#aliases)
and the advanced and powerful [Targets](#targets) and [Formats](#formats)
concepts.

[*Technical stuff*](#technical-stuff) is a glimpse at tmux source code and how it
works under the hood. You may learn enough to impress colleagues who already use
tmux. If you like programming on Unix-like systems, this one is for you.

[*Tips and tricks*](11-tips-and-tricks.md) wraps up with a whirlwind of useful
terminal tutorials you can use with tmux to improve day to day development and
administration experience.

[*Cheatsheets*](99-cheatsheets.md) are organized tables of commands,
shortcuts, and formats grouped by section.

## 打赏

If you enjoy my learning material or my open source software projects, please
consider donating. Donations go directly to me and my current and future open source
projects and are not squandered. Visit <http://www.git-pull.com/support.html>
for ways to contribute.

## 书籍形式（Formats）

书本在 [Leanpub](https://leanpub.com/the-tao-of-tmux) 和 [Amazon Kindle](http://amzn.to/2gPfRhC)有销售。

免费的英文版本 [free on the web](https://leanpub.com/the-tao-of-tmux/read)。
中文版本 [https://tao-of-tmux.readthedocs.io](https://tao-of-tmux.readthedocs.io/zh_CN/latest/)

## 勘误说明（Errata） {#errata}

This is my first book. I am human and make mistakes.

If you find errors in this book, please submit them to me at tao.of.tmux <AT>
nospam git-pull.com.

You can also submit a pull request via <https://github.com/git-pull/tao-of-tmux>.

I will update digital versions of the book with the changes where applicable.

## 感谢

Thanks to the [contributors](https://github.com/git-pull/tao-of-tmux/graphs/contributors)
for spotting errors in this book and submitting errata through GitHub. In
addition, readers like Graziano Misuraca, who looked through the book closely,
providing valuable feedback.

Some copy, particularly in [cheatsheets](#appendix-cheatsheets), comes straight out
of the manual of tmux, which is [ISC-licensed](https://github.com/tmux/tmux/blob/master/COPYING).

## 本书跟新和 tmux 的变动

本书基于 tmux 2.3，发布于 2016年9月。

2017年1月，作者发布到 Leanpub，还有一个更新的版本在 Kindle。

tmux does intermittently receive updates. I've accommodated many over the past 5
years on my personal configurations and software libraries set with [continuous integration tests against multiple tmux versions](https://github.com/tony/libtmux/blob/master/.travis.yml).
Sometimes, publishers overplay version numbers to make it seem as if it's worth
striking a new edition of a book over it. It's effective for them, but I'd
rather be honest to my readership.

If you're considering keeping up to date with new features and adjustments to tmux,
the [`CHANGES`](https://github.com/tmux/tmux/blob/master/CHANGES) file in the
project source serves as a way to see what's updated between official releases.
