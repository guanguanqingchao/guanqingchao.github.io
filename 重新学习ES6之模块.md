## es6模块和Common模块区别：
- commonJS是值的拷贝   es6是引用的拷贝
- commonJS加载整个模块，es6加载其中某个方法
- commonJS运行时加载，es6编译时加载
- commonJS中this指向当前模块，es6this指向undefined
- commonJS加载同一个模块的时候去缓存中取值，es6动态更新
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
