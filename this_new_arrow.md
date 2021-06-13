# This 

In JavaScript, we use the this keyword as a shortcut, a referent; it refers to an object; that is, the subject in context, or the subject of the executing code.

## Q1. Output?

```
var firstName = 'Ramesh';
var person = {
  firstName: 'siddharth',
  lastName: 'gautam',
  getFullName: function() {
    console.log(this.firstName + ' ' + this.lastName);
  }
};

function callAfter1Sec(callback) {
  setTimeout(function() {
    callback();
  }, 1000);
}

callAfter1Sec(person.getFullName); // Ramesh undefined
```

## Q2. Why did this happen & how to fix it?

```
// 1st way
person.getFullName = person.getFullName.bind(person);
callAfter1Sec(person.getFullName); // siddharth gautam
```

```
// 2nd way
function callAfter1Sec(callback) {
  setTimeout(function() {
    callback.call(person);
  }, 1000);
}

callAfter1Sec(person.getFullName); // siddharth gautam
```

## Q2. this inside closure/inner function

```
var person = {
  firstName: 'siddharth',
  lastName: 'gautam',
  getFullName: function() {
    function printName() {
      console.log(this.firstName + ' ' + this.lastName);
    }
    printName();
  }
};

person.getFullName(); // undefined undefined
```

Fixes-

```
var person = {
  firstName: 'siddharth',
  lastName: 'gautam',
  getFullName: function() {
    var self = this;
    function printName() {
      console.log(self.firstName + ' ' + self.lastName);
    }
    printName();
  }
};

person.getFullName(); // siddharth gautam
```

or

```
var person = {
  firstName: 'siddharth',
  lastName: 'gautam',
  getFullName: function() {
    const printName = () => console.log(this.firstName + ' ' + this.lastName);
    printName();
  }
};

person.getFullName(); // siddharth gautam
```

## Q3. this when method is assigned to variable

```
var person = {
  firstName: 'siddharth',
  lastName: 'gautam',
  getFullName: function() {
    console.log(this.firstName + ' ' + this.lastName);
  }
};

var getFullName = person.getFullName;
getFullName(); // undefined undefined
```

Fixes

```
var getFullName = person.getFullName.bind(person);
```

## Q4. this when borrowing function

```
var person1 = {
  firstName: 'siddharth',
  lastName: 'gautam',
  getFullName: function() {
    console.log(this.firstName + ' ' + this.lastName);
  }
};

var person2 = {
  firstName: 'ranjan',
  lastName: 'kumar'
};

person2.fullName = person1.getFullName(); // siddharth gautam

```

Fix

```
person2.fullName = person1.getFullName.apply(person2);
```

## this keyword in arrow function
Unlike normal functions, arrowfunctions don't get their own this keyword.
They simply use the this keyword of the function they are written in. They have a lexical this variable.

```
var person = {
  firstName: 'siddharth',
  lastName: 'gautam',
  getFullName: function() {
    var printName = () => {
      console.log(this.firstName + ' ' + this.lastName);
    }
    printName();
  }
};

person.getFullName(); // siddharth gautam
```

catch-

```
var person = {
  firstName: 'siddharth',
  lastName: 'gautam',
  getFullName: () => {
    var printName = () => {
      console.log(this.firstName + ' ' + this.lastName);
    }
    printName();
  }
};

person.getFullName();  // undefined undefined
```

### Arrow functions establish "this" based on the scope the Arrow function is defined within.

### When to use arrow functions-

Perhaps the greatest benefit of using Arrow functions is with DOM-level methods (setTimeout, setInterval, addEventListener) that usually required some kind of closure, call, apply or bind to ensure the function executed in the proper scope.

```
var obj = {
    count : 10,
    doSomethingLater : function (){
        setTimeout(function(){ // the function executes on the window scope
            this.count++;
            console.log(this.count);
        }, 300);
    }
}

obj.doSomethingLater(); // console prints "NaN", because the property "count" is not in the window scope.

```

```
var obj = {
    count : 10,
    doSomethingLater : function(){ // of course, arrow functions are not suited for methods
        setTimeout( () => { // since the arrow function was created within the "obj", it assumes the object's "this"
            this.count++;
            console.log(this.count);
        }, 300);
    }
}

obj.doSomethingLater();
```

# new operator


It does 5 things:

1. It creates a new object. The type of this object is simply object.
2. It sets this new object's internal, inaccessible, [[prototype]] (i.e. __proto__) property to be the constructor function's external, accessible, prototype object (every function object automatically has a prototype property).
3. It makes the this variable point to the newly created object.
4. It executes the constructor function, using the newly created object whenever this is mentioned.
5. It returns the newly created object, unless the constructor function returns a non-null object reference. In this case, that object reference is returned instead.

## Q1. output-

```
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.print = () => {
    console.log(this.name, this.age);
  }
}

var p1 = new Person('Ramesh', 22);
p1.print(); // Ramesh 22

var p2 = Person('Suresh', 23);
p2.print(); // cannot read property print of undefined

console.log(name, age); // Suresh 23
```

## Q2. output-

```
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.print = () => {
    console.log(this.name, this.age);
  }
  return {
    name: 'Mahesh',
    age: 20
  };
}

var p1 = new Person('Ramesh', 22);
p1.print(); // p1.print is not a function

p1.age; // 20
p1.name; // Mahesh
```