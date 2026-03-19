# 面板（Panes） {#panes}

Panes 是[伪终端](https://en.wikipedia.org/wiki/Pseudoterminal)，封装了 shell（例如 Bash、Zsh）。它们位于 [window](06-window.md) 内。
一个终端中的终端，它们可以运行 shell 命令、脚本和程序，如 vim、emacs、top、htop、irssi、weechat 等。

![](images/info/pane.png)

## 创建新面板（Creating new panes）

要创建新窗格，你可以从当前所在的 [window](#windows) 和 pane 中执行 `split-window`。

| 快捷键            | 操作                                              |
|------------------|---------------------------------------------------|
| `Prefix` + `%`  | `split-window -h`（水平分割）                     |
| `Prefix` + `"`  | `split-window -v`（垂直分割）                     |

你可以继续创建窗格，直到达到终端能容纳的极限。这取决于你终端的尺寸。一个普通窗口通常会有 1 到 5 个窗格打开。

示例用法：

```shell
# 水平创建窗格，$HOME 目录，当前窗格 50% 宽度
$ tmux split-window -h -c $HOME -p 50 vim
```

![](images/07-pane/splitw/-h -c $HOME -p 50 vim - 2 panes.png)

```shell
# 创建新窗格，垂直分割，高度 75%
tmux split-window -p 75
```

![](images/07-pane/splitw/-p 75.png)

## 面板间切换（Traversing Panes） {#pane-traversal}

| 快捷键            | 操作                                              |
|------------------|---------------------------------------------------|
| `Prefix` + `;`  | 移动到之前活动的窗格。                             |
| `Prefix` + `Up` / | 切换到当前窗格上方、下方、                       |
| `Down` / `Left` / | 左侧或右侧的窗格。                               |
| `Right`          |                                                   |
| `Prefix` + `o`  | 选择当前窗口中的下一个窗格。                       |

> *使用 vim 风格移动*
>
> 如果你喜欢 vim（hjkl）键位绑定，在你的[配置](#config)中添加这些：
>
> ```
>     # hjkl 窗格遍历
>     bind h select-pane -L
>     bind j select-pane -D
>     bind k select-pane -U
>     bind l select-pane -R
> ```

## 面板最小化（Zoom in）{#zoom-pane}

要放大窗格，导航到它并按 `Prefix` + `z`。

你可以再次按 `Prefix` + `z` 取消放大。

此外，你可以使用[窗格遍历](#pane-traversal)键同时取消放大并移动到相邻窗格。

在幕后，键绑定是 `$ tmux resize-pane -Z` 的快捷方式。所以，如果你想编写 tmux 脚本来放大/缩小窗格或将此功能应用到自定义键绑定，你也可以这样做，例如：

```
    bind-key -T prefix y resize-pane -Z
```
这会让 `Prefix` + `y` 放大和缩小窗格。

## 面板大小调整（Resizing panes） {#resizing-panes}

窗格大小可以通过 [window layouts](#window-layouts) 和 `resize-pane` 在 [windows](#windows) 中调整。调整窗口布局会切换窗格的比例和顺序。调整窗格大小会针对包含它的窗口中的特定窗格，也会缩小或增大其他列或行的大小。这就像调整你的汽车座椅或在飞行中斜躺；如果你占用更多空间，其他东西就会有更少的空间。

| 快捷键            | 操作              |
|------------------|-------------------|
| `Prefix M-Up`   | `resize-pane -U 5`|
| `Prefix M-Down` | `resize-pane -D 5`|
| `Prefix M-Left` | `resize-pane -L 5`|
| `Prefix M-Right`| `resize-pane -R 5`|
| `Prefix C-Up`   | `resize-pane -U`  |
| `Prefix C-Down` | `resize-pane -D`  |
| `Prefix C-Left` | `resize-pane -L`  |
| `Prefix C-Right`| `resize-pane -R`  |

## 输出面板到文件（Outputting pane to a file）

你可以将窗格的显示输出到文件。

```
    $ tmux pipe-pane -o 'cat >>~/output.#I-#P'
```
`#I` 和 `#P` 是窗口索引和窗格索引的[格式](#formats)，所以创建的文件是唯一的。聪明！

## 小节（Summary）

窗格是 shell 中的 shell。你可以不断向 tmux 窗口添加窗格，直到屏幕上没有空间为止。在你的 shell 中，你可以 `tail -F` 日志文件、编写和运行脚本，以及运行 [curses](https://en.wikipedia.org/wiki/Curses_(programming_library)) 驱动的应用程序，如 vim、top、htop、ncmpcpp、irssi、weechat、mutt 等。

你总是至少有一个窗格打开。一旦你杀死窗口中的最后一个窗格，窗口就会关闭。窗格也可以调整大小；你可以通过针对特定窗格并更改窗口布局来调整窗格大小。

在下一章中，我们将介绍自定义 tmux 快捷键、状态栏和行为的方法。
