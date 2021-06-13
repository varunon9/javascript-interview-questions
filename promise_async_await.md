# Promises

```
function makeApiCall(id) {
  return fetch(`http://example.com/${id}`)
    .then(response => response.json);
}

makeApiCall(1)
 .then(result => console.log(result))
 .catch(error => console.log(error));
```

## Q1. Mock above function with setTimeout

```
function makeApiCall(id) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({id, success: true});
    }, 2000);
  });
}
```

## Q2. Lets say we have an array of ids, call API and get aggregate result

```
var getAggregateResponse = (ids = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]) => {
  // make use of makeApiCall
};

getAggregateResponse(); // should get all aggregated response (array)
```

Solution-

```
var getAggregateResponse = async (ids = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]) => {
  const responseArray = [];
  for (id of ids) {
    const response = await makeApiCall(id);
    responseArray.push(response);
  }
  return responseArray;
};

getAggregateResponse().then(response => console.log(response)); // after 20 secs
```

## Q3. Call APIs in parallel

```
var getAggregateResponse = async (ids = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]) => {
  const promises = [];
  ids.forEach(id => {
    promises.push(makeApiCall(id));
  });
  return Promise.all(promises);
};

getAggregateResponse().then(response => console.log(response)); // after ~2 secs
```

## Q4. If ids array is too long then above solution would crash due to I/O failure

Solution- call APIs in batches

```
function chunkArray(arr, chunkLength) {
  const chunks = [];
  while (arr.length) {
    chunks.push(arr.splice(0, chunkLength));
  }
  return chunks;
}

var getAggregateResponse = async (ids = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]) => {
  const responses = [];
  const chunkIds = chunkArray(ids, 3); // batch of 3
  for (ids of chunkIds) {
    const promises = [];
    ids.forEach(id => {
      promises.push(makeApiCall(id));
    });
    const responseArr = await Promise.all(promises);
    responses.push(...responseArr);
  }
  return responses;
};

getAggregateResponse().then(response => console.log(response)); // after (12 / 3) * 2 secs
```

#### Promise will be evaluated only once. It  caches the result
```
var p = new Promise((resolve, reject) => {
  console.log('executing promise');
  resolve(1);
});
p.then(result => {
  console.log('promise resolved', result);
}, error => {
  console.log('promise rejected', error);
});
p.then(result => {
  console.log('promise resolved', result);
}, error => {
  console.log('promise rejected', error);
});
// executing promise
// promise resolved 1
//promise resolved 1
```