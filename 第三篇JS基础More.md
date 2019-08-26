## promise
### 基本用法

    const promise = new Promise(function(resolve,reject){
        // codes……
        if(/*异步成功*/){
            resolve(value);
        } else {
            reject(error);
        }
    });

    promise.then(function(value){
        //sucess...
    },function(error){
        //failure...
    })



常规回调：

    function loadScript(src, callback) {
      let script = document.createElement('script');
      script.src = src;

      script.onload = () => callback(null, script);
      script.onerror = () => callback(new Error(`Script load error ` + src));

      document.head.append(script);
    }
    
改写：

    function loadScript(src) {
      return new Promise(function(resolve, reject) {
        let script = document.createElement('script');
        script.src = src;

        script.onload = () => resolve(script);
        script.onerror = () => reject(new Error("Script load error: " + src));

        document.head.append(script);
      });
    }
    
    let promise = loadScript("https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js");

    promise.then(
      script => console.log(`${script.src} is loaded!`),
      error => console.log(`Error: ${error.message}`)
    );

    promise.then(script => console.log('One more handler to do something else!'));
  
回调不足：
- 在调用 loadScript 时，我们必须已经有了一个 callback 函数。换句话说，在调用 loadScript 之前我们必须知道如何处理结果。
- 只能有一个回调。

promise优点：
- Promises 允许我们按照自然顺序进行编码。首先，我们运行 loadScript 和 .then 来编写如何处理结果。
- 无论何时，只要我们有需要，就可以在 promise 中调用 .then。

练习：
1.延迟函数：

    function delay(ms) {
      return new Promise(resolve => setTimeout(resolve, ms));
    }

    delay(3000).then(() => alert('runs after 3 seconds'));
    //此处resolve 是无参调用。delay 中无需返回任何值，只要确保会延迟即可。
    
2. 绘制动画圆：https://plnkr.co/edit/6rWhYsOKO0ZJOt0RRyCV?p=preview

### Promises 链

### 错误处理
### Promise静态方法
#### Promise.resolve

    let promise = Promise.resolve(value);
    
根据给定的 value 值返回 resolved promise。

等价于：

    let promise = new Promise(resolve => resolve(value));
当我们已经有一个 value 的时候，就会使用该方法，但希望将它“封装”进 promise。

#### Promise.reject

    let promise = Promise.reject(error);
创建一个带有 error 的 rejected promise。
等价于：

    let promise = new Promise((resolve, reject) => reject(error));
    
#### Promise.all
假设我想要并行执行多个 promise，并等待所有 promise 准备就绪。

语法：

    let promise = Promise.all([...promises...]);
    
    
返回的结果是按照顺序的promise的结果数组。
如果任意一个 promise 为 reject，Promise.all 返回的 promise 就会立即 reject 这个错误。
其他结果会被忽略

    let names = ['iliakan', 'remy', 'jeresig'];

    let requests = names.map(name => fetch(`https://api.github.com/users/${name}`));

    Promise.all(requests)
      .then(responses => {
        // 所有响应都就绪时，我们可以显示 HTTP 状态码
        for(let response of responses) {
          alert(`${response.url}: ${response.status}`); // 每个 url 都显示 200
        }

        return responses;
      })
      // 映射 response 数组到 response.json() 中以读取它们的内容
      .then(responses => Promise.all(responses.map(r => r.json())))
      // 所有 JSON 结果都被解析：“users” 是它们的数组
      .then(users => users.forEach(user => alert(user.name)));
      
