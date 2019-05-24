# 附录： 常见问题（troubleshooting） {#appendix-troubleshooting}

## 问题：在vim中粘贴时出现 `E353: Nothing in register *` 错误

macOS / OS X ，在 tmux 中使用 vim，尝试粘贴时，出现bug，可以尝试通过 [brew](http://brew.sh) 安装 [`reattach-to-user-namespace`](https://github.com/ChrisJohnsen/tmux-MacOSX-pasteboard) 解决问题。

```shell
    $ brew install reattach-to-user-namespace
```
## 问题：`tmuxp: command not found` 和 `powerline: command not found` {#troubleshoot-site-paths}

这个问题时你安装的python包并没有在你的python环境的site package里面。首先，找到你的user site packages base directory：

```shell
    $ python -m site --user-base
```
在 macOS系统上 ，这会返回 `/Users/me/Library/Python/2.7`  ，或者在 Linux/BSD上 会返回`/home/me/.local` 。

这些应用的包在 `bin/`，把他们加到你的 [`PATH`](https://en.wikipedia.org/wiki/PATH_(variable))里面。一般在 `~/.bashrc` 和 `~/.zshrc` 文件上配置。

```bash
    export PATH=/Users/me/Library/Python/2.7/bin:$PATH     # macOS w/ python 2.7
    export PATH=$HOME/.local/bin:$PATH                     # Linux/BSD
    export PATH="`python -m site --user-base`/bin":$PATH   # May work all-around
```
接着开一个新的 terminal，或者使能 `. ~/.zshrc` / `. ~/.bashrc` 在当前的 terminal。现在你可以运行`$ tmuxp -V`, `$ tmuxp load` 和 `$ powerline tmux right` 等命令了。
