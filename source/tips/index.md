---
title: Tips
date: 2017-09-12 12:35:32
---

# macOS食用指南

### 抹盘重装
* 需要 Install macOS High Sierra.app、8GB至少的U盘。
* 格式化U盘为 Mac OS 拓展（日志式）GUID分区表
* 终端执行

```
sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/U盘labelname --applicationpath /Applications/Install\ macOS\ High\ Sierra.app --nointeraction
```

* 插着U盘，按住 option 键启动 Mac，选择 USB 启动盘
* 你可以直接覆盖安装系统(升级)，也可以在磁盘工具里面格式化抹掉整个硬盘，实现全新的干净的安装。

### 科学上网 
* [ShadowsocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG/releases/latest) 配合 [Proxy SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif) 食用更佳

### 终端代理
* 首先配置一下难看的Terminal

下载 [透明终端描述文件](透明.terminal) 双击打开，再去Terminal的偏好设置->描述文件中将其设为默认，然后重启Terminal

* 然后从ShadowsocksX-NG的状态栏纸飞机那里点击Copy HTTP Proxy Shell Export Line，然后去终端里粘贴执行。
更方便的方法是

```
vi ~/.bash_profile
```

```
//这三行是Android开发的sdk配置，请忽略
export ANDROID_HOME=~/Library/Android/sdk/
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/platform-tools
//这两行才是终端代理的快捷配置
alias proxy-on='export http_proxy=127.0.0.1:1087;export https_proxy=$http_proxy'
alias proxy-off='unset http_proxy;unset https_proxy'
```

保存之后，再执行 

```
source ~/.bash_profile
```

然后你就可以在通过 proxy-on 和 proxy-off 来控制终端代理了。

### 安装 Xcode Command Line Tools
* 终端执行

```
xcode-select --install
```

### 配置远程服务器SSH密钥认证自动登录
* 终端执行

```
ssh-keygen -t rsa -C  'youremail@domain.com'
scp ~/.ssh/id_rsa.pub username@hostname:~/ #将公钥文件复制至ssh服务器
ssh username@hostname #使用用户名和密码方式登录至ssh服务器
mkdir .ssh  #若.ssh目录已存在，可省略此步
cat id_rsa.pub >> .ssh/authorized_keys #将公钥文件id_rsa.pub文件内容追加到authorized_keys文件
vi ~/.ssh/config #若没有该文件，直接新建即可
//添加文件内容格式如下：
Host            alias #自定义别名
HostName        hostname #替换为你的ssh服务器ip或domain
Port            port #ssh服务器端口，默认为22
User            user #ssh服务器用户名
IdentityFile    ~/.ssh/id_rsa #第一个步骤生成的公钥文件对应的私钥文件
```

注意把每行注释删掉，保存之后就可以通过 ssh alias #alias是你在~/.ssh/config文件配置的别名 登录ssh服务器

### 配置git
* 终端执行

```
git config --global user.name "yourname"
git config --global user.email  "youremail@domain.com"
cat ~/.ssh/id_rsa.pub
若不存在，则通过 ssh-keygen -t rsa -C  'youremail@domain.com' 生成 ~/.ssh/id_rsa.pub
把id_rsa.pub文件里的内容复制到GitHub
```

### 安装Homebrew
* 终端执行

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" #有条件的开一下终端代理会更快
brew install wget #顺带安装个wget
```

### 增强文件快速预览
* 终端执行

```
brew cask install qlcolorcode qlstephen qlmarkdown quicklook-json qlprettypatch quicklook-csv betterzipql qlimagesize webpquicklook suspicious-package quicklookase qlvideo
```

### 安装MPV播放器
* 终端执行

```
brew install mpv --with-bundle
brew linkapps mpv
vi ~/.config/mpv/mpv.conf

#作者：YANG Cage
#链接：https://www.zhihu.com/question/19552878/answer/49884947
#来源：知乎