#### Promise.allSettled
Promise.allSettled 等待所有的 promise 都被处理：即使其中一个 reject，它仍然会等待其他的 promise。
处理完成后的数组有：

    {status:"fulfilled", value:result} 对于成功的响应，
    {status:"rejected", reason:error} 对于错误的响应
    
         let urls = [
            'https://api.github.com/users/iliakan',
            'https://api.github.com/users/remy',
            'https://no-such-url'
        ];
        if (!Promise.allSettled) {
            Promise.allSettled = function (promises) {
                return Promise.all(promises.map(p => Promise.resolve(p).then(v => ({
                    state: 'fulfilled',
                    value: v,
                }), r => ({
                    state: 'rejected',
                    reason: r,
                }))));
            };
        }
        
        Promise.allSettled(urls.map(url => fetch(url)))
            .then(results => {                          // (*)
                results.forEach((result, num) => {
                    if (result.status == "fulfilled") {
                        console.log(`${urls[num]}: ${result.value.status}`);
                    }
                    if (result.status == "rejected") {
                        console.log(`${urls[num]}: ${result.reason}`);
                    }
                });
            });
            
#### Promise.race
与 Promise.all 类似，它接受一个可迭代的 promise 集合，但是它只等待第一个完成（或者 error）而不会等待所有都完成，然后继续执行。
### Promisification

Promisify，就是回调函数与 Promise 间的桥梁，将一个不是promise的方法变成 promise
普通回调：

        function loadScript(src, callback) {
                    let script = document.createElement('script');
                    script.src = src;

                    script.onload = () => callback(null, script);
                    script.onerror = () => callback(new Error(`Script load error for ${src}`));

                    document.head.append(script);
                }
                
                loadScript('path/script.js', (err, script) => {
                    console.log(err, script)     // Error: Script load error for path/script.js
                })

                loadScript('http://omega.intra.didiglobal.com/ba/point/detail?pointId=1000285&taskId=15777', (err, script) => {
                    console.log(err, script)  // null  
                })

可以看出：
- 回调函数在主参数loadScript中的位置是最后一个
- 回调函数的第一个参数必须是error


       function promisify(f) {
            return function (...args) { // 返回一个包装函数
                return new Promise((resolve, reject) => {
                    function callback(err, result) { // 给 f 用的自定义回调
                        if (err) {
                            return reject(err);
                        } else {
                            resolve(result);
                        }
                    }

                    args.push(callback); // 在参数的最后附上我们自定义的回调函数
               

                    f.call(this, ...args); // 调用原来的函数
                });
            };
        };

        // 用法：
        let loadScriptPromise = promisify(loadScript);
        loadScriptPromise('http://omega.intra.didiglobal.com/ba/point/detail?pointId=1000285&taskId=15777')
        .then((
            data) => console.log(data)
            );
        
如果原来的 f 接受一个带更多参数的回调 callback(err, res1, res2)，该怎么办？

下面是 promisify 的修改版，它返回一个装有多个回调结果的数组：

    // 设定为 promisify(f, true) 来获取结果数组
    function promisify(f, manyArgs = false) {
      return function (...args) {
        return new Promise((resolve, reject) => {
          function callback(err, ...results) { // 给 f 用的自定义回调
            if (err) {
              return reject(err);
            } else {
              // 如果 manyArgs 被指定值，则 resolve 所有回调结果
              resolve(manyArgs ? results : results[0]);
            }
          }

          args.push(callback);

          f.call(this, ...args);
        });
      };
    };

    // 用法：
    f = promisify(f, true);
    f(...).then(arrayOfResults => ..., err => ...)
    
 ## Async/await
 
 async 确保了函数的返回值是一个 promise，也会包装非 promise 的值。
 
    async function f() {
      return Promise.resolve(1);
      //return 1
    }

    f().then(alert); // 1
关键字 await 让 JavaScript 引擎等待直到 promise 完成并返回结果。

      async function test(s) {
                let res = await new Promise((resolve, reject) => {
                    setTimeout(() => {
                        resolve(s + 23232)

                }, 3000)
            })

            console.log('inner', res)
        }

        test('guan').then((a) => console.log(a, 'outer is behind'))
