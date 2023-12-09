---
title: Ubuntu科学上网
date: 2023-04-11 17:48:36
tags:
    - Ubuntu
    - 科学上网
declare: true
doc: 1
---
## clash
### 带桌面版本<!--more-->
#### 1. 去[github](https://github.com/Fndroid/clash_for_windows_pkg/releases)下载Linux版本的clash for windows
![20230411191443](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230411191443.png)

- 下载到的文件结构如下：
```
LICENSE.electron.txt    chrome_100_percent.pak   libEGL.so             libvulkan.so.1  snapshot_blob.bin
LICENSES.chromium.html  chrome_200_percent.pak   libGLESv2.so          locales         v8_context_snapshot.bin
cfw                     chrome_crashpad_handler  libffmpeg.so          resources       vk_swiftshader_icd.json
chrome-sandbox          icudtl.dat               libvk_swiftshader.so  resources.pak
```
- 其中**cfw**是clash for windows的缩写
`./cfw`就可以运行clash for windows

#### 2. 设置为应用
```bash
cd ~/.local/share/applications  #个人应用的放置地点
vim clash.desktop   # 编辑应用信息
chmod +x clash.desktop
```

clash.desktop的文件信息如下：
```bash
[Desktop Entry]
Name=clash for windows
Icon=   # 图标路径
Exec=~/clash/cfw    # cfw的路径
Type=Application
```

#### 3. 搜索clash for windows即可

---------------------------------------------

### 终端版本
#### 1. 去[github](https://github.com/Dreamacro/clash/releases)下载Linux版本的clash-core
![20230411193200](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230411193200.png)

#### 2. 解压之后仅有一个文件，要给权限
`chmod +x clash`

#### 3. 加配置文件
```bash
mkdir /opt/clash    #将clash可执行文件和yaml文件放在此目录下
./clash -f config.yaml  #指定配置文件
```
config.yaml为配置文件，如果不指定，默认配置文件在`~/.config/clash`

#### 4. 配置为系统服务
```bash
vim /etc/systemd/system/clash.service
```
clash.service文件内容如下:
```bash
[Unit]
Description=clash-core
[Service]
Type=simple
ExecStart=/opt/clash/clash -f /opt/clash/config.yaml
```

- 启用即可
```bash
systemctl daemon-reload
systemctl start clash
systemctl status clash
```

#### 5. 终端上网
[How do I use clash proxy wsl in windows?](https://corner430.github.io/2023/04/05/How-do-I-use-clash-proxy-wsl-in-windows/#more)

可以在`~/.bashrc`中通过`alias`命令进行变量开关定义
```bash
alias proxy="export http_proxy=http://127.0.0.1:7890;export https_proxy=http://127.0.0.1:7890"
alias unproxy="unset http_proxy;unset https_proxy;"
```
> 记得source ~/.bashrc

-----------------------------------------------

#### 6. 下载UI
![20230411195232](https://cdn.jsdelivr.net/gh/Corner430/Picture/images/20230411195232.png)
在`/opt/clash`下建一个`ui`文件夹，并将文件ui的文件移动到该目录下：
CNAME  assets  index.html  manifest.webmanifest  sw.js  workbox-e0782b83.js
- 其中index.html即为页面，需要在clash的config.yaml文件中指向一下
    - 在`external-controller: 0.0.0.0:9090`的下方添加`external-ui: /opt/clash/ui`
    - `systemctl restart clash`
    - 通过`127.0.0.1:9090/ui`访问即可

---------------------------------------------------

### docker版本
```bash
sudo apt install docker.io
docker -v
```
在[docker官网](https://hub.docker.com/)查找镜像`dreamacro/clash`
```bash
docker pull dreamacro/clash
docker run -d --name clash -p 7890:7890 -p 7891:7891 -p 9090:9090 -v /opt/clash/config.yaml:/root/.config/clash/config.yaml -v /opt/clash/ui:/opt/clash/ui dreamacro/clash
```
- `--name clash`: 名字起做clash
- `-p`: 端口映射
- `-v`: 文件映射
- `dreamacro/clash`: 启动镜像的名字

使用`docker logs -f 容器ID`查看日志
`docker ps`查看可查看容器ID

-----------------------------------------------
### 实验室服务器
```bash
# 设置代理
alias proxy="export http_proxy=http://127.0.0.1:7890; export https_proxy=http://127.0.0.1:7890"

# 取消代理
alias unproxy="unset http_proxy; unset https_proxy"

# 启动 clash
alias startclash="nohup /home/corner/.local/opt/clash -f /home/corner/.local/opt/config.yaml > /home/corner/.local/opt/output.log 2>&1 &"

# 停止 clash
alias stopclash="pkill -f '/home/corner/.local/opt/clash -f /home/corner/.local/opt/config.yaml'"

```


### References
[最全Linux科学上网三种方式-长风分享](https://www.youtube.com/watch?v=VOlWdNZAq_o&list=WL&index=15&t=942s&ab_channel=%E9%95%BF%E9%A3%8E%E5%88%86%E4%BA%AB)

## V2ray
- 下载 [v2ray core](https://github.com/v2ray/v2ray-core)的Linux版本，例如 `v2ray-linux-64.zip`

- `unzip`
其中的 `v2ray` 为可执行文件，`config.json` 为配置文件，配置文件可以通过 `v2rayN` 的 **导出所选服务器为客户端配置** 进行导出。

- `./v2ray -config config.json` 运行即可，回显如下：

```bash
V2Ray 4.28.2 (V2Fly, a community-driven edition of V2Ray.) Custom (go1.15.2 linux/amd64)
A unified platform for anti-censorship.
2023/11/21 07:15:11 [Info] v2ray.com/core/common/platform/ctlcmd: <v2ctl message>
v2ctl> Read config:  config.json
2023/11/21 07:15:11 [Warning] v2ray.com/core: V2Ray 4.28.2 started
```