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



```



