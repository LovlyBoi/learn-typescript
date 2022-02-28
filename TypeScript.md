# TypeScript

## 介绍

TypeScript 是 JavaScript 的超集。除了支持所有 JS 的新特性外，提供了静态类型检查。

TypeScript 可以编译出纯净、 简洁的 JavaScript 代码，并且可以运行在任何浏览器上、Node.js 环境中和任何支持 ECMAScript 3（或更高版本）的 JavaScript 引擎中。

## 工具和起步

我使用的是 VS Code，对 TS 的支持度很高（它本身也是 TS 开发的）。那我们想编写并运行 TS 程序，首先需要安装 TypeScript。

```shell
npm init -y
npm i -D typescript
# 也可以直接在全局安装
npm i -g typescript
```

安装完 TypeScript，我们就可以正式编写代码了：

```typescript
function sayHello(name: string) {
  return `Hello, ${name} !`
}

console.log(sayHello('Siven'))
```

然后，我们可以对代码进行编译，将它变为 JS 代码：

```shell
npx tsc ./hello.ts
```

随后，我们会发现当前文件夹下多了一个 `hello.js` 文件。这就是编译好的 JS 文件了。

我们会发现还有很多的问题没有解决：TS 的很多配置我们没有办法告知编译器，总不可能我们全写在命令行里；项目中文件的变化不会被察觉，不能自动编译。

所以接下来我们创建一个 TS 的配置文件：

```shell
npx tsc --init
```

那么我们又会发现：文件夹下多了一个 `tsconfig.json` 文件。这个文件就是记录了所有对 TS 的配置，编译时，编译器会检查目录下的这个文件，读取其中的配置信息。当然，我们的 VS Code 也可以读取这个文件的配置，在编写代码时就给我们提出错误。

如果想让 TS 检测文件的变化，并自动进行编译，我们需要执行：

```shell
npx tsc --watch
```

这样，TS 就会自动编译代码啦！

当然，在实际的项目开发中，我们更多的可能是使用 webpack 配合 ts-loader（或者 babel-loader）对项目进行打包编译，或者直接使用框架的脚手架。 

## 配置

在 `tsconfig.json` 中，我们可以对 TS 编译进行详细的配置：

**/* Language and Environment */** ：

- target：TS 最终被编译为哪个版本的 JS 代码，默认是 `es2016`，即 `ES7`。

**/* Type Checking */** :

- strict：严格模式，默认为 `true`，注释掉或改为 `flase` 取消严格模式。
- noImplicitAny：不允许隐式的 `any`，打开严格模式会打开这个选项，当然，关闭严格模式后我们也可以单独打开这个选项。
- strictNullChecks：严格的空值检查，不允许 `null` 和 `undefined` 赋值给其他类型。

## 类型

对于类型，我们可以在变量的右侧添加「类型注释」来显示的指定这是一个什么类型的变量。

### string

字符串，对应 JS 的 String 类型。

```typescript
let str: string = 'Hello World!';
```

### number

数字，对应 JS 的 Number 类型。

```typescript
let num: number = 3;
```

### boolean

布尔类型，对应 JS 的 Boolean 类型。

```typescript
let bool: boolean = true;
```

### 数组

数组有两种定义方法，一种是在类型后加 `[]` 代表是一个某个类型的数组类型，另一种是通过 `Array` 泛型来创建。

- `type[]`

  ```typescript
  const strArr: string[] = ['a', 'b', 'c'];
  ```

- `Array<type>`

  ```typescript
  const strArr: Array<string> = ['a', 'b', 'c'];
  ```

在 TS 中，可以发现语言规范在尽量让我们不去书写混合类型的数组。

### any

`any` 会阻止 TS 对变量的类型检查。当我们不希望某个特定的值到类型检查失败，我们可以将它设为 `any`。

```typescript
let a: any = 2;
let b: string = a; // 不会报错
```

如果一个 `any` 类型的值，被传递给了其他类型的变量，并不会导致报错。这就会导致 `any` 类型会污染其他变量的类型，所以在开发中，我们要尽量避免过多的 `any`。

### 函数

函数在 JS 中的地位极高。在 TS 中，对于函数，它的形参可以指定类型，它的返回值可以指定类型，函数本身也可以指定类型。

```typescript
function add(a: number, b: number): number {
  return a + b;
}

// add函数本身的类型为：(foo: number, bar: number) => number，这里形参名是无所谓的
let getSum: (foo: number, bar: number) => number = add;
```

当然，TS 是可以进行类型推断的，一般来说，我们可以不指明返回值类型，而是让 TS 自己推断出来返回值类型。

如果没有返回值，那么返回值类型为 `void`；如果永远都不会执行到 `return` 语句（比如抛出异常），那么返回值为 `never`。

