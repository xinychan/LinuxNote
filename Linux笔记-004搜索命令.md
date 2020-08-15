# 搜索命令

## 文件搜索 locate

```
locate 文件名
在后台数据库中按文件名搜索，搜索速度快
缺点：只能按照文件名搜索

/var/lib/mlocate
locate命令所搜索的后台数据库保存位置，不同的Linux系统位置可能不同

updatedb
刚新增的文件无法通过locate立即搜索到，因为数据库没有更新，使用updatedb更新数据库

/etc/updatedb.conf
locate命令配置的规则文件；里面定义了哪些搜索规则生效

[root@localhost ~]# cat /etc/updatedb.conf
// 开启搜索限制
PRUNE_BIND_MOUNTS = "yes"
// 不搜索的文件系统
PRUNEFS = "9p afs anon_inodefs auto autofs bdev binfmt_misc cgroup cifs coda configfs cpuset debugfs devpts ecryptfs exofs fuse fusectl gfs gfs2 hugetlbfs inotifyfs iso9660 jffs2 lustre mqueue ncpfs nfs nfs4 nfsd pipefs proc ramfs rootfs rpc_pipefs securityfs selinuxfs sfs sockfs sysfs tmpfs ubifs udf usbfs"
// 不搜索的文件类型
PRUNENAMES = ".git .hg .svn"
// 不搜索的文件目录
PRUNEPATHS = "/afs /media /net /sfs /tmp /udev /var/cache/ccache /var/spool/cups /var/spool/squid /var/tmp"
```

```
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log.syslog
[root@localhost ~]# locate install.log
/root/install.log
/root/install.log.syslog
[root@localhost ~]# touch install.log2
[root@localhost ~]# locate install.log
/root/install.log
/root/install.log.syslog
[root@localhost ~]# updatedb
[root@localhost ~]# locate install.log
/root/install.log
/root/install.log.syslog
/root/install.log2
```

/etc/updatedb.conf 搜索配置文件的作用

```
[root@localhost ~]# touch /tmp/install.log3
[root@localhost ~]# cd /tmp
[root@localhost tmp]# ls
install.log3
[root@localhost tmp]# cd
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log2  install.log.syslog
// 更新数据库后也仍然无法搜索到 /tmp/install.log3，因为在 /etc/updatedb.conf 配置了搜索规则
[root@localhost ~]# updatedb
[root@localhost ~]# locate install.log
/root/install.log
/root/install.log.syslog
/root/install.log2
```

## 命令搜索命令 whereis 与 which

```
whereis [命令]
搜索命令所在路径以及帮助文档所在位置
shell自带命令无法搜索到可执行文件，只能搜索帮助文件，比如 cd 命令
选项：
-b 只查找可执行文件
-m 只查找帮助文件
```

```
// ls 命令所在路径以及帮助文档的位置
[root@localhost ~]# whereis ls
ls: /bin/ls /usr/share/man/man1/ls.1.gz /usr/share/man/man1p/ls.1p.gz
// ls 命令作用
[root@localhost ~]# whatis ls
ls                   (1)  - list directory contents
ls                   (1p)  - list directory contents
```

```
which [命令]
搜索命令所在路径以及别名
shell自带命令无法搜索，比如 cd 命令
```

```
[root@localhost ~]# which ls
alias ls='ls --color=auto'
        /bin/ls
```

```
// PATH 环境变量：定义系统搜索命令的路径
// PATH 环境变量中的命令，只需要直接使用，比如 ls/cd 等，不在 PATH 中的命令，需要输入命令的全路径
[root@localhost ~]# echo $PATH
/usr/lib/qt-3.3/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
```

## 文件搜索命令 find

```
命令格式：
find [搜索范围][搜索条件]

find / -name install.log
避免大范围搜索，会非常耗费系统资源
find是在系统当中搜索符合条件的文件名，如果需要匹配，使用通配符匹配，通配符是完全匹配
```

```
// 从整个 / 根目录，搜索文件名为 install.log 的文件
[root@localhost ~]# find / -name install.log
/root/install.log
```

```
Linux 中的常用通配符
* 匹配任意内容
? 匹配任意一个字符
[] 匹配任意一个中括号内的字符
```

