# 从零搭建react框架

# init项目

1. 创建文件夹并进入

   `mkdir react-family && cd react-family`

2. init npm

   `npm init` 按照提示填写项目基本信息

# webpack

1. 安装 `webpack`

   `npm install --save-dev webpack@3`

   Q: 什么时候用`--save-dev`，什么时候用`--save`？

   A: `--save-dev` 是你开发时候依赖的东西，`--save` 是你发布之后还依赖的东西。看[这里](https://segmentfault.com/q/1010000005163089)

2. 根据[webpack文档](https://doc.webpack-china.org/guides/getting-started)编写最基础的配置文件

   新建`webpack`开发配置文件 `touch webpack.dev.config.js`

   `webpack.dev.config.js`

```
const path = require('path');

module.exports = {
 
    /*入口*/
    entry: path.join(__dirname, 'src/index.js'),
    
    /*输出到dist文件夹，输出文件名字为bundle.js*/
    output: {
        path: path.join(__dirname, './dist'),
        filename: 'bundle.js'
    }
};
```

1. 学会使用`webpack`编译文件

   新建入口文件

   `mkdir src && touch ./src/index.js`

   `src/index.js` 添加内容

   `document.getElementById('app').innerHTML = "Webpack works"`

   现在我们执行命令 `webpack --config webpack.dev.config.js`

   > webpack如果没有全局安装，这里会报错哦。命令建议全局安装，如果编译有问题看这里 [#1 (comment)](https://github.com/brickspert/blog/issues/1#issuecomment-326899255)

   我们可以看到生成了`dist`文件夹和`bundle.js`。

2. 现在我们测试下~

   `dist`文件夹下面新建一个`index.html`

   `touch ./dist/index.html`

   `dist/index.html`填写内容

   ```
   <!doctype html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Document</title>
   </head>
   <body>
   <div id="app"></div>
   <script type="text/javascript" src="./bundle.js" charset="utf-8"></script>
   </body>
   </html>
   ```

   用浏览器打开`index.html`,可以看到`Webpack works`!

   [![webpack](https://camo.githubusercontent.com/f011eda1205f66f52dea4c7d7d8ae6c0de1c8630/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f7765627061636b2e706e67)](https://camo.githubusercontent.com/f011eda1205f66f52dea4c7d7d8ae6c0de1c8630/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f7765627061636b2e706e67)

   现在回头看下，我们做了什么或者说`webpack`做了什么。

   把入口文件 `index.js` 经过处理之后，生成 `bundle.js`。就这么简单。

# babel

> Babel 把用最新标准编写的 JavaScript 代码向下编译成可以在今天随处可用的版本。 这一过程叫做“源码到源码”编译， 也被称为转换编译。

通俗的说，就是我们可以用ES6, ES7等来编写代码，Babel会把他们统统转为ES5。

- [babel-core](https://github.com/babel/babel/tree/master/packages/babel-core) 调用Babel的API进行转码

- [babel-loader](https://github.com/babel/babel-loader)

- [babel-preset-es2015](https://github.com/babel/babel/tree/master/packages/babel-preset-es2015) 用于解析 ES6

- [babel-preset-react](https://github.com/babel/babel/tree/master/packages/babel-preset-react) 用于解析 JSX

- [babel-preset-stage-0](https://github.com/babel/babel/tree/master/packages/babel-preset-stage-0) 用于解析 ES7 提案

  `npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react babel-preset-stage-0`

  新建`babel`配置文件`.babelrc`

  `touch .babelrc`

  `.babelrc`

```
    {
      "presets": [
        "es2015",
        "react",
        "stage-0"
      ],
      "plugins": []
    }
```

修改`webpack.dev.config.js`，增加`babel-loader`！

```
    /*src文件夹下面的以.js结尾的文件，要使用babel解析*/
    /*cacheDirectory是用来缓存编译结果，下次编译加速*/
    module: {
        rules: [{
            test: /\.js$/,
            use: ['babel-loader?cacheDirectory=true'],
            include: path.join(__dirname, 'src')
        }]
    }
```

现在我们简单测试下，是否能正确转义ES6~
​
修改 `src/index.js`

```
    /*使用es6的箭头函数*/
    var func = str => {
        document.getElementById('app').innerHTML = str;
    };
    func('我现在在使用Babel!');
```

执行打包命令`webpack --config webpack.dev.config.js`

浏览器打开`index.html`，我们看到正确输出了`我现在在使用Babel!`

[![babel](https://camo.githubusercontent.com/5690689fbf4b45f863e3f7d50b347acc3138446d/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f626162656c2e706e67)](https://camo.githubusercontent.com/5690689fbf4b45f863e3f7d50b347acc3138446d/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f626162656c2e706e67)

然后我们打开打包后的`bundle.js`,翻页到最下面,可以看到箭头函数被转换成普通函数了！

[![babel-bundle.png](https://camo.githubusercontent.com/bb95602c7614071feeadbfab6deb89e1da4a3999/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f626162656c2d62756e646c652e706e67)](https://camo.githubusercontent.com/bb95602c7614071feeadbfab6deb89e1da4a3999/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f626162656c2d62756e646c652e706e67)

Q: `babel-preset-state-0`,`babel-preset-state-1`,`babel-preset-state-2`,`babel-preset-state-3`有什么区别？

A: 每一级包含上一级的功能，比如 `state-0`包含`state-1`的功能，以此类推。`state-0`功能最全。具体可以看这篇文章：[babel配置-各阶段的stage的区别](https://www.vanadis.cn/2017/03/18/babel-stage-x/)

参考地址：

1. https://segmentfault.com/a/1190000008159877
2. http://www.ruanyifeng.com/blog/2016/01/babel.html

# react

```
npm install --save react react-dom
```

修改 `src/index.js`使用`react`

```
import React from 'react';
import ReactDom from 'react-dom';

ReactDom.render(
    <div>Hello React!</div>, document.getElementById('app'));
```

执行打包命令`webpack --config webpack.dev.config.js`

打开`index.html` 看效果。

我们简单做下改进，把`Hello React`放到组件里面。体现组件化~

```
cd src
mkdir component
cd component
mkdir Hello
cd Hello
touch Hello.js
```

按照React语法，写一个Hello组件

```
import React, {Component} from 'react';

export default class Hello extends Component {
    render() {
        return (
            <div>
                Hello,React!
            </div>
        )
    }
}
```

然后让我们修改`src/index.js`，引用`Hello`组件！

```
src/index.js
import React from 'react';
import ReactDom from 'react-dom';
import Hello from './component/Hello/Hello';

ReactDom.render(
    <Hello/>, document.getElementById('app'));
```

在**根目录**执行打包命令

```
webpack --config webpack.dev.config.js
```

打开`index.html`看效果咯~

# 命令优化

Q：每次打包都得在根目录执行这么一长串命令`webpack --config webpack.dev.config.js`,能不打这么长吗？

A：修改`package.json`里面的`script`，增加`dev-build`。

```
package.json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev-build": "webpack --config webpack.dev.config.js"
  }
```

现在我们打包只需要执行`npm run dev-build`就可以啦！

参考地址：

http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html

# react-router

```
npm install --save react-router-dom
```

新建`router`文件夹和组件

```
cd src
mkdir router && touch router/router.js
```

按照`react-router`[文档](http://reacttraining.cn/web/guides/quick-start)编辑一个最基本的`router.js`。包含两个页面`home`和`page1`。

```
src/router/router.js
import React from 'react';

import {BrowserRouter as Router, Route, Switch, Link} from 'react-router-dom';

import Home from '../pages/Home/Home';
import Page1 from '../pages/Page1/Page1';


const getRouter = () => (
    <Router>
        <div>
            <ul>
                <li><Link to="/">首页</Link></li>
                <li><Link to="/page1">Page1</Link></li>
            </ul>
            <Switch>
                <Route exact path="/" component={Home}/>
                <Route path="/page1" component={Page1}/>
            </Switch>
        </div>
    </Router>
);

export default getRouter;
```

新建页面文件夹

```
cd src
mkdir pages
```

新建两个页面 `Home`,`Page1`

```
cd src/pages
mkdir Home && touch Home/Home.js
mkdir Page1 && touch Page1/Page1.js
```

填充内容：

```
src/pages/Home/Home.js
import React, {Component} from 'react';

export default class Home extends Component {
    render() {
        return (
            <div>
                this is home~
            </div>
        )
    }
}
```

Page1.js

```
import React, {Component} from 'react';

export default class Page1 extends Component {
    render() {
        return (
            <div>
                this is Page1~
            </div>
        )
    }
}
```

现在路由和页面建好了，我们在入口文件`src/index.js`引用Router。

修改`src/index.js`

```
import React from 'react';
import ReactDom from 'react-dom';

import getRouter from './router/router';

ReactDom.render(
    getRouter(), document.getElementById('app'));
```

现在执行打包命令`npm run dev-build`。打开`index.html`查看效果啦！

那么问题来了~我们发现点击‘首页’和‘Page1’没有反应。不要惊慌，这是正常的。

我们之前一直用这个路径访问`index.html`，类似这样：`file:///F:/react/react-family/dist/index.html`。
这种路径了，不是我们想象中的路由那样的路径`http://localhost:3000`~我们需要配置一个简单的WEB服务器，指向
`index.html`~有下面两种方法来实现

1. `Nginx`, `Apache`, `IIS`等配置启动一个简单的的WEB服务器。
2. 使用`webpack-dev-server`来配置启动WEB服务器。

下一节，我们来使用第二种方法启动服务器。这一节的DEMO，先放这里。

参考地址

1. http://www.jianshu.com/p/e3adc9b5f75c
2. http://reacttraining.cn/web/guides/quick-start

# webpack-dev-server

简单来说，`webpack-dev-server`就是一个小型的静态文件服务器。使用它，可以为`webpack`打包生成的资源文件提供Web服务。

```
npm install webpack-dev-server@2 --save-dev
```

> 2017.11.16补充：这里webpack-dev-server需要全局安装，要不后面用的时候要写相对路径。需要再执行这个 `npm install webpack-dev-server@2 -g`

修改`webpack.dev.config.js`,增加`webpack-dev-server`的配置。

```
webpack.dev.config.js
    devServer: {
        contentBase: path.join(__dirname, './dist')
    }
```

现在执行

```
webpack-dev-server --config webpack.dev.config.js
```

浏览器打开[http://localhost:8080](http://localhost:8080/)，OK,现在我们可以点击`首页`,`Page1`了，
看URL地址变化啦！我们看到`react-router`已经成功了哦。

Q: `--content-base`是什么？

A：`URL的根目录。如果不设定的话，默认指向项目根目录。`

**重要提示：webpack-dev-server编译后的文件，都存储在内存中，我们并不能看见的。你可以删除之前遗留的文件`dist/bundle.js`，
仍然能正常打开网站！**

每次执行`webpack-dev-server --config webpack.dev.config.js`,要打很长的命令，我们修改`package.json`，增加`script->start`:

```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev-build": "webpack --config webpack.dev.config.js",
    "start": "webpack-dev-server --config webpack.dev.config.js"
  }
```

下次执行`npm start`就可以了。

既然用到了`webpack-dev-server`，我们就看看[它的其他的配置项](https://doc.webpack-china.org/configuration/dev-server)。
看了之后，发现有几个我们可以用的。

- color（CLI only） `console`中打印彩色日志
- historyApiFallback 任意的`404`响应都被替代为`index.html`。有什么用呢？你现在运行
  `npm start`，然后打开浏览器，访问`http://localhost:8080`,然后点击`Page1`到链接`http://localhost:8080/page1`，
  然后刷新页面试试。是不是发现刷新后`404`了。为什么？`dist`文件夹里面并没有`page1.html`,当然会`404`了，所以我们需要配置
  `historyApiFallback`，让所有的`404`定位到`index.html`。
- host 指定一个`host`,默认是`localhost`。如果你希望服务器外部可以访问，指定如下：`host: "0.0.0.0"`。比如你用手机通过IP访问。
- hot 启用`Webpack`的模块热替换特性。关于热模块替换，我下一小节专门讲解一下。
- port 配置要监听的端口。默认就是我们现在使用的`8080`端口。
- proxy 代理。比如在 `localhost:3000` 上有后端服务的话，你可以这样启用代理：

```
    proxy: {
      "/api": "http://localhost:3000"
    }
```

- progress（CLI only） 将编译进度输出到控制台。

根据这几个配置，修改下我们的`webpack-dev-server`的配置~

```
webpack.dev.config.js
    devServer: {
        port: 8080,
        contentBase: path.join(__dirname, './dist'),
        historyApiFallback: true,
        host: '0.0.0.0'
    }
```

`CLI ONLY`的需要在命令行中配置

```
package.json
"start": "webpack-dev-server --config webpack.dev.config.js --color --progress"
```

现在我们执行`npm start` 看看效果。是不是看到打包的时候有百分比进度？在`http://localhost:8080/page1`页面刷新是不是没问题了？
用手机通过局域网IP是否可以访问到网站？

参考地址：

1. https://segmentfault.com/a/1190000006670084
2. https://webpack.js.org/guides/development/#using-webpack-dev-server

# 模块热替换（Hot Module Replacement）

到目前，当我们修改代码的时候，浏览器会自动刷新，不信你可以去试试。（如果你的不会刷新，看看这个[调整文本编辑器](https://github.com/brickspert/blog/issues/1#editor)）

我相信看这个教程的人，应该用过别人的框架。我们在修改代码的时候，浏览器不会刷新，只会更新自己修改的那一块。我们也要实现这个效果。

我们看下[webpack模块热替换](https://doc.webpack-china.org/guides/hot-module-replacement)教程。

我们接下来要这么修改

```
package.json` 增加 `--hot
"start": "webpack-dev-server --config webpack.dev.config.js --color --progress --hot"
```

`src/index.js` 增加`module.hot.accept()`,如下。当模块更新的时候，通知`index.js`。

```
src/index.js
import React from 'react';
import ReactDom from 'react-dom';

import getRouter from './router/router';

if (module.hot) {
    module.hot.accept();
}

ReactDom.render(
    getRouter(), document.getElementById('app'));
```

现在我们执行`npm start`，打开浏览器，修改`Home.js`,看是不是不刷新页面的情况下，内容更新了？惊不惊喜？意不意外？

**做模块热替换，我们只改了几行代码，非常简单的。纸老虎一个~**

现在我需要说明下我们命令行使用的`--hot`，可以通过配置`webpack.dev.config.js`来替换，
向文档上那样,修改下面三处。但我们还是用`--hot`吧。下面的方式我们知道一下就行，我们不用。同样的效果。

```
const webpack = require('webpack');

devServer: {
    hot: true
}

plugins:[
     new webpack.HotModuleReplacementPlugin()
]
```

`HRM`配置其实有两种方式，一种`CLI`方式，一种`Node.js API`方式。我们用到的就是`CLI`方式，比较简单。
`Node.js API`方式，就是建一个`server.js`等等，网上大部分教程都是这种方式，这里不做讲解了。

你以为模块热替换到这里就结束了？no~~no~~no~

上面的配置对`react`模块的支持不是很好哦。

例如下面的`demo`，当模块热替换的时候，`state`会重置，这不是我们想要的。

修改`Home.js`,增加计数`state`

```
src/pages/Home/Home.js
import React, {Component} from 'react';

export default class Home extends Component {
    constructor(props) {
        super(props);
        this.state = {
            count: 0
        }
    }

    _handleClick() {
        this.setState({
            count: ++this.state.count
        });
    }

    render() {
        return (
            <div>
                this is home~<br/>
                当前计数：{this.state.count}<br/>
                <button onClick={() => this._handleClick()}>自增</button>
            </div>
        )
    }
}
```

你可以测试一下，当我们修改代码的时候，`webpack`在更新页面的时候，也把`count`初始为0了。

为了在`react`模块更新的同时，能保留`state`等页面中其他状态，我们需要引入[react-hot-loader](https://github.com/gaearon/react-hot-loader)~

Q:　请问`webpack-dev-server`与`react-hot-loader`两者的热替换有什么区别？

A: 区别在于`webpack-dev-serve`r自己的`--hot`模式只能即时刷新页面，但状态保存不住。因为`React`有一些自己语法(JSX)是`HotModuleReplacementPlugin`搞不定的。
而`react-hot-loader`在`--hot`基础上做了额外的处理，来保证状态可以存下来。（来自[segmentfault](https://segmentfault.com/q/1010000005612845)）

下面我们来加入`react-hot-loader v3`,

安装依赖

```
npm install react-hot-loader@next --save-dev
```

根据[文档](https://gaearon.github.io/react-hot-loader/getstarted/)，
我们要做如下几个修改~

1. `.babelrc` 增加 `react-hot-loader/babel`

```
.babelrc
{
  "presets": [
    "es2015",
    "react",
    "stage-0"
  ],
  "plugins": [
    "react-hot-loader/babel"
  ]
}
```

1. `webpack.dev.config.js`入口增加`react-hot-loader/patch`

```
webpack.dev.config.js
    entry: [
        'react-hot-loader/patch',
        path.join(__dirname, 'src/index.js')
    ]
```

1. `src/index.js`修改如下

```
src/index.js
import React from 'react';
import ReactDom from 'react-dom';
import {AppContainer} from 'react-hot-loader';

import getRouter from './router/router';

/*初始化*/
renderWithHotReload(getRouter());

/*热更新*/
if (module.hot) {
    module.hot.accept('./router/router', () => {
        const getRouter = require('./router/router').default;
        renderWithHotReload(getRouter());
    });
}

function renderWithHotReload(RootElement) {
    ReactDom.render(
        <AppContainer>
            {RootElement}
        </AppContainer>,
        document.getElementById('app')
    )
}
```

现在，执行`npm start`，试试。是不是修改页面的时候，`state`不更新了？

参考文章：

1. [gaearon/react-hot-loader#243](https://github.com/gaearon/react-hot-loader/issues/243)

# 文件路径优化

做到这里，我们简单休息下。做下优化~

在之前写的代码中，我们引用组件，或者页面时候，写的是相对路径~

比如`src/router/router.js`里面，引用`Home.js`的时候就用的相对路径

```
import Home from '../pages/Home/Home';
```

webpack提供了一个别名配置，就是我们无论在哪个路径下，引用都可以这样

```
import Home from 'pages/Home/Home';
```

下面我们来配置下，修改`webpack.dev.config.js`，增加别名~

```
webpack.dev.config.js
    resolve: {
        alias: {
            pages: path.join(__dirname, 'src/pages'),
            component: path.join(__dirname, 'src/component'),
            router: path.join(__dirname, 'src/router')
        }
    }
```

然后我们把之前使用的绝对路径统统改掉。

```
src/router/router.js
import Home from 'pages/Home/Home';
import Page1 from 'pages/Page1/Page1';
src/index.js
import getRouter from 'router/router';
```

我们这里约定，下面，我们会默认配置需要的别名路径，不再做重复的讲述哦。

# redux

接下来，我们就要就要就要集成`redux`了。

要对`redux`有一个大概的认识，可以阅读阮一峰前辈的[Redux 入门教程（一）：基本用法](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)

如果要对`redux`有一个非常详细的认识，我推荐阅读[中文文档](http://cn.redux.js.org/index.html)，写的非常好。读了这个教程，有一个非常深刻的感觉，`redux`并没有任何魔法。

**不要被各种关于 reducers, middleware, store 的演讲所蒙蔽 ---- Redux 实际是非常简单的。**

当然，我这篇文章是写给新手的，如果看不懂上面的文章，或者不想看，没关系。先会用，多用用就知道原理了。

开始整代码！我们就做一个最简单的计数器。自增，自减，重置。

先安装`redux` `npm install --save redux`

初始化目录结构

```
cd src
mkdir redux
cd redux
mkdir actions
mkdir reducers
touch reducers.js
touch store.js
touch actions/counter.js
touch reducers/counter.js
```

先来写`action`创建函数。**通过`action`创建函数，可以创建`action`~**
`src/redux/actions/counter.js`

```
/*action*/

export const INCREMENT = "counter/INCREMENT";
export const DECREMENT = "counter/DECREMENT";
export const RESET = "counter/RESET";

export function increment() {
    return {type: INCREMENT}
}

export function decrement() {
    return {type: DECREMENT}
}

export function reset() {
    return {type: RESET}
}
```

再来写`reducer`,**`reducer`是一个纯函数，接收`action`和旧的`state`,生成新的`state`.**

```
src/redux/reducers/counter.js
import {INCREMENT, DECREMENT, RESET} from '../actions/counter';

/*
* 初始化state
 */

const initState = {
    count: 0
};
/*
* reducer
 */
export default function reducer(state = initState, action) {
    switch (action.type) {
        case INCREMENT:
            return {
                count: state.count + 1
            };
        case DECREMENT:
            return {
                count: state.count - 1
            };
        case RESET:
            return {count: 0};
        default:
            return state
    }
}
```

一个项目有很多的`reducers`,我们要把他们整合到一起

```
src/redux/reducers.js
import counter from './reducers/counter';

export default function combineReducers(state = {}, action) {
    return {
        counter: counter(state.counter, action)
    }
}
```

到这里，我们必须再理解下一句话。

**`reducer`就是纯函数，接收`state` 和 `action`，然后返回一个新的 `state`。**

看看上面的代码，无论是`combineReducers`函数也好，还是`reducer`函数也好，都是接收`state`和`action`，
返回更新后的`state`。区别就是`combineReducers`函数是处理整棵树，`reducer`函数是处理树的某一点。

接下来，我们要创建一个`store`。

前面我们可以使用 `action` 来描述“发生了什么”，使用`action`创建函数来返回`action`。

还可以使用 `reducers` 来根据 `action` 更新 `state` 。

那我们如何提交`action`？提交的时候，怎么才能触发`reducers`呢？

`store` 就是把它们联系到一起的对象。`store` 有以下职责：

- 维持应用的 `state`；
- 提供 `getState()` 方法获取 `state`；
- 提供 `dispatch(action)` 触发`reducers`方法更新 `state`；
- 通过` subscribe(listener)` 注册监听器;
- 通过 `subscribe(listener)` 返回的函数注销监听器。

```
src/redux/store.js
import {createStore} from 'redux';
import combineReducers from './reducers.js';

let store = createStore(combineReducers);

export default store;
```

到现在为止，我们已经可以使用`redux`了~

下面我们就简单的测试下

```
cd src
cd redux
touch testRedux.js
src/redux/testRedux.js
import {increment, decrement, reset} from './actions/counter';

import store from './store';

// 打印初始状态
console.log(store.getState());

// 每次 state 更新时，打印日志
// 注意 subscribe() 返回一个函数用来注销监听器
let unsubscribe = store.subscribe(() =>
    console.log(store.getState())
);

// 发起一系列 action
store.dispatch(increment());
store.dispatch(decrement());
store.dispatch(reset());

// 停止监听 state 更新
unsubscribe();
```

当前文件夹执行命令

```
webpack testRedux.js build.js

node build.js
```

是不是看到输出了`state`变化？

```
{ counter: { count: 0 } }
{ counter: { count: 1 } }
{ counter: { count: 0 } }
{ counter: { count: 0 } }
```

做这个测试，就是为了告诉大家，`redux`和`react`没关系，虽说他俩能合作。

到这里，我建议你再理下`redux`的数据流，看看[这里](http://cn.redux.js.org/docs/basics/DataFlow.html)。

1. 调用`store.dispatch(action)`提交`action`。
2. `redux store`调用传入的`reducer`函数。把当前的`state`和`action`传进去。
3. 根 `reducer` 应该把多个子 `reducer` 输出合并成一个单一的 `state` 树。
4. `Redux store` 保存了根 `reducer` 返回的完整 `state` 树。

就是酱紫~~

这会`webpack.dev.config.js`路径别名增加一下，后面好写了。

```
webpack.dev.config.js
          alias: {
              ...
              actions: path.join(__dirname, 'src/redux/actions'),
              reducers: path.join(__dirname, 'src/redux/reducers'),
              redux: path.join(__dirname, 'src/redux')
          }
  
```

把前面的相对路径都改改。

下面我们开始搭配`react`使用。

写一个`Counter`页面

```
cd src/pages
mkdir Counter
touch Counter/Counter.js
src/pages/Counter/Counter.js
import React, {Component} from 'react';

export default class Counter extends Component {
    render() {
        return (
            <div>
                <div>当前计数为(显示redux计数)</div>
                <button onClick={() => {
                    console.log('调用自增函数');
                }}>自增
                </button>
                <button onClick={() => {
                    console.log('调用自减函数');
                }}>自减
                </button>
                <button onClick={() => {
                    console.log('调用重置函数');
                }}>重置
                </button>
            </div>
        )
    }
}
```

修改路由，增加`Counter`

```
src/router/router.js
import React from 'react';

import {BrowserRouter as Router, Route, Switch, Link} from 'react-router-dom';

import Home from 'pages/Home/Home';
import Page1 from 'pages/Page1/Page1';
import Counter from 'pages/Counter/Counter';

const getRouter = () => (
    <Router>
        <div>
            <ul>
                <li><Link to="/">首页</Link></li>
                <li><Link to="/page1">Page1</Link></li>
                <li><Link to="/counter">Counter</Link></li>
            </ul>
            <Switch>
                <Route exact path="/" component={Home}/>
                <Route path="/page1" component={Page1}/>
                <Route path="/counter" component={Counter}/>
            </Switch>
        </div>
    </Router>
);

export default getRouter;
```

`npm start`看看效果。

下一步，我们让`Counter`组件和`Redux`联合起来。使`Counter`能获得到`Redux`的`state`，并且能发射`action`。

当然我们可以使用刚才测试`testRedux`的方法，手动监听~手动引入`store`~但是这肯定很麻烦哦。

`react-redux`提供了一个方法`connect`。

> 容器组件就是使用 store.subscribe() 从 Redux state 树中读取部分数据，并通过 props 来把这些数据提供给要渲染的组件。你可以手工来开发容器组件，但建议使用 React Redux 库的 connect() 方法来生成，这个方法做了性能优化来避免很多不必要的重复渲染。

`connect`接收两个参数，一个`mapStateToProps`,就是把`redux`的`state`，转为组件的`Props`，还有一个参数是`mapDispatchToprops`,
就是把发射`actions`的方法，转为`Props`属性函数。

先来安装`react-redux`

```
npm install --save react-redux
src/pages/Counter/Counter.js
import React, {Component} from 'react';
import {increment, decrement, reset} from 'actions/counter';

import {connect} from 'react-redux';

class Counter extends Component {
    render() {
        return (
            <div>
                <div>当前计数为{this.props.counter.count}</div>
                <button onClick={() => this.props.increment()}>自增
                </button>
                <button onClick={() => this.props.decrement()}>自减
                </button>
                <button onClick={() => this.props.reset()}>重置
                </button>
            </div>
        )
    }
}

const mapStateToProps = (state) => {
    return {
        counter: state.counter
    }
};

const mapDispatchToProps = (dispatch) => {
    return {
        increment: () => {
            dispatch(increment())
        },
        decrement: () => {
            dispatch(decrement())
        },
        reset: () => {
            dispatch(reset())
        }
    }
};

export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```

下面我们要传入`store`

> 所有容器组件都可以访问 Redux store，所以可以手动监听它。一种方式是把它以 props 的形式传入到所有容器组件中。但这太麻烦了，因为必须要用 store 把展示组件包裹一层，仅仅是因为恰好在组件树中渲染了一个容器组件。

> 建议的方式是使用指定的 React Redux 组件 来 魔法般的 让所有容器组件都可以访问 store，而不必显示地传递它。只需要在渲染根组件时使用即可。

```
src/index.js
import React from 'react';
import ReactDom from 'react-dom';
import {AppContainer} from 'react-hot-loader';
import {Provider} from 'react-redux';
import store from './redux/store';

import getRouter from 'router/router';

/*初始化*/
renderWithHotReload(getRouter());

/*热更新*/
if (module.hot) {
    module.hot.accept('./router/router', () => {
        const getRouter = require('router/router').default;
        renderWithHotReload(getRouter());
    });
}

function renderWithHotReload(RootElement) {
    ReactDom.render(
        <AppContainer>
            <Provider store={store}>
                {RootElement}
            </Provider>
        </AppContainer>,
        document.getElementById('app')
    )
}
```

到这里我们就可以执行`npm start`，打开localhost:8080/counter看效果了。

**但是你发现`npm start`一直报错**

```
ERROR in ./node_modules/react-redux/es/connect/mapDispatchToProps.js
Module not found: Error: Can't resolve 'redux' in 'F:\Project\react\react-family\node_modules\react-redux\es\connect'

ERROR in ./src/redux/store.js
Module not found: Error: Can't resolve 'redux' in 'F:\Project\react\react-family\src\redux'
```

WTF？这个错误困扰了半天。我说下为什么造成这个错误。我们引用`redux`的时候这样用的

```
import {createStore} from 'redux'
```

然而，我们在`webapck.dev.config.js`里面这样配置了

```
    resolve: {
        alias: {
            ...
            redux: path.join(__dirname, 'src/redux')
        }
    }
```

然后`webapck`编译的时候碰到`redux`都去`src/redux`去找了。但是找不到啊。所以我们把`webpack.dev.config.js`里面`redux`这一行删除了，就好了。
并且把使用我们自己使用`redux`文件夹的地方改成相对路径哦。

现在你可以`npm start`去看效果了。

这里我们再缕下（可以读[React 实践心得：react-redux 之 connect 方法详解](http://taobaofed.org/blog/2016/08/18/react-redux-connect/)）

1. `Provider`组件是让所有的组件可以访问到`store`。不用手动去传。也不用手动去监听。
2. `connect`函数作用是从 `Redux state` 树中读取部分数据，并通过 props 来把这些数据提供给要渲染的组件。也传递`dispatch(action)`函数到`props`。

接下来，我们要说异步`action`

参考地址： http://cn.redux.js.org/docs/advanced/AsyncActions.html

想象一下我们调用一个异步`get`请求去后台请求数据：

1. 请求开始的时候，界面转圈提示正在加载。`isLoading`置为`true`。
2. 请求成功，显示数据。`isLoading`置为`false`,`data`填充数据。
3. 请求失败，显示失败。`isLoading`置为`false`，显示错误信息。

下面，我们以向后台请求用户基本信息为例。

1. 我们先创建一个`user.json`，等会请求用，相当于后台的API接口。

```
cd dist
mkdir api
cd api
touch user.json
dist/api/user.json
{
  "name": "brickspert",
  "intro": "please give me a star"
}
```

1. 创建必须的`action`创建函数。

```
cd src/redux/actions
touch userInfo.js
src/redux/actions/userInfo.js
export const GET_USER_INFO_REQUEST = "userInfo/GET_USER_INFO_REQUEST";
export const GET_USER_INFO_SUCCESS = "userInfo/GET_USER_INFO_SUCCESS";
export const GET_USER_INFO_FAIL = "userInfo/GET_USER_INFO_FAIL";

function getUserInfoRequest() {
    return {
        type: GET_USER_INFO_REQUEST
    }
}

function getUserInfoSuccess(userInfo) {
    return {
        type: GET_USER_INFO_SUCCESS,
        userInfo: userInfo
    }
}

function getUserInfoFail() {
    return {
        type: GET_USER_INFO_FAIL
    }
}
```

我们创建了请求中，请求成功，请求失败三个`action`创建函数。

1. 创建`reducer`

再强调下，`reducer`是根据`state`和`action`生成新`state`的**纯函数**。

```
cd src/redux/reducers
touch userInfo.js
src/redux/reducers/userInfo.js
import {GET_USER_INFO_REQUEST, GET_USER_INFO_SUCCESS, GET_USER_INFO_FAIL} from 'actions/userInfo';


const initState = {
    isLoading: false,
    userInfo: {},
    errorMsg: ''
};

export default function reducer(state = initState, action) {
    switch (action.type) {
        case GET_USER_INFO_REQUEST:
            return {
                ...state,
                isLoading: true,
                userInfo: {},
                errorMsg: ''
            };
        case GET_USER_INFO_SUCCESS:
            return {
                ...state,
                isLoading: false,
                userInfo: action.userInfo,
                errorMsg: ''
            };
        case GET_USER_INFO_FAIL:
            return {
                ...state,
                isLoading: false,
                userInfo: {},
                errorMsg: '请求错误'
            };
        default:
            return state;
    }
}
```

**这里的`...state`语法，是和别人的`Object.assign()`起同一个作用，合并新旧state。我们这里是没效果的，但是我建议都写上这个哦**

组合`reducer`

```
src/redux/reducers.js
import counter from 'reducers/counter';
import userInfo from 'reducers/userInfo';

export default function combineReducers(state = {}, action) {
    return {
        counter: counter(state.counter, action),
        userInfo: userInfo(state.userInfo, action)
    }
}
```

1. 现在有了

   ```
   action
   ```

   ，有了

   ```
   reducer
   ```

   ，我们就需要调用把

   ```
   action
   ```

   里面的三个

   ```
   action
   ```

   函数和网络请求结合起来。

   - 请求中 `dispatch getUserInfoRequest`
   - 请求成功 `dispatch getUserInfoSuccess`
   - 请求失败 `dispatch getUserInfoFail`

`src/redux/actions/userInfo.js`增加

```
export function getUserInfo() {
    return function (dispatch) {
        dispatch(getUserInfoRequest());

        return fetch('http://localhost:8080/api/user.json')
            .then((response => {
                return response.json()
            }))
            .then((json) => {
                    dispatch(getUserInfoSuccess(json))
                }
            ).catch(
                () => {
                    dispatch(getUserInfoFail());
                }
            )
    }
}
```

我们这里发现，别的`action`创建函数都是返回`action`对象：

```
{type: xxxx}
```

但是我们现在的这个`action`创建函数 `getUserInfo`则是返回函数了。

为了让`action`创建函数除了返回`action`对象外，还可以返回函数，我们需要引用`redux-thunk`。

```
npm install --save redux-thunk
```

这里涉及到`redux`中间件`middleware`，我后面会讲到的。你也可以读这里[Middleware](http://cn.redux.js.org/docs/advanced/Middleware.html)。

简单的说，中间件就是`action`在到达`reducer`，先经过中间件处理。我们之前知道`reducer`能处理的`action`只有这样的`{type:xxx}`，所以我们使用中间件来处理
函数形式的`action`，把他们转为标准的`action`给`reducer`。这是`redux-thunk`的作用。
使用`redux-thunk`中间件

我们来引入`redux-thunk`中间件

```
src/redux/store.js
import {createStore, applyMiddleware} from 'redux';
import thunkMiddleware from 'redux-thunk';
import combineReducers from './reducers.js';

let store = createStore(combineReducers, applyMiddleware(thunkMiddleware));

export default store;
```

到这里，`redux`这边OK了，我们来写个组件验证下。

```
cd src/pages
mkdir UserInfo
cd UserInfo
touch UserInfo.js
src/pages/UserInfo/UserInfo.js
import React, {Component} from 'react';
import {connect} from 'react-redux';
import {getUserInfo} from "actions/userInfo";

class UserInfo extends Component {

    render() {
        const {userInfo, isLoading, errorMsg} = this.props.userInfo;
        return (
            <div>
                {
                    isLoading ? '请求信息中......' :
                        (
                            errorMsg ? errorMsg :
                                <div>
                                    <p>用户信息：</p>
                                    <p>用户名：{userInfo.name}</p>
                                    <p>介绍：{userInfo.intro}</p>
                                </div>
                        )
                }
                <button onClick={() => this.props.getUserInfo()}>请求用户信息</button>
            </div>
        )
    }
}

export default connect((state) => ({userInfo: state.userInfo}), {getUserInfo})(UserInfo);
```

这里你可能发现`connect`参数写法不一样了，`mapStateToProps`函数用了`es6`简写，`mapDispatchToProps`用了`react-redux`提供的简单写法。

增加路由
`src/router/router.js`

```
import React from 'react';

import {BrowserRouter as Router, Route, Switch, Link} from 'react-router-dom';

import Home from 'pages/Home/Home';
import Page1 from 'pages/Page1/Page1';
import Counter from 'pages/Counter/Counter';
import UserInfo from 'pages/UserInfo/UserInfo';

const getRouter = () => (
    <Router>
        <div>
            <ul>
                <li><Link to="/">首页</Link></li>
                <li><Link to="/page1">Page1</Link></li>
                <li><Link to="/counter">Counter</Link></li>
                <li><Link to="/userinfo">UserInfo</Link></li>
            </ul>
            <Switch>
                <Route exact path="/" component={Home}/>
                <Route path="/page1" component={Page1}/>
                <Route path="/counter" component={Counter}/>
                <Route path="/userinfo" component={UserInfo}/>
            </Switch>
        </div>
    </Router>
);

export default getRouter;
```

现在你可以执行`npm start`去看效果啦！

[![redux](https://camo.githubusercontent.com/0c5933060e6e1752261350747052191228ee4990/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f72656475782e706e67)](https://camo.githubusercontent.com/0c5933060e6e1752261350747052191228ee4990/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f72656475782e706e67)

到这里`redux`集成基本告一段落了，后面我们还会有一些优化。

## combinReducers优化

`redux`提供了一个`combineReducers`函数来合并`reducer`，不用我们自己合并哦。写起来简单，但是意思和我们
自己写的`combinReducers`也是一样的。

```
src/redux/reducers.js
import {combineReducers} from "redux";

import counter from 'reducers/counter';
import userInfo from 'reducers/userInfo';


export default combineReducers({
    counter,
    userInfo
});
```

# devtool优化

现在我们发现一个问题，代码哪里写错了，浏览器报错只报在`build.js`第几行。

[![错误图片](https://camo.githubusercontent.com/f938c838265055b480e5a4902c0e6407b46c30f4/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f6572726f722d6275696c642e706e67)](https://camo.githubusercontent.com/f938c838265055b480e5a4902c0e6407b46c30f4/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f6572726f722d6275696c642e706e67)

这让我们分析错误无从下手。看[这里](https://doc.webpack-china.org/configuration/devtool)。

我们增加`webpack`配置`devtool`！

`webpack.dev.config.js`增加

```
devtool: 'inline-source-map'
```

这次看错误信息是不是提示的很详细了？

[![错误图片](https://camo.githubusercontent.com/af2786344ccdd42dc3892f41ffac682ae84a284b/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f6572726f722d6275696c642d322e706e67)](https://camo.githubusercontent.com/af2786344ccdd42dc3892f41ffac682ae84a284b/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f6572726f722d6275696c642d322e706e67)

同时，我们在`srouce`里面能看到我们写的代码，也能打断点调试哦~

[![错误图片](https://camo.githubusercontent.com/f28c34ecd0e1bae80d51fc0e14fc614c3681e327/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f736f757263652e706e67)](https://camo.githubusercontent.com/f28c34ecd0e1bae80d51fc0e14fc614c3681e327/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f736f757263652e706e67)

# 编译css

先说这里为什么不用`scss`，因为`Windows`使用`node-sass`，需要先安装[ Microsoft Windows SDK for Windows 7 and .NET Framework 4](https://www.microsoft.com/en-us/download/details.aspx?id=8279)。
我怕有些人copy这份代码后，没注意，运行不起来。所以这里不用`scss`了，如果需要，自行编译哦。

```
npm install css-loader style-loader --save-dev
```

`css-loader`使你能够使用类似`@import` 和 `url(...)`的方法实现 `require()`的功能；

`style-loader`将所有的计算后的样式加入页面中； 二者组合在一起使你能够把样式表嵌入`webpack`打包后的JS文件中。

`webpack.dev.config.js` `rules`增加

```
{
   test: /\.css$/,
   use: ['style-loader', 'css-loader']
}
```

我们用`Page1`页面来测试下

```
cd src/pages/Page1
touch Page1.css
src/pages/Page1/Page1.css
.page-box {
    border: 1px solid red;
}
src/pages/Page1/Page1.js
import React, {Component} from 'react';

import './Page1.css';

export default class Page1 extends Component {
    render() {
        return (
            <div className="page-box">
                this is page1~
            </div>
        )
    }
}
```

好了，现在`npm start`去看效果吧。

# 编译图片

```
npm install --save-dev url-loader file-loader
```

`webpack.dev.config.js` `rules`增加

```
{
    test: /\.(png|jpg|gif)$/,
    use: [{
        loader: 'url-loader',
        options: {
            limit: 8192
        }
    }]
}
```

`options limit 8192`意思是，小于等于8K的图片会被转成`base64`编码，直接插入HTML中，减少`HTTP`请求。

我们来用`Page1` 测试下

```
cd src/pages/Page1
mkdir images
```

给`images`文件夹放一个图片。

修改代码，引用图片

```
src/pages/Page1/Page1.js
import React, {Component} from 'react';

import './Page1.css';

import image from './images/brickpsert.jpg';

export default class Page1 extends Component {
    render() {
        return (
            <div className="page-box">
                this is page1~
                <img src={image}/>
            </div>
        )
    }
}
```

可以去看看效果啦。

# 按需加载

为什么要实现按需加载？

我们现在看到，打包完后，所有页面只生成了一个`build.js`,当我们首屏加载的时候，就会很慢。因为他也下载了别的页面的`js`了哦。

如果每个页面都打包了自己单独的JS，在进入自己页面的时候才加载对应的js，那首屏加载就会快很多哦。

在 `react-router 2.0`时代， 按需加载需要用到的最关键的一个函数，就是`require.ensure()`，它是按需加载能够实现的核心。

在4.0版本，官方放弃了这种处理按需加载的方式，选择了一个更加简洁的处理方式。

[传送门](https://reacttraining.com/react-router/web/guides/code-splitting)

根据官方示例，我们开搞

1. `npm install bundle-loader --save-dev`
2. 新建`bundle.js`

```
cd src/router
touch Bundle.js
src/router/Bundle.js
import React, {Component} from 'react'

class Bundle extends Component {
    state = {
        // short for "module" but that's a keyword in js, so "mod"
        mod: null
    };

    componentWillMount() {
        this.load(this.props)
    }

    componentWillReceiveProps(nextProps) {
        if (nextProps.load !== this.props.load) {
            this.load(nextProps)
        }
    }

    load(props) {
        this.setState({
            mod: null
        });
        props.load((mod) => {
            this.setState({
                // handle both es imports and cjs
                mod: mod.default ? mod.default : mod
            })
        })
    }

    render() {
        return this.props.children(this.state.mod)
    }
}

export default Bundle;
```

1. 改造路由器

```
src/router/router.js
import React from 'react';

import {BrowserRouter as Router, Route, Switch, Link} from 'react-router-dom';

import Bundle from './Bundle';

import Home from 'bundle-loader?lazy&name=home!pages/Home/Home';
import Page1 from 'bundle-loader?lazy&name=page1!pages/Page1/Page1';
import Counter from 'bundle-loader?lazy&name=counter!pages/Counter/Counter';
import UserInfo from 'bundle-loader?lazy&name=userInfo!pages/UserInfo/UserInfo';

const Loading = function () {
    return <div>Loading...</div>
};

const createComponent = (component) => (props) => (
    <Bundle load={component}>
        {
            (Component) => Component ? <Component {...props} /> : <Loading/>
        }
    </Bundle>
);

const getRouter = () => (
    <Router>
        <div>
            <ul>
                <li><Link to="/">首页</Link></li>
                <li><Link to="/page1">Page1</Link></li>
                <li><Link to="/counter">Counter</Link></li>
                <li><Link to="/userinfo">UserInfo</Link></li>
            </ul>
            <Switch>
                <Route exact path="/" component={createComponent(Home)}/>
                <Route path="/page1" component={createComponent(Page1)}/>
                <Route path="/counter" component={createComponent(Counter)}/>
                <Route path="/userinfo" component={createComponent(UserInfo)}/>
            </Switch>
        </div>
    </Router>
);

export default getRouter;
```

现在你可以`npm start`，打开浏览器，看是不是进入新的页面，都会加载自己的JS的~

但是你可能发现，名字都是`0.bundle.js`这样子的，这分不清楚是哪个页面的`js`呀！

我们修改下`webpack.dev.config.js`,加个`chunkFilename`。`chunkFilename`是除了`entry`定义的入口`js`之外的`js`~

```
    output: {
        path: path.join(__dirname, './dist'),
        filename: 'bundle.js',
        chunkFilename: '[name].js'
    }
```

现在你运行发现名字变成`home.js`,这样的了。棒棒哒！

那么问题来了`home`是在哪里设置的？`webpack`怎么知道他叫`home`？

其实在这里我们定义了，`router.js`里面

```
import Home from 'bundle-loader?lazy&name=home!pages/Home/Home';
```

看到没。这里有个`name=home`。嘿嘿。

参考地址：

1. http://www.jianshu.com/p/8dd98a7028e0
2. https://github.com/ReactTraining/react-router/blob/master/packages/react-router-dom/docs/guides/code-splitting.md
3. https://segmentfault.com/a/1190000007949841
4. http://react-china.org/t/webpack-react-router/10123
5. https://juejin.im/post/58f9717e44d9040069d06cd6

# 缓存

想象一下这个场景~

我们网站上线了，用户第一次访问首页，下载了`home.js`，第二次访问又下载了`home.js`~

这肯定不行呀，所以我们一般都会做一个缓存，用户下载一次`home.js`后，第二次就不下载了。

有一天，我们更新了`home.js`，但是用户不知道呀，用户还是使用本地旧的`home.js`。出问题了~

怎么解决？每次代码更新后，打包生成的名字不一样。比如第一次叫`home.a.js`，第二次叫`home.b.js`。

文档[看这里](https://doc.webpack-china.org/guides/caching)

我们照着文档来

```
webpack.dev.config.js
    output: {
        path: path.join(__dirname, './dist'),
        filename: '[name].[hash].js',
        chunkFilename: '[name].[chunkhash].js'
    }
```

每次打包都用增加`hash`~

现在我们试试，是不是修改了文件，打包后相应的文件名字就变啦？

[![package](https://camo.githubusercontent.com/e9c041242d52d16bc841253126000ef029ebfd99/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f7061636b6167652e706e67)](https://camo.githubusercontent.com/e9c041242d52d16bc841253126000ef029ebfd99/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f7061636b6167652e706e67)

但是你可能发现了，网页打开报错了~因为你`dist/index.html`里面引用`js`名字还是`bundle.js`老名字啊,改成新的名字就可以啦。

啊~那岂不是我每次编译打包，都得去改一下js名字？欲知后事如何，且看下节分享。

# HtmlWebpackPlugin

这个插件，每次会自动把js插入到你的模板`index.html`里面去。

```
npm install html-webpack-plugin --save-dev
```

新建模板`index.html`

```
cd src
touch index.html
src/index.html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
<div id="app"></div>
</body>
</html>
```

修改`webpack.dev.config.js`，增加`plugin`

```
var HtmlWebpackPlugin = require('html-webpack-plugin');

    plugins: [new HtmlWebpackPlugin({
        filename: 'index.html',
        template: path.join(__dirname, 'src/index.html')
    })],
```

`npm start`运行项目，看看是不是能正常访问啦。~

说明一下：`npm start`打包后的文件存在内存中，你看不到的。~ 你可以把遗留`dist/index.html`删除掉了。

# 提取公共代码

想象一下，我们的主文件，原来的`bundle.js`里面是不是包含了`react`,`redux`,`react-router`等等
这些代码？？这些代码基本上不会改变的。但是，他们合并在`bundle.js`里面，每次项目发布，重新请求`bundle.js`的时候，相当于重新请求了
`react`等这些公共库。浪费了~

我们把`react`这些不会改变的公共库提取出来，用户缓存下来。从此以后，用户再也不用下载这些库了，无论是否发布项目。嘻嘻。

`webpack`文档给了教程，[看这里](https://doc.webpack-china.org/guides/caching#-extracting-boilerplate-)

```
webpack.dev.config.js
    var webpack = require('webpack');

    entry: {
        app: [
            'react-hot-loader/patch',
            path.join(__dirname, 'src/index.js')
        ],
        vendor: ['react', 'react-router-dom', 'redux', 'react-dom', 'react-redux']
    }
    
        /*plugins*/
        new webpack.optimize.CommonsChunkPlugin({
            name: 'vendor'
        })
```

把`react`等库生成打包到`vendor.hash.js`里面去。

但是你现在可能发现编译生成的文件`app.[hash].js`和`vendor.[hash].js`生成的`hash`一样的，这里是个问题，因为呀，你每次修改代码,都会导致`vendor.[hash].js`名字改变，那我们提取出来的意义也就没了。其实文档上写的很清楚，

```
   output: {
        path: path.join(__dirname, './dist'),
        filename: '[name].[hash].js', //这里应该用chunkhash替换hash
        chunkFilename: '[name].[chunkhash].js'
    }
```

但是无奈，如果用`chunkhash`，会报错。和`webpack-dev-server --hot`不兼容，具体[看这里](https://github.com/webpack/webpack-dev-server/issues/377)。

现在我们在配置开发版配置文件，就向`webpack-dev-server`妥协，因为我们要用他。问题先放这里，等会我们配置正式版`webpack.config.js`的时候要解决这个问题。

# 生产坏境构建

> 开发环境(development)和生产环境(production)的构建目标差异很大。在开发环境中，我们需要具有强大的、具有实时重新加载(live reloading)或热模块替换(hot module replacement)能力的 source map 和 localhost server。而在生产环境中，我们的目标则转向于关注更小的 bundle，更轻量的 source map，以及更优化的资源，以改善加载时间。由于要遵循逻辑分离，我们通常建议为每个环境编写彼此独立的 webpack 配置。

文档[看这里](https://doc.webpack-china.org/guides/production)

我们要开始做了~

```
touch webpack.config.js
```

在`webpack.dev.config.js`的基础上先做以下几个修改~

1. 先删除`webpack-dev-server`相关的东西~
2. `devtool`的值改成`cheap-module-source-map`
3. 刚才说的`hash`改成`chunkhash`

```
webpack.config.js
const path = require('path');
var HtmlWebpackPlugin = require('html-webpack-plugin');
var webpack = require('webpack');

module.exports = {
    devtool: 'cheap-module-source-map',
    entry: {
        app: [
            path.join(__dirname, 'src/index.js')
        ],
        vendor: ['react', 'react-router-dom', 'redux', 'react-dom', 'react-redux']
    },
    output: {
        path: path.join(__dirname, './dist'),
        filename: '[name].[chunkhash].js',
        chunkFilename: '[name].[chunkhash].js'
    },
    module: {
        rules: [{
            test: /\.js$/,
            use: ['babel-loader'],
            include: path.join(__dirname, 'src')
        }, {
            test: /\.css$/,
            use: ['style-loader', 'css-loader']
        }, {
            test: /\.(png|jpg|gif)$/,
            use: [{
                loader: 'url-loader',
                options: {
                    limit: 8192
                }
            }]
        }]
    },
    plugins: [
        new HtmlWebpackPlugin({
            filename: 'index.html',
            template: path.join(__dirname, 'src/index.html')
        }),
        new webpack.optimize.CommonsChunkPlugin({
            name: 'vendor'
        })
    ],

    resolve: {
        alias: {
            pages: path.join(__dirname, 'src/pages'),
            component: path.join(__dirname, 'src/component'),
            router: path.join(__dirname, 'src/router'),
            actions: path.join(__dirname, 'src/redux/actions'),
            reducers: path.join(__dirname, 'src/redux/reducers')
        }
    }
};
```

在`package.json`增加打包脚本

```
"build":"webpack --config webpack.config.js"
```

然后执行`npm run build`~看看`dist`文件夹是不是生成了我们发布要用的所有文件哦？

接下来我们还是要优化正式版配置文件~

# 文件压缩

`webpack`使用`UglifyJSPlugin`来压缩生成的文件。

```
npm i --save-dev uglifyjs-webpack-plugin
webpack.config.js
const UglifyJSPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
  plugins: [
    new UglifyJSPlugin()
  ]
}
```

`npm run build`发现打包文件大小减小了好多。

[![uglify](https://camo.githubusercontent.com/b8635df066a19aba692872f69ffd11efefa70ff7/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f75676c6966792e706e67)](https://camo.githubusercontent.com/b8635df066a19aba692872f69ffd11efefa70ff7/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f75676c6966792e706e67)

# 指定环境

> 许多 library 将通过与 process.env.NODE_ENV 环境变量关联，以决定 library 中应该引用哪些内容。例如，当不处于生产环境中时，某些 library 为了使调试变得容易，可能会添加额外的日志记录(log)和测试(test)。其实，当使用 process.env.NODE_ENV === 'production' 时，一些 library 可能针对具体用户的环境进行代码优化，从而删除或添加一些重要代码。我们可以使用 webpack 内置的 DefinePlugin 为所有的依赖定义这个变量：

```
webpack.config.js
module.exports = {
  plugins: [
       new webpack.DefinePlugin({
          'process.env': {
              'NODE_ENV': JSON.stringify('production')
           }
       })
  ]
}
```

`npm run build`后发现`vendor.[hash].js`又变小了。

[![uglify](https://camo.githubusercontent.com/ee7c18ab0dd27e2bab82f5875309cd23ad44dba6/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f70726f647563742e706e67)](https://camo.githubusercontent.com/ee7c18ab0dd27e2bab82f5875309cd23ad44dba6/687474703a2f2f6f71636238646a6c772e626b742e636c6f7564646e2e636f6d2f70726f647563742e706e67)

# 优化缓存

刚才我们把`[name].[hash].js`变成`[name].[chunkhash].js`后，`npm run build`后，
发现`app.xxx.js`和`vendor.xxx.js`不一样了哦。

但是现在又有一个问题了。

你随便修改代码一处，例如`Home.js`，随便改变个字，你发现`home.xxx.js`名字变化的同时，
`vendor.xxx.js`名字也变了。这不行啊。这和没拆分不是一样一样了吗？我们本意是`vendor.xxx.js`
名字永久不变，一直缓存在用户本地的。~

[官方文档](https://doc.webpack-china.org/guides/caching)推荐了一个插件[HashedModuleIdsPlugin](https://doc.webpack-china.org/plugins/hashed-module-ids-plugin)

```
    plugins: [
        new webpack.HashedModuleIdsPlugin()
    ]
```

现在你打包，修改代码再试试，是不是名字不变啦？错了，现在打包，我发现名字还是变了，经过比对文档，我发现还要加一个`runtime`代码抽取，

```
new webpack.optimize.CommonsChunkPlugin({
    name: 'runtime'
})
```

加上这句话就好了~为什么呢？看下[解释](https://doc.webpack-china.org/concepts/manifest)。

**注意，引入顺序在这里很重要。CommonsChunkPlugin 的 'vendor' 实例，必须在 'runtime' 实例之前引入。**

# public path

想象一个场景，我们的静态文件放在了单独的静态服务器上去了，那我们打包的时候，如何让静态文件的链接定位到静态服务器呢？

看文档[Public Path](https://doc.webpack-china.org/guides/public-path)

`webpack.config.js` `output` 中增加一个`publicPath`，我们当前用`/`，相对于当前路径，如果你要改成别的`url`，就改这里就好了。

```
    output: {
        publicPath : '/'
    }
```

# 打包优化

你现在打开`dist`，是不是发现好多好多文件，每次打包后的文件在这里混合了？我们希望每次打包前自动清理下`dist`文件。

```
npm install clean-webpack-plugin --save-dev
webpack.config.js
const CleanWebpackPlugin = require('clean-webpack-plugin');


plugins: [
    new CleanWebpackPlugin(['dist'])
]
```

现在`npm run build`试试，是不是之前的都清空了。当然我们之前的`api`文件夹也被清空了，不过没关系哦~本来就是测试用的。

# 抽取css

目前我们的`css`是直接打包进`js`里面的，我们希望能单独生成`css`文件。

我们使用[extract-text-webpack-plugin](https://github.com/webpack-contrib/extract-text-webpack-plugin)来实现。

```
npm install --save-dev extract-text-webpack-plugin
webpack.config.js
const ExtractTextPlugin = require("extract-text-webpack-plugin");

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ExtractTextPlugin.extract({
          fallback: "style-loader",
          use: "css-loader"
        })
      }
    ]
  },
  plugins: [
     new ExtractTextPlugin({
         filename: '[name].[contenthash:5].css',
         allChunks: true
     })
  ]
}
```

`npm run build`后发现单独生成了`css`文件哦

# 使用`axios`和`middleware`优化API请求

先安装下[axios](https://github.com/mzabriskie/axios)

```
npm install --save axios
```

我们之前项目的一次API请求是这样写的哦~

`action`创建函数是这样的。比我们现在写的`fetch`简单多了。

```
export function getUserInfo() {
    return {
        types: [GET_USER_INFO_REQUEST, GET_USER_INFO_SUCCESS, GET_USER_INFO_FAIL],
        promise: client => client.get(`http://localhost:8080/api/user.json`)
        afterSuccess:(dispatch,getState,response)=>{
            /*请求成功后执行的函数*/
        },
        otherData:otherData
    }
}
```

然后在dispatch(getUserInfo())后，通过`redux`中间件来处理请求逻辑。

中间件的教程看[这里](http://cn.redux.js.org/docs/advanced/Middleware.html)

我们想想中间件的逻辑

1. 请求前`dispatch` `REQUEST`请求。
2. 成功后`dispatch` `SUCCESS`请求，如果定义了`afterSuccess()`函数，调用它。
3. 失败后`dispatch` `FAIL`请求。

来写一个

```
cd src/redux
mkdir middleware
cd middleware
touch promiseMiddleware.js
src/redux/middleware/promiseMiddleware.js
import axios from 'axios';

export default  store => next => action => {
    const {dispatch, getState} = store;
    /*如果dispatch来的是一个function，此处不做处理，直接进入下一级*/
    if (typeof action === 'function') {
        action(dispatch, getState);
        return;
    }
    /*解析action*/
    const {
        promise,
        types,
        afterSuccess,
        ...rest
    } = action;

    /*没有promise，证明不是想要发送ajax请求的，就直接进入下一步啦！*/
    if (!action.promise) {
        return next(action);
    }

    /*解析types*/
    const [REQUEST,
        SUCCESS,
        FAILURE] = types;

    /*开始请求的时候，发一个action*/
    next({
        ...rest,
        type: REQUEST
    });
    /*定义请求成功时的方法*/
    const onFulfilled = result => {
        next({
            ...rest,
            result,
            type: SUCCESS
        });
        if (afterSuccess) {
            afterSuccess(dispatch, getState, result);
        }
    };
    /*定义请求失败时的方法*/
    const onRejected = error => {
        next({
            ...rest,
            error,
            type: FAILURE
        });
    };

    return promise(axios).then(onFulfilled, onRejected).catch(error => {
        console.error('MIDDLEWARE ERROR:', error);
        onRejected(error)
    })
}
```

修改`src/redux/store.js`来应用这个中间件

```
import {createStore, applyMiddleware} from 'redux';
import combineReducers from './reducers.js';

import promiseMiddleware from './middleware/promiseMiddleware'

let store = createStore(combineReducers, applyMiddleware(promiseMiddleware));

export default store;
```

修改`src/redux/actions/userInfo.js`

```
export const GET_USER_INFO_REQUEST = "userInfo/GET_USER_INFO_REQUEST";
export const GET_USER_INFO_SUCCESS = "userInfo/GET_USER_INFO_SUCCESS";
export const GET_USER_INFO_FAIL = "userInfo/GET_USER_INFO_FAIL";

export function getUserInfo() {
    return {
        types: [GET_USER_INFO_REQUEST, GET_USER_INFO_SUCCESS, GET_USER_INFO_FAIL],
        promise: client => client.get(`http://localhost:8080/api/user.json`)
    }
}
```

是不是简单清新很多啦？

修改`src/redux/reducers/userInfo.js`

```
        case GET_USER_INFO_SUCCESS:
            return {
                ...state,
                isLoading: false,
                userInfo: action.result.data,
                errorMsg: ''
            };
```

`action.userInfo`修改成了`action.result.data`。你看中间件，请求成功，会给`action`增加一个`result`字段来存储响应结果哦~不用手动传了。

`npm start`看看我们的网络请求是不是正常哦。

# 调整文本编辑器

使用自动编译代码时，可能会在保存文件时遇到一些问题。某些编辑器具有“安全写入”功能，可能会影响重新编译。

要在一些常见的编辑器中禁用此功能，请查看以下列表：

- Sublime Text 3 - 在用户首选项(user preferences)中添加 atomic_save: "false"。
- IntelliJ - 在首选项(preferences)中使用搜索，查找到 "safe write" 并且禁用它。
- Vim - 在设置(settings)中增加 :set backupcopy=yes。
- WebStorm - 在 Preferences > Appearance & Behavior > System Settings 中取消选中 Use "safe write"。

# 合并提取`webpack`公共配置

想象一个场景，现在我想给`webpack`增加一个`css modules`依赖，你会发现，WTF?我即要修改`webpack.dev.config.js`，又要修改`webpack.config.js`~

这肯定不行啊。所以我们要把公共的配置文件提取出来。提取到`webpack.common.config.js`里面~

`webpack.dev.config.js`和`webpack.config.js`写自己的特殊的配置。

这里我们需要用到[webpack-merge](https://github.com/survivejs/webpack-merge)来合并公共配置和单独的配置。

这样说一下，应该看代码就能看懂了。下次公共配置直接就写在`webpack.common.config.js`里面啦。

> 这里偷偷说下，我修改了`CleanWebpackPlugin`的参数，不让他每次构建都删除`api`文件夹了。要不每次都得复制进去。麻烦~

```
npm install --save-dev webpack-merge
touch webpack.common.config.js
webpack.common.config.js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');

commonConfig = {
    entry: {
        app: [
            path.join(__dirname, 'src/index.js')
        ],
        vendor: ['react', 'react-router-dom', 'redux', 'react-dom', 'react-redux']
    },
    output: {
        path: path.join(__dirname, './dist'),
        filename: '[name].[chunkhash].js',
        chunkFilename: '[name].[chunkhash].js',
        publicPath: "/"
    },
    module: {
        rules: [{
            test: /\.js$/,
            use: ['babel-loader?cacheDirectory=true'],
            include: path.join(__dirname, 'src')
        }, {
            test: /\.(png|jpg|gif)$/,
            use: [{
                loader: 'url-loader',
                options: {
                    limit: 8192
                }
            }]
        }]
    },
    plugins: [
        new HtmlWebpackPlugin({
            filename: 'index.html',
            template: path.join(__dirname, 'src/index.html')
        }),
        new webpack.HashedModuleIdsPlugin(),
        new webpack.optimize.CommonsChunkPlugin({
            name: 'vendor'
        }),
        new webpack.optimize.CommonsChunkPlugin({
            name: 'runtime'
        })
    ],

    resolve: {
        alias: {
            pages: path.join(__dirname, 'src/pages'),
            components: path.join(__dirname, 'src/components'),
            router: path.join(__dirname, 'src/router'),
            actions: path.join(__dirname, 'src/redux/actions'),
            reducers: path.join(__dirname, 'src/redux/reducers')
        }
    }
};

module.exports = commonConfig;
webpack.dev.config.js
const merge = require('webpack-merge');
const path = require('path');

const commonConfig = require('./webpack.common.config.js');

const devConfig = {
    devtool: 'inline-source-map',
    entry: {
        app: [
            'react-hot-loader/patch',
            path.join(__dirname, 'src/index.js')
        ]
    },
    output: {
        /*这里本来应该是[chunkhash]的，但是由于[chunkhash]和react-hot-loader不兼容。只能妥协*/
        filename: '[name].[hash].js'
    },
    module: {
        rules: [{
            test: /\.css$/,
            use: ["style-loader", "css-loader"]
        }]
    },
    devServer: {
        contentBase: path.join(__dirname, './dist'),
        historyApiFallback: true,
        host: '0.0.0.0',
    }
};

module.exports = merge({
    customizeArray(a, b, key) {
        /*entry.app不合并，全替换*/
        if (key === 'entry.app') {
            return b;
        }
        return undefined;
    }
})(commonConfig, devConfig);
webpack.config.js
const merge = require('webpack-merge');

const webpack = require('webpack');
const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const ExtractTextPlugin = require("extract-text-webpack-plugin");

const commonConfig = require('./webpack.common.config.js');

const publicConfig = {
    devtool: 'cheap-module-source-map',
    module: {
        rules: [{
            test: /\.css$/,
            use: ExtractTextPlugin.extract({
                fallback: "style-loader",
                use: "css-loader"
            })
        }]
    },
    plugins: [
        new CleanWebpackPlugin(['dist/*.*']),
        new UglifyJSPlugin(),
        new webpack.DefinePlugin({
            'process.env': {
                'NODE_ENV': JSON.stringify('production')
            }
        }),
        new ExtractTextPlugin({
            filename: '[name].[contenthash:5].css',
            allChunks: true
        })
    ]

};

module.exports = merge(commonConfig, publicConfig);
```

# 优化目录结构并增加404页面

现在我们优化下目录结构，把`router`和`nav`分开，新建根组件`App`。

1. `component`改名为`components`,因为是复数。。。注意修改引用的地方哦。
2. 新建根组件`components/App/APP.js`

```
import React, {Component} from 'react';

import Nav from 'components/Nav/Nav';
import getRouter from 'router/router';

export default class App extends Component {
    render() {
        return (
            <div>
                <Nav/>
                {getRouter()}
            </div>
        )
    }
}
```

1. 新建`components/Nav/Nav`组件，把`router/router.js`里面的`nav`提出来。
2. 新建`components/Loading/Loading`组件,把`router/router.js`里面的`Loading`提出来。
3. 入口文件`src/index.js`修改

```
import React from 'react';
import ReactDom from 'react-dom';
import {AppContainer} from 'react-hot-loader';
import {Provider} from 'react-redux';
import store from './redux/store';
import {BrowserRouter as Router} from 'react-router-dom';
import App from 'components/App/App';

renderWithHotReload(App);

if (module.hot) {
    module.hot.accept('components/App/App', () => {
        const NextApp = require('components/App/App').default;
        renderWithHotReload(NextApp);
    });
}

function renderWithHotReload(RootElement) {
    ReactDom.render(
        <AppContainer>
            <Provider store={store}>
                <Router>
                    <RootElement/>
                </Router>
            </Provider>
        </AppContainer>,
        document.getElementById('app')
    )
}
```

1. 新建`pages/NotFound/NotFound`组件。
2. 修改`router/router.js`，增加`404`

```
import NotFound from 'bundle-loader?lazy&name=notFound!pages/NotFound/NotFound';

<Route component={createComponent(NotFound)}/>
```

# 加入 babel-plugin-transform-runtime 和 babel-polyfill

1. 先来说说[babel-plugin-transform-runtime](https://github.com/babel/babel/tree/master/packages/babel-plugin-transform-runtime)

   > 在转换 ES2015 语法为 ECMAScript 5 的语法时，babel 会需要一些辅助函数，例如 _extend。babel 默认会将这些辅助函数内联到每一个 js 文件里，这样文件多的时候，项目就会很大。

   > 所以 babel 提供了 transform-runtime 来将这些辅助函数“搬”到一个单独的模块 babel-runtime 中，这样做能减小项目文件的大小。

   `npm install --save-dev babel-plugin-transform-runtime`

```
修改`.babelrc`配置文件,增加配置

.babelrc

​```javascript
     "plugins": [
       "transform-runtime"
     ]
​```
```

1. 再来看[babel-polyfill](https://babeljs.io/docs/usage/polyfill/)

   Q: 为什么要集成`babel-polyfill`?

   A:

   > Babel默认只转换新的JavaScript句法（syntax），而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。
   > 举例来说，ES6在Array对象上新增了Array.from方法。Babel就不会转码这个方法。如果想让这个方法运行，必须使用babel-polyfill，为当前环境提供一个垫片。

   网上很多人说，集成了`transform-runtime`就不用`babel-polyfill`了，其实不然，看看官方怎么说的：

   > NOTE: Instance methods such as "foobar".includes("foo") will not work since that would require modification of existing built-ins (Use babel-polyfill for that).

   所以，我们还是需要`babel-polyfill`哦。

   `npm install --save-dev babel-polyfill`

   修改webpack两个配置文件。

   `webpack.common.config.js`

   ```
        app: [
            "babel-polyfill",
            path.join(__dirname, 'src/index.js')
        ]
   ```

   `webpack.dev.config.js`

   ```
        app: [
            'babel-polyfill',
            'react-hot-loader/patch',
            path.join(__dirname, 'src/index.js')
        ]
   ```

参考地址：

1. http://www.ruanyifeng.com/blog/2016/01/babel.html
2. [lmk123/blog#45](https://github.com/lmk123/blog/issues/45)
3. https://github.com/thejameskyle/babel-handbook/blob/master/translations/zh-Hans/README.md

# 集成PostCSS

[官方文档看这里](https://github.com/postcss/postcss)

Q: 这是啥？为什么要用它？

他有很多很多的插件，我们举几个例子~

[Autoprefixer](https://github.com/postcss/autoprefixer)这个插件,可以自动给css属性加浏览器前缀。

```
/*编译前*/
.container{
    display: flex;
}
/*编译后*/
.container{
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
}
```

[postcss-cssnext](http://cssnext.io/) 允许你使用未来的 CSS 特性（包括 autoprefixer）

当然，它有很多很多的插件可以用，你可以去官网详细了解。我们今天只用`postcss-cssnext`。（它包含了autoprefixer）

```
npm install --save-dev  postcss-loader
npm install --save-dev  postcss-cssnext
```

修改`webpack`配置文件,增加`postcss-loader`

webpack.dev.config.js

```
        rules: [{
            test: /\.(css|scss)$/,
            use: ["style-loader", "css-loader", "postcss-loader"]
        }]
```

webpack.config.js

```
        rules: [{
            test: /\.css$/,
            use: ExtractTextPlugin.extract({
                fallback: "style-loader",
                use: ["css-loader", "postcss-loader"]
            })
        }]
```

根目录增加`postcss`配置文件。

```
touch postcss.config.js
postcss.config.js
module.exports = {
    plugins: {
        'postcss-cssnext': {}
    }
};
```

现在你运行代码，然后写个css，去浏览器审查元素，看看，属性是不是生成了浏览器前缀？

# redux 模块热替换配置

今天突然发现，当修改reducer代码的时候，页面会整个刷新，而不是局部刷新唉。

这不行，就去查了webpack文档，果然是要配置的。[看这里](https://survivejs.com/webpack/appendices/hmr-with-react/#configuring-hmr-with-redux)

代码修改起来也简单,增加一段监听reducers变化，并替换的代码。

```
src/redux/store.js
if (module.hot) {
    module.hot.accept("./reducers", () => {
        const nextCombineReducers = require("./reducers").default;
        store.replaceReducer(nextCombineReducers);
    });
}
```

哦了~

# 模拟AJAX数据之Mock.js

每个改进都是为了解决问题。

现在我在开发中碰到了问题，我先描述下问题：

我们现在做前后端完全分离的应用，前端写前端的，后端写后端的，他们通过API接口连接。

前端同学心理路程："后端同学接口写的好慢，我都没法调试了。"

是不是有这个问题呢？一般我们怎么解决？

第一种：自己这边随便造点数据，等后端接口写好了之后，再小修改,再调试。

第二种：想想我们之前获得用户信息的`dist/api/user.json`，我们可以用这种方式来调试。
但是想象下，我们要模拟一个文章列表，就要手动写几十列。oh~no!

并且，后端接口一般都不带`.json`，到时候对接，是不是还得改代码？

好了，下面介绍下今天的主角[Mock.js](http://mockjs.com/)。

他会做一件事情：**拦截AJAX请求，返回需要的数据！**

我们写AJAX请求的时候，正常写，Mock.js会自动拦截的。

Mock.js提供各种随机生成数据。具体可以去官网看~

下面我们就在项目中集成咯：

1. `npm install mockjs --save-dev`

2. 新建mock文件夹

   `touch mock`

3. 模拟一个我们之前用到的`/api/user`接口

   ```
    cd mock
    touch mock.js
   ```

   `mock/mock.js`

   ```
    import Mock from 'mockjs';
    
    let Random = Mock.Random;
    
    Mock.mock('/api/user', {
        'name': '@cname',
        'intro': '@word(20)'
    });
   ```

   上面代码的意思就是，拦截`/api/user`，返回随机的一个中文名字，一个20个字母的字符串。

   我知道你看不懂，你去看看[Mock.js](http://mockjs.com/)文档就能看懂啦！

4. 与我们的项目连接。到目前为止，刚才定义的接口和我们的项目还没有关系。

   先来做，在`src/index.js`里面增加一行代码：

   `src/index.js`

   ```
   import '../mock/mock';
   ```

5. 现在我们删除`dist/api`文件夹，然后修改之间的接口路径，把`.json`去掉。

   `rm -rf dist/api`

   `src/redux/actions/userInfo.js`

   ```
   /*promise: client => client.get(`/api/user.json`)*/
   
   promise: client => client.get(`/api/user`)
   ```

   现在我们运行`npm start`，到获取用户信息界面，看每次获取用户信息都会变化呀？

到这里还没完，我们还要配置：只有在开发坏境下，才引入`mock`，在生产坏境，不引入。

跟着我做：

先给`mock`文件夹加个别名，这个我就不单独介绍了：

```
webpack.common.config.js
    resolve: {
        alias: {
            ...
            mock: path.join(__dirname, 'mock')
        }
    }
```

`webpack.dev.config.js`增加

```
   const webpack = require('webpack');


   plugins:[
        new webpack.DefinePlugin({
               MOCK: true
        })
    ]
```

然后修改`src/index.js`刚才加的那句话为下面这样

```
if (MOCK) {
    require('mock/mock');
}
```

这样，就只会在`npm start` 开发模式下，才会应用mock，如果你不想用，就把MOCK改成false就好了。

哦了，到这里就结束了~回头缕下：

我们定义了mock，在index.js引入。

mock的工作就是，拦截AJAX请求，返回模拟数据。

参考文章：

http://www.jianshu.com/p/dd23a6547114

https://segmentfault.com/a/1190000005793320

# 使用 CSS Modules

关于什么是`CSS Modules`，我这里不介绍。

可以去看阮一峰的文章[CSS Modules 用法教程](http://www.ruanyifeng.com/blog/2016/06/css_modules.html)

修改以下几个地方：

1. `webpack.dev.config.js`

   ```
   module: {
           rules: [{
               test: /\.css$/,
               use: ["style-loader", "css-loader?modules&localIdentName=[local]-[hash:base64:5]", "postcss-loader"]
           }]
       }
   ```

2. `webpack.config.js`

   ```
   module: {
       rules: [{
           test: /\.css$/,
           use: ExtractTextPlugin.extract({
               fallback: "style-loader",
               use: ["css-loader?modules&localIdentName=[local]-[hash:base64:5]", "postcss-loader"]
           })
       }]
   }
   ```

3. `src/pages/Page1/page1.css`

   ```
   .box {
       border: 1px solid red;
   }
   ```

4. `src/pages/Page1/Page1.js`

   ```
   import React, {Component} from 'react';
   
   import style from './Page1.css';
   
   import image from './images/brickpsert.jpg';
   
   export default class Page1 extends Component {
       render() {
           return (
               <div className={style.box}>
                   this is page1~
                   <img src={image}/>
               </div>
           )
       }
   }
   ```

enjoy it!

# 使用 json-server 代替 Mock.js

[json-server](https://github.com/typicode/json-server)和`Mock.js`一样，都是用来模拟接口数据的。

`json-server`功能更强大，支持分页，排序，筛选等等，具体的可以去看[文档](https://github.com/typicode/json-server)。

我们用`json-server`代替之前的`Mock.js`。

1. 删除`Mock.js`相关代码。

   一共两处，`webpack.dev.config.js`,`src/index.js`

2. `npm install --save-dev json-server`

3. 写个demo，我们生成虚假数据还是用`mockjs`。

```
mock/mock.js
let Mock = require('mockjs');

var Random = Mock.Random;

module.exports = function () {
    var data = {};
    data.user = {
        'name': Random.cname(),
        'intro': Random.word(20)
    };
    return data;
};
```

1. 设置启动脚本

```
package.json
"mock": "json-server mock/mock.js --watch --port 8090",
"mockdev": "npm run mock & npm start"
```

1. webpack.dev.config.js 增加个代理，把我们的API请求，代理到`json-server`服务器去。

```
   devServer: {
        ...
        proxy: {
                    "/api/*": "http://localhost:8090/$1"
                }
    }
```

哦了，你可以`npm run mockdev`启动项目，然后访问我们之前的用户信息接口，试试啦。

**问题：`windows`不支持命令并行执行`&`，你可以分开执行，或者使用[npm-run-all](https://www.npmjs.com/package/npm-run-all)**