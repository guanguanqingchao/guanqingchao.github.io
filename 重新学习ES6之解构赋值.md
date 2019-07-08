## 对象的解构赋值

      对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
      
      let [foo, [[bar], baz]] = [1, [[2], 3]];
      foo // 1
      bar // 2
      baz // 3
      
      let [head, ...tail] = [1, 2, 3, 4];
      head // 1
      tail // [2, 3, 4]
      
      //默认值
      let [x, y = 'b'] = ['a']; // x='a', y='b'
      let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'




## 数组的解构赋值

      数组的元素是按次序排列的，变量的取值由它的位置决定；
      
      let { log, sin, cos } = Math;
      let { bar, foo } = { foo: 'aaa', bar: 'bbb' };
      
      //重命名
      
      let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
      baz // "aaa"
      
      //赋值
      let obj = {};
      let arr = [];

      ({ foo: obj.prop, bar: arr[0] } = { foo: 123, bar: true });

      obj // {prop:123}
      arr // [true]
      
      //默认值 
      
      let { baz='aaa' } = { foo: 'aaa', bar: 'bbb' };
      baz // "aaa"
      
      //嵌套解构
      
      let obj = {
        p: [
          'Hello',
          { y: 'World' }
        ]
      };

      let { p: [x, { y }] } = obj;
      x // "Hello"
      y // "World"
      
      const node = {
        loc: {
          start: {
            line: 1,
            column: 5
          }
        }
      };

      let { loc, loc: { start }, loc: { start: { line }} } = node;
      line // 1
      loc  // Object {start: Object}
      start // Object {line: 1, column: 5}
      
## 用途
- 交换变量的值

    let x = 1;
    let y = 2;

    [x, y] = [y, x];

- 函数参数解构赋值

    function add([x, y]){
      return x + y;
    }

    add([1, 2]); // 3
    
    函数add的参数表面上是一个数组，但在传入参数的那一刻，数组参数就被解构成变量x和y。对于函数内部的代码来说，它们能感受到的参数就是x和y。
    
    
    function move({x = 0, y = 0} = {}) {
      return [x, y];
    }

    move({x: 3, y: 8}); // [3, 8]
    move({x: 3}); // [3, 0]
    move({}); // [0, 0]
    move(); // [0, 0]
    
    
    总结：
    // 参数是一组有次序的值
    function f([x, y, z]) { ... }
    f([1, 2, 3]);

    // 参数是一组无次序的值
    function f({x, y, z}) { ... }
    f({z: 3, y: 2, x: 1});

      
      
      

