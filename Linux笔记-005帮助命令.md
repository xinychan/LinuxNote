# 帮助命令

## 帮助命令 man

```
man = manual
man [命令]
获取指定命令的帮助

man ls
查看 ls 命令的帮助
```

```
man -f [命令]
相当于
whatis [命令]
查找命令所有等级的帮助
```

```
[root@localhost ~]# man -f passwd
// (1)(5)表明命令的不同等级的作用
passwd               (1)  - update user's authentication tokens
passwd               (5)  - password file
passwd [sslpasswd]   (1ssl)  - compute password hashes
[root@localhost ~]# whatis passwd
passwd               (1)  - update user's authentication tokens
passwd               (5)  - password file
passwd [sslpasswd]   (1ssl)  - compute password hashes
```

```
man 命令的级别
1 查看命令的帮助
2 查看可被内核调用的函数的帮助
3 查看函数和函数库的帮助
4 查看特殊文件的帮助
5 查看配置文件的帮助
6 查看游戏的帮助
7 查看其他杂项的帮助
8 查看系统管理员可用命令的帮助
9 查看内核相关文件的帮助
```

```
// 查看命令的不同等级的帮助
[root@localhost ~]# man 1 passwd
[root@localhost ~]# man 5 passwd
```

```
man -k [命令]
相当于
apropos [命令]
查找包含指定命令的所有命令的帮助
```

## 其他帮助命令

```
[命令] --help
获取命令选项的帮助
```

```
[root@localhost ~]# ls --help
```

```
help shell内部命令
获取shell内部命令的帮助

whereis cd
是否是shell内部命令
help cd
获取内部命令帮助
```

```
// 有文件路径位置，不是shell内部命令
[root@localhost ~]# whereis ls
ls: /bin/ls /usr/share/man/man1/ls.1.gz /usr/share/man/man1p/ls.1p.gz
// 无文件路径位置，是shell内部命令
[root@localhost ~]# whereis cd
cd: /usr/share/man/man1/cd.1.gz /usr/share/man/man1p/cd.1p.gz
```

```
info [命令]
- 回车 ：进入子帮助页面（带有*标记）
- u ：进入上层页面
- n ：进入下一个帮助小结
- p ：进入上一个帮助小结
- q ：退出
```