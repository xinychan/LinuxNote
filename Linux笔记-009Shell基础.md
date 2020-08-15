# Shell 基础

## 脚本执行方式

```
echo 输出命令
echo [选项] [输出内容]
选项：
-e 支持反斜线控制的字符转换
```

```
[root@localhost ~]# vi hello.sh
[root@localhost ~]# ls
anaconda-ks.cfg  hello.sh  install.log install.log.syslog
// 赋予执行权限，直接执行脚本
[root@localhost ~]# /root/hello.sh
-bash: /root/hello.sh: 权限不够
[root@localhost ~]# chmod 755 hello.sh
[root@localhost ~]# /root/hello.sh
exec hello sh
// 通过Bash调用执行脚本,无序赋予执行权限
[root@localhost ~]# bash hello.sh
exec hello sh
```

## 别名与快捷键

```
// 查看系统中所有的命令别名
[root@localhost ~]# alias

// 设定命令别名；临时生效
[root@localhost ~]# alias 别名= '原命令'

// 在 bashrc 文件中设定命令别名
// 别名写入环境变量配置文件；永久生效
[root@localhost ~]# vi /root/.bashrc

// 让修改的 bashrc 文件立即生效
[root@localhost ~]# source .bashrc

// 删除别名
[root@localhost ~]# unalias
```

```
// 查看系统中所有的命令别名
[root@localhost ~]# alias
alias cp='cp -i'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
```

```
// 设定命令别名
[root@localhost ~]# alias ls='ls --color=never'
[root@localhost ~]# alias
alias cp='cp -i'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=never'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
```

```
命令生效顺序
1-第一顺位执行用绝对路径或相对路径的命令
2-第二顺位执行别名
3-第三顺位执行Bash内部命令
4-第四顺位执行按照$PATH环境变量定义的目录查找顺序找到的第一个命令
```

```
常用快捷键
ctrl+c 强制终止当前命令
ctrl+l 清屏
ctrl+a 光标移动到命令行首
ctrl+e 光标移动到命令行尾
ctrl+u 从光标所在位置删除到行首
ctrl+z 把命令放入后台
ctrl+r 在历史命令中搜索
```

## 历史命令

```
// 查看历史命令
[root@localhost ~]# history
// 清空和保存命令
history [选项] [历史命令保存文件]
选项：
-c 清空历史命令
-w 把缓存中的历史命令写入历史命令保存文件 ~/.bash_history
```

## 输出重定向

| 类型               | 符号              | 作用                                           |
| ------------------ | ----------------- | ---------------------------------------------- |
| 标准输出重定向     | 命令 > 文件       | 以覆盖的方式，把命令的正确输出保存到指定文件中 |
| 标准输出重定向     | 命令 >> 文件      | 以追加的方式，把命令的正确输出保存到指定文件中 |
| 标准错误输出重定向 | 错误命令 2> 文件  | 以覆盖的方式，把命令的错误输出保存到指定文件中 |
| 标准错误输出重定向 | 错误命令 2>> 文件 | 以追加的方式，把命令的错误输出保存到指定文件中 |

```
// 输出正确信息
[root@localhost ~]# ifconfig > ifconfig.log
[root@localhost ~]# cat ifconfig.log
// 输出错误信息
[root@localhost ~]# ifconfig2 2> ifconfig.log
[root@localhost ~]# cat ifconfig.log
```

| 类型                       | 符号                      | 作用                                                       |
| -------------------------- | ------------------------- | ---------------------------------------------------------- |
| 正确输出和错误输出同时保存 | 命令 > 文件 2>&1          | 以覆盖的方式，把命令的正确输出和错误输出都保存到指定文件中 |
| 正确输出和错误输出同时保存 | 命令 >> 文件 2>&1         | 以追加的方式，把命令的正确输出和错误输出都保存到指定文件中 |
| 正确输出和错误输出同时保存 | 命令 &> 文件              | 以覆盖的方式，把命令的正确输出和错误输出都保存到指定文件中 |
| 正确输出和错误输出同时保存 | 命令 &>> 文件             | 以追加的方式，把命令的正确输出和错误输出都保存到指定文件中 |
| 正确输出和错误输出同时保存 | 命令 >> 文件 a 2>> 文件 b | 正确的输出追加到文件 a，错误的输出追加到文件 b             |

## 输入重定向

```
[root@localhost ~]# wc [选项] [文件名]
选项：
-c 统计字节数
-w 统计单词数
-l 统计行数
```

```
[root@localhost ~]# wc -l ifconfig.log
```

```
命令 < 文件：把文件作为命令的输入
命令 << 标识符:把标识符之间的内容作为命令的输入
```

```
[root@localhost ~]# wc < ifconfig.log
 2 10 72
[root@localhost ~]# wc << add
// 统计到 add 出现的地方作为输入内容
> asd
> das
> ddd
> add
 3  3 12
```

## 管道符

| 多命令执行符 | 格式               | 作用                                             |
| :----------: | ------------------ | ------------------------------------------------ |
|      ;       | 命令 a ; 命令 b    | 多个命令顺序执行，命令之间没有逻辑关系           |
|      &&      | 命令 a && 命令 b   | 逻辑与：命令 a 正确执行，命令 b 才会执行         |
|     \|\|     | 命令 a \|\| 命令 b | 逻辑或：命令 a 执行错误，命令 b 才会执行         |
|      \|      | 命令 a \| 命令 b   | 管道符：命令 a 的正确输出，作为命令 b 的操作对象 |

## 通配符

| 通配符 | 作用                                                                           |
| :----: | ------------------------------------------------------------------------------ |
|   ?    | 匹配一个任意字符                                                               |
|   \*   | 匹配 0 个或任意多个字符                                                        |
|   []   | 匹配中括号里任意一个字符。例如：[ab]代表匹配 a，或者匹配 b                     |
|  [-]   | 匹配中括号里任意一个字符，-代表一个范围。例如：[0-9]代表匹配数字               |
|  [^]   | 逻辑非，表示匹配不是中括号里的一个字符。例如：[^0-9]代表匹配一个不是数字的字符 |

| 特殊符号 | 作用                                                                                                                |
| :------: | ------------------------------------------------------------------------------------------------------------------- |
|    ''    | 单引号。单引号中所有的特殊符号，都没有特殊含义，仅表示字符串                                                        |
|    ""    | 双引号。双引号中只有"\$""\`""\"几个特殊符号有含义，其余仅表示字符串。 "\$"=调用变量的值， "\`"=引用命令，"\"=转义符 |
|   \`\`   | 反引号。反引号代表系统命令，在 Bash 中会先执行。作用同 &()                                                          |
|   \$()   | 作用同反引号，用于引用系统命令                                                                                      |
|    #     | Shell 脚本中，#开头的行代表注释                                                                                     |
|    \$    | 调用变量的值                                                                                                        |
|    \     | 转义符。转义符后面连接的特殊符号，不再有特殊含义，成为普通字符                                                      |
