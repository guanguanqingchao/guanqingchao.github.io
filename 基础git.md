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
    
    
    
    git reset HEAD file 暂存--未暂存
    git checkout -- file  未暂存--未修改
    
 ### 打标签
 
    git tag v1.0
    git tag -a v1.4 -m 'my version 1.4'
    git tag -a v1.2 9fceb02
    
    git tag
    git show v1.0
    
    git push origin [tagname]  
    git push origin --tags  全部推送
    
    git checkout -b [banchname] [tagname]特定标签新建一条分支 
    
