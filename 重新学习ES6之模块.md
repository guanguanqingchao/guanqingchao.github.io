## 浏览器加载模块

  
    <script type="module" src="./foo.js"></script>
    <!-- 等同于 -->
    <script type="module" src="./foo.js" defer></script>
    
    浏览器遇到type是module，异步加载，效果等同于defer
    <script type="module">
    //顶层的this是undefined
    </script>
    





## es6模块和Common模块区别：

- commonJS运行时加载，es6编译时加载
- commonJS是值的拷贝   es6是值的引用  
- commonJS中this指向当前模块，es6this指向undefined
- commonJS加载同一个模块的时候去缓存中取值，es6动态更新
- commonJS加载整个模块，es6加载其中某个方法

## 循环加载
### CommonJs循环加载

     CommonJS 的一个模块，就是一个脚本文件。require命令第一次加载该脚本，就会执行整个脚本，然后在内存生成一个对象。

    {
      id: '...',
      exports: { ... },
      loaded: true,
      ...
    }
    上面代码就是 Node 内部加载模块后生成的一个对象。该对象的id属性是模块名，exports属性是模块输出的各个接口，loaded属性是一个布尔值，表示该模块的脚本是否执行完毕。其他还有很多属性，这里都省略了。

    以后需要用到这个模块的时候，就会到exports属性上面取值。即使再次执行require命令，也不会再次执行该模块，而是到缓存之中取值。也就是说，CommonJS 模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回第一次运行的结果，除非手动清除系统缓存。
    
    exports.done = false;
    var b = require('./b.js');
    console.log('在 a.js 之中，b.done = %j', b.done);
    exports.done = true;
    console.log('a.js 执行完毕');
    上面代码之中，a.js脚本先输出一个done变量，然后加载另一个脚本文件b.js。注意，此时a.js代码就停在这里，等待b.js执行完毕，再往下执行。

    再看b.js的代码。

    exports.done = false;
    var a = require('./a.js');
    console.log('在 b.js 之中，a.done = %j', a.done);
    exports.done = true;
    console.log('b.js 执行完毕');
    上面代码之中，b.js执行到第二行，就会去加载a.js，这时，就发生了“循环加载”。系统会去a.js模块对应对象的exports属性取值，可是因为a.js还没有执行完，从exports属性只能取回已经执行的部分，而不是最后的值。

    a.js已经执行的部分，只有一行。

    exports.done = false;
    因此，对于b.js来说，它从a.js只输入一个变量done，值为false。

    然后，b.js接着往下执行，等到全部执行完毕，再把执行权交还给a.js。于是，a.js接着往下执行，直到执行完毕。我们写一个脚本main.js，验证这个过程。

    var a = require('./a.js');
    var b = require('./b.js');
    console.log('在 main.js 之中, a.done=%j, b.done=%j', a.done, b.done);
    执行main.js，运行结果如下。

    $ node main.js

    在 b.js 之中，a.done = false
    b.js 执行完毕
    在 a.js 之中，b.done = true
    a.js 执行完毕
    在 main.js 之中, a.done=true, b.done=true
    上面的代码证明了两件事。一是，在b.js之中，a.js没有执行完毕，只执行了第一行。二是，main.js执行到第二行时，不会再次执行b.js，而是输出缓存的b.js的执行结果，即它的第四行。
    
### ES6 模块的循环加载
    ES6 处理“循环加载”与 CommonJS 有本质的不同。ES6 模块是动态引用，如果使用import从一个模块加载变量（即import foo from 'foo'），那些变量不会被缓存，而是成为一个指向被加载模块的引用，需要开发者自己保证，真正取值的时候能够取到值。

    请看下面这个例子。

    // a.mjs
    import {bar} from './b';
    console.log('a.mjs');
    console.log(bar);
    export let foo = 'foo';

    // b.mjs
    import {foo} from './a';
    console.log('b.mjs');
    console.log(foo);
    export let bar = 'bar';
    上面代码中，a.mjs加载b.mjs，b.mjs又加载a.mjs，构成循环加载。执行a.mjs，结果如下。

    $ node --experimental-modules a.mjs
    b.mjs
    ReferenceError: foo is not defined
    上面代码中，执行a.mjs以后会报错，foo变量未定义，这是为什么？

    让我们一行行来看，ES6 循环加载是怎么处理的。首先，执行a.mjs以后，引擎发现它加载了b.mjs，因此会优先执行b.mjs，然后再执行a.mjs。接着，执行b.mjs的时候，已知它从a.mjs输入了foo接口，这时不会去执行a.mjs，而是认为这个接口已经存在了，继续往下执行。执行到第三行console.log(foo)的时候，才发现这个接口根本没定义，因此报错。

    解决这个问题的方法，就是让b.mjs运行的时候，foo已经有定义了。这可以通过将foo写成函数来解决。

    // a.mjs
    import {bar} from './b';
    console.log('a.mjs');
    console.log(bar());
    function foo() { return 'foo' }
    export {foo};

    // b.mjs
    import {foo} from './a';
    console.log('b.mjs');
    console.log(foo());
    function bar() { return 'bar' }
    export {bar};
    这时再执行a.mjs就可以得到预期结果。

    $ node --experimental-modules a.mjs
    b.mjs
    foo
    a.mjs
    bar

## es6模块方法
### export
 
    // profile.js
    export var firstName = 'Michael';
    export var lastName = 'Jackson';
    export var year = 1958;
    export function multiply(x, y) {
      return x * y;
    };
    
    或
    
    // profile.js
    var firstName = 'Michael';
    var lastName = 'Jackson';
    var year = 1958;

    export { firstName, lastName, year，multiply as v1 }; //通过as进行重命名
    
    
    // export-default.js
    //export default命令用于指定模块的默认输出。一个模块只能有一个默认输出，因此export default命令只能使用一次。
    //所以，import命令后面才不用加大括号，因为只可能唯一对应export default命令。
    export default function () {
      console.log('foo');
    }
    export default function foo() {
      console.log('foo');
    }
    
### import

    // main.js
    import { firstName as surname, lastName, year } from './profile.js'; //通过as进行重新命名
    import * as info from './profile.js'; //整体加载

    function setName(element) {
      element.textContent = info.firstName + ' ' + info.lastName;
      
    }
    
    // import-default.js    
    //import命令导入默认的导出模块的时候 不需要大括号
    import customName,{year} from './export-default';
    customName(); // 'foo'                   

### 复合写法

    export { foo, bar } from 'my_module';

    // 可以简单理解为
    import { foo, bar } from 'my_module';
    export { foo, bar };
    
    // 接口改名
    export { foo as myFoo } from 'my_module';

    // 整体输出
    export * from 'my_module';
