---
title: github添加ssh key
date: 2016-08-31 22:19:47
categories: study_blog
tags: github
---
*  ## 首先在电脑上创建ssh  

运行git bash，然后在终端输入以下命令：  

``` bash
$ ssh-keygen -t rsa -C "email@example.com"
```

说明：  
* -t 指定密钥类型，默认是 rsa ，可以省略。
* -C 设置注释文字，比如邮箱。
* -f 指定密钥文件存储文件名。

<!-- more -->

上面的命令忽略了 -f 参数，所以执行上述命令后，会提示你输入文件名，默认为id_rsa(推荐默认，直接回车)，如下：  

``` bash
Enter file in which to save the key (/c/Users/username/.ssh/id_rsa):
```

然后提示输入密码，并确认密码，此密码使用来push文件时的密码。可以不输，则push时无需输入密码。

* ## 在github上添加ssh

首先，copy刚刚生成的ssh key  

``` bash
$ clip < ~/.ssh/id_rsa.pub
```

然后，登录github账号，点击右上角头像选择“setting”，在新打开的页面左侧点击“SSH and GPG keys”。  
最后，在右侧页面点击“New SSH key”,输入title，以及key，title不输入默认为邮箱

* ## 测试

在git bash终端下输入：  

``` bash
$ ssh -T git@github.com
```

执行，可能会有警告，输入yes继续。然后输入创建ssh key时设置的密码（如果有）。  
如果出现用户名，则表示成功，提示信息如下：  

``` bash
Hi sniperbo! You‘ve successfully authenticated, but GitHub does not provide shell access.
```
