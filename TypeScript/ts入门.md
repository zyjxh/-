```ts
/*
 * @Description: 
 * @Version: 2.0
 * @Autor: ZhengYun
 * @Date: 2020-08-31 19:45:09
 * @LastEditTime: 2020-09-01 17:09:02
 */
 function sayHello(person: string) {
     return 'Hello,'+ person;
 }
 let user = 'Tom';
 console.log(sayHello(user));

 let isDone: boolean = false;

 let decLiteral: number = 6;
 let hexLiteral: number = 0xf00d;
 let binaryLiteral: number = 0b1010;
 let octalLiteral: number = NaN;
 let infinityNumber: number = Infinity;

 let myName: string = 'Tom';
 let myAge: number = 25;

 let sentence: string = `Hello, my name is ${myName}.   I'll be ${myAge + 1} years old next month.`;

 function alertName(): void {
     alert('My name is Tom');
 }

 let unusable: void = undefined;

 let num: number = undefined; // undefined 和 null 是所有类型的子类型

 //任意类型
 let myFavoriteNumber: any = 'seven';
 myFavoriteNumber = 7;
// 如果是任意类型，则允许被赋值给任意类型
//变量如果在声明的时候，未指定类型，那么它会被识别未任意值类型

//联合类型: 取值为多种类型中的一种
let myFavoriteNumber1: string | number;
myFavoriteNumber1 = 'seven';
myFavoriteNumber1 = 7;

//访问联合类型的属性和方法，只能是共有的属性或者方法；

function getLength(something: string | number) : string {
    return something.toString();
}

//接口  对行为的抽象，以及对 对象的形状 进行描述
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
}

//定义的变量比接口少了一些属性是不允许的
let tom1: Person = {
    name: 'Tom'
}

//定义的变量比接口多了一些属性也是不允许的
let tom2: Person = {
    name: 'Tom',
    age: 29,
    gender: 'male'
}

//任意属性
interface Person1 {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom3: Person1 = {
    name: 'Tom',
    gender: 'male'
}
//一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集

//number 不是string 的子类型 报错
interface Person2 {
    name: string;
    age?: number;
    [propName: string]: string;
}


//如果接口中有多个类型的属性，则可以在任意属性中使用 联合属性

interface Person3 {
    name: string;
    age?: number;
    [propName: string]: string | number;
}

//只读属性
interface Person4 {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
};
let tom4: Person4 = {
    id: 12321,
    name: 'tom',
    gender: 'male'
}

//数组的类型
//【类型+方括号】 表示法
let fibonacci1: number[] = [1,2,3] 
//数组的项中不允许出现其他的类型


//数组泛型
let fibonacci2: Array<number> = [1,2,3]

//用接口表示数组
interface NumberArray {
    [index: number]: number //只要索引的类型是数字时，那么值的类型也必须是数字
}

//类数组 应该用接口来表示
function sum() {
    let args: {[index: number]: number; length: number; callee: Function;} = arguments
}

//any在数组中的应用： 用any表示数组中允许出现任意类型
let list: any[] = ['sss', 23, {a: 'b'}];


//函数的类型

//函数声明 输入多余或者少于要求的参数 是不被允许的
function sum(x: number, y: number) : number {
    return x+y ;
}

//函数表达式
// 很有可能会写成这些
let mySum = function (x: number, y: number): number {
    return x+y;
} // 可以编译，但是不够正确

let mySum1 : (x: number, y: number) => number = function (x : number, y: number): number {
    return x+y;
}
//在ts中=>用来表示函数的定义


//用接口定义函数的形状
interface SearchFunc {
    (source: string, subString: string): boolean ;
}
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}

//可选参数
//与接口中的可选参数一样， 用？表示可选的参数
function buildName(firstName: string, lastName?: string){ //可选参数必须放在必需参数后面
    if(lastName){
        return firstName + lastName;
    } else {
        return firstName;
    }
}

let tomcat =  buildName("Tom", "cat");
let tom12 = buildName("tom");

//参数默认值
function buildName1(firstName: string, lastName: string = 'cat') { //或者对换位置，此时就不受可选参数必须在必需参数后面的限制了
    return firstName + lastName;
}
let tomcat1 = buildName("tom", "cat");
let cat = buildName('tom')  

//剩余参数   使用。。。rest的方式获取函数中的剩余参数
function push(array, ...items) {
    items.forEach(function(item){
        array.push(item);
    })
}

let a: any[]  = []
push(a,1,2,3)

//事实上，items是一个数组，所以我们可以用数组的类型来定义它
function push1(array: any[], ...items: any[]) {
    items.forEach(item => {
        array.push(item);
    });
}
push(a,1,2,3)