#for intel HD4000 above
vo=opengl-hq:icc-profile-auto
#osd message, you can press o to display the osd message
osd-status-msg="${time-pos/full} / ${length/full} (${percent-pos}%)"
#makes the player window stay on top of other windows
ontop=yes
#always save the current playback position on quit
save-position-on-quit=yes
#adjust the initial window size to 50%
geometry=50%
#for network play
cache=8192
#choose the default subtitle to chinese
slang=zh,chi
#for GB2312 GBK BIG5 charset, use enca convert them to utf8
sub-codepage=enca:zh:utf8
```

###hexo博客
* Node.js

```
curl https://raw.github.com/creationix/nvm/master/install.sh | sh
或者
brew install node
```

* hexo

```
#先解决国内npm缓慢
npm config set registry http://registry.npm.taobao.org
npm install -g hexo-cli
```

### 信任“任何来源”
* 终端执行 

```
sudo spctl --master-disable
//注：如果在系统偏好设置的“安全与隐私”中重新选中允许App Store 和被认可的开发者App，允许“任何来源”App的选项会再次消失，可运行上述命令再次打开“任何来源”。
```

### 重置launchpad默认布局
* 终端执行

```
defaults write com.apple.dock ResetLaunchPad -bool true && killall Dock
```

### 开启macOS原生读写NTFS分区功能
* 下载 [macOS-ntfs](macOS-ntfs.zip) 并解压

cd to Script Directory, then:

```
sudo cp ntfsmount.sh ntfsfix /usr/bin
sudo cp libntfs.9.dylib /usr/local/lib/libntfs.9.dylib
之后每次使用
sudo ntfsmount.sh labelname #挂载对应分区名
```


### 下载工具
* [Aria2Gui](https://github.com/yangshun1029/aria2gui/releases/latest) 配合[YAAW-for-Chrome](https://chrome.google.com/webstore/detail/yaaw-for-chrome/dennnbdlpgjgbcjfgaohdahloollfgoc) 或者 [BaiduExporter](https://github.com/acgotaku/BaiduExporter) 食用更佳

### 常用软件推荐

| 名称 | 简介 |
| :-- | :-- |
| [1Password](https://1password.com/) | 用过就离不开的密码管理软件 |
| [Aerial](https://github.com/JohnCoates/Aerial/releases/latest) | Apple TV 中提取出来的屏保 |   
| [Amphetamine](https://itunes.apple.com/cn/app/amphetamine/id937984704?mt=12) | Mac防休眠工具 |
| [AppCleaner](http://freemacsoft.net/appcleaner/) | 强力卸载工具 |
| [Backgrounds](https://itunes.apple.com/cn/app/backgrounds/id808501572?mt=12) | Mac上的动态壁纸 |
| [coconutBattery](http://www.coconut-flavour.com/coconutbattery/) | 检测Mac和iOS电池 |
| [Dash](http://kapeli.com/dash) | 程序员必备 |
| [f.lux](https://justgetflux.com/) | 自动调整色温护眼 |
| [iHash](https://itunes.apple.com/cn/app/ihash/id763902043?mt=12) | 文件校验工具 |
| [Mactracker](https://itunes.apple.com/cn/app/mactracker/id430255202?mt=12) | Apple公司产品年鉴 |
| [MWeb](http://zh.mweb.im/) | 最好的技术文章编辑器 |
| [Office](https://www.office.com/) | 办公必备 |
| [Sublime Text 3](https://www.sublimetext.com/3) | 程序员必备纯文本编辑器 |
| [The Unarchiver](https://itunes.apple.com/cn/app/the-unarchiver/id425424353?mt=12) | 解压工具 |
| [Typera](https://www.typora.io/) | 跨平台MarkDown编辑器 |
| [迅雷](http://mac.xunlei.com/) | 会员加速很厉害 |

### Sublime Text 3 build 3143 License Key

```
—– BEGIN LICENSE —–
TwitterInc
200 User License
EA7E-890007
1D77F72E 390CDD93 4DCBA022 FAF60790
61AA12C0 A37081C5 D0316412 4584D136
94D7F7D4 95BC8C1C 527DA828 560BB037
D1EDDD8C AE7B379F 50C9D69D B35179EF
2FE898C4 8E4277A8 555CE714 E1FB0E43
D5D52613 C3D12E98 BC49967F 7652EED2
9D2D2E61 67610860 6D338B72 5CF95C69
E36B85CC 84991F19 7575D828 470A92AB
—— END LICENSE ——
```


