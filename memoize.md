## Problem statement

Let's say `getSomeData` accepts two params - a number & a callback.
It does a heavy calculation and then call callback function with result & error 

```
var getSomeData = (num, callback) => {
  // long async running function
  setTimeout(() => {
    callback(num + 1, null);
  }, 2000);
};
```

```
getSomeData(1, (result, error) => {
  console.log(result); // slow - after 2 secs
});

getSomeData(2, (result, error) => {
  console.log(result); // slow - after 2 secs
});

getSomeData(1, (result, error) => {
  console.log(result); // slow - after 2 secs - Make it fast using memoize
});
```

Define a function `memoize` such that when `getSomeData` is called again with same parameter, it returns result instantly.

```
var memoizedGetSomeData = memoize(getSomeData);

memoizedGetSomeData(1, (result, error) => {
  console.log(result); // slow - after 2 secs
});

memoizedGetSomeData(2, (result, error) => {
  console.log(result); // slow - after 2 secs
});

memoizedGetSomeData(1, (result, error) => {
  console.log(result); // fast
});
```

## Solution

```
var memoize = fun => {
  var map = {};
  return (num, callback) => {
    let result = map[num];
    if (result) {
      callback(result, null);
    } else {
      fun(num, (result, error) => {
        map[num] = result;
        callback(result, error);
      });
    }
  };
};
```

### Variant 2

`getSomeData` function can take any no of arguments but the last argument will be callback. Result here should be sum of all args except callback

```
getSomeData(1, callback); // result 1
getSomeData(1, 2, callback); // result 3
getSomeData(1, 2, 3, callback); // result 6
```

### Solution

```
var getSomeData = function(/*...args*/) {
  var args = arguments;
  var argumentsLength = args.length;
  var callback = args[argumentsLength - 1];

  setTimeout(() => {
    let result = 0;
    for (let i = 0; i < argumentsLength - 1; i++) {
      result += args[i];
    }
    callback(result, null);
  }, 2000);
};
```

### Variant 3 

Write memoize function such that order of params matter-

```
var memoizedGetSomeData = memoize(getSomeData);

memoizedGetSomeData(1, (result, error) => {
  console.log(result); // slow - after 2 secs
});

memoizedGetSomeData(1, (result, error) => {
  console.log(result); // fast
});

memoizedGetSomeData(1, 2, (result, error) => {
  console.log(result); // slow - after 2 secs
});

memoizedGetSomeData(1, 2, (result, error) => {
  console.log(result); // fast
});

memoizedGetSomeData(2, 1, (result, error) => {
  console.log(result); // slow - after 2 secs
});

memoizedGetSomeData(2, 1, (result, error) => {
  console.log(result); // fast
});
```

### Solution 

```
var memoize = fun => {
  var map = {};
  return (...args) => {
    var argsLength = args.length;
    var callback = args[argsLength - 1];
    let key = '';
    var funArgs = [];
    for (let i = 0; i < argsLength - 1; i++) {
      key += args[i];
      funArgs.push(args[i]);
    }
    let result = map[key];
    if (result) {
      callback(result, null);
    } else {
      fun(...funArgs, (result, error) => {
        map[key] = result;
        callback(result, error);
      });
    }
  };
};
```

### Variant 4

`arguments` except callback can be number/string/object

`use key += JSON.stringify(args[i]);`

```
var getSomeData = function(/*...args*/) {
  var args = arguments;
  var argumentsLength = args.length;
  var callback = args[argumentsLength - 1];

  setTimeout(() => {
    let result = '';
    for (let i = 0; i < argumentsLength - 1; i++) {
      result += JSON.stringify(args[i]);
    }
    callback(result, null);
  }, 2000);
};
```

```
var memoize = fun => {
  var map = {};
  return (...args) => {
    var argsLength = args.length;
    var callback = args[argsLength - 1];
    let key = '';
    var funArgs = [];
    for (let i = 0; i < argsLength - 1; i++) {
      key += JSON.stringify(args[i]);
      funArgs.push(args[i]);
    }
    let result = map[key];
    if (result) {
      callback(result, null);
    } else {
      fun(...funArgs, (result, error) => {
        map[key] = result;
        callback(result, error);
      });
    }
  };
};
```
