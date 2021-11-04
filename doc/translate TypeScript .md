# TypeScript 文档简介

- 官方文档传送门：https://www.typescriptlang.org/docs/handbook/2/basic-types.html

  

## 概念三问：

- 是什么？

  是一种由微软开发的自由和开源的编程语言。它是 JavaScript 的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。

- 为了什么？

  强化javaScript的类型概念，可以在编译期间发现并纠正错误。最终被编译成 JavaScript 代码，使浏览器可以理解

- 怎么写？（以下）



环境安装：(tsc --> TypeScript 编译器)

```js
// 安装 
npm install -g typescript

// 验证
 tsc -v  // 出现版本号即成功

// 运行某个文件
tsc fileName.ts  // 即将ts文件编译成js文件
```



## 日常类型：

- 语法：声明 变量名：类型注释 = 值

- 注意：此语法中的类型注释是为了学习时方便理解，但实际开发中，不是必需的，每当定义一个变量时，TypeScript 会尝试自动推断代码中的类型。

```js
// string 
const myStr:string = 'a'
// number
const myNum:number = 11
// boolean
const myBool:boolean = true

// 数组 两种写法：number --> 指数组中元素的类型
const list:number[] = [1,2,3]
const list:Array<number> = [1,2,3]

/*
	一个特殊类型 ，`any`当您不希望某个特定值导致类型检查错误时，可以使用它。
	any当您不想写出长类型只是为了让 TypeScript 相信特定的代码行没问题时，该类型很有用。
	当您不指定类型，并且 TypeScript 无法从上下文推断它时，编译器通常会默认为any.
*/
const anything:any = {}

```

函数（联合类型、类型收缩）

```javascript
/* 函数基础语法
	params后规定参数后定义类型
	函数后可规定函数的return类型
*/
function funName(para1:string,para2:number){}

function funName(para1:number):number{
  return 123
}

/*
	当函数的params的类型不确定时，可使用联合类型
	但相对的，函数内部需要对不同类型的参数分别处理，即类型收缩
*/ 
function morePram(pamra:number | string ){
 // do something
  if(typeof param === "string"){
    // handle string
  }else if(typeof param === 'number'){
    // handle number
  }else{
    // 兜底逻辑
  }
}
```



## 对象类型：interface 和 type

在 JavaScript 中，我们分组和传递数据的基本方式是通过对象。在 TypeScript 中，我们通过对象类型表示那些。

它们可以是匿名的：

```javascript
function greet(person: { name: string; age: number }) {
  return "Hello " + person.name;
}
```

或者它们可以使用接口命名 --> interface

```js
interface Person {
  name: string;
  age: number;
}
 
function greet(person: Person) {
  return "Hello " + person.name;
}
```

或类型别名 --> type

```js
type Person = {
  name: string;
  age: number;
};
 
function greet(person: Person) {
  return "Hello " + person.name;
}
```

- Interface 与 type 区别

  - 同：

    - 都可以描述一个对象

    - 都可以extends（语法不同）

      ```js
      interface Name { 
        name: string; 
      }
      interface User extends Name { 
        age: number; 
      }
      
      
      type Name = { 
        name: string; 
      }
      type User = Name & { age: number  };
      ```

  - 不同

    - interface可 type不可

      ```js
      // interface 同名会合并 不能重命名
      interface User {
        name: string
        age: number
      }
      
      interface User {
        sex: string
      }
      
      /*
      User 接口为 {
        name: string
        age: number
        sex: string 
      }
      */
      ```

    - type可 interface不可

      ```js
      // interface 只能表示像对象那种结构的类型，如果想表示 JS 中的原始类型（undefined, null, boolean, string，number）只能交给 type ：
      type A = string
      
      //  type 可以动态计算属性
      type Keys = "小王" | "小文"
      
      type X = {
        [key in Keys]: string
      }
      
      const test: X = {
          '小王': '肌肉男',
          '小文': '也是肌肉男'
      }
      ```

      

- 属性修饰符

  对象类型中的每个属性都可以指定几项内容：类型、属性是否可选以及是否可以写入该属性

  ```js
  // 可选属性（可传可不传）
  interface Person {
    name:string,
    age:number,
    gender?:boolean
  }
    
   function greet(person:Person){
     // do something
   }
    
   greet({'xiaoming','20',true})
   greet({'xiaolan','18'})
  ```



至此，你已经掌握了TypeScript的基本语法使用，可在项目中使用。以下是一些更深层的理解，便于写出优雅的代码：



## 类型断言

断言：是编程术语，表示为一些布尔表达。（概念扫盲）

```js
// 两种写法  推荐第一种。第二种与tsx会有冲突
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas")
```

类型转换：

（官方原文）：TypeScript 只允许类型断言转换为更具体或不太具体的类型版本。此规则可防止“不可能”的强制。有时，此规则可能过于保守，并且将不允许可能有效的更复杂的强制转换。如果发生这种情况，您可以使用两个断言，首先是 to `any`（或`unknown`，我们将在后面介绍）

