---
title:  "frp proxy"
date:  2021-10-14 
tags: [frp]
key: frp
---

# Remote Server

https://github.com/fatedier/frp/releases

## install 

```bash
wget https://github.com/fatedier/frp/releases/download/v0.38.0/frp_0.38.0_linux_amd64.tar.gz
tar -zxvf frp_0.38.0_linux_amd64.tar.gz
```
<!--more-->

## service

```bash
sudo vim /lib/systemd/system/frps.service
```

```shell
[Unit]
Description=Frp Server Service
After=network.target

[Service]
Type=simple
User=root
Restart=on-failure
RestartSec=5s
ExecStart="/home/ubuntu/frp_0.38.0_linux_amd64"/frps -c "/r/home/ubuntu/frp_0.38.0_linux_amd64"/frps.ini
LimitNOFILE=1048576

[Install]
WantedBy=multi-user.target
```

然后启动 frps
`sudo systemctl start frps`
再打开自启动
`sudo systemctl enable frps`
同时

重启 `sudo systemctl restart frps`
停止 `sudo systemctl stop frps`
查看应用日志 `sudo systemctl status frps`


# Client


```bash
sudo vim /lib/systemd/system/frpc.service
```

```shell
[Unit]
Description=Frp Client Service
After=network.target

[Service]
Type=simple
User=root
Restart=on-failure
RestartSec=5s
ExecStart=/home/lyn/Applications/frp_0.38.0_linux_amd64/frpc -c /home/lyn/Applications/frp_0.38.0_linux_amd64/frpc.ini
ExecReload=/home/lyn/Applications/frp_0.38.0_linux_amd64/frpc reload -c /home/lyn/Applications/frp_0.38.0_linux_amd64/frpc.ini
LimitNOFILE=1048576

[Install]
WantedBy=multi-user.target
```

```ini
[common]
server_addr = 39.96.1.62
server_port = 7000
token = aliyun
[ssh1] #不同的客户端需要取不同的名字
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6001
```