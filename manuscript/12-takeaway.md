# 总结或最后（Takeaway） {#takeaway}

在这本书中，我们采用了一种有组织的方法来理解tmux。随着越来越多地使用tmux，请继续返回并使用此资源帮助来记忆。不必一次就理解tmux的复杂性，更不用说终端了。适应是随着时间的推移会发生的。

tmux's userbase varies in skill level. Some readers of this book may have just
learned how to use the `Prefix` key yesterday. Others are looking to tweak their
configurations and host it in their "dot files" on github. There also exists a
very clever hacker who utilizes the advanced scripting capabilities tmux
offers to pilot the terminal in ways previously thought impossible.

We've covered the [server](#server), [session](#sessions), [window](#windows),
and [pane](#panes) concepts. Panes are shells, AKA pseudoterminals or
PTYs. The command system. That [configuration](#config) is basically a file
filled with commands. An overview of the [target](#targets) system lets
you specify objects to interact with tmux commands. A breeze through [formats](#formats),
a template system with variables to retrieve information on tmux's current
state. How to [send keystrokes](#send-keys) and [copy from tmux panes](#capture-pane)
programmatically. A lot of [terminal tricks](#tips-and-tricks) that work across
platforms and well with tmux, including a [file watching workflow](#file-watching)
to run linting, testing, and build commands on file changes. [Two permissively licensed open source projects](#example-projects)
for demonstration. A [tmux configuration](https://www.github.com/tony/tmux-config)
you can copy and paste from. An object oriented [tmux API wrapper](https://libtmux.git-pull.com)
and a [tmux session manager](https://tmuxp.git-pull.com).

If you liked this book, please leave a review on [Amazon](http://amzn.to/2gPfRhC) and
[Goodreads](https://www.goodreads.com/book/show/33246223-the-tao-of-tmux). I
would also appreciate you leaving something in my [tip jar](https://www.git-pull.com/support.html).
I am an independent software developer and could use all the help I can get.

If you found an error or have a suggestion, please contact me at
`tao.of.tmux@git-pull.com`. I want this book to be the best it can be.
If you are having technical difficulties with Kindle, please send me your
receipt and I will comp you a leanpub coupon.