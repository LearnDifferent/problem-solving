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
