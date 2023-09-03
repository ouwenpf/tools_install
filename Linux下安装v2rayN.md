#安装说明
官方网站:https://v2ray.com/chapter_00/install.html
图形界面下载地址：https://github.com/2dust/v2rayN/releases  
客户端内核下载地址：https://github.com/v2fly/v2ray-core/releases

1.安装依赖
```
yum install -y unzip zip openssl

```
2.下载安装脚本

```sh
# 官方脚本
wget https://github.com/v2fly/fhs-install-v2ray/blob/master/install-release.sh  --user-agent="Mozilla/5.0"

# 备用，与官方脚本一致
wget https://github.com/v2fly/fhs-install-v2ray/blob/master/install-release.sh --user-agent="Mozilla/5.0"

# 下载v2ray-linux-64.zip 
https://github.com/v2fly/v2ray-core/releases
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
4.安装 proxychains
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

5.配置curl、wget等命令使用代理  
```sh
#创建文件"/etc/profile.d/setproxy.sh	，在文件结束位置增加如下内容：

# set proxy
function setproxy() {
    export http_proxy=socks5://127.0.0.1:10808
    export https_proxy=socks5://127.0.0.1:10808
    export ftp_proxy=socks5://127.0.0.1:10808
    export no_proxy="172.16.x.x"
}
​
# unset proxy
function unsetproxy() {
    unset http_proxy https_proxy ftp_proxy no_proxy
}

#在终端执行 setproxy 使代理生效
#在终端执行 unsetproxy 使代理生效
```
