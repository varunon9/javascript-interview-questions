# Chaining in Javascript

Q. Define an object `firebase` such that-

```
firebase
  .firestore()
  .collection('chat')
  .doc('123')
  .collection('sessions')
  .where('status', '==', 'open')
  .orderBy('time', 'desc')
  .limit(10)
  .get()
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

will output - `[]`

Ans-

```
const firebase = {
  firestore: function() {
    return {
      collection: function() {
        return this;
      },
      doc: function() {
        return this;
      },
      where: function() {
        return this;
      },
      orderBy: function() {
        return this;
      },
      limit: function() {
        return this;
      },
      get: function() {
        return new Promise((resolve) => {
          resolve([]);
        });
      }
    };
  }
};
```

Q. Why would arrow function not work here e.g.

```
const firebase = {
  firestore: function() {
    return {
      collection: () => {
        return this;
      },
      doc: () => {
        return this;
      },
      where: () => {
        return this;
      },
      orderBy: () => {
        return this;
      },
      limit: () => {
        return this;
      },
      get: () => {
        return new Promise((resolve) => {
          resolve([]);
        });
      }
    };
  }
};
```

```
firebase.firestore().collection()
// {firestore: ƒ}

firebase.firestore().collection().firestore().doc() 
// {firestore: ƒ}

firebase.firestore().collection().doc() 
// Uncaught TypeError: firebase.firestore(...).collection(...).doc is not a function
```

Ans-

Normal functions in JavaScript bind their own this value, however the this value 
used in arrow functions is actually fetched lexically from the scope it sits inside.
It has no this, so when you use this you’re talking to the outer scope.

Example-

```
var man = {eat: function() {console.log(this)}}
man.eat() // {eat: ƒ}

var man1 = {eat: () => {console.log(this)}}
man1.eat() // window
```