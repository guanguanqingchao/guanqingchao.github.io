参考：
https://www.cnblogs.com/peida/archive/2012/10/23/2734829.html


### cd
     cd /      系统根目录
     cd ~      当前用户主目录
     cd ..     返回上一级
     cd -      返回进入此目录之前所在的目录
     cd !$     把上个命令的参数作为cd参数使用

### pwd 显示文件路径
     pwd 

### mkdir
     mkdir -p  t/t1 可以是一个路径名称。此时若路径中的某些目录尚不存在,加上此选项后,系统将自动建立好那些尚不存在的目录,即一次可以建立多个目录; 
     mkdir -v t     每次创建新目录都显示信息

### rm

    -f, --force    忽略不存在的文件，从不给出提示。

    -i, --interactive 进行交互式删除

    -r, -R, --recursive   指示rm将参数中列出的全部目录和子目录均递归地删除。

    -v, --verbose    详细显示进行的步骤

       --help     显示此帮助信息并退出

       --version  输出版本信息并退出、
       
    rmdir 删除空目录
### mv

    mv [选项] 源文件或目录 目标文件或目录
    源文件：如果目标不是目录  则重命名
           如果是目录  则移动文件
    源目录：如果目标是目录  移动
           如果目标是不是目录  重新命名
    -b ：若需覆盖文件，则覆盖前先行备份。 

    -f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；

    -i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！

    -u ：若目标文件已经存在，且 source 比较新，才会更新(update)

	-t  ： --target-directory=DIRECTORY move all SOURCE arguments into DIRECTORY，即指定mv的目标目录，该选项适用于移动多个源文件到一个目录的情况，此时目标目录在前，源文件在后。
    
    
    mv * ../   移动当前目录下的文件档上一级目录
    mv test3/*.txt test5  把当前目录的一个子目录里的文件移动到另一个子目录里
