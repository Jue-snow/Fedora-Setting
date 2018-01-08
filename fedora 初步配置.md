# 安装 Fedora 后的配置

[TOC]

## 科学上网

### Shadowsocks

#### 安装

首先使用下面命令确认你的系统里有没有 Python 的相关组件：

```bash
$ python --version 
$ pip --version
```

如果没有的话需要安装相关的组件

```bash
$ sudo dnf install python-setuptools
```

安装 `Shadowsocks `

```bash
$ pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```

使用上面的命令可能会提示你权限不足，你需要在命令的前端加上 `sudo` 虽然这样并不是所建议的做法，但是是简单直接的做法。

> 所建议的方法是在 `pip install` 后加 `--user` 选项而不是使用 `sudo`。

```bash
$ pip install --user git+https://github.com/shadowsocks/shadowsocks.git@master
```

#### 配置

在某个目录下新建 Shadowsocks 相关的 json 配置文件，然后使用 `sslocal` 命令执行就可以了。

具体操作如下:

```bash
$ cd /etc/
$ sudo mkdir shadowsocks #在 etc 目录下新建 shadowsocks 文件夹
$ cd shadowsocks
$ sudo vim config.json #在 shadowsocks 下新建 config.json 配置文件
```

在 `config.json` 文件中写入以下内容

```json
{
    "server":"0.0.0.0",
    "server_port":443,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"password",
    "timeout":300,
    "method":"aes-256-gcm",
    "fast_open": true,
    "workers": 1
}
```

做完上面的操作后，使用 `sslocal` 命令查看 shadowsocks 是否开始工作。

如果想在后台运行，可以运行下面的命令：

```bash
$ nohup sslocal -c /etc/shadowsocks/config.json &
```

#### libsodium

libsodium 是一个解密模块。

```bash
$ sudo dnf install libsodium
```

#### 设置系统代理

在系统的 `Network` 中设置 `Network Proxy`，如果你使用的是 `shadowsocks` 的话，请使用 `Manual` 模式，并将 `Socks Host` 设为 `127.0.0.1`，端口设为 `1080`。

### Proxychains

```bash
$ sudo dnf install proxychains-ng
```

编辑 `/etc/proxychians.conf` 文件，将最后一行改为：

```ini
socks5 127.0.0.1 1080
```

### genpac

```bash
$ pip install genpac --user
```

```bash
genpac --format=pac --pac-proxy="SOCKS5 127.0.0.1:1080" -o /etc/genpac.txt
```

> 在系统的 `Network` 中设置 `Network Proxy`，使用 `Automatic` 模式，将 `URL` 改为 `file:///etc/genpac.txt`。
>
> 若是针对浏览器的代理，则需要在浏览器中的插件 (如 `ProxyOmega`) 中设置 `PAC` 地址为 `file:///etc/genpac.txt`。

## 添加更改软件源

### RPM Fusion

```bash
$ sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

### 添加 FZUG 源


```bash
sudo dnf config-manager --add-repo=http://repo.fdzh.org/FZUG/FZUG.repo
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

#### Docker

#### CLion

#### PyCharm

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

#### Tim

#### VLC media player

想在 linux 下爽快的看视频，那么骚年你需要这款播放器带你飞。下面是安装步骤：~~其实只要输入一行命令~~

```bash
$ sudo dnf install vlc
```



#### Steam



#### WPS

#### Sougou Pinyin

```bash
$ sudo dnf install sogoupinyin
```

#### ibus-rime

#### 网易云音乐

```bash
$ sudo dnf install musicbox
```

#### xsensors

xsensors 是可以显示 CPU 和 GPU 温度的实用小工具。可以通过 Fedora 自带的 Software 下载安装，也可以在 Terminal 中直接执行下面的命令:

```bash
$ sudo dnf install xsensors
```

#### Zeal

```bash
$ sudo dnf install zeal
```



### 网盘

#### Dropbox

[安装方法](https://blog.triplez.cn/index.php/2017/09/05/install-dropbox-on-fedora-26/)



## 开发环境

### 基础配置

`Vim`, `Git`, `gcc`, `cmake`, `gdb` etc.

```bash
$ sudo dnf install vim git gcc cmake gdb
```

## 驱动

### 显卡驱动

### 文件系统

#### NTFS

```bash
$ sudo dnf install ntfs-3g fuse fuse-libs
```

```bash
$ sudo ntfsfix /dev/sdc1
```

#### exFAT

```bash
$ sudo dnf install fuse-exfat
```

> Based on RPM Fusion repository.

## 界面设置

### GNOME

#### Tweak 的使用配置

Tweak 是 gnome 桌面的美化工具，通过 Tweak 可以让桌面变得多彩多样。可以去 https://www.gnome-look.org/browse/cat/135/  这个网站去下载一些很好看的主题和图标。去 https://extensions.gnome.org/ 这个网站下载很实用的插件。

##### 安装

```bash
$ sudo dnf install gnome-tweak-tool
```

##### Themes

| Name         | Theme                 |
| ------------ | --------------------- |
| Applications | OSX-Arc-White         |
| Cursor       | Breeze_Snow           |
| Icons        | MacOS                 |
| Shell        | Gnome-OSX-Light-Shell |

##### 实用的一些插件

- Dash to dock 
- Netspeed 显示网速的小插件
- Topicons plus 显示后台应用图标的小工具
- Docker integration
- Places status indicator
- User Themes
- OpenWeather
- Web search dialog


### Terminal

#### Zsh

## 安装脚本

[Fedora 27](https://github.com/Triple-Z/Fedora-Fast-Init/blob/master/Fedora27-init.sh)