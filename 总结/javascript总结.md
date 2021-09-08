[Toc]

## 创建对象的三种方法

- 工厂模式
  - 通过直接调用函数的方式，在函数内部创建一个对象，然后给这个对象添加属性后返回。
  - 缺点：通过这种方式创建的对象与工厂函数没有任何联系，其隐式原型对象直接绑定到Object.prototype。通过工厂函数创建的实例对象彼此之间也没有任何联系。
- 构造函数模式
  - 特点：不显式地创建对象，属性与方法赋值给this，没有返回值，通过new调用来创建对象。
  - 补充new操作符调用执行的操作：
    - 在堆内存中创建一个新对象
    - 在这个新对象的[[prototype]]特性被赋值为构造函数的原型对象值(.prototype)
    - 构造函数的this被赋值为这个新对象
    - 执行构造函数代码给对象赋予属性与方法
    - 如果构造函数有返回值，且返回值为非空对象，则返回这个对象。如果没有返回值，则返回新创建的对象。
  - 构造函数的特点
    - 函数声明与函数表达式均可
    - 不传参的情况下可以不写括号(new Person)
    - 构造函数与其它的函数没有区别，通过new调用的就是构造函数，返回值参见上面第五条。
  - 构造函数的问题
    - 其定义的方法会为每一个实例对象都创建一遍。（sayName:function(){console.log(this.name)}相当于sayName:new Function("console.log(this.name)")）浪费内存。
    - 解决方法可以是(sayName:sayName)，在构造函数外部声明函数。但这样做会搞乱全局作用域，因为这个函数只会在一个函数里调用，如果构造函数里有多个方法，那么就可能需要在全局作用域中声明多个只适用于一个函数的函数。
- 原型模式
  - 将实例对象共享的属性和方法赋予原型对象。
  - 原理：
    - 实例对象在原型链中有[[prototype]]特性绑定在构造函数的原型对象上，如果一个属性或方法在对象本身中无法找到，就会沿着原型链的[[prototype]]特性向上查找。（即实例对象与原型对象之间有直接的联系）
  - 问题：
    - 在原型对象上保存的引用类型值，比如数组。这个值是不可重写的，因为person1.friends这个数组属性会遮蔽原型对象的同名属性，但如果对其进行增删改查则会在原型上进行修改。造成修改一个实例对象却影响所有实例对象。一般来说，不同的实例应该有自己的属性副本，实际开发中通常不单独使用原型模式。解决方法放在继承中写。

## 对象的属性枚举

- for-in循环
  - in操作符只要你能通过对象访问到属性就返回true，无论这个属性存在于实例还是原型。
  - 只要可以通过对象访问属性，即属性存在于实例对象或者原型对象上。那么就可以通过for-in返回。
  - 配合.hasOwnProperty()方法可以判断属性或方法来源与实例或是原型
- Object.keys()
  - 返回所有可枚举的实例对象的属性键名组成的数组。
- Object.getOwnPropertyNames()
  - 返回所有实例属性键名组成的数组，无论是否可枚举

- 对象迭代

  - 为了避免多次修改原型对象Person.prototype，通过修改对象字面量的方式重写原型对象能够一次性写完所有属性和方法

    - ```
      function Person(){}
      Person.prototype={
      	// 省略
      }
      ```

    - 重写会造成Person.prototype.constructor===function Object()。以及在重写之前通过构造函数创建的对象不再与Person.prototype有任何联系。（因为`person1.__proto__`依然保存有旧的原型对象，和现有的Person.prototype不再有任何关系）

    - 可以显式地为Person.prototype.constructor赋值，但要注意人为赋值的enumerable值为true是可枚举的，而默认是不可枚举的。建议使用Obejct.defineProperty()为其赋值。

## 六种继承

- 原型链继承

  - 将一个构造函数(SuperType)的实例对象赋值给另一个构造函数(SubType)的原型对象，那么就在new SubType与SuperType.prototype之间形成了原型链继承。
  - 函数的原型对象是一个对象，也就是Object构造函数的实例，那么函数的的原型链就有 function foo(){} --> Function() --> Function.prototype --> Object() --> Object.prototype
  - 问题：原型链继承的问题在于引用类型的操作上。任何对引用类型的增删改查都会改变堆内存中的数据。

- 盗用构造函数（经典继承）

  - 优点：可以使每一个实例对象独立地创建引用类型数据的副本。允许传参。

  - 缺点：必须在构造函数中定义方法，同样会每次创建实例都创建一遍方法。实例对象与SuperType没关系。

  - 使用方法：在SubType构造函数中调用SuperType。

    - ```
      function SuperType(){
      	this.colors=['red','blue','green']
      }
      
      function SubType(){
      	SuperType.call(this)
      }
      
      let instance1 = new SubType()
      instance1.colors.push('black')
      console.log(instance1.colors) // ["red", "blue", "green", "black"]
      
      let instance2 = new SubType()
      console.log(instance2.colors) // ["red", "blue", "green"]
      ```

  - 盗用构造函数允许传递参数

    - ```
              function SuperType(name) {
                  this.name = name
              }
      
              function SubType(name) {
                  SuperType.call(this, name)
              }
      
              let instance1 = new SubType('Nick')
              console.log(instance1.name) // 'Nick'
      
              let instance2 = new SubType('Greece')
              console.log(instance2.name) // 'Greece'

- 组合继承

  - 综合了原型链继承与经典继承
  - 通过原型链继承原型上的属性和方法，而通过盗用构造函数继承实例属性。
  - 在盗用构造函数的基础上，将SuperType的实例赋值为SubType的原型对象。

- 原型式继承

  - ```
    function object(obj){
    	function F(){}
    	F.prototype = obj
    	return new F()
    } 
    ```

  - 上面新这段代码执行的试一次浅复制，也因此对于引用值属性的修改会直接的修改原型对象的值。

  - ES5通过Object.create()将这段代码规范化了

- 寄生式继承

  - 在原型式继承的基础上添加了为这个新的实例对象赋值函数的操作。
  - 缺点与构造函数一样，使得函数被重复创建浪费内存。

- 寄生式组合继承

  - 组合继承的效率问题在于父类构造函数会被调用两次：一次在赋值给子类原型对象上，一次在创建子类实例对象时。
  - 原理：1.浅复制SuperType.prototype对象，2.作为SubType的原型对象，3.并修改这个对象的constructor为SubType。4.通过寄生式继承调用SuperType。
  - 这是引用类继承的最佳模式。

## 原型对象与原型链

- 可以通过isPrototypeOf()确定两个对象之间的关系
- 可以通过Object.getPrototypeOf()获取实例对象的原型对象
- 可以通过Object.create()来创建一个新对象，被创建的对象的[[prototype]]特性绑定为参数对象
  - Object.create()创建的对象属于浅拷贝，因为创建的对象本身并不会复制参数对象的属性为自己的属性，而是通过原型链查找获取属性和方法。
- 