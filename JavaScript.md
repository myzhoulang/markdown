1. 原型如何实现继承

   > * 组合继承方式
   >
   >   这种方式是在子类中使用`Parent.call`调用父类构造函数，继承父类属性。将子类的`prototype`属性赋值为父类的实例。来继承父类方法。这种方式的缺点是在子类的原型中会会存在多余的父类属性。
   >
   > * 寄生组合继承
   >
   >   介于组合继承方式的缺点，需要改变子类继承父类方法的方式。可以在子类中可以将`prototype`属性赋值为父类的`prototype`，同时将`prototype`的`constructor`属性指向子类构造函数。
   >
   >   ```js
   >   function Parent(name){
   >     this.name = name
   >   }
   >   Parent.prototype.say = function(){
   >     console.log('parent say')
   >   }
   >   
   >   function Child(name, age){
   >     Parent.call(this, name);
   >     this.age = age;
   >   }
   >   
   >   Child.prototype = Object.create(Parent.prototype, {
   >     constructor: {
   >       value: Child
   >     }
   >   }
   >   new Child('jack', 12).say(); // parent say
   >   ```
   >
   >   
   >
   > * 使用`es6 class`方式