3秒后，打印 inner guan23232  和   undefined outer is behind
和用f().then拿到结果相比更优雅
可以将await包裹在匿名函数中：

    (async () => {
      let response = await fetch('/article/promise-chaining/user.json');
      let user = await response.json();
      ...
    })();
    
当我们使用 async/await 时，几乎就不会用到 .then 了（用await替换），因为为我们 await 处理了异步等待。并且我们可以用 try..catch 来替代 .catch。这通常更加方便（当然不是绝对的）。

    async function f() {

      try {
        let response = await fetch('/no-user-here');
        let user = await response.json();
      } catch(err) {
        // 捕获到 fetch 和 response.json 中的错误
        alert(err);
      }
    }

    f();
    
当我们需要同时等待多个 promise 时，我们可以用 Promise.all 来包裹他们，然后使用 await：

    // 等待多个 promise 结果
    let results = await Promise.all([
      fetch(url1),
      fetch(url2),
      ...
    ]);
    
    
## 事件循环

## 常见练习：

1.红灯三秒亮一次，绿灯一秒亮一次，黄灯2秒亮一次；如何让三个灯不断交替重复亮灯？（用 Promse 实现）

promise解决：

    function red(){
            console.log('red')
        }

        function green(){
            console.log('green')
        }

        function yellow(){
            console.log('yellow')
        }

        function delay(ms,cb){
            return new Promise((resolve,reject)=>{
                setTimeout(() => {
                    cb();
                    resolve()
                }, ms);
            })
        }

        function light(){
           Promise.resolve().then(()=>{
               return delay(1000,red)
           }).then(()=>{
               return delay(2000,green)
           }).then(()=>{
               return delay(3000,yellow)
           }).then(()=>{
               light()
           })
        }

        light()
        
  async解决：
        
        function red() {
            console.log('red')
        }

        function green() {
            console.log('green')
        }

        function yellow() {
            console.log('yellow')
        }

        function delay(ms, cb) {
            return new Promise((resolve, reject) => {
                setTimeout(() => {
                    cb();
                    resolve()
                }, ms);
            })
        }

        async function light() {
            let redlight = await delay(1000, red);
            let greenlight = await delay(2000, green);
            let yellowlight = await delay(3000, yellow);
            return light();
        }

        light()

2.链式调用

        class A {
          constructor() {
          this.list = Promise.resolve();
        }

        say(message) {
          this.list = this.list.then(() => {
            console.log(message);
          });
          return this;
        }

        delay(duration) {
          this.list = this.list.then(
          () =>
          new Promise(resolve => {
          setTimeout(() => {
          resolve();
          }, duration);
          })
          );
          return this;
          }
        }

        const a = new A();

        a.say("1")
        .delay(1000)
        .say("2")
        .delay(4000)
        .say("fsaffa");
3.实现 mergePromise 函数，把传进去的函数数组按顺序先后执行，并且把返回的数据先后放到数组 data 中。

        //全局定义一个promise实例p
        //循环遍历函数数组，每次循环更新p，将要执行的函数item通过p的then方法进行串联，并且将执行结果推入data数组
        //最后将更新的data返回，这样保证后面p调用then方法，如何后面的函数需要使用data只需要将函数改为带参数的函数。

        //模拟异步
        const timeout = ms => new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve();
            }, ms);
        });

        const ajax1 = () => timeout(2000).then(() => {
            console.log('1');
            return 1;
        });

        const ajax2 = () => timeout(1000).then(() => {
            console.log('2');
            return 2;
        });

        const ajax3 = () => timeout(2000).then(() => {
            console.log('3');
            return 3;
        });

        let p = Promise.resolve()

        function mergePromise(arr) {
            let dataArr = []
            arr.forEach(ajax => {
                p = p.then(ajax).then((data) => {
                    dataArr.push(data)
                    return dataArr
                })
            });

            return p

        }

        mergePromise([ajax1, ajax2, ajax3]).then(data => console.log('done', data))

