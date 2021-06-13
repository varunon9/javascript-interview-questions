# Hoisting, Scope & Closure

## Hoisting
Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution.

## IIFE
The primary reason to use an IIFE is to obtain data privacy.

## Scope

Scope is the accessibility of variables, functions, and objects in some particular part of your code during runtime. In other words, scope determines the visibility of variables and other resources in areas of your code.

## Closure

A closure is an inner function that has access to the outer (enclosing) function’s variables — scope chain. The closure has three scope chains: it has access to its own scope (variables defined between its curly brackets), it has access to the outer function’s variables, and it has access to the global variables.

## Currying

Currying is a technique of evaluating the function with multiple arguments, into a sequence of function with a single argument.

## Q1. Write a function called fun such that-
```
function fun(str) {
  // todo
}

const str1 = fun('javascript');
const str2 = fun('reactjs');

str1.next(); // 'j'
str1.next(); // 'a'
str1.next(); // 'v'

str2.next(); // 'r'
str2.next(); // 'e'
```

Solution-
```
const fun = function(str) {
  let i = 0;
  return {
    next: function() {
      return str.charAt(i++);
    }
  };
};

const str1 = fun('javascript');
str1.next();
```

## Q2

```
function Comp() {
  const onBtClick = () => {
    // clicking on bt1 should print 2
    // clicking on bt2 should print 4
    // clicking on bt3 should print 6
  };

  return () {
    <div>
      <button id="bt1" onClick={() => {}} />
      <button id="bt2" onClick={() => {}} />
      <button id="bt3" onClick={() => {}} />
    </div>
  }
}
```

## Q3

```
  var a = 1;
  function b() {
    a = 10;
    return;
    function a() {}
    }
  b();
  console.log(a); // 1
```

## Q4

```
console.log(square(5)); // 25
function square(n) { return n * n; }

console.log(square(5)); // 25
```

```
console.log(square(5)); // square is not a function
var square = function(n) { 
  return n * n; 
}
console.log(square(5)); // 25
```

## Q5

```
console.log(1);
(function(arg) {
  console.log(arg);
})(2);
setTimeout(() => {
  console.log(3);
}, 0);
Promise.resolve(4).then(res => console.log(res));
setTimeout(function() {
  console.log(5);
}, 0);
console.log(6);
function print() {
  console.log(7);
}
console.log(8);
print(); // 1, 2, 6, 8, 7, 4, 3, 5
```

## Q6-

HTTP Methods
GET, POST, PUT, DELETE

## Q7-

API contract for login & signup

## Q8-

order of execution- componentDidMount & render

```
<Comp1>
  <Comp2>
    <Comp3 />
  </Comp2>
</Comp1>
```  