如果不确定参数个数，那么可以使用 `...args` 这种 rest 参数的形式编写；如果参数是可选的，那么可以在参数后加上 `?` 来表示这是一个可选参数。

### 对象类型

对于对象类型，我们的类型注释需要这么写：

```typescript
// 可以使用逗号或分号分隔
let obj: {a: number, b: number} = {
  a: 1,
  b: 2
}

class Obj {
  a: number;
  b: number;

  constructor(a: number, b: number) {
    this.a = a;
    this.b = b;
  }
}

// 也是可以的
obj = new Obj(2, 3);
```

### 联合类型

我们联合！

联合类型允许我们组合多个类型，因为有时一个变量的类型有可能为两种或更多种。

```typescript
let id: string | number = '1234';

id = 2345 // 也是可以的
```

但是因为联合类型可能是多种类型之一，所以不能直接确定类型，也就不能直接调用对应的方法（当然共有的方法可以直接使用）：

```typescript
function sayHello(name: string | { name: string }) {
  console.log(`Hello, ${name.toUpperCase()}`); // 类型“string | { name: string; }”上不存在属性“toUpperCase”。
}
```

我们可以通过分支判断来对类型进行缩小，让 TS 知道这个变量的确定类型：

```typescript
function sayHello(name: string | { value: string }) {
  if (typeof name === 'string') {
    console.log(`Hello, ${name.toUpperCase()}`); // 不会报错
  } else {
    console.log(`Hello, ${name.value.toUpperCase()}`); // 也不会报错
  }
}
```

### 类型别名

如果我们某一个类型使用的很频繁，那么我们就会反复的写很多冗余代码；或者我们希望某一个类型的名字可以更具体，让我们看一眼就知道这个变量是用来做什么的。我们可以使用类型别名来完成这些功能。

```typescript
// 使用type关键字来声明我们自己的类型（起别名）
type Point = {
  x: number,
  y: number
}

function printPoint(p: Point) {
  console.log(`x: ${p.x}, y: ${p.y}`);
}

printPoint({x: 1, y: 2}); // x: 1, y: 2
```

### 接口

接口在 TS 中也是一个很重要的概念，和别的语言不太相同，TS 只关注值的外形是否相同。

```typescript
interface IPoint {
  x: number,
  y: number
}

function printPoint(p: IPoint) {
  console.log(`x: ${p.x}, y: ${p.y}`);
}

printPoint({x: 1, y: 2}) // x: 1, y: 2
```

类型别名和接口很类似，但接口可以像类一样被继承扩展。

```typescript
interface IPoint {
  x: number,
  y: number
}

interface IPointIn3D extends IPoint {
  z: number
}

const p: IPointIn3D = {
  x: 1,
  y: 2,
  z: 3
}
```

而 type 想要实现这个功能，则需要使用交叉类型：

```typescript
type Point = {
  x: number,
  y: number
}

// & 表示交叉类型，即新的类型拥有两种类型的特性，一般交叉类型都使用在对象上
type PointIn3D = Point & {
  z: number
}

const p: PointIn3D = {
  x: 1,
  y: 2,
  z: 3
}
```

并且，多个同名的 interface 声明，会合并起来：

```typescript
interface IInterface {
  prop1: number
}

interface IInterface {
  prop2: string
}

const obj: IInterface = {
  prop1: 3,
  prop2: 'Hello' // 这里一个属性也不能少，两个同名接口会合并起来
}
```

### 类型断言

类型断言（Type Assertion）可以用来手动指定一个值的类型。

类型断言有两种语法：

```typescript
值 as 类型
// 或
<类型>值
```

但是，在 TSX 中，只能使用 as 语法，而且第二种语法类似泛型，所以最好使用 as 语法。

如果一个变量可能是多种类型，那么我们只能访问他们共有的方法，如果真的需要访问其中一个确定的类型方法，我们可以使用「类型断言」，来告诉编译器，这个变量就是这个类型，这会取消编译器的类型检查。但是需要注意的是，这样只能“骗过”编译器，实际运行时可能还是会报错。

```typescript
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function isFish(animal: Cat | Fish) {
  	// 通过断言来访问swim
    if (typeof (animal as Fish).swim === 'function') {
        return true;
    }
    return false;
}
```

类型断言另一个用途是将父类断言为子类，这样可以访问子类中独有的属性：

```typescript
class ApiError extends Error {
    code: number = 0;
}
class HttpError extends Error {
    statusCode: number = 200;
}

function isApiError(error: Error) {
    if (typeof (error as ApiError).code === 'number') {
        return true;
    }
    return false;
}
```

我们还可以将任何一个值断言为 `any` 来阻止报错：

```typescript
(window as any).foo = 1;
```

