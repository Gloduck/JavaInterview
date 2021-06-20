# 基本

+ 基本概念：
  + 工作区：电脑能看到的目录
  + 暂存区：放在`.git`目录下的index文件
  + 版本库：工作区的一个隐藏目录
+ 简介：Git是一款分布式版本控制工具，常见的SVN是集中式的版本控制工具，在分布式版本控制中，每个人的电脑上就有一份完整的代码。
+ git的实现：
  + git调用add files会把文件添加到暂存区，当通过commit的时候，会把暂存区的修改提交到当前的分支，然后暂存区就会被清空。
+ 分支实现：git使用指针将每个提交连成一条时间线，HEAD指针指向当前分支指针。每次提交时只会让分支指针向前移动，而其他分支指针不会移动。

# 常用命令

```shell
git init # 新建仓库
git status # 查看状态
git add pattern # 添加文件
git commit -m "注释" # 添加说明
git remote add origin 仓库 # 添加远程仓库
git pull 分支 # 拉取分支
git push -u origin 分支 # 提交分支
git reset --hard # 版本号前六位 回归到指定版本
git branch # 查看分支
git branch newname # 创建一个叫newname的分支
git checkout newname # 切换到叫newname的分支上
git merge newname # 把newname分支合并到当前分支上
git branch -m 旧名称 新名称 # 重置分支名
```

# 设置代理

```shell
git config --global http.proxy http://IP:Port
git config -–get -–global http.proxy
git config --global --unset http.proxy
```

# 其他

## 换行符

+ 在windows下面提交和linux提交的换行符不一样
  + windows中的换行符为 CRLF 
  + 而在linux下的换行符为LF
+ 解决办法：
  + `git config --global core.autocrlf false`

# 流程图

![](图片/GIT/Git常用命令.jpg)

![](图片/GIT/GIT常用命令表.jpg)