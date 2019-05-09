---
title: 常用命令记录
date: 2019-05-09 17:16:14
tags:
- linux
---

# git 命令

1. 打包某次提交的文件
```
git archive --format=zip HEAD `git diff --name-only 8bbf69c253801228ff504ab080ce7cf44a924971 a27d045d8c60d6c62a4061b94763886577e1c0eb` > a.zip
```

2. 常用命令
```
# 拉取文件
git pull

# 查看本地变动文件
git status

# 添加文件到git版本关键(.* 表示全部，建议后面跟需要的文件)
git add .* 

# 提交到本地仓库
git commit -m 'update'

# 推送文件
git push origin master 
```

3. 使用github托管静态资源

```
// --prefix=表示需要把那个文件夹提交到gh-pages
git subtree push --prefix=dist origin gh-pages
```
>通过 你的github名称.github.io/你的项目名称 可以访问你的项目

例如：https://mylovegy.github.io/gallery/

# openssl 证书格式互转
1. p12 转 pem
```
openssl pkcs12 -in 文件名.p12 -out 输出文件.pem -nodes
```
2. cer 转 pem
```
openssl x509 -inform der -in 文件名.cer -out 输出文件名.pem
```
