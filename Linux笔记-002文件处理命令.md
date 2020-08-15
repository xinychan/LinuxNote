# 文件处理命令

## 常用目录的作用

* / 根目录
* /bin 命令保存目录（普通用户可以读取的命令）
* /boot 启动目录，启动相关文件
* /dev 设备文件目录
* /etc 配置文件保存目录
* /home 普通用户的家目录
* /lib 系统库保存目录
* /mnt 系统挂载目录
* /media 挂载目录
* /root 超级用户的家目录
* /tmp 临时目录
* /sbin 命令保存目录（超级用户才可以使用的命令）
* /proc 直接写入内存目录，保存内存的过载点
* /sys 直接写入内存目录，保存内存的过载点
* /usr 系统软件资源目录
    - /usr/bin/系统命令（普通用户）
    - /usr/sbin/系统命令（超级用户）
* /var 系统相关文档目录

## 创建目录命令 mkdir

```
mkdir -p [目录名]
-p 表示递归创建，若创建多层文件，使用-p参数
若单独创建一个目录，则可以直接用 mkdir [目录名]
```

如下示例命令：

```
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log.syslog
[root@localhost ~]# mkdir xiaoming
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log.syslog  xiaoming
[root@localhost ~]# mkdir student/xiaoming2
mkdir: 无法创建目录"student/xiaoming2": 没有那个文件或目录
[root@localhost ~]# mkdir -p student/xiaoming2
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log.syslog  student  xiaoming
```

## 切换目录命令 cd

```
cd [目录]
命令英文原意：change directory
```

```
简化操作：
cd ~    进入当前用户的家目录
cd      进入当前用户的家目录
cd -    进入上次目录
cd ..   进入上一级目录
cd .    进入当前目录
```

相对路径：参照当前所在目录，进行查找。
绝对路径：从根目录开始指定，一级一级递归查找。在任何目录下，都能进入指定位置。
建议优先绝对路径，避免路径错乱，使用了错误目录。

## 查询所在目录 pwd

```
[root@localhost ~]# pwd
/root
```

pwd = print working directory

## 删除空目录 rmdir

```
rmdir [目录名]
只能删除一个空的目录，如果目录中有数据，则无法删除
```

```
[root@localhost ~]# ls
anaconda-ks.cfg  class  install.log  install.log.syslog
[root@localhost ~]# rmdir class/
rmdir: 删除 "class/" 失败: 目录非空
[root@localhost ~]# mkdir class2
[root@localhost ~]# ls
anaconda-ks.cfg  class  class2  install.log  install.log.syslog
[root@localhost ~]# rmdir class2
[root@localhost ~]# ls
anaconda-ks.cfg  class  install.log  install.log.syslog
```

## 删除文件或目录 rm

```
rm -rf [文件目录]

选项：
-r  删除目录
-f  强制
```

```
[root@localhost ~]# mkdir class
[root@localhost ~]# ls
anaconda-ks.cfg  class  install.log  install.log.syslog
[root@localhost ~]# rm -rf class/
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log.syslog
```

## 复制文件 cp

```
cp [选项][原文件或目录][目标目录]

选项：
-r  复制目录
-p  连带文件属性复制
-d  若源文件是链接文件，则复制链接属性
-a  相当于 -pdr
```

一般使用 -a 使用所有选项

```
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log.syslog
[root@localhost ~]# cp anaconda-ks.cfg  /tmp/anaconda
[root@localhost ~]# ls /tmp/
anaconda
[root@localhost ~]# cp -a anaconda-ks.cfg /tmp/
[root@localhost ~]# ls /tmp/
anaconda  anaconda-ks.cfg
[root@localhost ~]# ll /tmp/
总用量 8
-rw-------. 1 root root 1208 6月   5 07:52 anaconda
-rw-------. 1 root root 1208 5月  21 08:13 anaconda-ks.cfg
```

## 剪切或改名文件 mv

```
mv [原文件或目录][目标目录]
```

原文件与目标目录在同一目录下，则改名

```
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log.syslog
[root@localhost ~]# cd /tmp/
[root@localhost tmp]# ls
anaconda  anaconda-ks.cfg
[root@localhost tmp]# mv anaconda anaconda2
[root@localhost tmp]# ls
anaconda2  anaconda-ks.cfg
```

原文件与目标目录不在同一目录下，则剪切

```
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log.syslog
[root@localhost ~]# mkdir class
[root@localhost ~]# ls
anaconda-ks.cfg  class  install.log  install.log.syslog
[root@localhost ~]# mv class /tmp/
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log.syslog
[root@localhost ~]# cd /tmp/
[root@localhost tmp]# ls
anaconda2  anaconda-ks.cfg  class
```

## 修改文件时间或创建文件 touch

```
touch [选项] [目标文件]
修改文件或者目录的时间属性，若文件不存在则创建文件
选项：
a 改变档案的读取时间记录。
m 改变档案的修改时间记录。
c 假如目的档案不存在，不会建立新的档案。与 --no-create 的效果一样。
f 不使用，是为了与其他 unix 系统的相容性而保留。
r 使用参考档的时间记录，与 --file 的效果一样。
d 设定时间与日期，可以使用各种不同的格式。
t 设定档案的时间记录，格式与 date 指令相同。
```

创建文件

```
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log.syslog
[root@localhost ~]# touch test.log
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log.syslog  test.log
```

修改文件时间

```
[root@localhost ~]# ls -l
总用量 44
-rw-------. 1 root root  1208 5月  21 08:13 anaconda-ks.cfg
-rw-r--r--. 2 root root 24772 5月  21 08:13 install.log
-rw-r--r--. 1 root root  7690 5月  21 08:13 install.log.syslog
-rw-r--r--. 1 root root     0 6月   5 08:50 test.log
[root@localhost ~]# touch test.log
[root@localhost ~]# ls -l
总用量 44
-rw-------. 1 root root  1208 5月  21 08:13 anaconda-ks.cfg
-rw-r--r--. 2 root root 24772 5月  21 08:13 install.log
-rw-r--r--. 1 root root  7690 5月  21 08:13 install.log.syslog
-rw-r--r--. 1 root root     0 6月   5 08:51 test.log

```

## 显示文件内容 cat

```
cat [选项] [文件]
cat主要有三大功能：
1.一次显示整个文件:cat filename
2.从键盘创建一个文件:cat > filename 只能创建新文件,不能编辑已有文件.
3.将几个文件合并为一个文件:cat file1 file2 > file
选项：
-n 或 --number：由 1 开始对所有输出的行数编号。
-b 或 --number-nonblank：和 -n 相似，只不过对于空白行不编号。
-s 或 --squeeze-blank：当遇到有连续两行以上的空白行，就代换为一行的空白行。
-v 或 --show-nonprinting：使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外。
-E 或 --show-ends : 在每行结束处显示 $。
-T 或 --show-tabs: 将 TAB 字符显示为 ^I。
-A, --show-all：等价于 -vET。
-e：等价于"-vE"选项；
-t：等价于"-vT"选项；
```

显示文件内容
```
[root@localhost ~]# cat test.log
[root@localhost ~]# echo "test echo" > test.log
[root@localhost ~]# cat test.log
test echo
```

## 打印指定内容或者写入指定内容到文件 echo

```
echo "content"
打印指定内容
echo "content" > file
写入指定内容到文件
```

打印指定内容

```
[root@localhost ~]# echo "log"
log
```

写入指定内容到文件

```
[root@localhost ~]# echo "test echo" > test.log
[root@localhost ~]# cat test.log
test echo
```
