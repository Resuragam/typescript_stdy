# TypeScript类型

> TypeScript具有自动判别类型的功能，当变量的类型声明以及赋值同时进行时，会自动判别类型，因此此时可以省略变量类型声明的过程。

## TypeScript的类型

| 类型    | 例子                     | 描述                                           |
| ------- | ------------------------ | ---------------------------------------------- |
| string  | 'abcd'，'hello'          | 任意字符串                                     |
| number  | 1，2，-4，999            | 任意数字                                       |
| boolean | true，false              | 布尔值                                         |
| 字面量  | let a:123，let b:'hello' | 对于变量设置特定数据类型以及值，并且不能修改值 |
| any     | *                        | 任意数据类型                                   |
| unknown | *                        | 任意数据类型，**但是比any安全**                |
| void    | 空值(undefined)          | 空值(undefined)                                |
| never   | 没有值                   | 不能是任何值                                   |
| object  | {name:"张三"}            | 任意对象                                       |
| array   | [1,2,3,4]                | 任意数组                                       |
| tuple   | [1,2]                    | 元组类型，固定大小、元素数据类型的数组         |
| enum    | enum{a,b}                | 枚举                                           |

### string

~~~typescript
// 声明一个变量，同时指定变量的类型为string
let str:string


str = 'abcd'
// str = 2545 //报错
~~~

### number

~~~typescript
let b:number
b = 3
// b = 'string' // 报错
~~~

### boolean

布尔类型只能赋值`true`或者`false`

~~~typescript
let c = false
c = true
// c = 2 //报错
~~~

### 字面量

字面量赋值变量判定为初始赋值的类型，并且值不能改变，类似于常量赋值

~~~typescript
// 直接使用字面量进行类型声明
let a:10
a = 10;
// a = 11; // 报错，类似于常量赋值
~~~

### any

any表示任何类型，当变量设置为any时，表示关闭类型检测，并且他能赋值给任何类型的变量数据

~~~typescript
let data1:any
data1 = 'hello'
data1 = 222

let data2:string
data2 = 'data'
data2 = data1 // 不报错
~~~

### unknown

表示未知的类型，可以赋值任意类型数据，但是不会关闭类型检测

~~~typescript
let s:string
d = true
s = d // any类型影响类型检查

let e:unknown
e = true
// s = e //报错，未知类型会进行检查，any类型不会进行检查
// unknown 是一个类型安全的any
// unknown 不能直接赋值给其他变量

if(typeof e === 'string'){
    s = e
}

/*
*  语法
*  变量 as 类型
*  <类型>变量
* */
// 类型断言, 用来告诉解析器变量类型
s = e as string
s = <string>e
~~~

### void

 void 表示为空，以函数为例，就表示没有返回值的函数。 

~~~typescript
let c:void // 空值
c = undefined

function fn_void():void{
    return null
}
~~~

### never

表示永远不会返回结果

~~~typescript
// never表示永远都不会有返回值
function fn_never():never{
    throw new Error('报错了')
}
~~~

### object

object表示一个JS对象，可以指定该对象包含属性

~~~typescript
// 指定对象可以包含的属性，属性结构必须一致
// 当属性名后面加上？,表示属性是可选的
let b:{name:string,age?:number}
b = {name:'张三'}
// b = {} // 报错
~~~

如果想要指定该对象包含任意类型的属性，`[propName:string]:any`可以指定任意类型的属性

~~~typescript
let c:{[propName:string]:any}
c = {name:'张三'}
~~~

当需要声明一个函数的类型检测的时候

~~~typescript
/*
* 设置函数结构的声明
*   语法:(形参：类型, 形参：类型，....)=>返回值类型
* */
// 表示d是一个函数，参数a,b是number，返回值是number,限制函数的结构
let d:(a:number,b:number)=>number
d = function (n1,n2){
    return n1 + n2
}
~~~

### array

表示指定类型的数组

~~~typescript
// string[]表示字符串数组
let e:string[]
e = ['a','v']

// number[]表示字符串数组
let f:number[]
f = [1,2,3]

let g:Array<number>
g = [1,2,3]
~~~

### tuple

元组表示固定长度以及元素类型的数组，效率比数组更高

~~~typescript
let h:[string,string]
h = ['a','abc']
~~~

### enum

枚举表示列出对象所有的可能情况

~~~typescript
enum Gender{
    male = 0,
    feMale = 1
}
let i:{name:string,gender:Gender}
i = {
    name:"张三",
    gender:Gender.male
}
console.log(i.gender === Gender.male) // true
~~~

### 注意点

#### null和undefined

默认情况下，null和undefined可以赋值给任意其他类型，是任意类型的子类型

~~~typescript
let a:null|undefined
let b:number
let c:string
let d:Array<string>

b = a
c = a
d = a
~~~

#### any与unknown的区别？

* any会关闭TS的类型检查，而unknown不会关闭类型检查
* 可以将任何东西赋给 `unknown` 类型，但在进行类型检查或类型断言之前，不能对 `unknown` 进行操作
* 可以把任何东西分配给`any`类型，也可以对`any`类型进行任何操作

~~~typescript
function invokeAnything(callback: unknown) {
    // 可以将任何东西赋给 `unknown` 类型，
    // 但在进行类型检查或类型断言之前，不能对 `unknown` 进行操作
    // if (typeof callback === 'function') {
    //     callback();
    // }
    (callback as Function)()
}
invokeAnything(1); // You can assign anything to `unknown` type
~~~

***unknown类型在操作之前需要进行类型检查或者断言***

