---
title: Git使用方法
date: 2018-10-06 16:07:40
categories:
- git学习
tags:
- git
---

## Usage of git

### 1.Git Tools - Submodules
#### 1.Why you should use sumbmodules
```
It often happens that while working on one project, you need to use another project from within it. Perhaps it’s a library that 
a third party developed or that you’re developing separately and 
using in multiple parent projects. A common issue arises in these scenarios: you want to be able to treat the two projects 
as separate yet still be able to use one from within the other.
```
#### 2.Starting with Submodules
```
$ git submodule add https://github.com/chaconinc/DbConnector
Cloning into 'DbConnector'...
remote: Counting objects: 11, done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 11 (delta 0), reused 11 (delta 0)
Unpacking objects: 100% (11/11), done.
Checking connectivity... done.
```
<!-- more -->
#### 3.How to clone a project with a sumbmodule
```
$ git submodule init
Submodule 'DbConnector' (https://github.com/chaconinc/DbConnector) registered for path 'DbConnector'
$ git submodule update
Cloning into 'DbConnector'...
remote: Counting objects: 11, done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 11 (delta 0), reused 11 (delta 0)
Unpacking objects: 100% (11/11), done.
Checking connectivity... done.
Submodule path 'DbConnector': checked out 'c3f01dc8862123d317dd46284b05b6892c7b29bc'
```

### 2.Set your username
1.Open Git Bash.

2.Set a Git username:
```
git config --global user.name "Mona Lisa"
```

3.Confirm that you have set the Git username correctly:
```
git config --global user.name
```

4.Set your email 
```
git config --global user.email yzihong307@gmail.com
```

### 3. Git log 
1.Get the note that you can get a documentation 
```
man git-log 
```
```
git help log 
```




