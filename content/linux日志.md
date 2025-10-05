## Linux日志系统

```C++
 linux日志系统：
 /var/log/
 查看日志：
 cat:查看较小的日志
 view查看较大的日志
 head:只看日志最开始
 tail:只看日志最末尾 tail -1只看末尾的一条
 
 系统日志类型：
 asuth:pam产生的日志
 authpriv:ssh之类产生的日志
 cron:计划任务日志
 lpr:打印日志
 mail:邮件日志
 uucp:主机之间相关的通信
 news：新闻组
 系统日志级别：
 0-2立刻处理 3-6
 0 emerg 系统不可用
 1 alert 必须立刻采取措施
 2 crit 严重情况
 3 err 非严重错误
 4 warning 警告
 5 notice:正常但较重要
 6 info ：一般信息
 7 debug:调试信息
 8 none ：什么都不记录
 
查看日志的方法1： 
journalctl可以让用户查看系统服务、内核和其他应用程序生成的日志
消息，包括进程启动和关闭、错误报告、警告、系统状态变化等。
-f：实时流式显示日志
-u:查看特定进程的日志

查看日志的方法2：
导出syslog，用vim查看
```

## 命令提示器

```C
用户名@主机名：用户所在目录
~表示用户根目录
$代表普通用户
#代表超级用户
root@iZ2ze84zwrzp1mlr3amtaoZ:~/Desktop/code/cloud-pan#
表示超级用户且在根目录的/Desktop/code/cloudpan下
```