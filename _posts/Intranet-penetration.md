---
title: Intranet penetration
date: 2023-04-03 08:59:06
declare: true
tags:
toc: 1
password: ysyhl_9T
---
### 1 frp
- 在[官方github](https://github.com/fatedier/frp)下载对应版本<!--more-->
- 我们只需要关注如下几个文件
```shell
frps
frps.ini
frpc
frpc.ini
```
前两个文件（s结尾代表server）分别是服务端程序和服务端配置文件，后两个文件（c结尾代表client）分别是客户端程序和客户端配置文件。
因为我们正在配置服务端，可以删除客户端的两个文件
```shell
rm frpc
rm frpc.ini
```

- 然后修改frps.ini文件
这个文件应有如下格式
```bash
[common]
bind_port = 7000
dashboard_port = 7500(网页登录端口)
token = meng123456（密码可以自己修改）
dashboard_user = meng（网页登录帐号）
dashboard_pwd = meng123456（网页登录密码）
vhost_http_port = 10080
vhost_https_port = 10443
```
如果没有必要，端口均可使用默认值，token、user和password项请自行设置。

    - “bind_port”表示用于客户端和服务端连接的端口，这个端口号我们之后在配置客户端的时候要用到。
    - “dashboard_port”是服务端仪表板的端口，若使用7500端口，在配置完成服务启动后可以通过浏览器访问 x.x.x.x:7500 （其中x.x.x.x为VPS的IP）查看frp服务运行信息。
    - “token”是用于客户端和服务端连接的口令，请自行设置并记录，稍后会用到。
    - “dashboard_user”和“dashboard_pwd”表示打开仪表板页面登录的用户名和密码，自行设置即可。
    - “vhost_http_port”和“vhost_https_port”用于反向代理HTTP主机时使用，本文不涉及HTTP协议，因而照抄或者删除这两条均可。


- 编辑完成后保存

如果看到屏幕输出这样一段内容，即表示运行正常，如果出现错误提示，请检查上面的步骤。
```shell
2019/01/12 15:22:39 [I] [service.go:130] frps tcp listen on 0.0.0.0:7000
2019/01/12 15:22:39 [I] [service.go:172] http service listen on 0.0.0.0:10080
2019/01/12 15:22:39 [I] [service.go:193] https service listen on 0.0.0.0:10443
2019/01/12 15:22:39 [I] [service.go:216] Dashboard listen on 0.0.0.0:7500
2019/01/12 15:22:39 [I] [root.go:210] Start frps success
```
此时访问 x.x.x.x:7500 并使用自己设置的用户名密码登录，即可看到仪表板界面


- 服务端后台运行

至此，我们的服务端仅运行在前台，如果Ctrl+C停止或者关闭SSH窗口后，frps均会停止运行，因而我们使用 nohup命令将其运行在后台。
nohup后台程序管理或关闭相关命令可自行查询资料。
`nohup ./frps -c frps.ini &`
输出如下内容即表示正常运行
```shell
nohup: ignoring input and appending output to 'nohup.out'
```
此时可先使用Ctrl+C关闭nohup，frps依然会在后台运行，使用jobs命令查看后台运行的程序
`jobs`
在结果中我们可以看到frps正在后台正常运行
```shell
[1]+  Running                 nohup ./frps -c frps.ini &
```

- 此处附上开机自启动的方式
`tar -zxvf frp_0.29.0_linux_amd64.tar.gz`
运行命令
./frps -c ./frps.ini
**************************************************************************
设置自动启动
vim /lib/systemd/system/frps.service
写入以下配置
**************************************************************************
```shell
[Unit]
Description=frps service
After=network.target network-online.target syslog.target
Wants=network.target network-online.target

[Service]
Type=simple

#启动服务的命令（此处写你的frps的实际安装目录）
ExecStart=/root/frp/frps -c /root/frp/frps.ini

[Install]
WantedBy=multi-user.target
```
**************************************************************************
然后启动 frps
`systemctl start frps`
再打开自启动
`systemctl enable frps`
同时

重启 `systemctl restart frps`
停止 `systemctl stop frps`
查看应用日志 `systemctl status frps`


frp的客户端就是我们想要真正进行访问的那台设备，大多数情况下应该会是一台Windows主机，因而本文使用Windows主机做例子；Linux配置方法类似，不再赘述。
同样地，根据客户端设备的情况选择相应的frp程序进行下载，Windows下下载和解压等步骤不再描述。
假定你下载了“frp_0.22.0_windows_amd64.zip”，将其解压在了C盘根目录下，并且将文件夹重命名为“frp”，可以删除其中的frps和frps.ini文件。
用文本编辑器打开frpc.ini，与服务端类似，内容如下。

```shell
[common]
server_addr = 182.92.189.71
server_port = 7000
token= meng123456

[RDP]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 7001

[VNC]
type = tcp
local_ip = 127.0.0.1
local_port = 5900
remote_port = 7002

[SMB]
type = tcp
local_ip = 127.0.0.1
local_port = 445
remote_port = 7003
```
    - 其中common字段下的三项即为服务端的设置。
    - “server_addr”为服务端IP地址，填入即可。
    - “server_port”为服务器端口，填入你设置的端口号即可，如果未改变就是7000
    - “token”是你在服务器上设置的连接口令，原样填入即可。


- 自定义规则
frp实际使用时，会按照端口号进行对应的转发，
上面frpc.ini的rdp、smb字段都是自己定义的规则，自定义端口对应时格式如下。
“[xxx]”表示一个规则名称，自己定义，便于查询即可。
“type”表示转发的协议类型，有TCP和UDP等选项可以选择，如有需要请自行查询frp手册。
“local_port”是本地应用的端口号，按照实际应用工作在本机的端口号填写即可。
“remote_port”是该条规则在服务端开放的端口号，自己填写并记录即可。
RDP，即Remote Desktop 远程桌面，Windows的RDP默认端口是3389，协议为TCP，建议使用frp远程连接前，在局域网中测试好，能够成功连接后再使用frp穿透连接。
SMB，即Windows文件共享所使用的协议，默认端口号445，协议TCP，本条规则可实现远程文件访问。
配置完成frpc.ini后，就可以运行frpc了
frpc程序不能直接双击运行！
使用命令提示符或Powershell进入该目录下
`cd C:\frp`
并执行
`./frpc -c frpc.ini`
运行frpc程序，窗口中输出如下内容表示运行正常。
```shell
2019/01/12 16:14:56 [I] [service.go:205] login to server success, get run id [2b65b4e58a5917ac], server udp port [0]
2019/01/12 16:14:56 [I] [proxy_manager.go:136] [2b65b4e58a5917ac] proxy added: [rdp smb]
2019/01/12 16:14:56 [I] [control.go:143] [smb] start proxy success
2019/01/12 16:14:56 [I] [control.go:143] [rdp] start proxy success
```
不要关闭命令行窗口，此时可以在局域网外使用相应程序访问 x.x.x.x:xxxx （IP为VPS的IP，端口为自定义的remote_port）即可访问到相应服务。


此后利用winsw将客户端设置为开机自启动
编辑`winsw.xml`
其格式如下：
```shell
<service>
	<!-- 该服务的唯一标识 -->
    <id>frpc</id>
    <!-- 该服务的名称 -->
    <name>frp</name>
    <!-- 该服务的描述 -->
    <description>frpc客户端 这个服务用 frpc 实现内网穿透</description>
    <!-- 要运行的程序路径 -->
    <executable>frpc.exe</executable>
    <!-- 携带的参数 -->
    <arguments>-c frpc.ini</arguments>
    <!-- 日志模式 -->
    <logmode>none</logmode>
</service>
```

用cmd进入这一切的目录下
在命令框输入`winsw.exe install`
显示 `INFO -Installing the servics with id "***"`
即为成功
之后打开任务管理器，服务，打开服务，找到这个服务
将其设置为**失败但不断重复**，之后启动就好，大功告成！

-----------------

- Boot up
`frpc.ini`

```shell
[common]
server_addr = 150.158.17.239
server_port = 80
privilege_token = 11110000

[RDP]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 8001

[Storage-Computing-System]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 8002

[ssh_154]
type = tcp
local_ip = 172.17.0.154
local_port = 22
remote_port = 8003

[ssh_155]
type = tcp
local_ip = 172.17.0.155
local_port = 22
remote_port = 8004

[ssh_156]
type = tcp
local_ip = 172.17.0.156
local_port = 22
remote_port = 8005

[ssh_157]
type = tcp
local_ip = 172.17.0.157
local_port = 22
remote_port = 8006

[ssh_158]
type = tcp
local_ip = 172.17.0.158
local_port = 22
remote_port = 8007

[ssh_159]
type = tcp
local_ip = 172.17.0.159
local_port = 22
remote_port = 8008
```



1. Create a systemd service unit file called **frpc.service** on Linux systems.
`sudo nano /etc/systemd/system/frpc.service`

2. Configuration
```shell
[Unit]
Description=Frp client
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/frp/frpc -c /usr/local/frp/frpc.ini
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```

Description: Description information of the service.
After: The service needs to start after which services start.

Type: Service startup type. simple means that the service will directly execute the commands in ExecStart after startup, and will not fork a new process.

ExecStart: The command executed when the service starts.

Restart: How systemd will handle the service when the service is abnormal. always means always restart the service.

User: The user the service runs as.

WantedBy: The runlevel the service is in.
In the above example, the command path and configuration file path in ExecStart need to be modified according to the actual situation.


3. Reload the systemd configuration file:
`sudo systemctl daemon-reload`

4. Start the frpc service:
`sudo systemctl start frpc`

5. Verify that the service has started:
`sudo systemctl status frpc`

If the service is already started, output similar to the following will be displayed
```bash
● frpc.service - Frp client
     Loaded: loaded (/etc/systemd/system/frpc.service; disabled; vendor preset: enabled)
     Active: active (running) since Sat 2023-03-13 07:30:22 UTC; 15s ago
   Main PID: 12345 (frpc)
      Tasks: 7 (limit: 1136)
     Memory: 2.2M
     CGroup: /system.slice/frpc.service
             └─12345 /usr/local/frp/frpc -c /usr/local/frp/frpc.ini
```

6. Set the service to start automatically at boot:
`sudo systemctl enable frpc`

Now, the frpc service has been configured to start automatically at boot and run in the background. The service can be stopped with the following command:
`sudo systemctl stop frpc`

If you need to disable automatic startup on boot, you can use the following command:
`sudo systemctl disable frpc`

### 2. Zerotier
Official website: https://www.zerotier.com/
Account: sikes60407@loongwin.com
Password: yFKjXv46JT@.43d
Network ID: c7c8172af125f9c1
- Linux install ZeroTier
`curl -s https://install.zerotier.com | sudo bash`

- Join the network by ID
`sudo zerotier-cli join <NETWORK_ID>`

- Go to the official website to authorize

- Check connection status
`sudo zerotier-cli listnetworks`

- Addition
```bash
zerotier-cli leave Network ID   #leave the network

sudo systemctl enable zerotier-one.service

sudo systemctl start zerotier-one.service

sudo zerotier-cli join <NETWORK_ID>
```

### 3. 花生壳