（我的理解）：当需要将某个变量由一个类型转换为另一个类型（即强制转换）时，需要先转换为`any` 或`unknow`（中转一下）

```js
const x = "hello" as number // 由string -> number
const a = (expr as any) as T; // 加any中转
```



## 文字类型

- 不同的声明变量方式会使变量的类型不一致：

  const --> 字面量

  let、var --> 数据类型

而当一个变量的类型为字面量时，再次修改这个变量会报错

![image-20211103113425377](/Users/nier/Library/Application Support/typora-user-images/image-20211103113425377.png)



使用场景：当一个函数的入参为某几个特定的值时。

```js
function printText(s: string, alignment: "left" | "right" | "center") {
  // ...
}
printText("Hello, world", "left");
printText("G'day, mate", "centre"); // centre 报错

// 数字文字类型
function compare(a: string, b: string): -1 | 0 | 1 {
  return a === b ? 0 : a > b ? 1 : -1;
}

// 与非文字类型联合使用
interface Options {
  width: number;
}
function configure(x: Options | "auto") {
  // ...
}
configure({ width: 100 });
configure("auto");
configure("automatic"); // 报错
```

文字推理

```js
// 类型会被推断为 { url: string, method: string }
const req = { url: "http://httpbin.org/get", method: "GET" }
// 在以下调用中会出错
handleRequest(req) // string 不能赋给 "GET" | "POST"

// 解决
type REQ = {
  url: string
  method: "GET" | "POST"
}
const req: REQ = { url: "http://httpbin.org/get", method: "GET" }
const req = { url: "http://httpbin.org/get", method: "GET" } as REQ

// 该as const后缀就像const但是对于类型系统，确保所有属性分配的文本类型，而不是一个数据类型string或number。
const req = { url: "http://httpbin.org/get", method: "GET" as "GET" }
const req = { url: "http://httpbin.org/get", method: "GET" } as cons
```

## 

## undefined 和 null

推荐打开配置中的**`strictNullChecks`**

非空运算符（后缀！）：保证一定有值时

```js
function liveDangerously(x?: number | null) {
  // No error
  console.log(x!.toFixed());
}
```



## 枚举（ts独有）

后续补充



## 缩小范围

回顾：当一个函数的入参是联合类型时，需要对不同类型的入参对应处理（类型收缩），尽可能的减少代码报错。

目的：此处的缩小范围，会更优雅的处理类型收缩，有时，并不需要对每个类型都去做处理。

TypeScript 可以理解几种不同的缩小结构：

在我们的`if`检查中，TypeScript 将`typeof padding === "number"`其视为一种称为*类型保护*的特殊代码形式

```js
function padLeft(padding: number | string, input: string) {
  if (typeof padding === "number") {
    return new Array(padding + 1).join(" ") + input;
                       
(parameter) padding: number
  }
  return padding + input;
           
(parameter) padding: string
}
```

- typeof 类型守卫

  正如我们所见，JavaScript 支持一个`typeof`运算符，它可以提供有关我们在运行时拥有的值类型的非常基本的信息。TypeScript 期望它返回一组特定的字符串：

  - `"string"` `"number"` `"bigint"` `"boolean"` `"symbol"` `"undefined"` `"object"` `"function"`

- 真实性缩小

  目的：通过真实性缩小数据范围

  场景：当传入的变量不是一个布尔值，但需要当作布尔来处理时，如下：

  ```js
  function getUsersOnlineMessage(numUsersOnline: number) {
    if (numUsersOnline) {
      return `There are ${numUsersOnline} online now!`;
    }
    return "Nobody's here. :(";
  }
  ```

  此时ts会强制转换，转换规则同 js，`0` `NaN` `""` `null` `undefined` 转为`false`，其余均为`true`。

  当然，也可以手动转换，如下：

  ```js
  // 两种方式，转换结果的类型会不一致，使用!!类型会是字面量
  Boolean("hello"); // type: boolean, value: true
  !!"world"; // type: true,    value: true
  ```

- 平等性缩小

  目的：利用条件判断或运算符，推断出两个或多个变量的类型，以此减少一些冗余的处理。

```js
// 扫盲：条件判断中 `==`只判断值是否相等，`===`判断值和数据类型
// 场景：当传入两个联合类型的参数，而两个参数的类型中都包含`string`，此时，用`===`判断两个参数相等，即可确定类型为`string`
function example(x: string | number, y: string | boolean) {
  if (x === y) {
    // We can now call any 'string' method on 'x' or 'y'.
    x.toUpperCase();
          
(method) String.toUpperCase(): string
    y.toLowerCase();
          
(method) String.toLowerCase(): string
  } else {
    console.log(x);
               
(parameter) x: string | number
    console.log(y);
               
(parameter) y: string | boolean
  }
}

// in
type Fish = { swim: () => void };
type Bird = { fly: () => void };
 
function move(animal: Fish | Bird) {
  if ("swim" in animal) {
    return animal.swim();
  } 
  return animal.fly();
}

// intenceof
function logValue(x: Date | string) {
  if (x instanceof Date) {
    console.log(x.toUTCString()); 
(parameter) x: Date
  } else {
    console.log(x.toUpperCase());               
(parameter) x: string
  }
}
```



