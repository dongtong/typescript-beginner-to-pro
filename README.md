## TypeScript From Beginning To Professionals



### Basic

#### Install prerequisite softwares

* [Node.js](https://nodejs.org)

Install corresponding distribution in your environment.

After instanllation, check the node version

```bash
> node --version
> npm --version
```

* [Yarn](https://yarnpkg.com/) package manager

Install yarn globally.

```bash
> npm install yarn -g
```

* [Visual Studio Code](https://code.visualstudio.com)

#### Getting started

Create a workspace and initial a project with npm command

```bash
> mkdir basic
> cd basic
> npm init -y
```

Install typescript under the project, please do not install it globally

```bash
> yarn add typescript
info All dependencies
└─ typescript@4.4.3
```

From output to see, we use typescript v4.4.3. Next, we could use npx to generate the typescript configuration(`tsconfig.json`)

```bash
> npx tsc --init
message TS6071: Successfully created a tsconfig.json file.
```

We could tell tsc command the root dir and output dir when we run the command 

```bash
> npx tsc --init --rootdir src --outdir lib
```

Use vscode to open the project and create the directories and index.ts(TypeScript extension is ts)

```bash
> mkdir src lib
> touch src/index.ts
> code .
```

Input the below contect in `src/index.ts`

```typescript
const message: string = "Hello TypeScript";
console.log(message)
```

> We added type annotation to variable message.

Browser can not execute the typescript directly, we need to compile TypeScript file to JavaScript file. Use tsc to watch(watch mode) the file change. If there is change in source code(`src`), it will compile them on the fly, and generate the file under lib directory.

```bash
> npx tsc --watch
[8:55:08 AM] Starting compilation in watch mode...

[8:55:08 AM] Found 0 errors. Watching for file changes.
```

You will found lib directory has index.js file.

```JavaScript
"use strict";
var message = "Hello TypeScript";
console.log(message);
```

This content could run in browser or we could run it with node

```bash
❯ node lib/index.js 
Hello TypeScript
```



#### Primitive Types

TypeScript has some built-in primitive types.

* number
* boolean
* string
* undefined
* null
* symbol
* bigint

> Note: that the type name is lowercase



#### Instance Types

We could also call it reference types. We could use generic type to define them.

* RegExp

  JavaScript

  ```javascript
  const regex = new RegExp('^d+$');
  ```

  TypeScript

  ```typescript
  const regex: RegExp = new RegExp('^d+$');
  ```

  

* Array

  JavaScript

  ```javascript
  const array = [1, 2, 3];
  ```

  TypeScript

  ```typescript
  const array: Array<number> = [1, 2, 3];
  ```

  

* Set

  JavaScript

  ```javascript
  const set = new Set([1, 2, 3]);
  ```

  TypeScript

  ```typescript
  const set: Set<number> = new Set([1, 2, 3]);
  ```

  

Java/C# developer should be  familar with below generic definition.

```typescript
class Queue<T> {
  private data: Array<T> = [];
  push(item: T) {
    this.data.push(item);
  }
  popup(): T | undefined {
    return this.data.shift();
  }
}

let queue: Queue<number> = new Queue();
```



#### Array

Use JavaScript to define an array variable

```javascript
const a = [1, 2, 3];
```

TypeScript has multiple ways to define array variable.

* Use TypeScript tp define an array variable, using generic, the second parameter is generic name.

```typescript
const a: Array<number> = [1, 2, 3];
```

* Use square brackets

```typescript
const a: number[] = [1, 2, 3];
```



When you define an array with certain type, you can not re-assign other type value.

```typescript
let a: number[] = [1, 2, 3];
a = [1, 2] ;
a = ["typescript"]; // Error
```



##### Tuple

Variable must match type and count.

```typescript
let tuple: [number, number] = [1, 2];
tuple = [3, 4];
tuple = [5, 6, 7]; // Error: must be 2 items
tuple = [8]; // Error: must be 2 items
tuple = ["TypeScript", 9]; // Error: must be number
```

Type must be number, and count must be 2 items.



#### Object Types and Type Aliases

Add type to object's key, then you can not assign other type value to object.

```typescript
const pos: {x: number, y: number} = {
  x: 0,
  y: 0
};
```

If there are multiple variables restricted by same type, we could use type alias to reuse and reduce the duplication.

```typescript
type Axis = {x: number, y: number}
const location: Axis = {
  x: 1,
  y: 1
};

const pos: Axis = {
  x: 0,
  y: 0
};
```



#### Declare const

You can not re-assign value to a const variable. Just like JavaScript, but TypeScript has type check.

```typescript
type Axis = {x: number, y: number};
const point: Axis = {x: 0, y: 0};
const point2: Axis = {xx: 0, y: 0}; // Error
point = {x: 1, y: 1}; // Error
point.x = 1;
point.y = 1;
```

But you can operate const object internal attributes or methods.



#### Function

JavaScript defines function

```javascript
function log(message) {
  console.log(message);
}
```

This function returns void value.

TypeScript defines function

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

This function returns number and must return number type value. Function also add annotation to parameters(a, b).

JavaScript could accept rest parameters

```javascript
function sum(...values) {
  return values.reduce((prev, curr) => prev + curr);
}
```

TypeScript could also add annotation to this function parameter

```typescript
function sum(...values: number[]): number {
  return values.reduce((prev, curr) => prev + curr); 
}
```

JavaScript supports first-class function

```javascript
let add;
add = (a, b) => a + b
```

TypeScript supports it

```typescript
let add: (a: number, b: number) => number;
add = function(a: number, b: number): number {
  return a + b;
}
// or
add = (a: number, b: number): number => a + b
```

We could also use type alias to reuse this function signature.

```typescript
type Add = (a: number, b: number) => number;
let add: Add;
add = (a: number, b: number): number => a + b
```



#### Structural Type

TypeScript type system is structural. If the type is same, then you could assign value.

```typescript
type User = { id: number};
type Book = { id: number};

let user: User = {id: 1};
let book: Book = {id: 2};

user = book;
```

TypeScript found that their type structs are same(type compatibility), it does not care type name, they could assign value to each other.



Let's look at type compatibility example (Duck Programming).

```typescript
type Point2D = {x: number, y: number};
type Point3D = {x: number, y: number, z: number};

let point2D: Point2D = {x: 0, y: 0};
let point3D: Point3D = {x: 0, y: 0, z: 1};

// additional info is OK
point2D = point3D; // OK: point3D has x and y
point3D = point2D; // Error: point2D does not supply z

function takesPoint3D(point: Point2D) {}
takesPoint3D(point3D); 
```

This is duck typing. If Point3D has x and y, then you could call it Point2D.



#### Class

JavaScript defines a class.

```javascript
class Car {
  name;
  
  constructor(name) {
    this.name = name;
  }
  
  move(distance) {
    console.log(`${this.name} moves ${distance} meters`);
  }
}

const car = new Car("Ferrari");
car.move(100);
```

TypeScript adds some features to JavaScript class.

```typescript
class Car {
  protected name: string;
  
  constructor(name: string) {
    this.name = name;
  }
  
  public move(distance: number): void {
    console.log(`${this.name} moves ${distance} meters`);
  }
}

class Roadster extends Car {
  fly(distance: number): void {
    console.log(`I am ${this.name}, I can fly.`);
  }
}

const car: Car = new Car("Ferrari");
car.move(100);
car.name = "Bugatti"； // Error: You can not assign value to private or protected attribute
const bugatti: Roadster = new Car("Bugatti"); // sub-class could access protected attribute
bugatti.fly();
```



#### Generics

We define a class to describe the FIFO(First In First Out) structure.

```js
class Queue {
  data = [];
  push(item) {
    this.data.push(item);
  }

  pop() {
    return this.data.shift();
  }
}

const q = new Queue();
q.push(1);
q.push('Hello');

console.log(q.pop().toPrecision(1)); // OK
console.log(q.pop().toPrecision(1)); // Runtime Error
```

You will find that second item can not call `toPrecision` method. We can use TypeScript to define `NumberQueue` class that just accepts number element.

```typescript
class NumberQueue extends Queue {
  push(item: number) {
    super.push(item);
  }
  
  pop(): number {
    return super.pop();
  }
}
```

But if you have string queue scenario, you have to define another class that extends `Queue`. We could use TypeScript generics to define queue class. It will only accept the specified type when you initilize the class instance.

```typescript
class Queue<T> {
  data: Array<T> = [];
  push(item: T) {
    this.data.push(item);
  }

  pop(): T {
    return this.data.shift();
  }
}

const q = new Queue<number>();
```



#### Special Types

Any and Unknown are universal super type in TypeScript. All types of variables can be assigned to any and unknown type.

```typescript
let anyVar: any;
let unknownVar: unknown;

anyVar = 1;
anyVar = 'Hello';

unknownVar = 1;
unknownVar = 'Hello';
```



##### Any

Any type just likes original javascript type, it can accpet any value, you do not need to care about type compatibility. If you do not know type and trust your code, you could use any.

##### Unknown

Unknown does not like any, it is safer. It can accept any type, but you can only use it in unsafe scenario.

```typescript
unknownVar = 'Hello';
const unknownBool: boolean = unknownVar; // unknownBool is safe scenario. Compile time error
```

If you really want to use it, you have to check the type

```typescript
unknownVar = 'Hello';
if(typeof unknownVar === 'boolean') {
  const unknownBool: boolean = unknownVar;
}
```

 You could think unknown is subset of any type. It can accept all type value, but only use it in unsafe scenario.



#### Migrate JavaScript to TypeScript

We could add any type to original JavaScript code to migrate JavaScript to TypeScript, but it still has runtime error.

```typescript
let result: any;
result = loadString(); // if result is undefined. it will have runtime error
console.log(result.trim());
```

We could use unknown type to annotate the type.

```typescript
let result: unknown;
result = loadString();
if(typeof result === 'string') {
  console.log(result.trim());
}
```

unknown can be used in unsafe scenario, and JavaScript is runtime language, it is unsafe. We use this method to migrate it to TypeScript, in the next, we need to change code step by step.



#### Utilities

We could use TypeScript to encapsulate some utility method.

```typescript
function log(value: unknown): void {
  if(typeof value === 'number') {
    console.log(value.toFixed(2));
  } else {
    console.log(value);
  }
}
```

Why do not use any type? because it is unsafe. We used unknown type, then we need to judge input value type. It makes scenario safe.



#### Start React with TypeScript

We could use [create-react-app](https://github.com/facebook/create-react-app) to generate application with TypeScript template

```bash
> npx create-react-app react-with-typescript --template typescript
```

Generated the below files

* tsx files (it supports JSX syntax)
* tsconfig.json

Common commands

* Start the application `npm start`
* Build the application with `npm run build`
* Serve the application(static files) locally with `npx serve build`

### Compiler Option

#### target

Default target is `es5`

* es5
* es2015
* esnext

For example:

```typescript
class Car {
  private name: string;
  
  constructor(name: string) {
    this.name = name;
  }
  
  public move(distance: number): void {
    console.log(`${this.name} moves ${distance} meters`);
  }
}
```

This TypeScript class will be compiled to ES5

```javascript
"use strict";
var Car = /** @class */ (function () {
    function Car(name) {
        this.name = name;
    }
    Car.prototype.move = function (distance) {
        console.log(this.name + " moves " + distance + " meters");
    };
    return Car;
}());
```

If we change target to es2015, it will be compiled to 

```javascript
"use strict";
class Car {
    constructor(name) {
        this.name = name;
    }
    move(distance) {
        console.log(`${this.name} moves ${distance} meters`);
    }
}
```

It looks same as TypeScript, es2015 has no private feature. If we change to esnext, it will be compiled to 

```javascript
"use strict";
class Car {
    name;
    constructor(name) {
        this.name = name;
    }
    move(distance) {
        console.log(`${this.name} moves ${distance} meters`);
    }
}
```

TypeScript private attribute has another format

```typescript
class Car {
  #name: string;
  
  constructor(name: string) {
    this.#name = name;
  }
  
  public move(distance: number): void {
    console.log(`${this.#name} moves ${distance} meters`);
  }
}
```

esnext option could compile this format to the below result

```javascript
"use strict";
class Car {
    #name;
    constructor(name) {
        this.#name = name;
    }
    move(distance) {
        console.log(`${this.#name} moves ${distance} meters`);
    }
}
```

es2015 option could compile this format to the below result

```javascript
"use strict";
var __classPrivateFieldSet = (this && this.__classPrivateFieldSet) || function (receiver, state, value, kind, f) {
    if (kind === "m") throw new TypeError("Private method is not writable");
    if (kind === "a" && !f) throw new TypeError("Private accessor was defined without a setter");
    if (typeof state === "function" ? receiver !== state || !f : !state.has(receiver)) throw new TypeError("Cannot write private member to an object whose class did not declare it");
    return (kind === "a" ? f.call(receiver, value) : f ? f.value = value : state.set(receiver, value)), value;
};
var __classPrivateFieldGet = (this && this.__classPrivateFieldGet) || function (receiver, state, kind, f) {
    if (kind === "a" && !f) throw new TypeError("Private accessor was defined without a getter");
    if (typeof state === "function" ? receiver !== state || !f : !state.has(receiver)) throw new TypeError("Cannot read private member from an object whose class did not declare it");
    return kind === "m" ? f : kind === "a" ? f.call(receiver) : f ? f.value : state.get(receiver);
};
var _Car_name;
class Car {
    constructor(name) {
        _Car_name.set(this, void 0);
        __classPrivateFieldSet(this, _Car_name, name, "f");
    }
    move(distance) {
        console.log(`${__classPrivateFieldGet(this, _Car_name, "f")} moves ${distance} meters`);
    }
}
_Car_name = new WeakMap();
```

But es5 can not compile this format

```bash
src/index.ts:2:3 - error TS18028: Private identifiers are only available when targeting ECMAScript 2015 and higher.

2   #name: string;
    ~~~~~

[10:36:33 AM] Found 1 error. Watching for file changes.
```


