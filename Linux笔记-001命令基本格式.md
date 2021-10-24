# Linux 命令基本格式

## 命令提示符

```
[root@localhost ~]#
```

提示符含义：

| 提示符    | 含义                                 |
| --------- | ------------------------------------ |
| root      | 当前登录用户                         |
| localhost | 主机名称                             |
| ~         | 当前所在目录                         |
| \#        | 超级用户的提示符，普通用户提示符是\$ |

## 命令格式

```
命令 [选项] [参数]
```

- 大部分命令都符合上述格式，少数个别命令的使用不遵循此格式。
- 当有多个选项时，选项可以写在一起。
- 有简化选项与完整选项之分。比如 [-a] 等同于 [-all]

## 查询目录中的内容：ls

```
ls [选项] [文件或目录]
```

选项：

- -a：显示所有文件，包括隐藏文件
- -l：显示详细信息
- -d：查看目录属性
- -h：人性化显示文件大小
- -i：显示 inode

比如执行如下命令，系统返回如下信息：

```
[root@localhost ~]# ls -l
总用量 44
-rw-------. 1 root root  1208 5月  21 08:13 anaconda-ks.cfg
-rw-r--r--. 1 root root 24772 5月  21 08:13 install.log
-rw-r--r--. 1 root root  7690 5月  21 08:13 install.log.syslog
```

[-rw-r--r--.][1] [root][root] [24772][5月 21 08:13] [install.log]

如上所述，将文件信息以`[]`分成七个部分。

第一部分[-rw-------.]代表文件的权限。
第 1 位，表示文件类型。（ - 文件 d 目录 l 软连接文件）
Linux 系统中主要有七种文件类型，常见的为上述的三种。
其他四种文件类型为块设备文件，字符设备文件，套接字文件，管道文件。这四种文件一般普通用户不需要操作。
在 Windows 系统中，有各种各样的文件类型，支持各种文件扩展名，比如 `xxx.md` `xxx.txt` 等格式。
Linux 系统中文件没有扩展名的概念，系统也不是靠扩展名来区分文件。
只是基于通用规范，一般会给文件名取相应的后缀，便于操作人员识别，但是系统并不依据文件名后缀来区分文件类型。
第 2-4 位，表示文件的所有者的权限。
第 5-7 位，表示文件的所有者所在群组的权限。
第 8-10 位，表示其他用户的权限。
第 11 位，CentOS6 之后出现的权限，表示 ACL 权限。[Access Control List（访问控制列表）]针对三种身份的权限处理不够用的情况。
第 2-10 位，每 3 位用[rwx]来表示，[r]表示有读取权限，[w]表示有写入权限，[x]表示有执行权限。
如果 3 位中任意一项是[-]，表示该项的权限不具备。
第二部分[1]表示引用计数，代表文件被调用过几次。
第三部分[root]表示该文件的所有者。
第四部分[root]表示该文件的所有者所在的群组。
第五部分[24772]表示该文件的大小。默认大小为 byte。
第六部分[5 月 21 08:13]表示文件最后一次修改的时间。
第七部分[install.log]表示文件的名称。

如下命令表示列出所有文件，包括隐藏文件。
其中以 [.] 开头的都是隐藏该文件，比如 [.bashrc]

```
[root@localhost ~]# ls -a
.   anaconda-ks.cfg  .bash_logout   .bashrc  install.log         .tcshrc
..  .bash_history    .bash_profile  .cshrc   install.log.syslog
```

## Linux 中的权限

chmod 命令：
Linux chmod（英文全拼：change mode）命令是控制用户对文件的权限的命令
Linux/Unix 的文件调用权限分为三级 : 文件所有者（user）、用户组（group）、其它用户（others）
文件权限类型分为三类： r 表示可读取，w 表示可写入，x 表示可执行
可以使用绝对模式（八进制数字模式），符号模式指定文件的权限

绝对模式（八进制数字模式）：r=4/w=2/x=1

```shell
# 给 test01.txt 文件的user/group/others添加rwx权限（7=4+2+1）
chmod 777 test01.txt
```

符号模式：u=user/g=group/o=others/a=all

```shell
# 给 test01.txt 文件的所有用户添加rwx权限
chmod a+rwx test01.txt
# 给 test01.txt 文件的所有用户删除x权限
chmod a-x test01.txt
```

chown 命令：
Linux chown（英文全拼：change owner）命令用于设置文件所有者和文件关联组的命令
chown 需要超级用户 root 的权限才能执行此命令

```shell
# 把 test01.txt 文件的所有者设置为 root
chown root test01.txt
# 把 test01.txt 文件的所有者设置为 root，所在组设置为root
chown root:root test01.txt
# 将当前目录下的所有文件与子目录的所有者设置为 root
chown -R root *
# 将当前目录下的所有文件与子目录的所有者设置为 root，所在组设置为root
chown -R root:root *
```

## 环境变量

临时环境变量：关闭 SSH 会话，变量失效

```shell
export num=100
echo $num
100
```

全局环境变量：保存在 /etc/profile 文件中

```shell
# 编辑 etc/profile 文件
vim etc/profile
# 修改 etc/profile 生效
source profile
echo $name
name
```
