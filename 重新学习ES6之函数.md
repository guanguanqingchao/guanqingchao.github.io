- 函数默认值，默认值是为参数

      function log(x, y = 'World') {
        console.log(x, y);
      }

      log('Hello') // Hello World
      log('Hello', 'China') // Hello China
      log('Hello', '') // Hello
      
- length 指定了默认值以后，函数的length属性，将返回没有指定默认值的参数个数

      (function (a) {}).length // 1
      (function (a = 5) {}).length // 0
      (function (a, b, c = 5) {}).length // 2
      
      如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数了。

      (function (a = 0, b, c) {}).length // 0
      (function (a, b = 1, c) {}).length // 1
      
      函数的length属性，不包括 rest 参数。
      (function(a) {}).length  // 1
      (function(...a) {}).length  // 0
      (function(a, ...b) {}).length  // 1
      
- rest 形式为...变量名，用于获取函数的多余参数，参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

      function add(...values) {
        let sum = 0;
        
        //values  [2,5,3]

        for (var val of values) {
          sum += val;
        }

        return sum;
      }

      add(2, 5, 3) // 10
