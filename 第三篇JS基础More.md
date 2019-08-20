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
