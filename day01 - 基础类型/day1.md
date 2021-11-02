# day01

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

- 注意：此语法中的类型注释是为了学习时方便理解，但实际开发中，不是必需的，每当定义一个变量时，TypeScript 会尝试自动*推断*代码中的类型。

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
	一个特殊类型 ，`any`当您不希望某个特定值导致类型检查错误时，您可以使用它。
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
  return 11
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



## interface 和 type

在 JavaScript 中，我们分组和传递数据的基本方式是通过对象。在 TypeScript 中，我们通过*对象类型来*表示那些。

正如我们所见，它们可以是匿名的：

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
