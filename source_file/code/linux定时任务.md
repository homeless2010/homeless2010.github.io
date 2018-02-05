title: "linux定时任务"
date: 2018-01-28 01:02:20 +0800
update: 2018-01-30 23:45:20 +0800
author: me
tags:
    - linux
preview: 备份。

---

### linux定时任务

最近在学习linux，关于定时任务：

1. 写好任务脚本；
2. 执行 crontab -e编辑定时命令如 0 1 * * * root /data/bakdb.sh  > /data/bak.log 2>&1 保存并退出
3. 重启crond服务

>Tips:网上说重启命令service crond restart或service crond reload  
这个命令在red hat当中常用，有的linux发行版本中没有这个命令，这时  
/etc/init.d/cron stop  
/etc/init.d/cron start

#### linux功能很强大，昨天准备装debian testing但是对分区还没整明白,担心丢失磁盘数据就没敢装，<br>总之前路慢慢...<br>未完待续...

####  接上

### rsync实现增量同步

scp和rsync(区别自行百度)
219.219.115.94 tomcat  
219.219.115.95 mysql  
219.219.115.89 备份服务器  
rsync参考http://www.blogjava.net/ruoyoux/articles/232075.html

95：

1. 在/etc目录下建立rsyncd.conf文件  
uid = nobody  
gid = nobody  
use chroot = no  
max connections = 4  
log file = /var/log/rsyncd.log  
pid file = /var/run/rsyncd.pid  
lock file = /var/run/rsync.lock  
<br>
[backup]  
path = /opt/sudytech/backup  
comment = backup folder  
uid = root  
ignore errors  
read only = no  
list = no  
auth users = root  
secrets file = /etc/backup.scrt  

2. 在/etc下建立backup.scrt文件  
输入： 用户名：密码  
例：rsynctest:testrsync  
将文件权限修改600，我修改成700了

3. 启动rsync服务：  
rsync --daemon （rsync运行在tcp 873端口，可以通过netstat -an|grep LISTEN察看）。    
重启 rsync服务rm /var/run/rsyncd.pid    
然后再启动89：在89上（rsync客户机）上建立/etc/backup文件，内容为A主机的密码  
例： testsync 修改档案权限我改成7005、在89用crontab建立脚本  
例：0　21　*　*　* rsync -vzrtp --progress --delete --password-file=/etc/backup root@192.168.1.10::backup /opt/sudytech/backup  

94:  

同95只是同步时报113 10 124的错误  
网上说防火墙问题执行service iptables start / stop无效原来在CentOS 7或RHEL 7或Fedora中防火墙由firewalld来管理  
vi /etc/sysconfig/iptables加入  
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 873 -j ACCEPT无效可能应该是  
-A INPUT -p tcp -m state --state NEW -m tcp --dport 873 -j ACCEPT  
直接执行了firewall-cmd --zone=public --add-port=873/tcp --permanent    （--permanent永久生效，没有此参数重启后失效）  
ok！