## 关于函数

说明：关于函数的一些拓展。就是不同入参的一些写法和处理方式（我的理解）

- 函数表达式

  描述函数的最简单方法是使用函数类型表达式。这些类型在语法上类似于箭头函数：

  ```js
  // 接收一个为函数的参数并调用 void --> 无返回
  function greeter(fn: (a: string) => void) {
    fn("Hello, World");
  }
   
  function printToConsole(s: string) {
    console.log(s);
  }
   
  greeter(printToConsole);
  ```

- 呼叫签名

  官方原文：在 JavaScript 中，函数除了可调用之外还可以有属性。但是，函数类型表达式语法不允许声明属性。如果我们想用属性来描述一些可调用的东西，我们可以在对象类型中写一个*调用签名*

  我的理解：就是可以给函数传入一个数据类型为对象的参数

  ```js
  type DescribableFunction = {
    description: string;
    (someArg: number): boolean;
  };
  function doSomething(fn: DescribableFunction) {
    console.log(fn.description + " returned " + fn(6));
  }
  ```

  

- 构造签名

  官方原文：JavaScript 函数也可以使用`new`运算符调用。TypeScript 将这些称为*构造函数，*因为它们通常会创建一个新对象。您可以通过在调用签名前添加关键字来编写*构造*`new`签名。

  我的理解：入参可以是一个构造函数，同时，函数内部支持new关键字

  ```js
  type SomeConstructor = {
    new (s: string): SomeObject;
  };
  function fn(ctor: SomeConstructor) {
    return new ctor("hello");
  }
  ```

  

- 通用函数

  当一个函数输出与输入的类型有联系时，可使用泛型，此时返回的数据类型不会是默认的any

  注意，我们不必`Type`在此示例中指定。类型由 TypeScript*推断*- 自动选择

  ```js
  // 这种方式，虽函数的功能实现了，但是返回的数据类型会是一个any
  function firstElement(arr: any[]) {
    return arr[0];
  }
  
  function firstElement<Type>(arr: Type[]): Type | undefined {
    return arr[0];
  }
  
  // 通过向Type这个函数添加一个类型参数并在两个地方使用它，我们在函数的输入（数组）和输出（返回值）之间创建了一个链接。现在当我们调用它时，会出现一个更具体的类型：
  // s is of type 'string'
  const s = firstElement(["a", "b", "c"]);
  // n is of type 'number'
  const n = firstElement([1, 2, 3]);
  // u is of type undefined
  const u = firstElement([]);
  ```

  

- 约束

  官方原文：我们编写了一些通用函数，可以处理*任何*类型的值。有时我们想关联两个值，但只能对某个值的子集进行操作。在这种情况下，我们可以使用*约束*来限制类型参数可以接受的类型种类

  我的理解：当入参的约束条件比较多时，可以利用extends对参数进行约束

  ```js
  // 约束参数要有length属性才可
  function longest<Type extends { length: number }>(a: Type, b: Type) {
    if (a.length >= b.length) {
      return a;
    } else {
      return b;
    }
  }
   
  // longerArray is of type 'number[]'
  const longerArray = longest([1, 2], [1, 2, 3]);
  // longerString is of type 'alice' | 'bob'
  const longerString = longest("alice", "bob");
  // Error! Numbers don't have a 'length' property
  const notOK = longest(10, 100);
  ```

  

- 指定类型参数

  当入参可能是多种数据类型时，ts的类型推断可能不准确，此时需要手动制定类型

  ```js
  function combine<Type>(arr1: Type[], arr2: Type[]): Type[] {
    return arr1.concat(arr2);
  }
  const arr = combine([1, 2, 3], ["hello"]) // 'hello' 报错：Type 'string' is not assignable to type 'number'
  
  // 手动指定类型即可
  const arr = combine<string | number>([1, 2, 3], ["hello"])
  ```

  

- 一些其他类型
  - void：表示没有返回就，即没有return
  - object：特殊类型`object`是指任何非原始值（`string`、`number`、`bigint`、`boolean`、`symbol`、`null`、 或`undefined`）
  - unknow：表示任何值。这类似于`any`类型，但更安全，因为对`unknown`值执行任何操作都是不合法的
  - never：表示从未观察到的值。在返回类型中，这意味着函数抛出异常或终止程序的执行
  - function：描述了 JavaScript 中所有函数值上存在的属性，如`bind`、`call`、`apply`和其他属性

- 休息参数

  可以使用*剩余参数*定义具有*无限*数量参数的*函数*。

  ```js
  function multiply(n: number, ...m: number[]) {
    return m.map((x) => n * x);
  }
  // 'a' gets value [10, 20, 30, 40]
  const a = multiply(10, 1, 2, 3, 4);
  ```

  
