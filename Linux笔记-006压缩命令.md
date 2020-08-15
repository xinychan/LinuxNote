# 压缩命令

Linux 下常用压缩格式
常用压缩格式： .zip .gz .bz2
常用压缩格式： .tar.gz .tar.bz2

## zip 压缩命令

```
zip [压缩文件名] [源文件]
用于压缩文件

zip -r [压缩文件名] [源目录]
用于压缩目录

unzip [压缩文件]
解压缩zip文件
```

## gz 压缩命令

```
gzip [源文件]
压缩为gz格式的压缩文件，源文件会删除

gzip -c [源文件] > [压缩文件]
压缩为gz格式的压缩文件，源文件会保留

gzip -r [目录]
压缩目录下所有子文件，但是不能压缩目录

gzip -d [压缩文件]
解压缩文件

gunzip [压缩文件]
解压缩文件
```

## bz2 压缩命令

```
bzip2 [源文件]
压缩为bz2格式的压缩文件，源文件会删除

bzip2 -k [源文件]
压缩为bz2格式的压缩文件，源文件会保留

注：bzip2命令不能压缩目录

bzip2 -d [压缩文件]
解压缩文件；-k 保留压缩文件

bunzip2 [压缩文件]
解压缩文件；-k 保留压缩文件
```

## tar 打包命令

```
tar -cvf [打包文件名] [源文件/目录]
解决目录不能被压缩成 gz/bz2 的问题
选项：
-c 打包
-v 显示过程
-f 指定打包后的文件名
```

```
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log2  install.log3  install.log.syslog  testfold
[root@localhost ~]# tar -cvf testfold.tar testfold
testfold/
testfold/fold2
testfold/fold3
testfold/fold1
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log2  install.log3  install.log.syslog  testfold  testfold.tar
[root@localhost ~]# bzip2 -k testfold.tar
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log2  install.log3  install.log.syslog  testfold  testfold.tar  testfold.tar.bz2
[root@localhost ~]# gzip -c testfold.tar > testfold.tar.gz
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log2  install.log3  install.log.syslog  testfold  testfold.tar  testfold.tar.bz2  testfold.tar.gz
```

## tar 解打包命令

```
tar -xvf [打包文件名]
选项：
-x 解打包
```

```
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log2  install.log3  install.log.syslog  testfold.tar  testfold.tar.bz2  testfold.tar.gz
[root@localhost ~]# tar -xvf testfold.tar
testfold/
testfold/fold2
testfold/fold3
testfold/fold1
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log2  install.log3  install.log.syslog  testfold  testfold.tar  testfold.tar.bz2  testfold.tar.gz
```

## tar.gz/tar.bz2 打包命令

```
tar -zcvf [压缩包名.tar.gz] [源文件/目录]
选项：
-z 压缩为.tar.gz格式

tar -zxvf [压缩包名.tar.gz]
选项：
-x 解压缩.tar.gz格式
```

```
tar -jcvf [压缩包名.tar.bz2] [源文件/目录]
选项：
-j 压缩为.tar.bz2格式

tar -jxvf [压缩包名.tar.bz2]
选项：
-x 解压缩.tar.bz2格式
```

```
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log2  install.log3  install.log.syslog  testfold
[root@localhost ~]# tar -zcvf testfold.tar.gz testfold
testfold/
testfold/fold2
testfold/fold3
testfold/fold1
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log2  install.log3  install.log.syslog  testfold  testfold.tar.gz
[root@localhost ~]# tar -jcvf testfold.tar.bz2 testfold
testfold/
testfold/fold2
testfold/fold3
testfold/fold1
[root@localhost ~]# ls
anaconda-ks.cfg  install.log  install.log2  install.log3  install.log.syslog  testfold  testfold.tar.bz2  testfold.tar.gz
```
