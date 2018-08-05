## 命令行窗口

操作电脑的两种方式：图形界面、命令行窗口。

- 我们平时都是通过图形界面来操作计算机，比如鼠标右键->新建->新建文件夹。
- 其实命令行窗口也是一种操作向计算机的方式。
- 开启命令行 win + R -> cmd -> enter
- 常用命令行工具
    + cmd
    + powershell
    + git bash

## 常用命令
- pwd (print working directory) 查看当前所在的目录
- cd (change directory) 切换目录
- ls (list) 查看当前目录下的内容
- mkdir (make directory) 创建目录
- touch 创建文件
- cat 查看文件内容
- less 查看文件内容
- rm (remove) 删除文件 如 rm index.html
- rmdir (remove directory) 删除文件夹 只能删除空文件夹 不常用
- rm -rf  删除非空目录
- mv (move) 移动文件或重命名
- cp (copy) 复制文件
- echo '内容' > 文件 输出内容到文件 每次输出都是覆盖原有文件内容
- ehco '内容' >> 文件 输出内容到文件 每次输出都是追新内容
- tar -cf 压缩包名字 要压缩的文件列表
  - -c表示创建新压缩包，-f指定包的文件名。 
- tar -xf 压缩包名字
  - -x表示解压，-f指定包的文件名。

## GIT是什么
源代码管理工具，是linux系统之父Linus Torvalds为管理linux系统内核而开发的开源的版本控制工具。

在写代码的过程中提供的反悔的机会。

开发过程中出现bug，越改越乱，可以选择撤销，当修改的步骤较多或者文件较多可能撤销不回去，如果编辑器在中途关闭过，就肯定撤销不回去了。想回到没改之前代码能运行的状态。

## GIT三大区域
- 工作目录
    + 存放项目代码的目录
- 暂存区
    + 临时存放更改的了文件
    + 防止工作目录中的代码丢失
- 代码仓库
    + 当开发的功能足以形成一个版本的时候 可以将代码形成版本提交到仓库
    + 相当于复制了一份当前的代码存储到了仓库中

## GIT常用命令
- 配置git用户名和邮箱
    + git config --global user.name zhangsan
    + git config --global user.email zhangsan@163.com
- 查看当前的git配置
    + git config --list	
- 初始化git仓库
    + git init
- 查看当前仓库的状态 
    + git status
- 将工作目录中的文件添加到暂存区
    + git add 
- 将暂存区中的代码提交到本地仓库 形成一个版本
    + git commit -m 备注 
- 查看本地仓库中的历史提交版本
    + git log 
- 将文件从暂存区中删除
    + git rm --cached 文件列表 
    + 说明:
        * 必须保证工作目录中的文件代码和暂存区中文件代码一致
        * 此时工作目录中有此文件 暂存区中没有此文件 这个文件不被git管理
- 用暂存区中的文件覆盖工作目录中的文件
    + git checkout  文件列表 
    + 说明：暂存区和工作目录都有此文件 这个文件依然被git管理
- 回滚到本地仓库中的特定版本并覆盖暂存区和工作目录
    + git reset --hard commitID 
- 回滚到本地仓库中的特定版本不覆盖暂存区和工作目录
    + git reset --soft commitID
- 回滚到本地仓库中的特定版本并覆盖暂存区
    + git reset --mixed commitID
- 回滚中HEAD的使用
    + git reset --hard HEAD 使用最新的版本覆盖工作目录和暂存区
    + git reset --hard HEAD^ 使用上一次提交的版本覆盖工作目录和暂存区
- 查看分支
    + git branch
- 创建分支
    + git branch 分支名称
- 切换分支
    + git checkout 分支名称
- 创建并切换分支
    + git checkout -b 分支名称 
- 删除分支(如果分支没有被合并不允许删除)
    + git branch -d 分支名称
- 删除分支(强制删除分支)
    + git branch -D 分支名称
- 合并分支
    + git merge 来源分支
- 初始化一个裸露仓库(公共代码仓库)
    + git init --bare 
- 向远程仓库推送代码
    + git push 远程仓库地址 本地分支名称:远程分支名称
- 从远程仓库中拉取代码(拉取最新版本到本地 开发过程中使用)
    + git pull 远程仓库地址 远程分支名称:本地分支名称
- 为远程仓库地址创建别名
    + git remote add 别名 远程仓库地址
- 查看远程地址的详情信息
    + git remote -v
- 查看当前别名所对应的远程仓库地址
    + git remote show 别名 
- 删除当前别名及所对应的远程仓库地址
    + git remote remove 别名 
- 从远程仓库获取代码(拉取所有版本到本地)
    + git clone 远程仓库地址 项目名称 
    + 使用场景：加入到已有项目的开发中 需要先拉取所有版本到本地 再进行开发
- 冲突修复
    - 创建公共仓库
    - 创建张三并向公共仓库中提交一次代码
    - 创建李四并拉取公共仓库中的代码
    - 张三修改文件再一次向公共仓库中提交代码
    - 李四修改同一个文件并向公共仓库中提交
        - 由于刚刚张三提交了一版代码到公共仓库中 李四本地仓库不是最新的 所以不能向公共仓库中提交代码 需要先拉取再提交
    - 李四再次拉取公共仓库中的代码
        - 由于公共仓库中张三修改了index.html文件 李四在本地也修改了inex.html文件
            	所以冲突了
    - 李四解决冲突并向仓库中提交
    - 张三从公共仓库中再拉取解决冲突后的代码
- 多人协作开发免登录操作 ssh-keygen
- gitignore忽略清单