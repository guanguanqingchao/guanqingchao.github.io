- 函数默认值

      function log(x, y = 'World') {
        console.log(x, y);
      }

      log('Hello') // Hello World
      log('Hello', 'China') // Hello China
      log('Hello', '') // Hello
