```
function Person(name,age){
	this.name=name
	this.age=age
}
```

- this:通常情况下任何函数都有this对象
- this本质是一个对象，代表着调用这个函数的对象



```
function Person(name,age){
	this.name=name
	this.age=age
}
Person('zs',23)
```

- 构造函数当做普通函数调用，相当于给window对象添加属性和方法

- 通过`let obj=new Person('zs',18)`

