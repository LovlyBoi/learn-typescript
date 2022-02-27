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

