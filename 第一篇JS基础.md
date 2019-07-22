## 数据类型
- number、string、boolean、null、undefined、object、symbol

        原始数据类型 number、string、boolean、null、undefined、symbol
        检测数据类型 typeof x  或 typeof(x)   结果是string的对应数据类型

        特例：typeof null ---- object
- 原始类型中除了null  undefined都有相应的包装类型

### 数字类型
        toFixed   [返回的是string，靠近的数]  
        Math.floor 
        Math.ceil  
        Math.round[靠近的数取整] 
        Math.random  [0,1) 
        //Number()和+  严格转换，如果不完全是数字 会返回NaN
        parseInt()      // alert( parseInt('100px') ); // 100   parseInt('12.3') // 12
        parseFloat()    //alert( parseFloat('12.3.4') ); // 12.3, the second point stops the reading
        Math.pow(n, power)  返回 n 的 power 次幂，即 npower
        
        JavaScript 中的所有数字都以 64 位格式 IEEE-754 存储，也成双精度存贮
        双精度的浮点数在这64位上划分为3段，而这3段也就确定了一个浮点数的值，64bit的划分是“1-11-52”的模式，具体来说：
        1.就是1位最高位（最左边那一位）表示符号位，0表示正，1表示负；
        2.11位表示指数部分；
        3.52位表示尾数部分，也就是有效域部分
        
        
         ● 0.1+0.2 ！= 0.3  进制转换和进阶过程中精度丢失
                https://www.barretlee.com/blog/2016/09/28/ieee754-operation-in-js/

                解决：
                1.  let sum = 0.1 + 0.2;
                    alert( +sum.toFixed(2) ); // 0.3

                2.  alert( (0.1 * 10 + 0.2 * 10) / 10 ); // 0.3
        
         ●  n~m 之间的随机数  
                 Math.random()生成[ 0，1 )的数， 
                 Math.random()*5生成[ 0，5)的数。

                通常希望得到整数
                parseInt()，Math.floor()，Math.ceil()，Math.round()都可以得到整数。 
                parseInt()和Math.floor()结果都是向下取整。



                生成[ 1, max ]的随机数，如下：

                parseInt（Math.random() * max , 10）+1;
                Math.floor（Math. random()*max）+1；
                Math.ceil(Math.random()*max);
                
                生成[ 0 , max ]的任意数的随机数，如下：

                parseInt（Math.random()*(max+1),10）;
                Math.floor(Math.random()*(max+1));
                
                生成[min,max]的随机数，如下：

                parseInt（Math.random*(max-min+1)+min,10）;
                Math.floor（Math.random()*(max-min+1)+min）;   
                
                I.不包括min max   
                  let rand = min + Math.random() * (max - min); 
                  return Math.round(rand);
                
                II.从 min 到 max 生成随机整数，包括 min 和 max 作为可能值。
                来自间隔 min..max 的任何数字必须以相同的概率出现。
                
                I的随机数不是相同概率出现，min=1,max=3
                1-1.499999 --> 1
                1.5-2.499999 ---> 2
                2.5-2.99999 ---> 3
                2的出现次数是1和3的两倍
                
                // here rand is from min to (max+1)
                let rand = min + Math.random() * (max + 1 - min); 
                return Math.floor(rand);
                
           ● 6.35.toFixed(1)  6.3而不是6.4
           
                alert( Math.round(6.35 * 10) / 10); // 6.35 -> 63.5 -> 64(rounded) -> 6.4
### 字符串
        length
        str.charAt(1000) 返回’‘
        str[1000]     返回undefined
        toUpperCase()
        toLowerCase()
        indexOf()
        includes, startsWith, endsWith
        
        substring（1，4）不包含4 start可以大于end
        slice(1,4) 不包含4
        substr(1,4)四个字符
        
        trim()
        
### 数组


        pop push
        shift unshift
        
        splice          改变原数组  返回删除的元素 删除 替换 添加
        slice           不改变原数组
        
        concact
        indexOf/lastIndexOf 和 includes
        find/findIndex  find方法查询的是使函数返回 true 的第一个元素。
        
        filter          返回的是所有匹配元素组成的数组
        map             对数组中每个元素调用函数并返回符合结果的数组
        reduce
        
        
        sort
        reverse
        
        split
        join
        
        for of 遍历数组
        arr.length=0 清空数组
        
        
        注意：
        []+1   '1'
        [1]+1  '11'
        [1,3]+1 '1,31'
        
        let users = [
                {id: 1, name: "John"}, 
                {id: 2, name: "Pete"}, 
                {id: 3, name: "Mary"}
        ];
        let user = users.find(item => item.id == 1);
        console.log(user.name); // John
        
        
