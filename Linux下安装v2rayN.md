#安装说明

图形界面下载地址：https://github.com/2dust/v2rayN/releases  
客户端内核下载地址：https://github.com/v2fly/v2ray-core/releases

1.安装依赖
```
yum install -y unzip zip openssl

```
2.下载安装脚本

```
# 官方脚本
wget https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh  --user-agent="Mozilla/5.0"

# 备用，与官方脚本一致
wget https://github.com/super1024201/v2ray_sh_copy/blob/main/install-release.sh --user-agent="Mozilla/5.0"

# 下载v2ray-linux-64.zip 

```
3.安装v2rayN
```
# 如果本机无法连接github手工进行安装,要安装v2rayN机器一般是无法连接的  
https://github.com/v2fly/v2ray-core/releases下载地址
sh install-release.sh -l ./v2ray-linux-64.zip 

# 安装完毕后,需要v2ray账户才可以使用,请配置/usr/local/etc/v2ray/config.json
建议在window上面配置好,直接导出配置文件进行覆盖,文件是通用的
启动systemctl  start  v2ray.service 
```
4. 安装 proxychains
```
# 需要epel源
yum install -y proxychains-ng
# 需修改配置文件
vim /etc/proxychains.conf
将socks4 127.0.0.1 9095修改为socks5 127.0.0.1 10808，因为我v2ray的配置端口是10808，所以这里填了10808,如果端口其它安装实际配置即可
如果内网其它机器需要共享修改为socks5 机器内网IP 10808
# 测试
proxychains curl -i www.google.com 或者  curl --socks5 127.0.0.1:10808 https://www.google.com

```