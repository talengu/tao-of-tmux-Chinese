# Terminal 基础知识（fundamentals） {#terminal-fundamentals}

在使用 tmux 之前，我们回顾一下命令行操作的基本知识。通常，我们使用命令行根据我们的使用经验和肌肉记忆，我们中很大一部人没有了解到工具之间的联系。

经验丰富的开发者对 Zsh, Bash, iTerm2, konsole, /dev/tty, shell scripting 等比较熟悉。如果使用 tmux，你将经常和这些工具打交道，不管你是使用GUI界面，还是使用SSH连接远程服务器。

如果你想了解在系统 kernel 层（数据结构等等）处理进程和TTY的细节，可以参考 Marshall Kirk McKusick 的 [*The Design and Implementation of the FreeBSD Operating System (2nd Edition)*](http://amzn.to/2iTmVyv)，尤其是 Chapter 4, *Process Management* 和 Section 8.6, *Terminal Handling*。以及 Linus Åkesson 写的 [*The TTY demystified*](http://www.linusakesson.net/programming/tty/index.php)（在线）深入 TTY，这些会帮助你理解。

从 Unix、4.2 BSD 等的历史中，我们可以找到更多相关知识，我们可以讨论一整天。我们可以从多个角度来看，比如 C 语言或者来自 Unix/BSD 谱系，或者是 Linux、GNU 等等。就像《权力的游戏》一样，你可以从多个故事中找到线索。这里有一些好的 YouTube 视频资源：Marshall Kirk McKusick 讲的 [*A Narrative History of BSD*](https://www.youtube.com/watch?v=bVSXXeiFLgk)、AT&T [*The UNIX Operating System*](https://www.youtube.com/watch?v=tc4ROCJYbm0)、Stephen R. Bourne [*Early days of Unix and design of sh*](https://www.youtube.com/watch?v=FI_bZhV7wpI)。

## POSIX 标准

操作系统如 macOS（原名 OS X）、Linux 和 BSD，在处理操作系统的各种职责和接口时，遵循类似 POSIX 规范的标准。它们被归类为["Mostly POSIX-compliant"（基本符合 POSIX）](https://en.wikipedia.org/wiki/POSIX#Mostly_POSIX-compliant)。

在日常生活中，出于实用性的考虑，我们经常会打破与 POSIX 标准的兼容性。像 macOS 这样的操作系统会直接让你进入 Bash。[`make(1)`](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/make.html) 是 POSIX 标准，但在 macOS 上默认是 [GNU Make](https://www.gnu.org/software/make/)。你知道吗，截至 2016 年 9 月，POSIX Make 还没有条件语句？

我这么说并不是要攻击纯粹主义者。作为一个在脚本中尽量保持兼容性的人，一段时间后做简单的事情也会变得困难。在 FreeBSD 上，默认的 Make [(PMake)](https://www.freebsd.org/doc/en_US.ISO8859-1/books/pmake/) 在条件语句之间使用点号：

```
    .IF

    .ENDIF
```

但在大多数 Linux 系统和 macOS 上，GNU Make 是默认的，所以它们可以这样做：

```
    IF

    ENDIF
```

这是跨越操作系统的众多微小不一致之一，包括它们的用户空间、二进制/库/包含路径，以及对 [文件系统层次结构标准（Filesystem Hierarchy Standard）](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)的遵守/解释，或者它们是否遵循自己的标准。

> **找到你的路径**
> 
> 大多数受 Unix 启发的操作系统（BSD、macOS、Linux）都允许你通过 [`hier(7)`](https://www.freebsd.org/cgi/man.cgi?hier(7)) 获取系统文件系统层次结构的信息。
>
> ```
> $ man hier
> ```

这些差异会累积。大量的软件基础设施的存在 solely 是为了抽象这些差异。例如：CMake、Autotools、SFML、SDL2、解释型编程语言及其标准库都致力于规范化 BSD 衍生系统和 Linux 发行版之间平淡无奇的差异。你的 C 和 C++ 应用程序中会有很多很多的 `#ifdef` 预处理指令。你想要开源，你就获得了选择，但要注意；保持这些上游项目（甚至你的个人项目）的兼容性需要大量的维护成本。但我离题了，回到终端话题。

为什么这很重要？为什么要提这个？你会在任何地方看到这些东西。所以，让我们把常见的嫌疑对象分门别类。

## Terminal interface（终端接口）

终端接口最好通过引用官方规范来介绍，它列出了其技术属性、接口和职责。可以在其 [POSIX 规范](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap11.html)中查看。

这包括 TTY，包括文本终端和其中的 X 会话。在 Linux / BSD 系统上，你可以通过 `<ctrl-alt-F1>` 到 `<ctrl-alt-F12>` 在会话之间切换。

## Terminal emulators（终端模拟器）

GUI 终端：Terminal.app、iterm、iterm2、konsole、lxterm、xfce4-terminal、rxvt-unicode、xterm、roxterm、gnome terminal、cmd.exe + bash.exe

## Shell 语言 {#shell-languages}

Shell 语言是编程语言。你可能不会用 [`gcc`](https://gcc.gnu.org/) 或 [`clang`](http://clang.llvm.org/) 将代码编译成二进制文件，也没有光鲜亮丽的 [npm](https://www.npmjs.com/) 包管理器，但语言就是语言。

每个 shell 解释器都有自己的语言特性。和 shell 一样，许多都会类似于 [POSIX shell 语言](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_01)并努力与之兼容。Zsh 和 Bash 应该能够理解你编写的 POSIX shell 脚本，但反过来不行（我们将在 [shell 解释器](#shells)中讨论这一点）。

shell 文件的第一行是 [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) 语句，指向运行脚本的解释器。它们通常使用 `.sh` 扩展名，但如果针对特定解释器，也可以是 `.zsh`、`.csh` 等。

Zsh 脚本由 Zsh shell 解释器实现，Bash 脚本由 Bash 实现。但这些语言不像 [C++ 标准委员会](http://www.open-std.org/jtc1/sc22/wg21/)工作组或 [Python 的 PEP](https://www.python.org/dev/peps/) 那样受到严格的监管和标准化。Bash 和 Zsh 从 Korn 和 C Shell 的语言中汲取特性，但没有其他语言所推崇的那些繁文缛节。

## Shell 解释器（Shells） {#shells}

示例：POSIX sh、Bash、Zsh、csh、tcsh、ksh、fish

Shell 解释器*实现*了 shell 语言。它们是内核之上的一层，允许你交互式地在其中运行命令和应用程序。

截至 2016 年 10 月，[最新的 POSIX 规范](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/sh.html)以技术细节涵盖了 shell 的职责。

对于 shell 和操作系统：每个发行版或组织都有自己的做法。在大多数 Linux 发行版和 macOS 上，你通常会被直接放入 Bash。

在 FreeBSD 上，除非你在安装过程中另有指定，否则你可能默认使用普通的 `sh`。在 Ubuntu 中，`/bin/sh` 曾经是 `bash`（[Bourne Again Shell](https://en.wikipedia.org/wiki/Bourne_shell)），但[被替换为 `dash`](https://wiki.ubuntu.com/DashAsBinSh)（[Debian Almquist Shell](https://en.wikipedia.org/wiki/Almquist_shell)）。所以，在这里你可能会想"嗯，`/bin/sh`，可能就是一个普通的 POSIX shell"；然而，Ubuntu 上的系统启动脚本曾经允许通过 Bash 进行非 POSIX 脚本编写。这是因为特殊的 [shell 语言](#shell-languages)，如 Bash 和 Zsh，添加了有用的实用功能，但它们不可移植。例如，如果你依赖 Zsh 专用脚本，你需要在所有系统上安装 Zsh 解释器。如果你遵循 POSIX shell 脚本编写，你的脚本将具有最高级别的兼容性，但代价是更加冗长。

最新版本的 macOS 默认包含 Zsh。Linux 发行版通常需要你通过包管理器安装它，并安装到 `/usr/bin/zsh`。BSD 系统通过 port 系统构建它，FreeBSD 上使用 [`pkg(8)`](https://www.freebsd.org/cgi/man.cgi?query=pkg&apropos=0&sektion=0&manpath=FreeBSD+10.3-RELEASE+and+Ports&arch=default&format=html)，OpenBSD 上使用 [`pkg_add(1)`](http://man.openbsd.org/pkg_add.1)，它会安装到 `/usr/local/bin/zsh`。

尝试不同的 shell 很有趣。在许多系统上，你可以使用 [`chsh -s`](https://en.wikipedia.org/wiki/Chsh) 来更新用户的默认 shell。

另一件要提到的事是，要让 `chsh -s` 工作，你通常需要将其添加到 [`/etc/shells`](https://bash.cyberciti.biz/guide//etc/shells)。

## 小节

总结一下，你会经常听到人们在谈论 shell。上下文是关键。它可能是：

- 一种通用的方式来指代你打开的任何终端。"在你的 shell 中输入 `$ top` 看看会发生什么。"（按 q 退出。）
- 一个他们必须登录的服务器。在云时代之前，小型主机销售带有 root 访问权限的"C Shells"很流行。
- tmux [pane（窗格）](#panes)中的一个 shell。
- 如果提到脚本编写，那可能是脚本文件、与脚本行为相关的问题，或者关于 shell 语言的一些事情。

但总的来说，在这个概述之后，继续做你正在做的事情。如果人们说 shell 并且他们理解它，那就用它。你在这里学到的知识应该让你更加自信。如今，让我们的实战经验跟上理论知识是一场持续的战斗。

在下一章，我们将在深入了解 tmux 之前，接触一些终端基础知识。
