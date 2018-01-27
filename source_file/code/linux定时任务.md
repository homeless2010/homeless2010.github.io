title: "linux定时任务"
date: 2018-01-28 01:02:20 +0800
author: me
tags:
    - linux
preview: 备份。

---

### linux定时任务

最近在学习linux，关于定时任务：

1.写好任务脚本；
2.执行 crontab -e编辑定时命令如 0 1 * * * root /data/bakdb.sh  > /data/bak.log 2>&1 保存并退出
3.重启crond服务

>Tips:网上说重启命令service crond restart或service crond reload  
这个命令在red hat当中常用，有的linux发行版本中没有这个命令，这时  
/etc/init.d/cron stop  
/etc/init.d/cron start

#### linux功能很强大，昨天准备装debian testing但是对分区还没整明白,担心丢失磁盘数据就没敢装，<br>总之前路慢慢...<br>未完待续...
