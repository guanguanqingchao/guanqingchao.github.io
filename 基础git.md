### git配置
 
    git config --list  查看配置列表
    git config user.name 查看某一配置
    
### git别名

    $ git config --global alias.co checkout
    $ git config --global alias.br branch
    $ git config --global alias.ci commit
    $ git config --global alias.st status
  
### git初始化

    已有仓库  git init
    克隆仓库  git clone [url]  rename 重新命名
    git remote add <shortname> <url> 添加一个新的远程 Git 仓库，同时指定一个你可以轻松引用的简写
    可以在命令行中使用字符串 shortname 来代替整个 URL, git fetch shortname
    
    git remote rename pb paul 将pb重新命名paul
    git remote rm paul
    
    git remote -v 查看远程仓库
    git remote show [remote-name]
    
    git fetch [remote-name] 从远程仓库中拉取数据，分支等，不会自动合并
    git pull 自动的抓取然后合并远程分支到当前分支
    
    
### git操作

    git status -s 
    git diff          查看未暂存的文件
    git diff --cached 查看已经暂存起来的变化
    git log --pretty=oneline
    git log --pretty --graph
    
    git log master..experiment  查看experiment 分支中有的提交 而master分支中没有的
    $ git log refA..refB
    $ git log ^refA refB
    $ git log refB --not refA
    
    $ git log refA refB ^refC
    $ git log refA refB --not refC 查看所有被 refA 或 refB 包含的但是不被 refC 包含的提交
    
    git log master...experiment  master 或者 experiment 中包含的但不是两者共有的提交
    git log --left-right master...experiment 显示属于哪个分支的提交
    
    git reflog 显示历史head指向
    
    git show [commit id] 展示本id的改动
    
    
    git stash 暂存
    git stash list
    git stash apply/pop  pop恢复栈顶 apply不会从list中移除
    git stash drop 列表栈顶删除
    git stash branch [branch name]从当前暂存中切除一条分支含有暂存信息
    
    
    
    git reset HEAD file 暂存--未暂存
    git checkout -- file  未暂存--未修改
    git reset --hard [commit id] 版本回退
    
 ### 打标签
 
    git tag v1.0
    git tag -a v1.4 -m 'my version 1.4'
    git tag -a v1.2 9fceb02
    
    git tag
    git show v1.0
    
    git push origin [tagname]  
    git push origin --tags  全部推送
    
    git checkout -b [banchname] [tagname]特定标签新建一条分支 
    
### git分支
   
    git branch [branchname]
    git checkout  -b [branchname]
    
    
    
    
    
    
