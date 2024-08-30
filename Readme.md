# Notes on "Tidy First" by Kent Beck

## Chapter 1: Guard Classes

**Don't**
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

**Don't**
```java
// if (condition) {
//  some old business logic 
//}
```
**Do**
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

## Chapter 3: Normalize Symmetries

For similar (pr the same) logic patterns use the same code structure.

**Don't**
```java
method1(String foo) {
  if (foo != null){
    return foo;
  }
  return 5;
}

method2(String foo) {
  return foo == null 
    ? 5 
    : foo;
}
```

**Do** (method logic is just the exmple, the point is to use the same structure)
```java
method1(String foo) {
  return foo == null 
    ? 5 
    : foo;
}

method2(String foo) {
  return foo == null 
    ? 5 
    : foo;
}
```
## Chapter 4: New Interface, Old Implementation
Create new shiny interface (signature) which under the hood will some old nasty one, so you can refactor the implementation later without changing the interface.

**Do**
```java
interface NewShinyInterface {
  void niceSignature();
}

class OldImplementation implements NewInterface {
  void niceSignature() {
    this.oldNastyMethod();
  }

  void oldNastyMethod() {
    // old implementation
  }
}
```
(or something like that, for example via **Adapter** pattern)
