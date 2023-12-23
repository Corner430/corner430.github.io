---
title: 小新pro16 安装 Ubuntu
date: 2023-12-09 21:26:42
tags:
    - Ubuntu
---
1. 开机黑屏<!--more-->
显卡驱动问题，安装显卡驱动。
点击 `ctrl+alt+F2` 进入命令行模式，输入用户名和密码，**直接更新重启**，如果还不行。执行以下命令：

```bash
ubuntu-drivers devices  #查看系统推荐的Nvidia版本，带有 recommended 标签的是推荐版本
sudo apt install nvidia-driver-xxx  #安装Nvidia驱动
sudo reboot  #重启
```

> 关闭 bios 中的安全启动，否则会掉驱动

2. 检测到窗口系统采用wayland协议,腾讯会议暂不兼容,程序即将退出!

```bash
sudo vim /etc/gdm3/custom.conf
#WaylandEnable=false #取消注释
sudo service gdm restart
```

3. 输入法
先在区域和语言中设置中文，需要点击**管理已安装的语言**，把缺少的语言包安装上。

注意到这时候的**键盘输入法系统**是`ibus`，`ibus`和 **智能拼音** 相互匹配。

设置中点击**键盘**，在**输入源**中添加**中文（智能拼音）**，重启后生效。

4. 可执行软件装在 `/usr/local/bin` 下

5. 科学上网 `docker pull mzz2017/v2raya`

```bash
docker run -d \
  --restart=always \
  --privileged \
  --network=host \
  --name v2raya \
  -e V2RAYA_LOG_FILE=/tmp/v2raya.log \
  -e V2RAYA_V2RAY_BIN=/usr/local/bin/v2ray \
  -e V2RAYA_NETABLES_SUPPORT=off \
  -v /lib/modules:/lib/modules:ro \
  -v /etc/resolv.conf:/etc/resolv.conf \
  -v /etc/v2raya:/etc/v2raya \
  mzz2017/v2raya
```
之后去 web 界面的 `http://127.0.0.1:2017` 设置

------------------------------
tweaks
[gnome extensions](https://extensions.gnome.org/)
[gnome look](https://www.gnome-look.org/browse/)

```bash
sudo apt install gnome-tweaks
sudo apt install chrome-gnome-shell
```

- Netspeed： 网速
- Dash to Dock： 底部栏
- Coverflow Alt-Tab： 切换窗口
- Applications menu： 应用菜单
- Extension List： 扩展列表
- Vitals： 系统监控
- Removable Drive Menu： 可移动设备菜单
- Clipboard Indicator： 剪贴板
- OpenWeather： 天气
- User Themes： 主题
- Burn My Windows： 窗口特效
- Application Volume Mixer： 音量控制
- Simple Timer： 计时器
- Removable Drive Menu： 可移动设备菜单
- Hide Activities Button： 隐藏活动按钮
- Global menu for Gnome（不好用）： 全局菜单
- Win Tile： 分屏
- Transparent Window Moving： 窗口移动透明
- Caffeine： 点击不熄屏
- Unite(optional): 窗口控制
- Glasa(optional)：顶栏之眼
- Fly-Pie(optional)： 鼠标手势

------------------------------