//重载
//利用联合类型实现
function reverse1(x: number | string) : number | string {
    if(typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if(typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}

//定义多个函数类型
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string{
    if(typeof x ==='number'){
        return Number(x.toString().split('').reverse().join(''));
    } else if(typeof x==='string'){
        return x.split('').reverse().join('');
    }
}

// 类型断言--可以用来手动指定一个值的类型

// 语法： 值as类型  或者 <类型>值

//类型断言的用途
// 将一个联合类型断言为其中一个类型，之前提到过，当ts不确定一个联合类型的变量到底是那个类型的时候，我们只能访问此联合类型的所有类型中共有的属性和方法

interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function isFish(animal: Cat | Fish) {
    if(typeof animal.swim === 'function') {
        return true;
    }
}

//上面的例子中，获取animal.swim的时候会报错
// 此时可以使用类型断言，将animal断言成Fish

function isFish1(animal: Cat | Fish){
    if(typeof (animal as Fish).swim === 'function'){
        return true;
    }
} 
// 类型断言可以欺骗ts，但是无法避免运行时的错误，反而滥用断言可能会导致运行时错误；



// 将一个父类断言为更加具体的子类： 当类之间有继承关系时，类型断言也是很常见的：


interface ApiError extends Error {
    code: number;
}
interface HttpError extends Error {
    statuscode: number;
}

function isApiError(error : Error) {
    if(typeof (error as ApiError).code === 'number') {
        return true;
    }
    return false;
}

// 将任何一个类型断言为any
(window as any).foo = 1

// 将any断言为一个具体的类型
function getCacheData(key: string) : any {
    return (window as any ).cache[key];
}

interface Cat {
    name: string;
    run(): void;
}

const tom21 = getCacheData('tom') as Cat;
tom21.run();



//声明文件

// 新语法索引
/**
 * declare var 声明全局变量
 * declare function 声明全局方法
 * declare calss 声明全局类
 * declare enum 声明全局枚举类型
 * declare namespace 声明(含有子属性) 全局对象
 * interface 和 type 声明全局类型
 * export 导出变量
 * export namespace 导出(含有子属性) 对象
 * export default ES6默认导出
 * export = COMMONJS导出模块
 * export as namespace UMD库声明全局变量
 * declare global 扩展全局变量
 * declare module 扩展模块
 * /// <reference>  三斜线指令
*/

// 什么是声明文件： 必须以 .d.ts为后缀
// 推荐使用 @type 统一管理第三方库的声明文件


// 如何书写声明文件
// 库的使用场景主要有以下几种
/**
 * 全局变量： 通过<script>标签引入第三方库，注入全局变量
 * npm包: 通过 import foo from 'foo' 导入， 符合es6模块规范
 * UMD库: 既可以通过<script>标签引入，又可以通过import导入
 * 直接扩展全局变量: 通过<script> 标签引入后, 改变一个全局变量的结构
 * 在npm包或者UMD库中扩展全局变量: 引用npm包或UMD库后，改变一个全局变量的结构
 * 模块插件: 通过<script>或 import 导入后， 改变另一个模块的结构
 */




//  内置对象
// ECMAScript的内e'g
// Boolean、Error、Date、RegExp,可以在TypeScript中直接定义这些类型
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;

//DOM和BOM的内置对象： Document、 HTMLElement、Event、NodeList;
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
document.addEventListener('click', function(e: MouseEvent) {

});
//进阶
/**
 * 类型别名
 * 字符串字面量类型
 * 元组
 * 枚举
 * 类
 * 类与接口
 * 泛型
 * 声明合并
 * 
 **/


 //1-类型别名用来给一个类型起个新名字
 type Name = string;
 type NameResolver = () => string;
 type NameOrResolver = Name | NameResolver;

 function getName(n: NameOrResolver): Name {
     if(typeof n === 'string') {
         return n;
     } else {
         return n();
     }
 }

 //2-字符串字面量类型： 用来约束取值只能是某几个字符串中的一个

 type EventNames = 'click' | 'scroll' | 'mousemove';
 function handleEvent(ele: Element, event: EventNames) {

 }
 handleEvent(document.getElementById('hello'), 'scroll');  //类型别名和字符串字面量类型都是使用type进行定义


 //3-元组： 数组合并了相同类型的对象，而元组合并了不同类型的对象。
 let tom31: [string, number] = ['Tom', 25];

 let tom32: [string, number];
     tom32 = ['Tom']

//越界的元素-当添加越界的元素时，它的类型会被限制为元组中每个类型的联合类型


//4-枚举： 枚举类型用于取值被限定在一定范围内的场景。
//枚举使用enum关键字来定义， 枚举成员会被赋值为从0开始递增的数字，同时也会对枚举名进行反向映射；

enum Days {Sun = 7, Mon, Tue, Web, Fri, Sat };
console.log(Days["Sun"] === 0); //true
console.log(Days["Mon"] === 1); //true
console.log(Days["Tue"] === 2); //true
console.log(Days["Web"] === 3); //true
console.log(Days["Fri"] === 4); //true

//枚举项和计算所得项
enum Color {Red, Green, Blue = 'blue'.length}

//外部枚举 是使用declare enum 定义的枚举类型


// ts中类的使用： 
//可以使用三种访问修饰符： public， private, protected


```

