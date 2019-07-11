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

    class ColorPoint extends Point {
      constructor(x, y, color) {
        super(x, y); // 调用父类的constructor(x, y)
        this.color = color;
      }

      toString() {
        return this.color + ' ' + super.toString(); // 调用父类的toString()
      }
    }
    
    let cp = new ColorPoint(25, 8, 'green');
    cp instanceof ColorPoint // true
    cp instanceof Point // true
    
    // Object.getPrototypeOf方法可以用来从子类上获取父类。
    Object.getPrototypeOf(ColorPoint) === Point
    // true 
    
    

    
    ***** 在子类的构造函数中，只有调用super之后，才可以使用this关键字，否则会报错 *****
    *** super ***
    
    super这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。

    1. super作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次super函数。作为函数时，super()只能用在子类的构造函数之中，用在其他地方就会报错。
    
    2. super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。
