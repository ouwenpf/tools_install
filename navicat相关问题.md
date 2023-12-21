- 1.Navicat连接postgresql时出现datlastsysoid does not exist报错
```sh

PostgresQ 15 从表中删除了 datlastsysoid 字段pg database因此 Navicat 15.0.29 或 16.1 之前的任何版本在查找此已弃用字段时都会引发此错误
要解决此问题，请升级到最新的 Navicat 15.0.29 或 6.1 及更高版本(可能需要新的许可证)，或者执行以下操作:

1.打开 Navicat 文件夹 (通常在 C:\Program Files\PremiumSoft\Navicat...) 下) ，取决于您的Navicat 版本找到libcc.dl并创建此文件的备份 (将其复制并粘贴为libcc-backup.dl或任何其他名)

3.备份这个文件
3.进入网站https://hexed.it/ 打开本地的libcc.dll 文件
4.右侧点击搜索，关键词SELECT DISTINCT datlastsysoid
5.找到之后，把datlastsysoid这几个字，改成dattablespace
6.然后把文件下载回来，放回原处
重启navicat，可以发现无论老和新版本的pgsql都可以正常访问了

https://cloud.tencent.com/developer/article/2314524
```



