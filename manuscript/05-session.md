# 会话（Sessions） {#sessions}

欢迎来到会话，这是驻留在 [server](04-server.md) 实例中的最高层级实体。服务器实例在启动新实例时 fork 到后台运行，并在重新附加会话时重新连接。你与 tmux 的交互*至少*有一个会话在运行。

一个会话包含多个窗口 [windows](06-window.md)。

![](images/info/session.png)

激活的窗口有一个 `*` 在标签 tab 上。

![第一个窗口，ID 1，标题为 "manuscript"，处于活动状态。第二个窗口，ID 2，标题为 zsh。](images/05-session/active-window.png)

## 创建会话（sessions）

最简单的创建会话的方法是直接打 `tmux`:

```
    $ tmux
```
 `$ tmux` 不带参数等价于 `$ tmux new-session` 命令。

默认情况下，session 名字是一个数字，没有什么描述。下面的命令更好一些：

```
    $ tmux new-session -s'my rails project'
```

## tmux 切换会话（sessions）

有些人养成脱离 tmux 客户端然后通过 `tmux att -t session_name` 重新附加的习惯。幸运的是，你可以从 tmux 内部切换会话！

| 快捷键            | 操作                                              |
|------------------|---------------------------------------------------|
| `Prefix` + `(`  | 将附加的客户端切换到上一个会话。                   |
| `Prefix` + `)`  | 将附加的客户端切换到下一个会话。                   |
| `Prefix` + `L`  | 将附加的客户端切换回上一个会话。                   |
| `Prefix` + `s`  | 交互式地为附加的客户端选择一个新会话。             |

`Prefix` + `s` 允许你在同一个 tmux 客户端中的会话之间切换。

这个命令名称可能容易混淆。`switch-client` 允许你在 [server](#server) 中的会话之间遍历。

示例用法：

```
    $ tmux switch-client -t dev
```
如果已经在客户端内部，这会切换到名为 "dev" 的会话（如果存在的话）。

## 重命名会话（Renaming sessions）

有时候，tmux 给出的默认会话名称不够描述性。只需几秒钟就可以更新它。

你可以随意命名它。通常，如果我在一个会话中处理多个 Web 项目，我会将其命名为 "web"。如果我将一个软件项目分配给单个会话，我会以软件项目命名它。你可能会形成自己的命名约定，但任何名称都比默认的更具描述性。

![将会话 '0' 重命名为 'react web'](images/05-session/rename.png)

如果你不命名你的会话，就很难跟踪会话包含什么。有时候，你可能会忘记你打开了一个项目，特别是如果你的机器已经运行了几天、几周或几个月。你可以通过重新附加会话来节省时间，避免创建重复的会话。

你可以通过 `Prefix` + `$` 在 tmux 内部重命名会话。状态栏将暂时变成一个文本字段，允许更改会话名称。

通过命令行，你可以尝试：

```shell
    $ tmux rename-session -t 1 "my session"
```

## 查找存在的会话（Finding sessions）

如果你在编写 tmux 脚本，你可能想知道会话是否存在。如果会话存在，`has-session` 将返回 0 [退出码](https://en.wikipedia.org/wiki/Exit_status)，但如果会话不存在，它将报告退出码 1 *并*打印错误。

```shell
    $ tmux has-session -t 1
```
它假设会话 "1" 存在；它只会返回 0 且没有输出。

但如果不存在，你会收到类似这样的响应：

```
    $ tmux has-session -t 1
    > can't find session 1
```
在 shell 脚本中尝试：

```
    if tmux has-session -t 0 ; then
        echo "has session 0"
    fi
```

## 小节

在本章中，你学会了如何为组织目的重命名会话，以及如何快速在它们之间切换。

当你在 tmux 中使用客户端时，你总是附加到一个会话。当最后一个会话关闭时，服务器也会关闭。

把会话想象成旨在帮助组织一组窗口的工作空间，类似于 GUI 计算中的[虚拟桌面](https://en.wikipedia.org/wiki/Virtual_desktop)空间。

在下一章中，我们将深入探讨 *windows*，它们像会话一样，也是可命名的，并允许你在它们之间切换。