```
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log2  install.log3  install.log.syslog
// 在 root 目录中查找 install.log 开头的文件
[root@localhost ~]# find /root -name "install.log*"
/root/install.log
/root/install.log2
/root/install.log.syslog
/root/install.log3
// 在 root 目录中查找 install.log 后带一个字符的文件
[root@localhost ~]# find /root -name "install.log?"
/root/install.log2
/root/install.log3
// 在 root 目录中查找 install.log 后带2或3的文件
[root@localhost ~]# find /root -name "install.log[23]"
/root/install.log2
/root/install.log3
```

```
// 不区分大小写搜索；Linux中严格区分大小写
[root@localhost ~]# find /root -iname install.log
/root/install.log
// 按照所有者搜索；
[root@localhost ~]# find /root -user root
/root
/root/anaconda-ks.cfg
/root/.bashrc
/root/install.log
/root/install.log2
/root/.bash_profile
/root/.bash_history
/root/.bash_logout
/root/install.log.syslog
/root/.cshrc
/root/install.log3
/root/.tcshrc
// 查找没有所有者的文件
[root@localhost ~]# find /root -nouser
[root@localhost ~]#
```

```
find /var/log/ -mtime +10
查找10天前修改的文件

-10 10天内修改的文件
10 10天当天修改的文件
+10 10天前修改的文件

atime 文件访问时间
ctime 改变文件属性时间
mtime 修改文件内容时间
```

```
find /root -size 25k
查找root目录中文件大小是25kb的文件

-25k 小于25kb的文件
25k 等于25kb的文件
+25k 大于25kb的文件

find /root -inum 262422
查找root目录中i节点是262422的文件
```

```
// 查找大于2k的文件；使用小写k
[root@localhost ~]# find /root -size +2k
/root
/root/install.log
/root/.bash_history
/root/install.log.syslog
// 查找小于2m的文件；使用大写M
[root@localhost ~]# find /root -size -2M
/root
/root/anaconda-ks.cfg
/root/.bashrc
/root/install.log
/root/install.log2
/root/.bash_profile
/root/.bash_history
/root/.bash_logout
/root/install.log.syslog
/root/.cshrc
/root/install.log3
/root/.tcshrc
```

```
find /etc -size +40k -a -size -50k
查找 etc 目录下，大于40kb并且小于50kb的文件
-a and 逻辑与，两个条件都满足
-o or 逻辑或，满足一个条件即可

find /etc -size +40k -a -size -50k -exec ls -l {} \;
查找 etc 目录下，大于40kb并且小于50kb的文件，并显示详细信息
在执行命令后面，加上 -exec ls -l {} \;
表明对命令执行结果进行后续命令的操作
```

```
[root@localhost ~]# find /etc -size +40k -a -size -50k
/etc/mime.types
/etc/selinux/targeted/modules/active/modules/staff.pp
// 对执行的命令结果再进行后续命令的操作
[root@localhost ~]# find /etc -size +40k -a -size -50k -exec ls -l {} \;
-rw-r--r--. 1 root root 43591 9月  23 2011 /etc/mime.types
-rw-------. 1 root root 42454 5月  21 08:11 /etc/selinux/targeted/modules/active/modules/staff.pp
```

## 字符串搜索命令 grep

```
grep [选项] 字符串 文件名
在指定文件当中匹配符合条件的字符串
选项：
-i 忽略大小写
-v 排除指定字符串
```

```
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log2  install.log3  install.log.syslog
// 在 anaconda-ks.cfg 文件中查找 size 字符串
[root@localhost ~]# grep "size" anaconda-ks.cfg
#part /boot --fstype=ext4 --size=200
#part /home --fstype=ext4 --size=2000
#part swap --size=1000
#part / --fstype=ext4 --grow --size=200
// 搜索不包含 size 的字符串
[root@localhost ~]# grep -v  "size" anaconda-ks.cfg
```

## find 命令与 grep 命令区别

find 命令：在系统当中搜索符合条件的文件名；
如果需要匹配，使用通配符匹配，通配符是完全匹配；

grep 命令：在文件当中搜索符合条件的字符串；
如果需要匹配，使用正则表达式匹配，正则表达式是包含匹配；
