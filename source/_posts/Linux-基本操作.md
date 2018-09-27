---
title: Linux 基本操作
date: 2018-04-28 10:13:48
tags:
---

这里以生产上主流的 Centos 为目标进行学习<!--more-->

# 权限管理

## 用户和用户组

![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043912.jpg)
![](https://blog-image-1257302654.cos.ap-guangzhou.myqcloud.com/2018-08-24-043831.jpg)

最早是没有 `gshadow`和 `shadow`两个文件的,这两个分别是存放组密码和用户密码的文件.早起组和用户
密码都是分别放在 `group`和`password`一起的,但是由于这两个文件经常需要被读取来判断当期那用户所属
的权限和组权限.所以权限不能太严格.处于安全的考虑,就把密码信息单独拿出来放到`gshadow`和`shadow`
中.


# 资源查看
## 查看内存
`free -m` 以 MB 的方式查看内存使用情况

##查看 CPU 情况
`cat /proc/cpuinfo` 查看 CPU情况

# 权限管理
## 更改文件,文件夹所属用户和用户组,需要 root 权限执行该命令
```shell
Change user and group ownership of files and folders.

- Change the owner user of a file/folder:
    chown user path/to/file

- Change the owner user and group of a file/folder:
    chown user:group path/to/file

- Recursively change the owner of a folder and its contents:
    chown -R user path/to/folder

- Change the owner of a symbolic link:
    chown -h user path/to/symlink

- Change the owner of a file/folder to match a reference file:
    chown --reference=path/to/reference_file path/to/file
```

## 更改文件权限
```
Change the access permissions of a file or directory.

- Give the [u]ser who owns a file the right to e[x]ecute it:
    chmod u+x file

- Give the user rights to [r]ead and [w]rite to a file/directory:
    chmod u+rw file

- Remove executable rights from the [g]roup:
    chmod g-x file

- Give [a]ll users rights to read and execute:
    chmod a+rx file

- Give [o]thers (not in the file owner's group) the same rights as the group:
    chmod o=g file

- Change permissions recursively giving [g]roup and [o]thers the abililty to [w]rite:
    chmod -R g+w,o+w directory
```

# 定时任务 Crontab
## 基本用法
参考 `tldr crontab`

## 使用日期时间命名重定向文件
```shell
0 12 * * * php /Users/fdipzone/test.php >> "/Users/fdipzone/$(date +"%Y-%m-%d").log" 2>&1
```

`2>&1` 表示把标准错误输出重定向到与标准输出一致，即test.log

`>>`表示追加
[参考这篇博文](https://blog.csdn.net/fdipzone/article/details/51778543)
