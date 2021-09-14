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


