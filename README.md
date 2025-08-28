# problem-solving
A collection of some problems and their corresponding solutions

## Mac 因为音频出现 Bug 无法播放，导致视频无法播放和蓝牙无法连接的问题

解决音频和视频无法播放：

```bash
# 使用这个命令行重启
sudo launchctl stop com.apple.audio.coreaudiod && sudo launchctl start com.apple.audio.coreaudiod

# 或者杀掉后台后让其自动重启
sudo killall coreaudiod
```

解决蓝牙连接问题：

1. 打开「活动监视器（Activity Monitor）」
2. 搜索 `bluetooth`
3. 找到并强制退出 **bluetoothaudiod** 
4. 重新连接蓝牙设备

## IDEA 安装新插件启动失败

问题描述：在安装 NR Coding Style 插件后，因为插件太老了，导致 Mac 上的 IntelliJ Idea 2020.3 版本启动失败

解决方法：

1. 进入插件安装目录

```bash
# {用户名} 需要替换为自己的用户名
cd /Users/{用户名}/Library/Application Support/JetBrains/IntelliJIdea2020.3/plugins
```

2. 找到并删除该插件

## 命令行代理

使用代理服务的时候，如果让命令行也通过代理来连接网络，可以这样设置：

使用 `vim ~/.bashrc` 或 `vim ~/.bash_profile` 去编辑 Bash 的配置。在该配置中加入：

```
my_proxy_url=http://127.0.0.1:1185
alias proxyon='export http_proxy=$my_proxy_url; export https_proxy=$my_proxy_url'
alias proxyoff='unset http_proxy;unset https_proxy'
```

如果使用了 zsh，就 `vim ~/.zshrc` 打开其配置。然后加入一行 `source ~/.bash_profile` 即可。

最后 `source ~/.zshrc` / `source ~/.bash_profile` 让配置生效。

使用的时候，输入 `proxyon` 打开命令行，输入 `proxyoff` 关闭命令行代理。

## Mac 中，使用 ClashX 实现命令行代理

直接在 `.zshrc` 或者 `.bash_profile` 中添加以下语句：

```
function proxy_on() {
    export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
    export http_proxy="http://127.0.0.1:7890"
    export https_proxy=$http_proxy
    #export all_proxy=socks5://127.0.0.1:7890 # or this line
    echo -e "已开启代理"
}

function proxy_off(){
    unset http_proxy
    unset https_proxy
    echo -e "已关闭代理"
}
```

然后使用的时候，在命令行输入 `proxy_on`。关闭的时候，输入 `proxy_off`。

