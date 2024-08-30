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

## Chapter 5: Reading Order
Use your judgement to decide how the file/class should be read in order to understand the logic better. For example, put the main logic at the top of the file, and helper methods at the bottom.

## Chapter 6: Cohesion Order
Put the methods/code that are used together close to each other. It will be easier to understand the logic and refactor later.

**Don't**
```java
class Stuff {
  parseXml() {}
  createUser() {}
  parseJson() {}
  updateUser() {}
}
```

**Do**
```java
class Stuff {
  parseXml() {}
  parseJson() {}
  createUser() {}
  updateUser() {}
}
```

## Chapter 7: Move Declaration and Initialization Together
If you declare a variable and initialize it later, it's harder to understand the code. Try to move the initialization to the declaration.
**Don't**
```java
int a;
// some logic that doesn't use a
a = ...;
int b;
// more code
b = ...a...
// more code
```
**Do**
```java
int a = ...;
// more code that uses a
int b = ...a...
// more code that uses b
```

## Chapter 8: Explaining Variables
Don't put too much logic in one line, use variables to explain the logic. It will be easier to understand the code and refactor later.

**Don't**
```java
if (a > 5 && b < 10 && c == 0) {
  // some logic
}
```

**Do**
```java
boolean isAValid = a > 5;
boolean isBValid = b < 10;
boolean isCValid = c == 0;

if (isAValid && isBValid && isCValid) {
  // some logic
}
```
## Chapter 9: Explaining Constants
Create a symbolic constant. Replace uses of the literal constant with the symbol.

**Don't**
```java
if (response.code == 404) {
  // some logic
}
```
**Do**
```java
NOT_FOUND = 404;
if (response.code == NOT_FOUND) {
  // some logic
}
```
## Chapter 10: Explicit Parameters
Don't use global variables, pass them as parameters to the method. 

Don't use map like parameters, unless you have option to call it with named parameters.

**Don't**
```java
params = {a: 1, b: 2}
foo(params)

function foo(params) {
  ...params.a... ...params.b...
}
```

**Do**
```java
function foo(params){
  foo_body(params.a, params.b)
}

function foo_body(a, b) {
  ...a... ...b...
}
```
```java
interface Params {
  int a;
  int b;
}

function foo(Params params) {
  ...params.a... ...params.b...
}

foo(Params
  .builder()
  .a(1)
  .b(2)
  .build());
```
## Chapter 11: Chunk Statements
If you have a long method, try to split it into smaller methods. It will be easier to understand the logic.

**Don't**
```java
method() {
  // 100 lines of code
  
  // intentional space to make it more readable
  // another 100 lines of code
}
```

**Do**
```java
method() {
  method1();
  method2();
}

method1() {
  // 100 lines of code
}

method2() {
  // another 100 lines of code
}
```