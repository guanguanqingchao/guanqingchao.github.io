参考：
https://www.cnblogs.com/peida/archive/2012/10/23/2734829.html


### cd
- cd /      系统根目录
- cd ~      当前用户主目录
- cd ..     返回上一级
- cd -      返回进入此目录之前所在的目录
- cd !$     把上个命令的参数作为cd参数使用

### pwd 显示文件路径
- pwd 

### mkdir
- mkdir -p  t/t1 可以是一个路径名称。此时若路径中的某些目录尚不存在,加上此选项后,系统将自动建立好那些尚不存在的目录,即一次可以建立多个目录; 
- mkdir -v t     每次创建新目录都显示信息

### rm

    -f, --force    忽略不存在的文件，从不给出提示。

    -i, --interactive 进行交互式删除

    -r, -R, --recursive   指示rm将参数中列出的全部目录和子目录均递归地删除。

    -v, --verbose    详细显示进行的步骤

       --help     显示此帮助信息并退出

       --version  输出版本信息并退出