解决方案来自[利用ClashX进行MAC（macOS Catalina）终端代理设置](https://blog.csdn.net/DSZhappy/article/details/108393159)

## 想在 Shell 中使用 vi 模式

在 Bash 下使用和 VIM 一样的操作模式，可以在命令行中输入 `set -o vi`。

也可以直接将 `set -o vi` 加入到 `~/.bashrc` 或 `~/.bash_profile` 配置文件中。

## IDEA 中的 IdeaVim 插件，如何实现自动切换输入法

解决方案：[IdeaVim扩展](https://github.com/hadix-lin/ideavim_extension)

首先，在 IDEA 中获取 IdeaVimExtension 插件并安装。然后，在命令行中 `vim ~/.ideavimrc`，添加 `set keep-english-in-normal-and-restore-in-insert`。重启 IDEA，即可。

## 在 Spring Boot 的 yaml (.yml) 文件中，使用冒号

解决方案来自 [How to escape indicator characters (i.e. : or - ) in YAML](https://stackoverflow.com/questions/11301650/how-to-escape-indicator-characters-i-e-or-in-yaml) 中 [Andy Brown 的回答](https://stackoverflow.com/a/52757905)：

>If you're using @ConfigurationProperties with Spring Boot 2 to inject maps with keys that contain colons then you need an additional level of escaping using square brackets inside the quotes because spring only allows alphanumeric and '-' characters, stripping out the rest. Your new key would look like this:

```yaml
"[8.11.32.120:8000]": GoogleMapsKeyforThisDomain
```

## 命令行无法连接 GitHub

当命令行出现：`ssh: connect to host github.com port 22: Connection timed out` 时，可以采取如下办法解决。

解决方案：GitHub 允许使用 443 端口进行 ssh 连接

1. 首先运行下面的命令，然后根据提示查看是否可以成功：

```bash
ssh -T -p 443 git@ssh.github.com
```

2. 使用 `vim ~/.ssh/config` 添加下面的配置：

```
Host github.com
Hostname ssh.github.com
Port 443
User # 用户名，可以使用 git config -l，然后查看 user.name 中填写的用户名
```

参考资料：[解决 ssh: connect to host github.com port 22: Connection timed out](https://segmentfault.com/a/1190000040896781)

## Windows 上无法在命令行使用 node 等命令

在环境变量都设置成功的情况下，如果无法运行一些程序，可能是权限不足。

解决方案：使用“管理员”来运行命令行工具

## Docker 无法在 CentOS 上使用

解决方案：使用 CentOS 7 的版本，并且不要使用 yum 来安装。

## 在 iPhone / iOS 设备拍摄的 HEIC 格式照片转换为 jpg 格式的图片

1. 在 Mac 上选择（可以批量选择） HEIC 格式的照片
2. 点击右键，在菜单中选择【快速操作】->【转换图像】
3. 点击【转换为 JPEG】选项

## 解决多台 Mac 设备之间无法使用 Universal Control 的问题

在【通用】->【共享】中，打开“屏幕共享”和“文件共享”的权限即可。

## 在微信上设置 Apple Watch 的快捷回复 / 回复模板

打开微信，点击【我】->【设置】->【设备】->【Watch 微信】->【回复模板】

## 网页长截图

按 F12 调出调试工具，使用快捷键 Command(Ctrl) + Shift + p 进入命令模式，然后输入 Capture full size screenshot 即可。

## 在 Chrome 上设置地址栏的快捷键，用于快速打开指定网站

进入【设置】，点击【搜索引擎】，点击【管理搜索引擎和网站搜索】。

找到标题为【网站搜索】标题部分，其正文部分写有“若要在某一特定网站或 Chrome 的某个部分中进行搜索，请先在地址栏中输入相应的快捷字词，然后按您的首选键盘快捷键。”。

在该部分中，点击【添加】按钮，会弹出标题为【添加搜索引擎】的小窗。在【搜索引擎】那栏，填写自己命名的网页标题。在【快捷字词】那栏，填写需要的快捷键。在【网址格式】那栏，填写需要打开的网址。最后点击【添加】按钮即可。

## IDEA 快速在 Finder 或 File Explorer 中打开文件

按住 command 或 ctrl 按键，点击 IDEA 的文件标签页（Tab），就可以弹出一个文件列表，点击文件列表中的文件，就可以在 Finder 或 File Explorer 中打开该文件。

## 在 IDEA 中编写 MyBatis 的 Mapper XML 文件时，连接 database 实现 SQL 提示

在 IDEA 的设置中，点击 Languages & Frameworks -> SQL Resolution Scopes，然后在 Project mapping 中，取消全选，最后选择当前连接的数据库。

如果在 Mapper XML 文件中还是没有关联成功，随便输入一个 `ON DUPLICATE KEY UPDATE` 之类的特殊语法，然后 IDEA 会提示切换当前文件的 SQL dialect，切换为需要的“方言”（比如 MySQL）即可。

## Homebrew 出现错误 Error: unknown or unsupported macOS version: :dunno

场景：在 Homebrew 使用 `brew uninstall --cask [app_name]` 命令的时候，返回了一个 `Error: unknown or unsupported macOS version: :dunno` 的错误信息

在 [devv.ai](https://devv.ai/) 中搜索了一下，叫我执行 `brew update-reset` 命令，我试了一下，又返回一个信息 `Do not report this issue until you've run brew update and tried again.` 。

最终，在我运行了 `brew update` 后，再次使用 `brew uninstall --cask [app_name]` 命令就成功卸载了该软件。

## Mac 通过 BootCamp 安装 Windows 10 后设置触控板滚动/滑动方向

[解决方案](https://blog.csdn.net/wjf_hb/article/details/122698482) ：

- 在 Win10 控制面版找到鼠标
- 在鼠标属性窗口选择【硬件】标签
- 通过 cmd 命令 `regedit` 打开注册表
- 在 `计算机\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\HID\VID_05AC&PID_027E&MI_02&Col01\8&31def279&0&0000\Device Parameters` 找到 `FlipFlopWheel` 将数据由 0 设置为 1
- 重启 Win10

![](https://img-blog.csdnimg.cn/54b4e222c8ce4ba6a9d48c993677ff8b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YWD6aOe,size_20,color_FFFFFF,t_70,g_se,x_16)


## 消除 Mac 上的未读信息 / 未读短信

打开【信息 App】，点击上方状态栏/菜单栏上的【显示】，点击【未读信息】，选中第一条后，按住 shift 按键，然后划到最后一条，最后鼠标右键，点击【标记为已读】。

或者，直接使用 Siri，让 Siri 帮忙“标记所有信息为已读”或“阅读所有未读短信”。
## Obsidian 的换行是普通的换行，不是 Typora 的那种两个换行的换行

[解决方案](https://forum-zh.obsidian.md/t/topic/9242) :

1. 在 Obsidian 的【设置】->【第三方插件】->【社区插件市场】中找到 `Easy Typing` 插件并安装
2. 在【已安装插件】中找到 `Easy Typing` ，点击【选项】进入设置选项页面，滑到最下面，打开【Strict Line breaks Mode Enter Twice】
3. 回到 Obsidian 的设置页面，找到【编辑器】选项，在该选项中，打开【严格换行】

这样就可以在 Obsidian 中实现一个回车键输入双换行。

## Windows 11 安装 WSL / Ubuntu 报错

通过命令行或 Microsoft Store 安装 Ubuntu 后，会报错：

```
Installing, this may take a few minutes... WslRegisterDistribution failed with error: 0x800701bc
```
解决方案：

参考 [知乎这篇文章 - Windows 11 安装 WSL2](https://zhuanlan.zhihu.com/p/475462241) 底下的回复，跳转到 [WSL安装无法打开（WslRegisterDistribution failed with error: 0x800701bc......）](https://blog.csdn.net/qq_40846862/article/details/119609971) 这篇文章，然后点击文章里面的链接下载 [WSL 2 的最新内核](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi) ，安装后就解决了。

## Mac 使用 Ivanti Secure Access Client 作为 VPN 连接公司内网，且设置了外部代理后，无法访问除了内网外的其他网站

Ivanti Secure Access Client 作为 VPN 连接公司内网后，需要设置系统网络代理，转发需要访问外部的流量请求，来实现访问外部网络的功能。但是明明在 Windows 电脑上成功设置代理，在 Mac 电脑上却不行。

解决方案：

问了 Kimi 后，给了一个答案，修改后如下所示：

macOS 上直接用 [Clash Verge Rev](https://github.com/clash-verge-rev/clash-verge-rev) 强行接管全部流量（含 VPN 内网流量），从而把「外网」全部甩给代理 10.168.60.41:8080 的完整步骤。
按顺序做，基本 3 分钟搞定。
1️⃣ 装内核 + 授权：就是找到系统的 TUN 模式开关，根据提示需要安装内核
2️⃣ 写配置文件
把下面内容保存为 ~/Documents/corp.yml（文件名随意），只改两处即可：

```yaml
mixed-port: 7890           # 本地监听端口，默认 7890
allow-lan: false
mode: rule
log-level: info
external-controller: 127.0.0.1:9090

# 1. 把你公司给的 HTTP 代理写成一个「节点」
proxies:
  - name: "Corp-HTTP"
    type: http
    server: 10.168.60.41
    port: 8080
    username: ""
    password: ""

proxy-groups:
  - name: "🌐 Proxy"
    type: select
    proxies: [Corp-HTTP, DIRECT]

# 2. 分流规则：把公司内网域名/网段全部 DIRECT，其余全部走代理
rules:
  - DOMAIN-SUFFIX,jenkins-rcicd-uat.apps.ocpuat.pccw.com,DIRECT
  - DOMAIN-SUFFIX,np3.uamp.hkt.com,DIRECT
  - DOMAIN-SUFFIX,np3.muleamp.hkt.com,DIRECT
  - DOMAIN-SUFFIX,rcicd-jenkins-uat.hkt.com,DIRECT
  - DOMAIN-SUFFIX,nspm-sc.pccw.com,DIRECT
  - DOMAIN-SUFFIX,prcportal.pccw.com,DIRECT
  - DOMAIN-SUFFIX,np4.uamp.hkt.com,DIRECT
  - IP-CIDR,10.168.124.0/24,DIRECT
  - IP-CIDR,10.168.60.0/24,DIRECT
  - IP-CIDR,10.211.167.70/24,DIRECT
  - IP-CIDR,10.168.169.132/24,DIRECT
  - IP-CIDR,10.211.166.6/24,DIRECT
  - MATCH,🌐 Proxy          # 剩下的全部走公司 HTTP 代理
```

3️⃣ 载入配置
Profiles → 新建 → 类型选 Local → 指向刚才的 hkt.yml → 设为激活配置。

这一步根据 Clash Verge Rev 的版本不同，有所区别，总体就是设置一个“本地订阅”。

4️⃣ 打开 TUN 模式（强制接管）
Settings → Network
关闭 System Proxy（系统代理）
打开 TUN Mode → 旁边齿轮 → Stack 选 Mixed，Strict Route ON
重启 Clash Verge（菜单栏图标 → Quit → 再开）
首次会再要一次密码创建虚拟网卡，看到日志里出现 TUN interface utunX created 即成功。

这一步也是根据版本不同，总之就是设置 TUN 模式

5️⃣ 验证
浏览器开 ip.skk.moe → IP 应显示 10.168.60.41 的出口。
nslookup jenkins-rcicd-uat.apps.ocpuat.pccw.com → 仍可解析并直连。
ping 10.168.124.8 正常；ping 8.8.8.8 也正常（说明外网走了 HTTP 代理）。

6️⃣ 一键恢复
如果哪天不用了，把 TUN Mode 关掉、退出 Clash Verge，系统网络瞬间恢复，无需改任何系统代理/PAC。

⚠️ 注意
公司代理需要认证的话，在 username/password 里填。
如果还想让手机等设备走你这台 Mac，打开 Settings → Allow LAN，然后在手机 Wi-Fi 手动填 HTTP 代理（Mac 的局域网 IP + 7890）。


