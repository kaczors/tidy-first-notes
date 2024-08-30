# Notes on "Tidy First" by Kent Beck

## Chapter 1: Guard Classes

Don't
```java
if (condition) {
  // do something
  // and something else
  // (main logic)
}
```
```java
if (condition){
  if (some other condition) {
    // do something
  }
}
```

use "guard" statements instead
```java
if (!condition) {
  return;
}

if (!some other condition) {
  return;
}

// main logic
```

* easier to read
* but don't overdo it, use your judgement (7 or 8 means you should refactor)

[Example on GitHub](https://github.com/Bogdanp/dramatiq/pull/470/files)

## Chapter 2: Dead code

Just exclude dead code, don't comment it out. If you are not sure if it's dead, add log statements to check and then remove it.

Don't
```java
// if (condition) {
//  some old business logic 
//}
```
Do
```java
```
;)
or eventually add a log statement and remove later
```java
log.info("executing some old business logic");
if (condition) {
  some old business logic
}
```