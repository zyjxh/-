## 正文

### 是什么?

apply,call,bind都是js给函数内置的一些api,调用他们可以为函数指定this的执行,同时也可以传参.

### 怎么用?

```
//apply 
func.apply(thisArg, [argsArray])
 
//call
fun.call(thisArg, arg1, arg2, ...)

//bind
const newFun = fun.bind(thisArg, arg1, arg2, ...)
newFun()
复制代码
```

apply和call就是传参不一样,但是两个都是会在调用的时候同时执行调用的函数,但是bind则会返回一个绑定了this的函数.

我们还需要知道一个事情,就是this的指向.

### this的指向

this的指向是在函数调用的时候确定下来的,this的指向大致可以分为五种.

#### 1. 默认绑定

默认绑定一般发生在回调函数,函数直接调用;

```
function test() {
    //严格模式下是undefined
    //非严格模式下是window
    console.log(this);
}
setTimeout(function () {
    //setTimeout的比较特殊
    //严格模式和非严格模式下都是window
    console.log(this);
});

arr.forEach(function () {
    //严格模式下是undefined
    //非严格模式下是window
    console.log(this);
});
复制代码
```

#### 2. 隐式绑定

这个通俗点用一句话概括就是谁调用就是指向谁

```
    const obj = {
        name:'joy',
        getName(){
            console.log(this); //obj
            console.log(this.name); //joy
        }
    };
    obj.getName();
复制代码
```

#### 3. 显示绑定call,apply,bind

```
const obj1 = {
    name: 'joy',
    getName() {
        console.log(this); 
        console.log(this.name); 
    }
};

const obj2 = {
    name: 'sam'
};

obj1.getName.call(obj2); //obj2 sam
obj1.getName.apply(obj2); //obj2 sam
const fn = obj1.getName.bind(obj2);
fn();//obj2 sam
复制代码
```

#### 4. new绑定

```
function Vehicle() {
    this.a = 2
    console.log(this);
}
new Vehicle(); //this指向Vehicle这个new出来的对象
复制代码
```

#### 5. es6的箭头函数

  es6的箭头函数比较特殊,箭头函数this为父作用域的this，不是调用时的this.要知道前四种方式,都是调用时确定,也就是动态的,而箭头函数的this指向是静态的,声明的时候就确定了下来.比较符合js的词法作用域吧

```
window.name = 'win';
const obj = {
    name: 'joy',
    age: 12,
    getName: () => {
        console.log(this); //其父作用域this是window,所以就是window
        console.log(this.name); //win 
    },
    getAge: function () {
        //通过obj.getAge调用,这里面this是指向obj
        setTimeout(() => {
            //所以这里this也是指向obj 所以结果是12
            console.log(this.age); 
        });
    }
};
obj.getName();
obj.getAge();
复制代码
```

既然有5种this的绑定方式,那么肯定有优先级的先后

> 箭头函数 -> new绑定 -> 显示绑定call/bind/apply -> 隐式绑定 -> 默认绑定

这里直接给出了结论,有兴趣的小伙伴们可以自己去验证一下

### 实现apply

先来实现apply吧

1. 先给Function原型上扩展个方法并接收2个参数,

```
Function.prototype.myApply = function (context, args) {

}
复制代码
```

1. 因为不传context的话,this会指向window的,args也做一下容错处理

```
Function.prototype.myApply = function (context, args) {
    //这里默认不传就是给window,也可以用es6给参数设置默认参数
    context = context || window
    args = args ? args : []
}
复制代码
```

1. 需要回想一下绑定this的五种方式,现在要来给调用的函数绑定this了, 这里默认绑定和new肯定用不了,这里就使用隐式绑定去实现显式绑定了

```
Function.prototype.myApply = function (context, args) {
    //这里默认不传就是给window,也可以用es6给参数设置默认参数
    context = context || window
    args = args ? args : []
    //给context新增一个独一无二的属性以免覆盖原有属性
    const key = Symbol()
    context[key] = this
    //通过隐式绑定的方式调用函数
    context[key](...args)
}
复制代码
```

1. 最后一步要返回函数调用的返回值,并且把context上的属性删了才不会造成影响

```
Function.prototype.myApply = function (context, args) {
    //这里默认不传就是给window,也可以用es6给参数设置默认参数
    context = context || window
    args = args ? args : []
    //给context新增一个独一无二的属性以免覆盖原有属性
    const key = Symbol()
    context[key] = this
    //通过隐式绑定的方式调用函数
    const result = context[key](...args)
    //删除添加的属性
    delete context[key]
    //返回函数调用的返回值
    return result
}
复制代码
```

这样一个简单的apply就实现了,可能会有一些边界问题和错误判断需要完善,这里就不做继续优化了

既然apply实现了,那么call同样也非常简单了,主要就是传参不一样

#### 实现call

这里就直接上代码吧

```
//传递参数从一个数组变成逐个传参了,不用...扩展运算符的也可以用arguments代替
Function.prototype.myCall = function (context, ...args) {
    //这里默认不传就是给window,也可以用es6给参数设置默认参数
    context = context || window
    args = args ? args : []
    //给context新增一个独一无二的属性以免覆盖原有属性
    const key = Symbol()
    context[key] = this
    //通过隐式绑定的方式调用函数
    const result = context[key](...args)
    //删除添加的属性
    delete context[key]
    //返回函数调用的返回值
    return result
}
复制代码
```

#### 实现bind

bind和apply的区别在于,bind是返回一个绑定好的函数,apply是直接调用.其实想一想实现也很简单,就是返回一个函数,里面执行了apply上述的操作而已.不过有一个需要判断的点,因为返回新的函数,要考虑到使用new去调用,并且new的优先级比较高,所以需要判断new的调用,还有一个特点就是bind调用的时候可以传参,调用之后生成的新的函数也可以传参,效果是一样的,所以这一块也要做处理 因为上面已经实现了apply,这里就借用一下,实际上不借用就是把代码copy过来

```
Function.prototype.myBind = function (context, ...args) {
    const fn = this
    args = args ? args : []
    return function newFn(...newFnArgs) {
        if (this instanceof newFn) {
            return new fn(...args, ...newFnArgs)
        }
        return fn.apply(context, [...args,...newFnArgs])
    }
}
复制代码
```

以上所有实现可以再加点判断啊,例如调用的不是function就返回或者抛出错误啊之类的.我这里就不处理了

以上就是apply,call,bind的实现了


