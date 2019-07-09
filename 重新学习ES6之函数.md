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
      
 - name 函数名字
 
            如果将一个匿名函数赋值给一个变量，ES5 的name属性，会返回空字符串，而 ES6 的name属性会返回实际的函数名。
            
            var f = function () {};
            // ES5
            f.name // ""
            // ES6
            f.name // "f"
            
            如果将一个具名函数赋值给一个变量，则 ES5 和 ES6 的name属性都返回这个具名函数原本的名字。
            const bar = function baz() {};
            // ES5
            bar.name // "baz"
            // ES6
            bar.name // "baz"
            
- 箭头函数

      （1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

      （2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

      （3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

      （4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。
      
      
      不使用箭头函数例子：
      const obj = {
            a: function() { console.log(this) }    
      }
      obj.a()  //打出的是obj对象
      
      使用箭头函数的例子：
      const obj = {
          a: () => {
              console.log(this)
          }
      }
      obj.a()  //打出来的是window
      在使用箭头函数的例子里，因为箭头函数默认不会使用自己的this，而是会和外层的this保持一致，最外层的this就是window对象。
      
      window.setTimeout()中函数里的this默认是window，我们也可以通过箭头函数使它的this和外层的this保持一致：
      const obj = {
          a: function() {
              console.log(this)
              window.setTimeout(() => { 
                  console.log(this) 
              }, 1000)
          }
      }
      obj.a.call(obj)  //第一个this是obj对象，第二个this还是obj对象
      
      
      多层对象嵌套里箭头函数里this是和最最外层保持一致
      const obj = {
          a: function() { console.log(this) },
          b: {
            c: () => {console.log(this)}
            }
      }
      obj.a()   //没有使用箭头函数打出的是obj
      obj.b.c()  //打出的是window对象！！
      
- 尾调用优化

      尾调用（Tail Call）是函数式编程的一个重要概念，是指某个函数的最后一步是调用另一个函数。
      function f(x){
        return g(x);
      }




      



