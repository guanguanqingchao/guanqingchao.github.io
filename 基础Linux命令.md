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

### more


	命令功能：
	more命令和cat的功能一样都是查看文件里的内容，但有所不同的是more可以按页来查看文件的内容，还支持直接跳转行等功能。

	+n      从笫n行开始显示
	-n       定义屏幕大小为n行
	+/pattern 在每个档案显示前搜寻该字串（pattern），然后从该字串前两行之后开始显示  
	-c       从顶部清屏，然后显示
	-d       提示“Press space to continue，’q’ to quit（按空格键继续，按q键退出）”，禁用响铃功能
	-l        忽略Ctrl+l（换页）字符
	-p       通过清除窗口而不是滚屏来对文件进行换页，与-c选项相似
	-s       把连续的多个空行显示为一行
	-u       把文件内容中的下画线去掉

	常用操作命令：

	Enter    向下n行，需要定义。默认为1行
	Ctrl+F   向下滚动一屏
	空格键  向下滚动一屏
	Ctrl+B  返回上一屏
	=       输出当前行的行号
	：f     输出文件名和当前行的行号
	V      调用vi编辑器
	!命令   调用Shell，并执行命令 
	q       退出more


         more +/day3 log2012.log  从文件中查找第一个出现"day3"字符串的行，并从该处前两行开始显示输出


### head 显示开头某个数量的文字区块

	-q 隐藏文件名
	-v 显示文件名
	-c<字节> 显示字节数
	-n<行数> 显示的行数
	
	head -n 5 log2014.log  显示文件开头的前5行
	
### tail 显示结尾某个数量的文字区块
### which 在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果
	which pwd
	which which
### whereis 


### grep 使用正则表达式搜索文  ***

	grep [option] pattern file
	
	-a   --text   #不要忽略二进制的数据。   
	-A<显示行数>   --after-context=<显示行数>   #除了显示符合范本样式的那一列之外，并显示该行之后的内容。   
	-b   --byte-offset   #在显示符合样式的那一行之前，标示出该行第一个字符的编号。   
	-B<显示行数>   --before-context=<显示行数>   #除了显示符合样式的那一行之外，并显示该行之前的内容。   
	-c    --count   #计算符合样式的列数。   
	-C<显示行数>    --context=<显示行数>或-<显示行数>   #除了显示符合样式的那一行之外，并显示该行之前后的内容。   
	-d <动作>      --directories=<动作>   #当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。   
	-e<范本样式>  --regexp=<范本样式>   #指定字符串做为查找文件内容的样式。   
	-E      --extended-regexp   #将样式为延伸的普通表示法来使用。   
	-f<规则文件>  --file=<规则文件>   #指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。 
	-F   --fixed-regexp   #将样式视为固定字符串的列表。   
	-G   --basic-regexp   #将样式视为普通的表示法来使用。   
	-h   --no-filename   #在显示符合样式的那一行之前，不标示该行所属的文件名称。   
	-H   --with-filename   #在显示符合样式的那一行之前，表示该行所属的文件名称。   
	-i    --ignore-case   #忽略字符大小写的差别。   
	-l    --file-with-matches   #列出文件内容符合指定的样式的文件名称。   
	-L   --files-without-match   #列出文件内容不符合指定的样式的文件名称。   
	-n   --line-number   #在显示符合样式的那一行之前，标示出该行的列数编号。   
	-q   --quiet或--silent   #不显示任何信息。   
	-r   --recursive   #此参数的效果和指定“-d recurse”参数相同。   
	-s   --no-messages   #不显示错误信息。   
	-v   --revert-match   #显示不包含匹配文本的所有行。   
	-V   --version   #显示版本信息。   
	-w   --word-regexp   #只显示全字符合的列。   
	-x    --line-regexp   #只显示全列符合的列。   
	-y   #此参数的效果和指定“-i”参数相同。
	
	
	
	grep的规则表达式:

	^  #锚定行的开始 如：'^grep'匹配所有以grep开头的行。    
	$  #锚定行的结束 如：'grep$'匹配所有以grep结尾的行。    
	.  #匹配一个非换行符的字符 如：'gr.p'匹配gr后接一个任意字符，然后是p。    
	*  #匹配零个或多个先前字符 如：'*grep'匹配所有一个或多个空格后紧跟grep的行。    
	.*   #一起用代表任意字符。   
	[]   #匹配一个指定范围内的字符，如'[Gg]rep'匹配Grep和grep。    
	[^]  #匹配一个不在指定范围内的字符，如：'[^A-FH-Z]rep'匹配不包含A-R和T-Z的一个字母开头，紧跟rep的行。    
	\(..\)  #标记匹配字符，如'\(love\)'，love被标记为1。    
	\<      #锚定单词的开始，如:'\<grep'匹配包含以grep开头的单词的行。    
	\>      #锚定单词的结束，如'grep\>'匹配包含以grep结尾的单词的行。    
	x\{m\}  #重复字符x，m次，如：'0\{5\}'匹配包含5个o的行。    
	x\{m,\}  #重复字符x,至少m次，如：'o\{5,\}'匹配至少有5个o的行。    
	x\{m,n\}  #重复字符x，至少m次，不多于n次，如：'o\{5,10\}'匹配5--10个o的行。   
	\w    #匹配文字和数字字符，也就是[A-Za-z0-9]，如：'G\w*p'匹配以G后跟零个或多个文字或数字字符，然后是p。   
	\W    #\w的反置形式，匹配一个或多个非单词字符，如点号句号等。   
	\b    #单词锁定符，如: '\bgrep\b'只匹配grep。
	
	
	grep 'appointment_limit_fee' new.txt  在new.txt文件中查找appointment_limit_fee关键字
	grep 'appointment_limit_fee' new.txt bundle.json 在多个文件中查找
	
