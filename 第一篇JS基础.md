## 基本数据类型
- number、string、boolean、null、undefined、object、symbol


        检测数据类型 typeof x  或 typeof(x)   结果是string的对应数据类型

        特例：typeof null ---- object
    
## 类型转换

        常见的数据类型转换
        
        ToString —— 输出内容时 ToString 发生转换，或通过 String(value) 进行显式转换。
        原始类型值的 string 类型转换通常是可预见的。
        
        ToNumber – 进行算术操作时发生 ToNumber 转换，或通过 Number(value) 进行显式转换。
              undefined  ---- NaN
              null       ----  0
              true/false ---- 1/0
              string     ---- 字符串“按原样读取”，两端的空白被忽略。空字符串变成 0 ,其余变成NaN

        ToBoolean – 进行逻辑操作时发生 ToBoolean 转换。或通过 Boolean(value) 进行显式转换。
              0,null,NaN,undefined,""  ---- false
              其他【"0"," "】     ---------- true
  
        Tips:
          "2"-0  ---  2
          "2"+0  ---  20
          "2"-1  ---  1
          "" -1  ---  -1
        
    
