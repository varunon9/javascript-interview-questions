# Debounce

The debounce() function forces a function to wait a certain amount of time before 
running again.
Events such as scrolling, mouse movement, and keypress bring great overhead with them if they are captured every single time they fire. The function aims to reduce overhead by preventing a function from being called several times in succession.

```
const debounce = (fn, time) => {
  let timeout;

  return function() {
    const functionCall = () => fn.apply(this, arguments);

    clearTimeout(timeout);
    timeout = setTimeout(functionCall, time);
  };
};
```

## Q1. Write a debounce function such as-

```
var makeApiCall = function(id) {
  console.log(id);
}

for (let i = 0; i < 5; i++) {
  makeApiCall(i);
}
// 0 1 2 3 4

var debouncedmakeApiCall = debounce(makeApiCall, 2000);

for (let i = 0; i < 5; i++) {
  debouncedmakeApiCall(i);
}
// 4 after 2 secs
```

# Throttle

Throttling is a technique in which, no matter how many times the user fires the event, 
the attached function will be executed only once in a given time interval. 

```
function throttle(fun, delay) {
  let timeout;
  return function() {
    const funCall = () => fun.apply(this, arguments);
    if (!timeout) {
      funCall();
      timeout = true;
      setTimeout(() => {
        timeout = false;
      }, delay);
    }
  }
}
```

# Polyfill

## Q1. Write a customBind function applicable over any function just like bind

```
var person = {
  name: 'Ramesh',
  age: 21,
  info: function(gender) {
    console.log(this.name, this.age, gender);
  }
};

var bindedInfo1 = person.info.bind({
  name: 'Mahesh',
  age: 25
});
bindedInfo1('M'); // Mahesh 25 M

var bindedInfo2 = person.info.bind({
  name: 'Mahesh',
  age: 25
}, 'F');
bindedInfo2('M'); // Mahesh 25 F

Function.prototype.customBind = function(context, ...args) {
  var fun = this;
  return function(...otherArgs) {
    fun.apply(context, [...args, ...otherArgs]);
  }
}

var bindedInfo3 = person.info.customBind({
  name: 'Mahesh',
  age: 25
});
bindedInfo3('M'); // Mahesh 25 M

var bindedInfo4 = person.info.bind({
  name: 'Mahesh',
  age: 25
}, 'F');
bindedInfo4('M'); // Mahesh 25 F
```

## Polyfill of reduce

```
Array.prototype.customReduce = function(callback, initial) {
  const arr = this;
  if (!initial) {
    initial = arr[0];
    arr.splice(0, 1);
  }
  let accumulator = initial;
  arr.map(item => accumulator = callback(accumulator, item));
  return accumulator;
};
```