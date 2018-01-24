# Objects

An **object** is a collection of **properties**, which are described in the form of **key/value** pairs. In other words, key/value pairs make up a property name and its value, which is assigned to an object. We can use objects to model "things" using code.

Three main types of objects:
1. **Host objects** - objects defined by the environment in which your code is run. E.g. a web browser is a host environment that have the defined objects: `document`, `window`, `console`, etc
2. **Core objects** - objects defined by and built into the js language. E.g. `Math`, `Date`, `Number`
3. Everything else - objects defined by code's author, or objects defined by js libraries

Quick links:
- [Creating Objects](#creatingobj)
  - [Object literals](#objliteral)
  - [Object constructors](#objconstrct)
- [Global objects](#globalobjects)
- [Local and global scopes](#scopes)
- [Properties](#properties)

## Object Oriented Programming

- Revolves around objects and how code moves back and forth between objects

Procedural | Object Oriented
----|----
Step-by-step instructions read from top to bottom | Objects pass code back and forth amongst one another


## <a name="creatingobj"></a> Creating Objects 

### <a name="objliteral"></a> Object Literal 

```js
// OBJECT LITERAL NOTATION
var myObject = {
  key: value,
  property: anotherValue 
};

var myCoffee = {
  flavor: "french vanilla",
  temperature: "hot",
  milk: true,
  sugar: 3
  };
```
Objer literals create and define an object at the same time; creating an **instance** of an object.

### Methods

A function in an object (a property with a function as the value). For example, in `console.log()`, log is a method of the `console` object.

When a function is associated only with a specific object, using the term method is more specific since it implies association with an object, and not a global function.

```js
var myCoffee = {
  flavor: "french vanilla",
  temperature: "hot",
  milk: true,
  sugar: 3
  reheat: function() {
    if (myCoffee.temperature === "cold") {
      myCoffee.temperature = "hot"; // set value back to hot if coffee temperature is cold
    }
  }
};

myCoffee.temperature = "cold"; // assign new value to property
myCoffee['temperature'] = "cold"; // another way to assign values to properties

myCoffee.reheat();

reheat(); // won't work because it's a method and requires and object it is associated to
```

### <a name="objconstrct"></a> Object Constructor 

- Defines a template for an object, also known as a **prototype**.
- Function used to create **multiple instances** of an object
  - Best practice to name the function with the first letter capitalized
  - Each instance inherits the properties and methods of its constructor

```js
function Friend(name, tshirtColor) {
  this.name = name;
  this.tshirtColor = tshirtColor;
}

// create an instance of the constructor
var joe = new Friend("Joe", "red");


// in object literal notation:
var joe = {
  name: "Joe",
  tshirtColor: "red"
};
```

#### `Object.create()`

- What's used behind the scenes for object literals and constructors

```js
// use Object.create() function to create new object
// pass in the object that will become the prototype for the new object
var joe = Object.create(Object.prototype,
  {
    name: {
      value: 'Joe',
      enumerable: true,
      writable: true,
      configurable: true
    },
    tshirtColor: {
      value: 'red',
      enumerable: true,
      writable: true,
      configurable: true
    }
  });
```
#### Create object using ES6 classes
```js
class Friend {
  constructor(name, tshirtColor) {
    this.name = name
    this.tshirtColor = tshirtColor
  }
  speak() { // add method to object
    console.log('Hello!')
  }
}

var joe = new Friend('Joe', 'red');
joe.speak(); // logs 'Hello!'
```

## <a name="globalobjects"></a> The Global Object

- In js when we're creating variables, functions, and objects, they become properties of the **global object**.
  - The window is the **global object** when the host environment is the web. When we write a function that isn't associated with an object, it becomes a method of the global object.

```js 
function sayHello() {
  alert('Hello');
};

// below do the same thing
sayHello();
window.sayHello();

// below overrides the original window.alert() method
function alert() {
  console.log('Hello');
};
```



## <a name="scopes"></a> Local & Global Scope

- Variables in the **global scope** (global variables) can be accessed, changed, and used anywhere in the code; Can be risky since it can accidentally overwrite other code
  - Should be careful when creating things that are global in scope, especially when js libaries such as jQuery is involved.
  

- JS uses *function scope*, meaning every time we create a new function the scope changes. Any code inside of a function can't be accessed outside of the function, because its scope is **local** to that function. 
  - Forgetting the `var` keyword when declaring a variable inside of a function can create a global variable
  
  
  
## <a name="properties"></a> Properties

Properties with **dot notation vs. bracket notation**

```js
var cat = {
  name: 'Fluffy',
  color: 'white'
}

console.log(cat.color) // dot notation
console.log(cat['color']) // bracket notation

cat['eye color'] = 'green'
console.log(cat['eye color']) // returns 'green'
```

Why would you do brackets instead of dot notation?
- Useful in creating an object with the values entered by a user

### Property descriptor

- Properties are more than just a name and value! Each property has a **property descriptor** that we can use to see the attributes of that property.

```js

// get the property descriptor for the 'name' property
Object.getOwnPropertyDescriptor(cat, 'name') 
/* returns:
Object {
  value: Fluffy,
  writable: true, // this attribute defines whether the property's value can be changed from its initial value
  enumerable: true,
  configurable: true
}
*/
```

#### Changing the property attributes

- `writable` attribute 

```js
// change the writable attribute to false
Object.defineProperty(cat, 'name', {writeable: false});
cat.name = 'Oreo'; // in strict mode this throws an error: cannot assign to read only property
```

But what if we have the `cat.name` property as an object?

```js
var cat = {
  // have name property point to an object
  name: { 
    // with the properties first and last
    first: 'Fluffy', 
    last: 'Jones'
  },
  color: 'white'
}