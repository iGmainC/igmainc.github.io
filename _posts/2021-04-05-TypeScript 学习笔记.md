---
title: TypeScript 学习笔记（未完）
tags: TypeScript
---

# 概述

## 什么是 TypeScript？

TypeScript 由微软开发的一款开源的编程语言。

TypeScript 是 JavaScript 的一个超集。

TypeScript 无法直接运行，但它可以编译成纯 JavaScript（是不是感觉脱裤子放屁？）。

技术上来说 TypeScript 就是有静态类型的 JavaScript。

## TypeScript 有什么优点？

TypeScript 的设计目的是解决 JavaScript 的“痛点”：弱类型和没有命名空间，导致很难模块化，不适合开发大型程序。另外它还提供了一些语法来帮助大家更方便地实践面向对象的编程。

简单来说 TypeScript 是为了用来开发大型项目的。

# 前期准备

## 下载 IDE

我推荐用 WebStorm

[下载链接](https://www.jetbrains.com/zh-cn/webstorm/)

## 安装 Node.js

Node.js 就是 JavaScript 的本地解释器

[下载链接](http://nodejs.cn/)

## IED 的配置

TypeScript 无法直接运行，需要编译成 JavaScript 后用 Node.js 进行解释运行。可每次运行都要编译实在太麻烦了，在运行的配置文件里添加参数就能实现 TypeScript 的一键运行

1. 安装依赖项：`npm install -g typescript ts-node --save-dev`

2. 添加运行配置，选择 Node.js

3. 在 *节点参数* 处添加参数：`--requeire ts-node/register`

4. 在 *JavaScript文件* 处添加宏：`$FilePathRelativeToProjectRoot$`。

   意思是运行当前正在编辑的文件

5. 应用该配置

## Hello World

```typescript
console.log("Hello World");
```

# 基础类型

## 布尔值

最基本的数据类型就是简单的true/false值，在JavaScript和TypeScript里叫做`boolean`（其它语言中也一样）。

```ts
let isDone: boolean = false;
```

## 数字

和 JavaScript 一样，TypeScript 里的所有数字都是浮点数。 这些浮点数的类型是 `number`。 除了支持十进制和十六进制字面量，TypeScript还支持ECMAScript 2015中引入的二进制和八进制字面量。

> 也就是说，在 TypeScript 中所有的数字的类型都是 `number`，且所有数字默认都是浮点数。

```ts
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

## 字符串

JavaScript 程序的另一项基本操作是处理网页或服务器端的文本数据。 像其它语言里一样，我们使用 `string`表示文本数据类型。 和 JavaScript 一样，可以使用双引号（ `"`）或单引号（`'`）表示字符串。

```ts
let name: string = "bob";
name = "smith";
```

你还可以使用*模版字符串*，它可以定义多行文本和内嵌表达式。 这种字符串是被反引号包围（`` `string` ``），并且以`${ expr }`这种形式嵌入表达式

```ts
let name: string = `Gene`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ name }.

I'll be ${ age + 1 } years old next month.`;
```

## 数组

TypeScript 像 JavaScript 一样可以操作数组元素。 有两种方式可以定义数组。 第一种，可以在元素类型后面接上 `[]`，表示由此类型元素组成的一个数组：

```ts
let list: number[] = [1, 2, 3];
```

第二种方式是使用数组泛型，`Array<元素类型>`：

```ts
let list: Array<number> = [1, 2, 3];
```

## 元组 Tuple

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 `string`和`number`类型的元组。

```ts
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

当访问一个已知索引的元素，会得到正确的类型：

```ts
console.log(x[0].substr(1)); // OK
console.log(x[1].substr(1)); // Error, 'number' does not have 'substr'
```

当访问一个越界的元素，会使用联合类型替代：

```ts
x[3] = 'world'; // OK, 字符串可以赋值给(string | number)类型

console.log(x[5].toString()); // OK, 'string' 和 'number' 都有 toString

x[6] = true; // Error, 布尔不是(string | number)类型
```

联合类型是高级主题，我们会在以后的章节里讨论它。

## 枚举

`enum`类型是对 JavaScript 标准数据类型的一个补充。 像 C# 等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

```ts
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

默认情况下，从`0`开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 `1`开始编号：

```ts
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;
```

或者，全部都采用手动赋值：

```ts
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green;
```

枚举类型提供的一个便利是你可以由枚举的值得到它的名字。 例如，我们知道数值为 2，但是不确定它映射到`Color`里的哪个名字，我们可以查找相应的名字：

```ts
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];

console.log(colorName);  // 显示'Green'因为上面代码里它的值是2
```

## Any

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 `any`类型来标记这些变量：

```ts
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

在对现有代码进行改写的时候，`any`类型是十分有用的，它允许你在编译时可选择地包含或移除类型检查。 你可能认为 `Object`有相似的作用，就像它在其它语言中那样。 但是 `Object`类型的变量只是允许你给它赋任意值 - 但是却不能够在它上面调用任意的方法，即便它真的有这些方法：

```ts
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```

当你只知道一部分数据的类型时，`any`类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据：

```ts
let list: any[] = [1, true, "free"];

list[1] = 100;
```

## Void

某种程度上来说，`void`类型像是与`any`类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 `void`：

```ts
function warnUser(): void {
    console.log("This is my warning message");
}
```

声明一个`void`类型的变量没有什么大用，因为你只能为它赋予`undefined`和`null`：

```ts
let unusable: void = undefined;
```

## Null 和 Undefined

TypeScript 里，`undefined`和`null`两者各自有自己的类型分别叫做`undefined`和`null`。 和 `void`相似，它们的本身的类型用处不是很大：

```ts
// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;
```

默认情况下`null`和`undefined`是所有类型的子类型。 就是说你可以把 `null`和`undefined`赋值给`number`类型的变量。

然而，当你指定了`--strictNullChecks`标记，`null`和`undefined`只能赋值给`void`和它们各自。 这能避免 *很多*常见的问题。 也许在某处你想传入一个 `string`或`null`或`undefined`，你可以使用联合类型`string | null | undefined`。 再次说明，稍后我们会介绍联合类型。

> 注意：我们鼓励尽可能地使用`--strictNullChecks`，但在本手册里我们假设这个标记是关闭的。

## Never

`never`类型表示的是那些永不存在的值的类型。 例如， `never`类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 `never`类型，当它们被永不为真的类型保护所约束时。

`never`类型是任何类型的子类型，也可以赋值给任何类型；然而，*没有*类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）。 即使 `any`也不可以赋值给`never`。

下面是一些返回`never`类型的函数：

```ts
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

## Object

`object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型。

使用`object`类型，就可以更好的表示像`Object.create`这样的API。例如：

```ts
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

## 类型断言

有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

通过*类型断言*这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript 会假设你，程序员，已经进行了必须的检查。

类型断言有两种形式。 其一是“尖括号”语法：

```ts
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

另一个为`as`语法：

```ts
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

两种形式是等价的。 至于使用哪个大多数情况下是凭个人喜好；然而，当你在 TypeScript 里使用JSX时，只有 `as`语法断言是被允许的。

> 推荐用`as`语法，可读性更好

# 变量声明

## var 定义

JavaScript 的变量定义形式，作用域相对`let`来说很奇怪

## let 定义

总之就用这种方式定义就完了

```ts
let abc: number = 123;
```

## 定义常量

`const` 声明是声明的变量赋值后不能再改变。 

```ts
const numLivesForCat = 9;
```

所有变量除了你计划去修改的都应该使用`const`。 基本原则就是如果一个变量不需要对它写入，那么其它使用这些代码的人也不能够写入它们，并且要思考为什么会需要对这些变量重新赋值。 使用 `const`也可以让我们更容易的推测数据的流动。

# 解构

## 解构数组

最简单的解构莫过于数组的解构赋值了：

```ts
let input = [1, 2];
let [first, second] = input;
```

这创建了2个命名变量 `first` 和 `second`。 相当于使用了索引，等价于：

```ts
first = input[0];
second = input[1];
```

也可以用于，两个变量的交换：

```ts
[first, second] = [second, first];
```

作用于函数参数：

```ts
function f([first, second]: [number, number]) {
    console.log(first);
    console.log(second);
}
f(input);
```

你可以在数组里使用`...`语法创建剩余变量：

```ts
let [first, ...rest] = [1, 2, 3, 4];
console.log(first); // outputs 1
console.log(rest); // outputs [ 2, 3, 4 ]
```

> 类似于 python 的`*args`

当然，由于是 JavaScript, 你可以忽略你不关心的尾随元素：

```ts
let [first] = [1, 2, 3, 4];
console.log(first); // outputs 1
```

或其它元素：

```ts
let [, second, , fourth] = [1, 2, 3, 4];
```

## 字典解构

在 JavaScript 中字典形式的变量称之为 **对象**（Object）

你也可以解构对象：

```ts
let o = {
    a: "foo",
    b: 12,
    c: "bar"
};
let { a, b } = o;
```

这通过 `o.a` and `o.b` 创建了 `a` 和 `b` 。 注意，如果你不需要 `c` 你可以忽略它。

就像数组解构，你可以用没有声明的赋值：

```ts
({ a, b } = { a: "baz", b: 101 });
```

注意，我们需要用括号将它括起来，因为Javascript通常会将以 `{` 起始的语句解析为一个块。

你可以在对象里使用`...`语法创建剩余变量：

```ts
let { a, ...passthrough } = o;
let total = passthrough.b + passthrough.c.length;
```

> 这种方式只能在变量声明时使用

# 运算符

TypeScript 主要包含以下几种运算：

- 算术运算符
- 逻辑运算符
- 关系运算符
- 按位运算符
- 赋值运算符
- 三元/条件运算符
- 字符串运算符
- 类型运算符

### 算数运算符

| 运算符 | 描述         | 例子  | x 运算结果 | y 运算结果 |
| :----: | :----------- | :---- | :--------- | :--------- |
|   +    | 加法         | `x=y+2` | 7          | 5          |
|   -    | 减法         | `x=y-2` | 3          | 5          |
|   *    | 乘法         | `x=y*2` | 10         | 5          |
|   /    | 除法         | `x=y/2` | 2.5        | 5          |
|   %    | 取模（余数） | `x=y%2` | 1          | 5          |
|   ++   | 自增         | `x=++y` | 6          | 6          |
|   --   | 自减         | `x=--y` | 4          | 4          |

### 关系运算符

关系运算符用于计算结果是否为 `true` 或者 `false`。

| 运算符 | 描述       | 比较   | 返回值  |
| :----: | :--------- | :----- | :------ |
|   ==   | 等于       | `x==8` | *false* |
|   !=   | 不等于     | `x!=8` | *true*  |
|   >    | 大于       | `x>8`  | *false* |
|   <    | 小于       | `x<8`  | *true*  |
|   >=   | 大于或等于 | `x>=8` | *false* |
|   <=   | 小于或等于 | `x<=8` | *true*  |

### 逻辑运算符

逻辑运算符用于测定变量或值之间的逻辑。

| 运算符 | 描述 | 例子                        |
| :----: | :--- | :-------------------------- |
|   &&   | and  | `(x < 10 && y > 1)` 为 true |
|  \|\|  | or   | `(x==5 || y==5)` 为 false   |
|   !    | not  | `!(x==y)` 为 true           |

### 位运算符

很不常用，忽略

### 赋值运算符

赋值运算符用于给变量赋值。

| 运算符 | 例子   | 实例      | x 值   |
| :----: | :----- | :-------- | :----- |
|   =    | x = y  | x = y     | x = 5  |
|   +=   | x += y | x = x + y | x = 15 |
|   -=   | x -= y | x = x - y | x = 5  |
|   *=   | x *= y | x = x * y | x = 50 |
|   /=   | x /= y | x = x / y | x = 2  |

### 三元运算符

三元运算有 3 个操作数，并且需要判断布尔表达式的值。该运算符的主要是决定哪个值应该赋值给变量。

```ts
表达式 ? 值1 : 值2
```

如果表达式为真，则该语句的实际值是值1

# 条件语句

在 TypeScript 中，我们可使用以下条件语句：

- **if 语句** - 只有当指定条件为 true 时，使用该语句来执行代码
- **if...else 语句** - 当条件为 true 时执行代码，当条件为 false 时执行其他代码
- **if...else if....else 语句**- 使用该语句来选择多个代码块之一来执行
- **switch 语句** - 使用该语句来选择多个代码块之一来执行

## if 语句

```ts
if (表达式){
    let a: number = 1;
}

// 也可以不加花括号
if (表达式)
    let a: number = 1;	// 这一行可以运行
let b: string = "xxx";	// 这一行不走 if
```

## if else 语句

```ts
if (表达式){
    let a: number = 1;
} else {
    let b: string = "xxx";
}

// 也可以不加花括号
if (表达式)
    let a: number = 1;
else
    let b: string = "xxx";
```

## if elif 语句

```ts
if (表达式1){
    let a: number = 1;
} else if (表达式2) {
    let b: string = "aaa";
} else if (表达式3) {
    let c: string = "bbb";
}
```

## switch case 语句

```ts
switch (表达式) {
    case value1  :
        console.log("这是 1");
        break;
    case value2  :
        console.log("这是 2");
        break;
    default :
        console.log("这是其他");
}
```

**switch** 语句必须遵循下面的规则：

- 当被测试的变量等于 case 中的常量时，case 后跟的语句将被执行，直到遇到 **break** 语句为止。
- 当遇到 **break** 语句时，switch 终止，控制流将跳转到 switch 语句后的下一行。
- 不是每一个 case 都需要包含 **break**。如果 case 语句不包含 **break**，控制流将会 *继续* 后续的 case，直到遇到 break 为止。
- 一个 **switch** 语句可以有一个可选的 **default** case，出现在 switch 的结尾。default case 可用于在上面所有 case 都不为真时执行一个任务。default case 中的 **break** 语句不是必需的。

# 循环

循环语句允许我们多次执行一个语句或语句组。

## for 循环

```ts
for ( 变量初始化; 循环进入条件; 循环增量 ){
    statement(s);
}

// 也可以不写花括号
for ( 变量初始化; 循环进入条件; 循环增量 )
    statement(s);	// 只有这一行能进入循环
```

## for in 循环（循环下标）

```ts
let list = [1, 2, 3, 4];
for (let a in list) {
    // a 是数组下标
    console.log("值：", list[a]);
    console.log("下标：", a, "\n");
}
```

## for of 循环（迭代器循环）

```ts
let list = [1, 2, 3, 4];
for (let a of list) {
    // a 是数组值
    console.log(a);
}
```

## forEach 循环

```ts
let list = [1, 2, 3, 4];
list.forEach((value,index,list)=>{
    console.log("值：",value);
    console.log("下标：",index);
});
```

`forEach` 在循环中是无法返回的

## every、some 循环

### every 循环

```ts
let list = [1, 2, 3, 4];
let xxx = list.every((value, index, list) => {
    console.log("值：", value);
    console.log("下标：", index);
    return false
});
```

循环中返回 `true` 是继续循环，返回 `false` 就跳出循环了

返回值都会被映射为 Boolean 值

### some 循环

```ts
let list = [1, 2, 3, 4];
let xxx = list.some((value, index, list) => {
    console.log("值：", value);
    console.log("下标：", index);
    return false;
});
```

有返回值，但返回值不中断循环

返回值都会被映射为 Boolean 值

## while 循环

```ts
while (表达式){
    console.log("xxx");
}
```

## do while 循环

```ts
do {
    console.log("xxx");
} while (表达式);
```

## 流程控制语句

### break 语句

当 `break` 语句出现在一个循环内时，循环会立即终止

### continue 语句

continue 会跳过当前循环中的代码，强迫开始下一次循环。

# 函数的定义

