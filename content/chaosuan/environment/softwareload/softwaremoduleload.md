---
title: "软件加载"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 3
---

## 软件环境加载

***

###### 软件不能直接使用，需要将软件的可执行文件路径等添加到对应的环境变量后才能使用。
###### **module**是一款环境变量管理工具，通过module实现软件环境变量的管理，快速加载和切换软件环境。

<br>

```bash
# module 常用命令
module avail      # 列出所有软件环境
module list       # 列出所有已加载的软件环境
module load       # 加载软件环境
module unload     # 删除相应的软件环境
```

***

##### （1）module avail 列出所有软件环境，可缩写为 module av
![](/chaosuan/environment/_image/module-av.png)
<br>

##### （2）module load 加载软件环境,命令后跟要加载的软件，以加载 gcc/11.2.0 为例
![](/chaosuan/environment/_image/module-load.png)
<br>

##### （3）module list 列出所有已加载的软件环境
![](/chaosuan/environment/_image/module-list.png)
<br>

##### （4）module unload 删除相应的软件环境，命令后跟要取消加载的软件，还是以 gcc/11.2.0 为例
![](/chaosuan/environment/_image/module-unload.png)
<br>