### 类型转换

        常见的数据类型转换
        
        ● ToString —— 输出内容时 ToString 发生转换，或通过 String(value) 进行显式转换。
        原始类型值的 string 类型转换通常是可预见的。
        
        ● ToNumber – 进行算术操作时发生 ToNumber 转换，或通过 Number(value) 进行显式转换。
              undefined  ---- NaN
              null       ----  0
              true/false ---- 1/0
              string     ---- 字符串“按原样读取”，两端的空白被忽略。空字符串变成 0 ,其余变成NaN

        ● ToBoolean – 进行逻辑操作时发生 ToBoolean 转换。或通过 Boolean(value) 进行显式转换。
              0,null,NaN,undefined,""  ---- false
              其他【"0"," "】     ---------- true
  
        Tips:
          "2"-0  ---  2 或  +"2"   字符串转数字
          2 + "" --- "2"          数字转字符串
          "2"+0  ---  20
          "2"-1  ---  1
          ""-1   --- -1
          
          
        字符串连接功能，二元运算符 +
        数字转化功能，一元运算符 +     // +"2" 同Number()
       
### 运算符

        ●  a % b 的结果是 a 除以 b 的余数。
        
                8%3   //2 是 8/3 的余数
        
        ●  ++/-- 自增自减 前置后置
        
                let counter = 1;
                let a = ++counter; 
                alert(a); // 2

                let counter = 1;
                let a = counter++;
                alert(a); // 1
                如果自相加/自相减的值不会被使用，那么两者形式没有区别:
                        let counter = 0;
                        counter++;
                        ++counter;
                        alert( counter ); // 2，以上两行作用相同
                如果我们想要对变量自相加 并且 立刻使用值，那么我们需要使用前置形式:
                        let counter = 0;
                        alert( ++counter ); // 1
                如果我们想要使用之前的值，那么我们需要使用后置形式:
                        let counter = 0;
                        alert( counter++ ); // 0
                
                
         ●  位运算符 & | ^ ~ >>  << 
         ●  修改并替换
  
                let n = 2;
                n = n + 5; 
                n = n * 2;
                
                let n = 2; 
                n += 5; //nown=7(同 n=n+5) 
                n *= 2;  // now n = 14 (同n = n * 2)
                alert( n ); // 14
                
          
         ●  条件运算符  if   ? :
         ●  逻辑运算符  || && ！
           短路取值：
              result = value1 || value2 || value3;
              从左到右依次计算，返回结果为true的值，若没有为true，返回最后一个操作数
              
              result = value1 && value2 && value3;
              从左到右依次计算，返回第一个结果为false的值，若没有，返回最后一个操作数
              
           注意： && 的优先级高于 ||
           
           ！！用来将某个值转化为布尔类型
           
               
              
              
              
### 值的比较

        ● 大于 / 小于: a > b ，   。
        ● 大于等于 / 小于等于: a <= b。
        ● 检测两个值的相等写为 a == b (注意表达式中是两个等号 = ，若写为单个等号 a = b 则表示赋值)。
        ● 检测两个值的不等，在数学中使用 ≠ 符号，而在 JavaScript 中则通过在赋值符号前增加叹号表示: a != b 。
        
        比较结果是boolean类型
        字符串中间的比较是按照字典顺序比较
        
        ● 不同类型见的比较 ==
                当不同类型的值进行比较时，它们会首先被转为数字(number)再判定大小
                '2' > 1
                '01' == 1
                true == 1
                null == undefined   // true 特殊
                
          注意！！！当使用数学式或其他比较方法 < > <= >= 时: null/undefined 的值会被转换为数字: null 转为 0 ， undefined 转为 NaN。
                
                null == 0 //false
                null > 0 //false   
                null >= 0  // true
                
                null 和 undefined 在相等性检测 == 中不会进行任何的类型转换
                它们有自己独立的比较规则，所以除了它 们之间互等外不会等于任何其他的值。这就解释了为什么 null == 0 会返回 false。
                
                undefined > 0 ; // false (1) 
                undefined < 0 ; // false (2) 
                undefined == 0 ; // false (3)
                

                (1) 和 (2) 中返回false 是因为 undefined 在比较中被转换为了 NaN ，而 NaN 是一个特殊的数值型取值，它与任何值进行比较都会返回 
                (3) 中返回 false 是因为这是一个相等性检测，而 undefined 只与 null 相等。
        
        ● === 不会做类型转换
        
### 函数

        
        
        

         
        
    
