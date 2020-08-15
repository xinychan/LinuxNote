# 链接命令

```
ln -s [原文件][目标文件]
用于生成链接文件
选项 -s 创建软链接
不带选项，创建硬链接
```

## 软连接

- 类似 Windows 快捷方式
- 软链接拥有自己的 i 节点和 block 块，但数据块中只保存原文件的文件名和 i 节点号，不保存实际的文件数据
- lrwxrwxrwx l 软连接：软连接文件权限都为 rwxrwxrwx
- 修改任意文件，另一个都改变
- 删除原文件，软连接不可用

## 硬链接

- 拥有相同的 i 节点和存储 block 块，可以看做是同一个文件
- 可通过 i 节点识别，因为 i 节点相同
- 不能跨分区
- 不能针对目录使用
- 删除其中一个硬链接或原文件，其余硬链接不受影响，仍然可用

```
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log.syslog
[root@localhost ~]# ln install.log /tmp/install.hard
[root@localhost ~]# ln -s install.log /tmp/install.soft
[root@localhost ~]# cd /tmp
[root@localhost tmp]# ll
总用量 32
-rw-r--r--. 2 root root 24772 5月  21 08:13 install.hard
lrwxrwxrwx. 1 root root    11 6月   5 08:37 install.soft -> install.log
```
