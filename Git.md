# Git初始化

- 每个项目都要初始化

- 在项目目录中打开git bash，初始化`git init`



# 自报家门

配置用户和邮箱，邮箱不一定是真实的，但格式必须正确

- 此方法使用远程仓库的https地址

- `git config --global user.name "xiaoming"`

- `git config --global user.email "xiaoming@qq.com"`



# 代码存储到仓库

- 将项目放到门口`git add ./readme.md `
- 将项目中所有的都放到门口`git add ./`
- 把代码放到房间（仓库）里`git commit -m "我们完成了第一个功能"`(代码说明)
- 直接把代码放到房间里`git commit --all -m "这是一次性的操作"`



# 查看状态

查看当前代码是否放入到房间去了`git status`



# 查看提交记录

查看所有提交记录`git log`

简写说明，简洁版的日志`git log --oneline`



# 版本回退

- 获取最近的版本并覆盖，0为最新的一个`git reset --hard Head~0`
- 查看功能码`git log --oneline`
- 回退到想要的版本`git reset --hard 版本号`
- 查看隐藏的版本号`git reflog`



# 分支

- 主分支master：要执行的代码
- 子分支：还未写完的代码存放的分支
- 创建dev分支，dev的分支和master分支里的东西是一样的`git branch dev`
- 查看分支`git branch`
- 切换分支`git checkout dev`
- 创建并切换分支`git checkout -b dev`
- 将本地分支推送到远程仓库`git push -u origin dev`
- 切换分支并进入子分支`git checkout -u dev`
- 在主分支合并分支，把当前分支与指定的分支进行合并
  - `git checkout master`
  - `git merge dev`



# 提交代码到GitHub

- 将房间master分支里的代码上传到Github仓库中`git push 服务器地址 master`
- 从Github仓库里的master分支拿到本地`git pull 服务器地址 master`

> 注意：本地要初始化一个克隆仓库，此方法为合并数据

- 把所有的内容拿到本地`git clone 服务器地址`

> 注意：此方法会覆盖本地的内容数据



# ssh上传代码

- 不用输入用户名和密码了

- 生成公钥和私钥`ssh-keygen -t rsa -C "xiaoxin@qq.com"`
- GitHub设置公钥
- 通过命令上传到GitHub`git push ssh地址 master`



# 多个用户提交

> 注意：在本地提交之前，先pull在push，不然会有冲突
>
> 先pull，解决冲突在上传，pull时说明，先按`Esc`在`:wq`保存并退出即可



# Github地址的简写

- `git remote add origin 服务器地址`
- `git push origin -u master`使用这方法加了-u参数，下次提交直接用`git push`，拿到本地`git pull`
- 加上了-u，git会把当前分支与远程分支进行关联

> 注意：此方法只在当前目录下有效