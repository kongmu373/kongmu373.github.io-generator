---
title: "Git常用命令"
date: 2020-05-25T14:00:44+08:00
draft: false
---
## git
### git 六行配置
```
git config --global user.name 你的英文名
git config --global user.email 你的邮箱
git config --global push.default simple
git config --global core.quotepath false 
git config --global core.editor "code --wait"
git config --global core.autocrlf input
```
### git 常用命令
	- git config 
	- git add 路径
	- git status -sb
	- git commit -v
	- git branch x
	- git checkout x
	- git merge 
	- git commit 
	- git branch -d x
	- git log 
	- git relog 
	- git reset --hard XXXXX


## GitHub 

### 存储代码
> 常用两行命令
`git remote add origin git@xxxxxx`
`git push -u origin master`

### SSH key 验证身份
+ 生成 SSH key
+ 测试 SSH key 

### 上传和下载代码
+ git pull / git push / git clone
+ 常用4连 git add/ git commit /[git pull] / git push 

## git高级操作
+ 设置快捷
+ git rebose 
+ git stash / stash pop
