---
title: Linux通过rm删除文件空间并未释放
tags:
  - Linux
  - rm -rf
  - 释放磁盘空间
categories: Linux
cover: img/fengmian/linux.png
abbrlink: d0f5c704
date: 2023-03-14 11:20:19
---
# Linux通过rm删除文件空间并未释放

在使用rm -rf等相关操作对Linux某些文件进行删除后只是将文件的目录项在文件系统中删除，目录项是一个指向文件数据块的指针，当目录项被删除时，文件数据块不会再被文件系统引用。

当删除一个文件时，文件系统会将该文件目录项标记为"已删除"，然后将文件数据块的内容保留在磁盘上，方便可以进行恢复。但是，已删除的文件数据块通常被标记为"未分配"，以便新文件可以重用

如果有进程正在使用被删除的文件，则该文件将继续占用磁盘空间，直到进程关闭文件句柄或退出，才能释放磁盘空间(直到关闭了对该文件的所有引用，当引用计数器为0时，文件数据块才会被释放标记为"未分配" 引用计数器机制)。但是Linux使用的是基于块的文件系统，通常数据块被分成多个部分存储，每个部分通常叫做文件分配表。当删除一个文件时，只有部分磁盘空间被释放

如果想立即释放被删除文件所占用的磁盘空间，可以使用`sync`命令刷新文件系统缓存，并使用`lsof`命令查找正在使用被删除的文件进程。然后通过结束进程来释放磁盘空间

通过`lsof` 命令可以查看所有打开的文件，显示有关进程和它们打开的文件的详细信息，这对于解决与文件访问有关的问题或识别消耗系统资源的进程有很大的帮助

几个常用的例子：

- 列出所有打开的文件：`lsof`
- 列出特定进程的所有打开文件：`lsof -p <PID> `
- 列出特定用户的所有打开文件：`lsof -u <username>`
- 列出特定端口的所有打开文件：`lsof -i :<PORT>`
- 列出特定文件的所有打开文件：`lsof /PATH/to/FILE`

通过`lsof -n | grep deleted`命令列出所有已经被删除但是仍然被一个或多个进程保持打开状态的文件

**建议使用正常方法退出进程，让系统自动回收磁盘空间**

不建议重启系统、强制结束进程但也可以通过`echo "" > /path/to/file`清空文件