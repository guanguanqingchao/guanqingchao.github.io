## 数据类型
- number、string、boolean、null、undefined、object、symbol


        检测数据类型 typeof x  或 typeof(x)   结果是string的对应数据类型

        特例：typeof null ---- object
    
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

        
        
        

         
        
    
