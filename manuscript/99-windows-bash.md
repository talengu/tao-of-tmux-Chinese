# 附录： tmux 在 Windows 10 使用{#appendix-windows-bash}

要在windows 系统上安装tmux，可以使用MSYS2，或者安装windows的子系统 Linux。

## 通过 MSYS2

下载安装  [MSYS2](http://www.msys2.org/)

```bash
pacman  -S tmux
```

![1563281005928](images/99-windows-bash/1563281005928.png)

![1563281982394](99-windows-bash/1563281982394.png)



## Window 的 Linux 子系统

从 Windows 10 build 14361开始，可以通过 Window 的 Linux 子系统 [运行 tmux](https://blogs.msdn.microsoft.com/commandline/2016/06/08/tmux-support-arrives-for-bash-on-ubuntu-on-windows/) 。

在 设置的 “Update & security”下的 "For Developers"，使能 **Developer mode** 选项。之后，打开 "Windows Features"。你可以在开始搜索 "Turn Windows features on or off"，然后打开 "Windows Subsystem for Linux (Beta)" 功能，需要重启电脑。

接下来打开 cmd窗口(Run cli.exe)，运行：

```bash
C:\Users\tony> bash.exe
```

可能需要同意一些条款，创建一个用户。在作者的电脑上，tmux已经安装好了。如果没有安装可以使用`sudo apt-get install tmux`安装。



![Find Turn Windows Features on or off](images/99-windows-bash/01-turn-features-onoff.jpg)

![Check Windows Subsystem for Linux (Beta)](images/99-windows-bash/02-turn-features-onoff-check.jpg)

![Windows completed the requested changes. Restart](images/99-windows-bash/03-turn-features-restart.jpg)

![Use Developer features](images/99-windows-bash/04-developer-mode.jpg)

![Select Developer mode in Update & Security](images/99-windows-bash/05-developer-mode-check.jpg)

![Installing Ubuntu from Windows Store](images/99-windows-bash/06-install-ubuntu.jpg)

![Create Linux user](images/99-windows-bash/07-create-user.jpg)

![In bash!](images/99-windows-bash/08-bash.jpg)

```shell
yourusername@COMPUTERNAME-ID321FJ:/mnt/c/Users/username$ tmux
```

![In tmux!](images/99-windows-bash/09-tmux.jpg)

这下你可以在`bash.exe`里面运行tmux了。

在这个ubuntu系统里面，你可以通过 `sudo apt-get install **packagename**` 继续安装其他软件和`sudo apt-get update && sudo apt-get upgrade`更新软件。

