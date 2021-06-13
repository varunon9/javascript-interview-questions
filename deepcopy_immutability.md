## Q1. Output & explanation

```
var a = {};
var b = {};

console.log(a == b); // false
console.log(a === b); // false
``` 

## Q2. Write a function for deep cloning

```
function deepClone(arg) {
  // code
}

deepClone(); // undefined
deepClone(1); // 1
deepClone({a: 1}); // {a: 1}
deepClone([1, 2, {a: 1}]); // [1, 2, {a: 1}]
```

```
const deepClone = arg => {
  if (arg === null || arg === undefined) {
    return null;
  }
  // check if arg is an array
  if (arg instanceof Array) {
    const arrCopy = [];
    arg.forEach(element => {
      arrCopy.push(deepClone(element));
    });
    return arrCopy;
  } else if (typeof arg === 'object') {
    const objCopy = {};
    Object.keys(arg).forEach(key => {
      objCopy[key] = deepClone(arg[key]);
    });
    return objCopy;
  } else {
    return arg;
  }
};
```

# Immutability

Mutable is a type of variable that can be changed. In JavaScript, only objects and arrays are mutable, not primitive values.

(You can make a variable name point to a new value, but the previous value is still held in memory. Hence the need for garbage collection.)

A mutable object is an object whose state can be modified after it is created.

Immutables are the objects whose state cannot be changed once the object is created.

Strings and Numbers are Immutable.

`Object.assign()` spread operators etc to acheive immutability

## Q1. Output

```
var x = {a: 1}, y = {b: 2}, z = 3;
function fun(obj1, obj2, arg3) {
  obj1.a = 5;
  obj2.b = 9;
  obj2 = { b: 8};
  arg3 = 4;
  console.log(obj1.a, obj2.b, arg3); // 5, 8, 4
}
fun(x, y, z);
console.log(x.a, y.b, z); // 5, 9, 3
```

## Q2. Output

```
var a = [9];
function fun(arr) {
  arr.push(8);
  arr = [];
}
fun(a);
console.log(a); // [9, 8]
```

## Q3. How can you make `input1` accessible to `ChildInput2`?

```
function MyComp() {
  return (
    <Parent>
      <ChildInput1 value={input1} onChange={() => {}}/>
      <ChildInput2 />
    </Parent>
  );
}

function ChildInput1() {
  const [value, setValue] = useState('');
  const onChange = text => {
    setValue(text);
  };

  return (
    <input onChange={onChange} value={value} />
  );
}
```

Solution- Lift the state up