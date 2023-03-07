# 安装fpm打包工具


1.安装软件
```
# 安装ruby语言
yum -y install ruby-devel gcc make rpm-build rubygems 
curl -sSL https://github.com/rvm/rvm/tarball/stable -o rvm-stable.tar.gz
tar -xzvf rvm-stable.tar.gz
cd rvm-rvm-cc69ed9
./install --auto-dotfiles
source /usr/local/rvm/scripts/rvm
rvm install 2.6.3
rvm use 2.6.3 --default

# 安装fpm软件
gem sources --add http://mirrors.aliyun.com/rubygems/
gem sources --remove https://rubygems.org/
gem sources -l
gem install fpm -v 1.4.0
fpm --help

```
2.软件使用(常用参数)

```
fpm的参数
参考：https: //github .com /jordansissel/fpm/wiki
% fpm -s < source  type > -t <target  type > [options]
-s                          源格式
-t                          目标格式
-n                          包名
- v                           version值，实际版本号
--iteration                 release值，发布序列号
--epoch                     epoch值
--vendor                    厂商
--maintainer                维护者
--description               描述
--url                       软件主页
--workdir                   fpm工作目录
-d                          依赖的软件包
--directories               递归指定的目录标记为属于这个包
-C                          切换到指定的目录
-p                          输出到指定的路径
--force                     强制覆盖文件
 
--after-install  FILE        包安装后执行的脚本
--before-install  FILE       包安装前执行的脚本
--after-remove FILE         包移除后执行的脚本
--before-remove FILE        包移除前执行的脚本
--after-upgrade FILE        包升级后执行的脚本
--before-upgrade FILE       包升级前执行的脚本
 
-e                          building前编辑spec文件

fpm -f -s dir -t rpm -n mysql-8.0.30  -v 1.0.0 --epoch 1  -d 'libaio libaio-devel'  -C    ./mysql-8.0.30-linux-glibc2.12-x86_64   --after-install /tools/mysql-8.0.30-linux-glibc2.12-x86_64/scripts/init.sh  --verbose --prefix /application/mysql-8.0.30

```


2. 安装后脚本
```
# File Name: MySQL_Install.sh
# Author: Owen
# mail: 
# update Time: 2022-11-13 Sun 
# package：mysql-x.x.xx-linux-glibc2.12-x86_64
# Discription: mkdir /application  tar
# MySQL的基本安装与部署地址：https://downloads.mysql.com/archives/community/
#########################################################################

#!/bin/sh
#
base_dir="/application/mysql-8.0.30"

data_dir="/data/mysql/mysql3306"
ip_str=`ip a|grep -A 3 'mtu 1500'|awk  -F '[ /]+' 'NR==3{print $3}'|awk -F '.' '{print $NF}'`

#server_id=${ip_str: -3}
server_id="3306${ip_str}"



# 
if ! id mysql &> /dev/null ;then
	useradd -r -M -s /sbin/nologin mysql
fi

# 
if [ ! -d $data_dir ];then
	mkdir -p $data_dir/{data,logs,etc,scripts,tmp}
	echo  "/usr/local/mysql/bin/mysqld_safe --defaults-file=$data_dir/etc/my.cnf  & "> $data_dir/scripts/start.sh
    echo  "/usr/local/mysql/bin/mysqladmin -S $data_dir/tmp/mysql3306.sock shutdown "> $data_dir/scripts/stop.sh
	chown -R mysql.mysql $data_dir
fi


if [ ! -f $data_dir/etc/my.cnf ];then

   	cp  ${base_dir}/my.cnf $data_dir/etc &&\
	
	sed  -ri  '16s/'3306'/'${server_id}'/g'  $data_dir/etc/my.cnf	
	echo -e '[mysql]\nno-auto-rehash\nprompt="\\u@\\h [\\d]>"' > /etc/my.cnf
fi
 

if [ ! -d /usr/local/mysql ];then
   	 ln -s ${base_dir}   /usr/local/mysql
fi




#PATH_file
if [ ! -f /etc/profile.d/mysql.sh ];then
	echo 'export PATH=/usr/local/mysql/bin:$PATH' >> /etc/profile.d/mysql.sh
	source /etc/profile.d/mysql.sh
fi

#lib64_file
if [ ! -f /etc/ld.so.conf.d/mysql.conf ];then
	echo '/usr/local/mysql/lib' > /etc/ld.so.conf.d/mysql.conf
fi

#include_file
if [ ! -d /usr/include/mysql ];then
	ln -s /usr/local/mysql/include/ /usr/include/mysql 
fi

#man_file
if [ -f /etc/man_db.conf -a `grep  '/usr/local/mysql/man'  /etc/man_db.conf|wc  -l` -eq 0 ];then
	sed -ri '22a \MANDATORY_MANPATH                       /usr/local/mysql/man'  /etc/man_db.conf
fi

/usr/local/mysql/bin/mysqld --defaults-file=$data_dir/etc/my.cnf  --initialize-insecure 
sleep 5
if [ $? -eq 0 ];then
	echo "MySQL initialization succeeded"
else
	echo "MySQL initialization failed"
	exit
fi

# 
/usr/local/mysql/bin/mysqld_safe  --defaults-file=$data_dir/etc/my.cnf & &> /dev/null
sleep 10 

if [ $? -eq 0 ];then

	echo "MySQL started successfully"
else
	echo "MySQL startup failed"
fi





```
