


# tmux 初识{#thinking-tmux}

在计算机发展中，交互界面有两个王国：

1. 命令行界面交互
2. 图行界面交互

tmux，它存在与terminal应用中，可以将terminal分割成多个块。

![](images/info/server-with-laptop.png)

## terminal的窗口管理器

tmux 对于控制台来说，就像windows的desktop对于GUI应用。文字界面也有一个美丽的世界。使用tmux，你可以：

- 多任务处理，运行多个程序

- 同一个窗口拥有多个指令输入的行（行文中叫pane）

- 在一个工作会话(session)中可以有多个窗口(window)

- 就像虚拟windows桌面一样，可以在(session)中相互切换

  

  <p align="center"><em> 表: tmux 与 windows desktop的对应</em></p>

|**tmux**           |**"Desktop"-Speak**   |**Plain English**                  |
|-------------------|----------------------|-----------------------------------|
|Multiplexer        |Multi-tasking         |Multiple applications simultaneously.              |
| Session | Desktop                         |Applications are visible here                    |
| Window      | Virtual Desktop or applications | Desktop containing its own screen     |
| Pane        | Application                     | Performs operations                   |

就像图形的桌面一样，也可以在tmux的状态栏放一个日期/时间。

![](images/01-thinking-tmux/clocks.png)
<p align="center"><em> 图: 左上： KDE（ubuntu等） 右上：Windows 10. 中间：macOS Sierra. 下方： tmux 2.3 默认的 status bar。</em></p></p>

## 多任务处理

tmux 在一个窗口保持多个termianl。“tmux”缩写来自**T**erminal **Mu**ltiple**x**er。

除了一个屏幕上的多个终端之外，tmux还允许您创建和链接多个“窗口”附加在tmux会话上。

更好的是，你可以复制，粘贴和滚动。对图形也没有要求，所以你有完全的操控能力，即使你是通过SSH连接或在没有显示服务器的系统上工作，如 [X](https://en.wikipedia.org/wiki/X.org_server)。

下面有些常见的场景：

- 在pane a中运行“$tail-F/var/log/apache2/error.log”查阅正在改变的日志文件。
- 运行 file watcher，如 [watchman](https://github.com/facebook/watchman)，[gulp-watch](https://github.com/gulpjs/gulp/blob/master/docs/API.md#gulpwatchglob-opts-tasks),
  [grunt-watch](https://github.com/gruntjs/grunt-contrib-watch), [guard](https://github.com/guard/guard),
  或者 [entr](http://entrproject.org/)。监测文件更改，可设定后续命令：
  - rebuild LESS or SASS files, minimize CSS and/or assets and static files
  - lint with linters, like [cpplint](https://github.com/google/styleguide/tree/gh-pages/cpplint),
    [Cppcheck](http://cppcheck.sourceforge.net/), [rubocop](https://github.com/bbatsov/rubocop),
    [ESLint](http://eslint.org/), or [Flake8](http://flake8.pycqa.org/en/latest/)
  - rebuild with `make` or [`ninja`](https://ninja-build.org/)
  - reload  [Express](http://expressjs.com/) server
  - 运行一些自定义的命令
- 运行一个 text editor, 如 vim, emacs, pico, nano, 等等。运行一个主pane,
  用其他两个，一个 CLI commands ，另一个 用来使用 `make` 或者
  `ninja`命令进行编译。

![vim + building a C++ project w/ CMake + Ninja using entr to rebuild on file changes, LLDB bottom right](images/01-thinking-tmux/dev-watch.png)

使用 tmux，可以便利地做个IDE开发界面，快来试一试。

## 在后台运行程序

Sometimes, GUI applications will have an option to be sidelined to the system
tray to run in the background.  The application is out of sight, but events and
notifications can still come in, and the app can be instantly brought to the
foreground.

In tmux, a similar concept exists, where we can "detach" a tmux session.

Detaching can be especially useful on:

- Local machines. You start all your normal terminal applications within
  a tmux session, you restart X. Instead of losing your processes as you
  normally would if you were using an X terminal, like xterm or konsole, you'd
  be able to `tmux attach` after and find all the processes inside that were
  alive and kicking all along.
- Remote SSH applications and workspaces you run in tmux. You
  can detach your tmux workspace at work before you clock out, then the next
  morning, reattach your session. Ahhh. Refreshing. :)
- Those servers you rarely log into. Perhaps, a cloud instance you log into 9
  months later, and as a reflex, `tmux attach` to see if there is anything on
  there. And boom, you're back in a session you've forgotten about, but still
  jogs your memory to what you were tweaking or fixing. It's like a hack to
  restore your memory.

## Powerful combos

Chatting on [irssi](https://irssi.org/) or [weechat](https://weechat.org/),
one of the "classic combos", along with a [bitlbee](https://www.bitlbee.org)
server to manage AIM, MSN, Google Talk, Jabber, ICQ, even Twitter. Then, you can
detach your IRC and "idle" in your favorite channels, stay online on instant
messengers, and get back to your messages when you return.

![Chatting on weechat w/ tmux](images/01-thinking-tmux/weechat.png)

Some keep development services running in a session. Hearty emphasis on
*development*, you probably will want to daemonize and wrap your production web
applications, using a tool like [supervisor](http://supervisord.org/), with its own safe environmental settings.

让多个用户将其客户机附加到同一个会话，可以结对编程（pair programming）了。如果在打开同一个session，其他人会看到相同的东西，相同的输入，相同的激活的window和pane。

The above are just examples; any general workspace you'd normally use in a
terminal could work, especially projects or repetitive efforts you multitask
on. The *[tips and tricks](#tips-and-tricks)* section will dive into specific
flows you can use today.

```
Q> ### 问题：tmux sessions 在系统重启后还会自动运行嘛?
Q>
Q> 不行。重启会关闭tmux server 和tmux 上正在运行的各种程序。
Q>
Q> Thankfully, the modern server can stay online for a long time. Even for
Q> consumer laptops and PC's with a day or two uptime, having tmux persist
Q> tasks for organizational purposes is satisfactory to run it.
Q>
Q> It comes as a disappointment, because some are interested in being able to
Q> persist a tree of processes after restart. It goes out of the scope of what
Q> tmux is meant to do.
Q>
Q> For tasks you repeat often, you can always use a tool, like
Q> [tmuxp](https://github.com/tony/tmuxp), [tmuxinator](https://github.com/tmuxinator/tmuxinator),
Q> or [teamocil](https://github.com/remiprev/teamocil), to resume common
Q> sessions.
Q>
Q> Besides session managers, [tmux-resurrect](https://github.com/tmux-plugins/tmux-resurrect)
Q> attempts to preserve running programs, working directories, and
Q> so on within tmux. The benefit with tmux-resurrect is there's no JSON/YAML
Q> config needed.
```
## 小节

tmux是对终端工具带的一个丰富的补充。它有助于弥补多任务处理和工作空间组织之间的差距，因为没有图形用户界面会不太方便。此外，它还提供了一种将工作区放到后台，稍后可以重新连接（reattach）。

在下一小节，我们会接触一些 terminal 的基本操作，进一步深入 tmux。