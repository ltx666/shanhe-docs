---
title: "文件传输"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 5
---

#### 上传文件
###### （上传下载推荐使用支持断点续传的winscp，下载地址：https://winscp.net/eng/download.php）

##### 1. 使用工具上传(以mobaxterm工具举例)

通过mobaxterm工具连接到集群后，并根据下图 
1. 可以使用上方上传按钮进行上传
2. 可以将要上传的文件拖动到下方框中

<img src="/chaosuan/jnsupercomputer/supercomputerintroduction/_image/upload.png" width = "90%" />

##### 2. 使用工具上传(以winscp工具举例)

打开winscp工具，依次确认下图中红框中内容（用户名请填写实际名字，下方以username用户为例）

<img src="/chaosuan/jnsupercomputer/supercomputerintroduction/_image/upload1-1.png" width = "90%" />

##### 3. 使用scp命令上传（如mac,linux系统）
以下命令以传输当前文件夹下的filename.tar文件到，集群username账户的家目录里为例
```bash
scp filename.tar username@10.100.19.41:~/
```
