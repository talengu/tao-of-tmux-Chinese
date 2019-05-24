# 附录： 安装 tmux （installation） {#appendix-installation}

## macOS / OS X

### brew

```shell
    $ brew install tmux
```
### macports

```shell
    $ sudo port install tmux
```
### fink

```shell
    $ fink install tmux
```
## Linux

### Ubuntu / Mint / Debian, etc.

```shell
    $ sudo apt-get install tmux
```
### CentOS / Fedora / Redhat, etc.

```shell
    $ sudo yum install tmux
```
### Arch Linux (pacman)

```shell
    $ sudo pacman -S tmux 
```
### Gentoo (portage)

```shell
    $ sudo emerge --ask app-misc/tmux
```
## BSD

### FreeBSD

#### pkg(1)

```shell
    # pkg install tmux
```
#### pkg_add(1)

```shell
    # pkg_add -r tmux
```
### OpenBSD

在 OpenBSD 4.6, [包含了 tmux ](https://www.openbsd.org/46.html).

使用以前的系统版本的话：

```shell
    # pkg_add tmux
```
### NetBSD

```shell
    $ make -C /usr/pkgsrc/misc/tmux install
```
## Windows 10

查看 [附录中 tmux 在 Windows 10 使用 小节](99-windows-bash.md).

