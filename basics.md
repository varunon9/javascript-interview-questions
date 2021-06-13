## Q1-

```
console.log(1);
(function(arg) {
  console.log(arg);
})(2);
setTimeout(() => {
  console.log(3);
}, 0);
setTimeout(function() {
  console.log(4);
}, 0);
console.log(5);
function print() {
  console.log(6);
}
console.log(7);
print();
```

## Q2-

```
for (var i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i);
  }, 0);
}
```

```
for (var i = 0; i < 5; i++) {
  setTimeout((function(arg) {
    return function() {
      console.log(arg);
    }
  })(i), 0);
}
```

## Q3-

```
var a;
if (a) {
  a = 1;
} else {
  a = 0;
}
if (a) {
  a = 11;
} else {
  a = 10;
}
console.log(a);
```

## Q4-

'goibibo'.printVowels(); // 4

## Q5-

```
var a = 1;
function b() {
  a = 10;
  return;
  function a() {}
  }
b();
console.log(a); // output ?
```

## Q6-

```
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.print = () => {
    console.log(this.name, this.age);
  };
}

var p = Person();
p.print();
```

## Q7-

HTTP Methods
GET, POST, PUT, DELETE

## Q8-

API contract for login & signup

## Q9-

order of execution- componentDidMount & render

```
<Comp1>
  <Comp2>
    <Comp3 />
  </Comp2>
</Comp1>  
```


## Q10. Array reduce polyfill

```
Array.prototype.myReduce = function (callback, initialValue) {
  let result = initialValue;
  for(let i = 0; i< this.length ; i++) {
       result = callback(result, this[i])
  }
  return result;
}
```

## Q11. Mock API call

```
function makeApiReuest(payload) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({id: payload.id, success: true});
    }, 1000);
  });
}
```

## Q12.

To send multicast notification-

```
firebaseAdmin.messaging().sendMulticast({
  notification: {
    title: '',
    body: ''
  },
  tokens: [500] // upto 500 tokens
});
```

Response-

```
{
  "responses":[
    {
      "success":true,
      "messageId":"projects/goibibo/messages/0:1610728742532571%8e4205a78e4205a7"
    }
  ],
  "successCount":1,
  "failureCount":0
}
```


`sendNoti(payload)`


`const tokens = [1, 2, 3, 4, 5, 6, 7, 8, 9, ...];`


