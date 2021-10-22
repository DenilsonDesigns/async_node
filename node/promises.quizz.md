# Question 1 - (10min)

Create a promise version of the async readFile function

```js
const fs = require("fs");

function readFile(filename, encoding) {
  return new Promise((resolve, reject) => {
    fs.readFile(filename, encoding, (err, data) => {
      if (err) return reject(err);
      resolve(data);
    });
  });
}
readFile("./files/demofile.txt", "utf-8").then(
  (data) => console.log("file read: ", data),
  (err) => console.error("failed to read file", err)
);
```

# Question 2

Load a file from disk using readFile and then compress it using the async zlib node library, use a promise chain to process this work.

```js
const fs = require("fs");
const zlib = require("zlib");

function zlibPromise(data) {
  return new Promise((resolve, reject) => {
    zlib.gzip(data, (err, result) => {
      //TODO
      if (err) reject(err);
      resolve(result);
    });
  });
}

function readFile(filename, encoding) {
  return new Promise((resolve, reject) => {
    fs.readFile(filename, encoding, (err, data) => {
      if (err) reject(err);
      resolve(data);
    });
  });
}

readFile("./files/demofiasdfle.txt", "utf-8")
  .then((data) => zlibPromise(data))
  .then(
    (res) => console.log(res),
    (err) => console.error(err)
  );
// --> Load it then zip it and then print it to screen
```

# Question 3

Convert the previous code so that it now chains the promise as well.

```js
const fs = require("fs");
const zlib = require("zlib");

function zlibPromise(data) {
  return new Promise((resolve, reject) => {
    zlib.gzip(data, (err, result) => {
      //TODO
      if (err) reject(err);
      resolve(result);
    });
  });
}

function readFile(filename, encoding) {
  return new Promise((resolve, reject) => {
    fs.readFile(filename, encoding, (err, data) => {
      if (err) reject(err);
      resolve(data);
    });
  });
}

readFile("./files/demofiasdfle.txt", "utf-8")
  .then((data) => zlibPromise(data))
  .then(
    (res) => console.log(res),
    (err) => console.error(err)
  );
// --> Load it then zip it and then print it to screen
```

# Question 4

Convert the previous code so that it now handles errors using the catch handler

```js
const fs = require("fs");
const zlib = require("zlib");

function zlibPromise(data) {
  return new Promise((resolve, reject) => {
    zlib.gzip(data, (err, result) => {
      //TODO
      if (err) reject(err);
      resolve(result);
      // reject(err);
    });
  });
}

function readFile(filename, encoding) {
  return new Promise((resolve, reject) => {
    fs.readFile(filename, encoding, (err, data) => {
      if (err) reject(err);
      resolve(data);
    });
  });
}

readFile("./files/demofile2.txt", "utf-8")
  .then((data) => zlibPromise(data))
  // .catch((err) => console.error("from catch1"))
  .then((res) => console.log(res))
  .then(() => {
    throw "fail";
  })
  .catch((err) => console.error("from catch2"));
// --> Load it then zip it and then print it to screen
```

# Question 5

Create some code that tries to read from disk a file and times out if it takes longer than 1 seconds, use `Promise.race`

```js
function readFileFake(sleep) {
  return new Promise((resolve) => setTimeout(resolve, sleep));
}

readFileFake(5000); // This resolves a promise after 5 seconds, pretend it's a large file being read from disk
```

# Question 6

Create a process flow which publishes a file from a server, then realises the user needs to login, then makes a login request, the whole chain should error out if it takes longer than 1 seconds. Use `catch` to handle errors and timeouts.

```js
function authenticate() {
  console.log("Authenticating");
  return new Promise(resolve => setTimeout(resolve, 2000, { status: 200 }));
}

function publish() {
  console.log("Publishing");
  return new Promise(resolve => setTimeout(resolve, 2000, { status: 403 }));
}

function timeout(sleep) {
  return new Promise((resolve, reject) => setTimeout(reject, sleep, "timeout"));
}

Promise.race( [publish(), timeout(3000)])
  .then(...)
  .then(...)
  .catch(...);
```
