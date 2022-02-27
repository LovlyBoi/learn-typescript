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

- target：TS 最终被编译为哪个版本的 JS 代码，eg：`es5`。

## 类型

