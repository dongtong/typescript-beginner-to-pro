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


