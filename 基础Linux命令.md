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

     新建目录或者文件夹
     mkdir -p  t/t1 可以是一个路径名称。此时若路径中的某些目录尚不存在,加上此选项后,系统将自动建立好那些尚不存在的目录,即一次可以建立多个目录; 
     mkdir -v t     每次创建新目录都显示信息
     
### touch

    新建文件
    修改文件时间戳
    
    	-a   或--time=atime或--time=access或--time=use 　只更改存取时间。
	-c   或--no-create 　不建立任何文档。
	-d 　使用指定的日期时间，而非现在的时间。
	-f 　此参数将忽略不予处理，仅负责解决BSD版本touch指令的兼容性问题。
	-m   或--time=mtime或--time=modify 　只更改变动时间。
	-r 　把指定文档或目录的日期时间，统统设成和参考文档或目录的日期时间相同。
	-t 　使用指定的日期时间，而非现在的时间
    
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
    
### cat
    cat主要有三大功能：

	1.一次显示整个文件:cat filename

	2.从键盘创建一个文件:cat > filename 只能创建新文件,不能编辑已有文件.

	3.将几个文件合并为一个文件:cat file1 file2 > file
	
	-A, --show-all           等价于 -vET
	-b, --number-nonblank    对非空输出行编号
	-e                       等价于 -vE
	-E, --show-ends          在每行结束处显示 $
	-n, --number     对输出的所有行编号,由1开始对所有输出的行数编号
	-s, --squeeze-blank  有连续两行以上的空白行，就代换为一行的空白行 
	-t                       与 -vT 等
	-T, --show-tabs          将跳格字符显示为 ^I
	-u                       (被忽略)
	-v, --show-nonprinting   使用 ^ 和 M- 引用，除了 LFD 和 TAB 之外
	
    cat mytext.txt mytext2.txt 同时显示两个文件内容
    cat mytext.txt > newfile.txt 如果newfile.txt存在 则覆盖原内容，否则新建
    cat mytext.txt mytext2.txt > newfile.txt 
    cat file.txt >> another-file.txt  在another-file.txt后面append
 
### nl
    nl 可以将输出的文件内容自动的加上行号
    
    -b  ：指定行号指定的方式，主要有两种：
	-b a ：表示不论是否为空行，也同样列出行号(类似 cat -n)；
	-b t ：如果有空行，空的那一行不要列出行号(默认值)；
	
    -n  ：列出行号表示的方法，主要有三种：
	-n ln ：行号在萤幕的最左方显示；
	-n rn ：行号在自己栏位的最右方显示，且不加 0 ；
	-n rz ：行号在自己栏位的最右方显示，且加 0 ；

    -w  ：行号栏位的占用的位数。

    -p 在逻辑定界符处不重新开始计算。

