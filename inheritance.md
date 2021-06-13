# Inheritance
```
function Person(name, age, gender) {
  this.name = name;
  this.age = age;
  this.gender = gender;

  this.info = function() {
    console.log(`Name: ${this.name}, Age: ${this.age}, Gender: ${this.gender}`);
  }
}

function Teacher(name, age, gender, subjects = []) {
  this.name = name;
  this.age = age;
  this.gender = gender;

  this.info = function() {
    console.log(`Name: ${this.name}, Age: ${this.age}, Gender: ${this.gender}`);
  }

  this.subjects = subjects;
  this.getSubjects = () => {
    return this.subjects;
  }
}

var ramesh = new Teacher('Ramesh', 22, 'M', ['Physics', 'Chemistry']);
ramesh.info(); // Name: Ramesh, Age: 22, Gender: M
ramesh.getSubjects(); // ["Physics", "Chemistry"]
```

## Q1: since many properties & methods are same, can you redfine Teacher class by reusing Person class? (Inherit Teacher from Person)

### solution-
```
function Teacher(name, age, gender, subjects = []) {
  Person.call(this, name, age, gender);

  this.subjects = subjects;
  this.getSubjects = () => {
    return this.subjects;
  }
}
```

## Q2: what if info method was declared on prototype? 
```
// example-
function Person(name, age, gender) {
  this.name = name;
  this.age = age;
  this.gender = gender;
}

Person.prototype.info = function() {
  console.log(`Name: ${this.name}, Age: ${this.age}, Gender: ${this.gender}`);
}
```

```
// solution-
function Teacher(name, age, gender, subjects = []) {
  Person.call(this, name, age, gender);

  this.subjects = subjects;
  this.getSubjects = () => {
    return this.subjects;
  }
}

// The Object.create() method creates a new object, 
// using an existing object as the prototype of the newly created object
Teacher.prototype = Object.create(Person.prototype); // or new Person()
Teacher.prototype.constructor = Teacher; // optional
```

## Q3

1. What is prototype
2. Object.assign vs Object.create
3. How to check if an object has property in its prototype or its own
4. Advantage of prototype methods over constructor methods
5. Inheritance in ES6
6. Convert ES6 to ES5
7. call vs apply vs bind

```
/*
The JavaScript prototype property also allows you to add new methods 
to objects constructors:

"goibibo".printVowels();

var vowels = ['a', 'e', 'i', 'o', 'u'];
String.prototype.printVowels = function() {
  this.split('').forEach(char => {
    var c = char.toLowerCase();
    if (vowels.indexOf(c) >= 0) {
      console.log(char);
    }
  });
}

var s1 = 'mmt';
var s2 = new String('goibibo');
var s3 = 'redbus';

s1.printVowels(); // extension function

prototype is a special object applicable over constructor function
*/
```
```
/*
The Object.create() method creates a new object, 
using an existing object as the prototype of the newly created object.

The Object.assign() method copies all enumerable own properties from 
one or more source objects to a target object. It returns the target object.

const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
*/
```
```
/*
ramesh.hasOwnProperty('subjects')
true
ramesh.hasOwnProperty('info')
false


Object.keys(ramesh).forEach(key => console.log(key)) // will not print info
for (key in ramesh) {console.log(key)} // will print info
*/
```

```
/*
function Person(name, age) {
  this.name = name;
  this.age = age;

  this.printDetails = function() {

  }
}

Person.prototype.increaseAge = function() {

}


class Person {
  constructor() {
    this.name = name;
    this.age = age;

    this.printDetails = function() {

    }
  }

  increaseAge() {

  }
}
*/
```

### Call, Apply, Bind
bind- returns a new function with fixed this
call & apply- invokes the function with this = passed object [Just one time]

```
/*var mathLib = {
  pi: 3.14,
  circleArea: function(r) {
    return this.pi * r * r
  }
}

mathLib.circleArea(2); // 12.56

var map = {
  pi: 3.149
}

mathLib.circleArea.apply(map, [2]); // 12.596
mathLib.circleArea.call(map, 2);*/ // 12.596

```
## Q4. I want to  get area of circle using pi = 3.149, solution ?

```
var f = mathLib.circleArea;
f();
```