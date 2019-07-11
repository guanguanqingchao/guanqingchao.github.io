## es6类

### 基本用法

    语法糖，本质上还是构造函数
    
    class Point{
    
      //静态属性
      static prop = 1;
      constructor(x,y){
        this.x = x;
        this.y = y;
      }
      
      //原型方法
      draw(){
        console.log(this.x+this.y) 
      }
      
      //静态方法 static关键字，就表示该方法不会被实例继承，而是直接通过类来调用
       static classMethod() {
        return 'hello';
      }
      
    }
    
    const p = new Point(12,20);
    p.draw();
    
    typeof Point // "function"
    Point === Point.prototype.constructor // true
    //类的实例上面调用方法，其实就是调用原型上的方法。
    //p是Point类的实例，它的constructor方法就是Point类原型的constructor方法。
    p.constructor === Point.prototype.constructor // true
    Point.name // "Point"
    //name属性总是返回紧跟在class关键字后面的类名。
    
    静态方法中的this指类  不是实例。父类的静态方法可以被子类继承
    Point.classMethod() // 'hello'

    
    
    ***** 类的内部所有定义的方法，都是不可枚举的 与ES5区别 *****
    Object.keys(Point.prototype);  []
    
    ***** 类必须使用new调用，否则报错， ES5区别 ******
    
    ***** 类没有变量提升 ********
    
### 继承

    class Point {
    }

    class ColorPoint extends Point {
    }
