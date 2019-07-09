- 扩展运算符

      扩展运算符（spread）是三个点（...）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。
      注意，只有函数调用时，扩展运算符才可以放在圆括号中，否则会报错。
      
      function push(array, ...items) {
          array.push(...items);
        }

        function add(x, y) {
          return x + y;
        }

        const numbers = [4, 38];
        add(...numbers) // 42
        
        
        扩展运算符还可以将字符串转为真正的数组。
        [...'hello']
        // [ "h", "e", "l", "l", "o" ]
        
        
        
        复制数组
        es5
        const a1 = [1, 2];
        const a2 = a1.concat();
        a2[0] = 2;
        a1 // [1, 2]
        上面代码中，a1会返回原数组的克隆，再修改a2就不会对a1产生影响。
        
        es6
        扩展运算符提供了复制数组的简便写法。
        const a1 = [1, 2];
        // 写法一
        const a2 = [...a1];
        // 写法二
        const [...a2] = a1;
        
        
        
        合并数组
        const arr1 = ['a', 'b'];
        const arr2 = ['c'];
        const arr3 = ['d', 'e'];

        // ES5 的合并数组
        arr1.concat(arr2, arr3);
        // [ 'a', 'b', 'c', 'd', 'e' ]
        // ES6 的合并数组
        [...arr1, ...arr2, ...arr3]
        // [ 'a', 'b', 'c', 'd', 'e' ]
        
        
        解构赋值，必须放在最后
        const [first, ...rest] = [1, 2, 3, 4, 5];
        first // 1
        rest  // [2, 3, 4, 5]

        const [first, ...rest] = [];
        first // undefined
        rest  // []

        const [first, ...rest] = ["foo"];
        first  // "foo"
        rest   // []
        
        
        只要具有 Iterator 接口的对象，都可以使用扩展运算符，比如 Map 结构。
        let map = new Map([
          [1, 'one'],
          [2, 'two'],
          [3, 'three'],
        ]);

        let arr = [...map.keys()]; // [1, 2, 3]
        
- Array.from
    
        Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象
        
        // arguments对象
        function foo() {
          var args = Array.from(arguments);
          // ...
        }
        
        let namesSet = new Set(['a', 'b'])
        Array.from(namesSet) // ['a', 'b']
        
        将字符串转成数组
        Array.from('hello');
        ["h", "e", "l", "l", "o"]
        
        第二个参数类似于map
        Array.from([1, , 2, , 3], (n) => n || 0)
        // [1, 0, 2, 0, 3]
        
- Array.of
        
        Array.of方法用于将一组值，转换为数组。
        Array.of(3, 11, 8) // [3,11,8]
        Array.of(3) // [3]
        Array.of(3).length // 1
        
- fill find findIndex copyWithin includes flat 

        fill填充数组，fill方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。
        new Array(3).fill(7)
        ['a', 'b', 'c'].fill(7, 1, 2)
        // ['a', 7, 'c']
        
        
        find  findIndex
        用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员【索引】，则返回undefined。
        [1, 4, -5, 10].find((n) => n < 0)
        [1, 5, 10, 15].find(function(value, index, arr) {
          return value > 9;
        }) // 10

        [1, 5, 10, 15].findIndex(function(value, index, arr) {
          return value > 9;
        }) // 2

        includes第二个参数表示开始搜索的位置，如果第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度（比如第二个参数为-4，但数组长度为3），则会重置为从0开始。
        [1, 2, 3].includes(3, 3);  // false
        [1, 2, 3].includes(3, -1); // true
        
        
        copyWithin(target, start = 0, end = this.length)
        在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会修改当前数组。
        target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
        start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
        end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算。
        
        // 将3号位复制到0号位
        [1, 2, 3, 4, 5].copyWithin(0, 3, 4)
        // [4, 2, 3, 4, 5]
        
        
        
        flat默认拉平一层
        [1, 2, [3, [4, 5]]].flat()
        // [1, 2, 3, [4, 5]]

        [1, 2, [3, [4, 5]]].flat(2)
        // [1, 2, 3, 4, 5]
        
        如果不管有多少层嵌套，都要转成一维数组，可以用Infinity关键字作为参数。
        [1, [2, [3]]].flat(Infinity)
        // [1, 2, 3]
        如果原数组有空位，flat()方法会跳过空位。

        [1, 2, , 4, 5].flat()
        // [1, 2, 4, 5]
        
        // 相当于 [[2, 4], [3, 6], [4, 8]].flat()
        [2, 3, 4].flatMap((x) => [x, x * 2])
        // [2, 4, 3, 6, 4, 8]







        
        

      
