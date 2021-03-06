---
title: 学习Linux基本命令
date: 2018-12-06 13:34:03
top: 100
tags:
- Linux 命令
categories:
- Linux
---
## 前言

本文主要介绍常用的 Linux 命令。

<!-- more -->

## Linux系统

* `pwd` 打印当前工作目录
* `cd` 改变目录

```shell
    cd /usr/bin  绝对路径从根目录出发，到达目标目录
    cd ./usr 相对路径从工作目录出发，到达目标目录
    cd .. 到达父目录
    cd(cd ~) 到达家目录，如果未root用户，pwd会打印出 /root，其上一层为 根目录/
    cd / (cd -) 回到根目录


```

* `ls` 列出目录内容

```shell
    ls -l 使用长格式显示结果
    ls -t 按修改时间排序
    ls -r 以相反的顺序显示
    ls -S 按文件大小对结果进行排序
    ls -R [文件夹] 列出文件树
    ......
```

* `file` 确定文件类型

```shell
    file filename
```

* `less` 查看文件内容

```shell
    less /etc/passwd
```

* `touch` 新建文件

## 操作文件与目录

* `mkdir` 创建目录

```shell
    mkdir dir1              创建单个目录
    mkdir dir1 dir2 dir3    创建多个目录
    mkdir -p dir{1..9}      创建多个目录a1到a9
    mkdir -p a{1..3}/b{1..3}创建多个目录a1到a3，并且在每个目录下创建b1到b3
```

* `cp` 复制文件或目录

```shell
    cp file1 file2          将文件file1复制到file2中,file2内容将会被覆盖
    cp -r dir1 dir2         复制目录时一定要加 -r，如果dir2目录存在，则会复制到dir2目录下和mv是一样的道理
    cp file1 file2 dir1     将多个文件复制到一个目录下
```

`cp`命令选项

`cp`在覆盖已存在的文件时默认情况下是 `cp -i`，即需要用户确认，我们可以这样 `\cp` 即可无需确认

```shell
    -i          在覆盖一个已存在的文件前，提示用户进行确认。
    -r          递归复制目录及其内容。复制目录时需要这个选项
    -u          将文件从一个目录复制到另一个目录时，只会复制目标目录不存在的文件或是目标目录相应文件的更新文件
    -v          复制文件时显示信息性消息
```

* `mv` 重命名或移动文件和目录

```shell
    mv item1 item2              将文件或目录item1移动或重命名为item2
    mv item1 item2 item3 dir1   将多个条目移动到dir1目录下
```

`mv`命令选项与`cp`大致相同，`mv`没有`-r`选项

```shell
    -i          在覆盖一个已存在的文件前，提示用户进行确认。
    -u          将文件从一个目录移动到另一个目录时，只会移动目标目录不存在的文件或是目标目录相应文件的更新文件
    -v          移动时显示信息性消息
```

* `rm` 删除文件或目录

```shell
    rm -r item1 item2 item3         删除item1,item2,item3,删除目录时需要-r
    rm *.html                       删除以.html结尾的文件
```

`rm`命令选项

```shell
    -i          删除前提示用户确认
    -r          递归删除目录及其内容。删除目录时需要这个选项
    -f          忽略不存在的文件，并无需提示确认
    -v          删除时显示信息性消息
```

* `ln` 创建硬链接和符号链接

```shell
    ln file hard-link-name      创建file文件的硬链接
    ln -s file sym-link-name    创建file文件的符号链接，符号链接指向源文件，与源文件内容保持一致
```

`file`为相对于`sym-link-name`的文件，即为相对路径，当然也可以是绝对路径

```shell
    ln -s ../file sym-link-name     file在当前目录的父目录中，即file相对于sym-link-name的位置
```

# 读写文件

```shell
echo "I am fine"                        打印 I am fine
echo "I am fine" > /root/test.txt       将 I am fine写入/root/test.txt中
echo "I am fine" >> /root/test.txt      将 I am fine追加到/root/test.txt末尾
grep "关键字" test.txt                  在test.txt中查找含有关键字的行并打印
grep -v "关键字" test.txt               在test.txt中查找不含有关键字的行并打印
grep ^"关键字" test.txt                 在test.txt中查找以关键字开头的行并打印
grep $"关键字" test.txt                 在test.txt中查找以关键字结尾的行并打印
```

## 管道

```shell
ls -l | grep "关键字" > /root/test.txt  列出当前目录文件信息并交给grep过滤，最后写入/root/test.txt
```
