#安装说明
中文文档：https://gofrp.org/docs/  
下载地址：https://github.com/fatedier/frp/releases

1.安装配置
```sh
客户端和服务端都可以配置systemctl启动方式

[Unit]

vim /etc/systemd/system/frps.service
# 服务名称，可自定义
Description = frp server
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
# 启动frps的命令，需修改为您的frps的安装路径
ExecStart = /path/to/frps -c /path/to/frps.ini

[Install]
WantedBy = multi-user.target


# 使用 systemd 命令，管理 frps。
# 启动frp
systemctl start frps
# 停止frp
systemctl stop frps
# 重启frp
systemctl restart frps
# 查看frp状态
systemctl status frps
# 配置 frps 开机自启。
systemctl enable frps
```

2.SSH 访问内网机器
```sh
# 服务端配置文件
[common]
bind_port = 7000

# 客户端配置文件
[common]
server_addr = x.x.x.x
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000

ssh -p6000 test@x.x.x.x
```

3.配置frp仪表盘及认证
```sh
# 服务端配置文件
[common]
bind_port = 7000
vhost_https_port = 80         #虚拟主机端口，如果要发布web服务则需配置该项
dashboard_port = 7500         #仪表盘端口公
dashboard_user = admin        #仪表盘用户名
dashboard_pwd = admin         #仪表盘密码
authentication_method = token #认证使用token
token = 123456                #token
注:配置文件中必须删除中文注释。

# 客户端配置文件
[common]
server_addr = x.x.x.x
server_port = 7000
token = 123456        #配置token必须和服务端一直

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000
```


