# 关机与重启命令

## shutdown 命令

```
shutdown [选项] [时间]
选项：
-c 取消前一个关机命令
-h 关机
-r 重启
```

```
[root@localhost ~]# shutdown -h now
[root@localhost ~]# shutdown -r now
```

```
其他关机命令：
[root@localhost ~]# halt
[root@localhost ~]# poweroff
[root@localhost ~]# init 0
```

```
其他重启命令：
[root@localhost ~]# reboot
[root@localhost ~]# init 6
```

```
系统运行界别
0 关机
1 单用户
2 不完全多用户，不含NFC服务
3 完全多用户
4 未分配
5 图形界面
6 重启
```

```
// 查询当前运行级别；N（Null）代表之前的级别，3代表当前的级别
[root@localhost ~]# runlevel
N 3
```

```
// 查看系统开机时默认进入的级别
[root@localhost ~]# cat /etc/inittab
id:3:initdefault:
```

## 退出登录命令

```
[root@localhost ~]# logout
```