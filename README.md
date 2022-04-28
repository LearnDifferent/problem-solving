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
