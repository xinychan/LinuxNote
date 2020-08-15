# 其他常用命令

## 挂载命令

```
// 查看挂载与自动挂载
// 查询系统中已经挂载的设备
[root@localhost ~]# mount
// 依据配置文件 /etc/fstab 的内容，自动挂载
[root@localhost ~]# mount -a
```

## 挂载命令格式

```
[root@localhost ~]# mount [-t 文件系统] [-o 特殊选项] [设备文件名] [挂载点]
选项：
-t 文件系统：加入文件系统类型来指定挂载的类型，可以ext3，ext4，iso9660等文件系统
-o 特殊选项：可以指定挂载的额外选项
```

```
// 卸载命令
[root@localhost ~]# umount [设备文件名或挂载点]
```

## 用户登录查看

```
命令格式：
[root@localhost ~]# w
输出结果：
USER 登录的用户名
TTY 登录终端；tty1 表示本地登录， pts/0 表示第一个远程登录
FROM 从哪个ip地址登录
LOGIN@ 登录时间
IDLE 用户闲置时间
JCPU 该终端连接的所有进程占用的时间
PCPU 当前进程所占用的时间
WHAT 当前正在运行的命令
```

```
[root@localhost ~]# w
 01:01:02 up 18:03,  2 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
root     tty1     -                Fri07   50:09   0.06s  0.06s -bash
root     pts/0    192.168.*.*      00:11    0.00s  0.08s  0.04s w
[root@localhost ~]#
```

```
// 查看登录用户信息
[root@localhost ~]# who
root     tty1         2020-06-05 07:33
root     pts/0        2020-06-06 00:11 (192.168.*.*)
```

```
// 查看当前登录和过去登录的用户信息
// last 命令默认读取 /var/log/wtmp 文件数据
// 输出结果：用户名，登录终端，登录IP，登录时间，退出时间（在线时间）
[root@localhost ~]# last
root     tty1                          Thu May 21 08:14 - down   (01:13)
```

```
// 查看所有用户最后一次登录信息
// lastlog 命令默认读取 /var/log/lastlog 文件数据
// 输出结果：用户名，登录终端，登录IP，最后登录时间
[root@localhost ~]# lastlog
用户名           端口     来自             最后登陆时间
root             pts/0     192.168.*.*     六 6月  6 00:11:08 +0800 2020
bin                                        **从未登录过**
```