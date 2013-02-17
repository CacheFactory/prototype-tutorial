prototype-tutorial
==================

Because the JavaScript prototype pattern sucks.

HTML version [JavaScript prototype tutorial](http://process-smith.com/prototype-tutorial)

### Simplifying inheritance in JavaScript

The JavaScript prototype pattern is one of the most confusing aspects of the JavaScript language. That being said, inheritance is a very useful pattern, so figuring out how to do it is important if you want to write powerful applications. JavaScript is also a very malleable language, so there are more than one way to achieve inheritance. 

Since the prototype pattern is so confusing, I would recommend against using the raw prototype property because there are cleaner solutions to the inheritance problem. In general I try to stay away from any coding patterns that are confusing or not well understood, since it makes more code more readable and bug free. The two solutions for implementing inheritance in JavaScript that I find cleaner are Underscore.js's "extend" method and Prototype.js "Class". 

*   [Underscore.js "extend"][1] Combines a parent and child objects into a new object where properties in the parent object are overwritten by the child object.
*   [Prototype.js "Class"][2] A wrapper around JavaScript's prototype that makes JavaScript inheritance more like traditional inheritance. 

These two approaches can be a solution to most problems where inheritance in needed. There are still a few scenarios where using JavaScript's prototype inheritance is the best solution. 

1.  Reducing memory footprint: In the prototype pattern objects that inherit from a prototype share one object.
2.  Properties of a super class changing after construction.
3.  To impress people in JavaScript interviews: Since the prototype is so confusing and involves low level knowledge of the language it is often used as a barometer of one's JavaScript knowledge in interviews. 

### Prototypical inheritance

In prototypical inheritance, inheritance is achieved by looking at an object's prototype if the property does not exist in the given object. The difference between prototypical inheritance and traditional inheritance is that the prototype must be an actual object and not an abstract class. For example, in Java you don't need to create a new object of a class to inherit from, in JavaScript you must create an actual object to set a prototype. 

### prototype function property

Setting the prototype property of a function is the most traditional prototype pattern. It should be noted this is the only prototype pattern that works on modern browsers and older IE browsers. 

In this pattern an initial function is created that will later be inherited from, called Bar. To set a prototype it must take an object, so we must initialize a new Bar. I call this barProto. Then we must define the Foo function and set its prototype to barProto. Now we can initialize a new Foo and its prototype will be set to Bar. 

 ```javascript
 var Bar = function(){
   this.name='Bar'
}
var barProto = new Bar()  
var Foo = function(){
}
Foo.prototype= barProto
var foo = new Foo()

console.log(foo.name) // Bar
```

You can see the the `var Foo = function()` does not have a `name` property. However, Bar does have the `name` property. At runtime JavaScript first tries to get `name` from foo, it does not exist so it then checks foo's prototype barProto for name. It does exist, so `foo.name` will get 'Bar' from `barProto`. 

The most confusing part of this pattern is understanding what the prototype property actually is. It is not like any other property in JavaScript because how the `new` keyword effects Foo during construction. Remember when `new` is used `this` inside the function is the new object that is being created and its prototype has already been set. When the actual new object is created by JavaScript from a function the prototype property of the function is looked at to set the object that is that is `this` new object is inherited from. In modern browsers the function's prototype property is the same object that is in the new objects `__proto__` property. However, in IE < 9 this is not the case. 

### \_\_proto\_\_ object property

Settings the `__proto__` property of an object skips the process described in the previous prototype property section and just sets the actual prototype object. Again, this method does not work in older browsers. I believe this method is a lot more transparent than the previous prototype function property method. Also in this method you don't have to use functions as what you inherit from you could just use objects. 

 ```javascript
 var Bar = function(){
   this.name='Bar'
}
var barProto = new Bar() 
var Foo = function(){
    this.__proto__= barProto
}
var foo = new Foo()

console.log(foo.name) // Bar
```

The only difference between this method and the previous example is how the prototype is set. Once the prototype is set, the inheritance chain works exactly the same in runtime. Again, if the object that a property is looked up and it's not there, its prototype will be then checked for the property. 

You can also use the `__proto__` method without using `new` at all as follows. 

 ```javascript
 var Bar = {
   name:'Bar'
}
var foo = {
    __proto__: Bar
}

console.log(foo.name) // Bar
```

### Object.create

`Object.create` is a new way (ECMAScript 5) to implement prototypical inheritance more succinctly. Without setting the `__proto__` directly, the object that is created from `Object.create` will have its prototype be the first parameter. 

 ```javascript
 var Bar = {
   name:'Bar'
}
var foo = Object.create(Bar,{});

console.log(foo.name) // Bar
```

 [1]: http://underscorejs.org/#extend
 [2]: http://prototypejs.org/learn/class-inheritance.html
