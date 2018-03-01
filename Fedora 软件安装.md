# 安装 Fedora 后常用软件的安装和一些基础配置

[TOC]

## 添加更改软件源

### 添加 RPM Fusion

```bash
$ sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```


## 软件

### 开发软件

#### Sublime Text

Sublime Text 众所周知是一款很强的编辑器，全平台支持。那么如何在 Linux 下安装呢？请您~~紧张~~的往下看。

* 首先安装 GPG key

```bash
$ sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg
```

* 接着选择你想安装的版本

**Stable**

```bash
$ sudo dnf config-manager --add-repo https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo
```

**Dev**

```bash
sudo dnf config-manager --add-repo https://download.sublimetext.com/rpm/dev/x86_64/sublime-text.repo
```

* 最后开始安装

```bash
$ sudo dnf install sublime-text
```



####  Visual Studio Code

[官方 wiki 在此](https://code.visualstudio.com/docs/setup/linux)

##### 具体步骤

- 下载安装 VScode 的软件源

```bash
$ sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
$ sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
```

- 更新源然后安装 VScode/

```bash
$ sudo dnf check-update
$ sudo dnf install code
```


### 常用软件

#### Chrome

Chrome 浏览器不用多说肯定是要安装的，毕竟是市场份额第一的浏览器。

1. 下载 google-chrome.repo 并保存

```bash
$ sudo wget https://repo.fdzh.org/chrome/google-chrome-mirrors.repo -P /etc/yum.repos.d/  # Fedora/RHEL
```

2. 安装 Chrome

```bash
$ sudo dnf install google-chrome-{stable,beta,unstable}      (Fedora 22+) 
```
> 三个版本选一个就好～

#### Telegram Desktop

安装 Telegram 有两种方法：

第一种方法首先我们执行是在 Terminal 中输入：

```bash
$ sudo dnf copr enable rommon/telegram
$ sudo dnf install telegram-desktop
```

第二种是去官网下载安装包进行安装。下面是第二种方法的具体步骤：

1. 首先去 Telegram 的官网的[客户端下载界面]((https://desktop.telegram.org/)下载安装包
2. 下载完成后解压安装包

```bash
sudo tar xvf tsetup.1.1.23.tar.xz -C /opt/
```

3. 解压完成后在安装目录下执行 `./Telegram` 就可以看到 Telegram 的登陆界面了。


> 记得使用之前要更改软件的代理。

#### Mendeley Desktop

Mendeley 是一个跨平台的文献管理软件，其内部自带了一个可以添加注释的 PDF 阅读器。换言之，我们通过 Mendeley 来阅读 pdf 文件，而且还能同步我们的文件。

下载界面[在此](https://www.mendeley.com/download-desktop/)

下载链接：https://www.mendeley.com/autoupdates/installer/Linux-x64/stable-incoming

**具体步骤：**

```bash
$ sudo tar -xvf mendeleydesktop-1.17.12-linux-x86_64.tar.bz2 -C /opt/ #解压
$ cd /opt/mendeleydesktop-1.17.12-linux-x86_64/bin/ #进入 bin 目录

$ ./install-mendeley-link-handler.sh /opt/mendeleydesktop-1.17.12-linux-x86_64/bin/mendeleydesktop

```

**修改图标**

```bash
$ cp opt/mendeleydesktop/share/icons/hicolor/128x128/apps/mendeleydesktop.png ~/.local/share/icons/
```

#### Typora

##### 安装

很遗憾的是 Typora 目前并不支持在 Fedroa 上直接使用命令来安装，所以我们需要先下载安装包然后在进行解压安装。

[下载界面在此](http://support.typora.io/Typora-on-Linux/)

1. 解压

```bash
$ sudo tar zxvf Typora-linux-x64.tar.gz -C /opt/
```

1. 设置桌面图标

详细信息查看此[博客](https://blog.triplez.cn/index.php/2017/11/03/add-application-shortcut-in-fedora/)

##### 配置

安装完 Typora 你会发现默认打开 .md 文件的并不是 Typora，所以我们需要设置一下使 Typora 成为默认打开 .md 文件的软件。

1. 首先打开位于 `/usr/share/applications/` 下的 `mimeapps.list` 文件

```bash
$ cd /usr/share/applications/
$ sudo vim  mimeapps.list
```

1. 然后在 `mimeapps.list` 文件中的 `[Default Application]` 下加入以下内容：

```
text/markdown=typora.desktop
```

做完之后我们双击 .md 文件就会看到熟悉的 Typora 界面了。

*注：虽然弄好了使用 Typora 打开 md 文件的问题，但是还是没有解决掉如何在 linux 下在打开方式下添加  Typora 的问题*

#### electronic-wechat

electronic-wechat 是一个基于 electron 平台的微信客户端。下载安装 electronic-wechat 就可以在 liunx 上使用微信了。可以通过 npm 来进行安装，也可以通过解压 tar.gz 压缩包来安装。[下载界面在此](https://github.com/geeeeeeeeek/electronic-wechat/releases)

下面是通过解压的方式来进行

```bash
$ sudo tar -zxvf linux-x64.tar.gz -C /opt/
$ cd /opt/electronic-wechat-linux-x64/ #进入目录
$ ./electronic-wechat #运行 wechat
```

虽然我们成功安装了 wechat，但是通过进入目录来运行比较麻烦。通过新建一个新的 .desktop 文件可以设置 wechat 的桌面图标。

进入 `/usr/share/applications/` 目录，使用 `sudo vim electronic-wechat.desktop` 新建 wechat 的图标文件。另外下载一张微信的图标图片放在 `/opt/electronic-wechat-linux-x64/resources` 下。然后在 `electrionic-wechat.desktop`  文件中写入下面内容

```ini
[Desktop Entry]
Name=Electronic Wechat
Name[zh_CN]=微信电脑版
Name[zh_TW]=微信电脑版
Exec=/opt/electronic-wechat-linux-x64/electronic-wechat
Icon=/opt/electronic-wechat-linux-x64/resources/icon.png
Terminal=false
X-MultipleArgs=false
Type=Application
Encoding=UTF-8
Categories=Application;Utility;Network;InstantMessaging;
StartupNotify=false
```

#### VLC media player

想在 linux 下爽快的看视频，那么骚年你需要这款播放器带你飞。下面是安装步骤：~~其实只要输入一行命令~~

```bash
$ sudo dnf install vlc
```

#### Steam
添加 RPM Fusion 源后，我们就可以在 Terminal 中输入下面的命令来安装 Steam 了。
```bash
$ sudo dnf install steam
```


#### xsensors

xsensors 是可以显示 CPU 和 GPU 温度的实用小工具。可以通过 Fedora 自带的 Software 下载安装，也可以在 Terminal 中直接执行下面的命令:

```bash
$ sudo dnf install xsensors
```

#### Zeal

Zeal 是一个离线文档查看软件，通过下面的命令就可以安装。

```bash
$ sudo dnf install zeal
```

### 基础配置

`Vim`, `Git`, `gcc`, `cmake`, `gdb` etc.

```bash
$ sudo dnf install vim git gcc cmake gdb
```

