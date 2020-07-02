# Node.js 第1天

## 上午总结

- Node.js 是什么
  + JavaScript 运行时
  + 既不是语言，也不是框架，它是一个平台
- Node.js 中的 JavaScript
  + 没有 BOM、DOM
  + EcmaScript 基本的 JavaScript 语言部分
  + 在 Node 中为 JavaScript 提供了一些服务器级别的 API
    * 文件操作的能力
    * http 服务的能力

## 总结

- Node 中的 JavaScript
  + EcmaScript
    * 变量
    * 方法
    * 数据类型
    * 内置对象
    * Array
    * Object
    * Date
    * Math
  + 模块系统
    * 在 Node 中没有全局作用域的概念
    * 在 Node 中，只能通过 require 方法来加载执行多个 JavaScript 脚本文件
    * require 加载只能是执行其中的代码，文件与文件之间由于是模块作用域，所以不会有污染的问题
      - 模块完全是封闭的
      - 外部无法访问内部
      - 内部也无法访问外部
    * 模块作用域固然带来了一些好处，可以加载执行多个文件，可以完全避免变量命名冲突污染的问题
    * 但是某些情况下，模块与模块是需要进行通信的
    * 在每个模块中，都提供了一个对象：`exports`
    * 该对象默认是一个空对象
    * 你要做的就是把需要被外部访问使用的成员手动的挂载到 `exports` 接口对象中
    * 然后谁来 `require` 这个模块，谁就可以得到模块内部的 `exports` 接口对象
    * 还有其它的一些规则，具体后面讲，以及如何在项目中去使用这种编程方式，会通过后面的案例来处理
  + 核心模块
    * 核心模块是由 Node 提供的一个个的具名的模块，它们都有自己特殊的名称标识，例如
      - fs 文件操作模块
      - http 网络服务构建模块
      - os 操作系统信息模块
      - path 路径处理模块
      - 。。。。
    * 所有核心模块在使用的时候都必须手动的先使用 `require` 方法来加载，然后才可以使用，例如：
      - `var fs = require('fs')`
- http
  + require
  + 端口号
    * ip 地址定位计算机
    * 端口号定位具体的应用程序
  + Content-Type
    * 服务器最好把每次响应的数据是什么内容类型都告诉客户端，而且要正确的告诉
    * 不同的资源对应的 Content-Type 是不一样，具体参照：http://tool.oschina.net/commons
    * 对于文本类型的数据，最好都加上编码，目的是为了防止中文解析乱码问题
  + 通过网络发送文件
    * 发送的并不是文件，本质上来讲发送是文件的内容
    * 当浏览器收到服务器响应内容之后，就会根据你的 Content-Type 进行对应的解析处理

- 模块系统
- Node 中的其它的核心模块
- 做一个小管理系统：
  + CRUD
- Express Web 开发框架
  + `npm install express`

# Node.js 第2天课堂笔记

## 知识点

## 反馈

- 老师像我一同学 但我知道 我同学没这么牛逼。。。
  + 学习、分享、交流
- 老师 讲讲sumblie需要安装哪些插件把 以及怎么用Md结尾的文档，对于我们来说好像就是一个阅读器一样使用………………
  + HTML 也是标记语言
  + markdown 标记语言
  + `#` 就是标题
  + `-`、`*` 就是列表
  + `**加粗内容**`
  + `GFM`
- 第一天上课 给我感觉挺好的 就是老师可能对早下课是不是有什么误解 我们平时都是 五点半放学的 ，还有就是有点啰嗦，那个不能起a呀b呀我感觉听了不下 五遍 我都要被你笑哭了
- 老师讲的挺好的
- 后来讲的速度有点上来了！
- 老师很耐心！
- 老师讲的课很好！
- 感觉老师讲的有点快，语速快
- 没有对比就没有伤害 体验了一把 普通话标准 英语发音又标准 幸福感
- 感觉老师讲课的风格简单利落，思路清晰。 nice
- 结尾不写分号是老师写的es6的代码风格，还是老师懒得写？

### 代码风格

```javascript
var foo = 'bar'
var foo ='bar'
var foo= 'bar'
var foo = "bar"

if (true) {
  console.log('hello') 
}

if (true) {
    console.log('hello') 
}

if (true ){
      console.log('hello') 
}
```

为了约定大家的代码风格，所以在社区中诞生了一些比较规范的代码风格规范：dnsajkndkjsabnjkdnjksandjknsajkdnjkasnjkdnjksandjknsajkdnjksajkdnas

- [JavaScript Standard Style](https://standardjs.com/)
- Airbnb JavaScript Style

## 复习

## 上午总结

- 代码风格
- 无分号
  + `(`
  + `[`
  + `
  + 最好前面补分号，避免一些问题
  + 《编写可维护的 JavaScript》
  + 不仅是功能，还要写的漂亮
- 服务端渲染
  + 说白了就是在服务端使用模板引擎
  + 模板引擎最早诞生于服务端，后来才发展到了前端

- 服务端渲染和客户端渲染的区别
  + 客户端渲染不利于 SEO 搜索引擎优化
  + 服务端渲染是可以被爬虫抓取到的，客户端异步渲染是很难被爬虫抓取到的
  + 所以你会发现真正的网站既不是纯异步也不是纯服务端渲染出来的
  + 而是两者结合来做的
  + 例如京东的商品列表就采用的是服务端渲染，目的了为了 SEO 搜索引擎优化
  + 而它的商品评论列表为了用户体验，而且也不需要 SEO 优化，所以采用是客户端渲染

## 下午总结

# Node.js 第3天课堂笔记

## 知识点

- 增删改查
- 登陆
- 注册
- 头像
  + 服务端图片
  + 水印
  + 图片水印
- 找回密码
- 密码修改

- 模块系统
  + 核心模块
  + 第三方模块
  + 自己写的模块
  + 加载规则以及加载机制
  + 循环加载
- npm
- package.json
- Express
  + 第三方 Web 开发框架
  + 高度封装了 http 模块
  + 更加专注于业务，而非底层细节
  + 知其所以然
- 增删改查
  + 使用文件来保存数据（锻炼异步编码）
- MongoDB
  + （所有方法都封装好了）

## 反馈

-  希望老师再推荐一些前端学习的书籍，谢谢！
   +  《JavaScript 高级编程》第3班
   +  学习，解决问题
   +  书本可以更好的系统的整理学过的内容，了解一些细节
   +  《JavaScript 语言精粹》
-  seo的资料，嘿嘿
   + 网站运营 SEO
   + SEO 运营专员
   + 百度、Google、搜狗、
-  最后老师那个怎么做案例的步骤真的是很有用 觉得今天的反馈 大概又是夸老师的比较多 老师声音很有特点
-  老师讲的很仔细,虽然语速有点快但是会重复很多遍,即使第一遍没听会第二遍第三遍也懂了.很好.
-  使用markdown一次只能打开一个文件,不知道怎么建文件夹，是需要安插件吗?
-  老师，软件版本的升级是以什么作为理论支持的，为什么跳跃间隙可以这么大？还有，看上了老师的电子图书馆，瞬间好爱学习呀，真的！
   +  软件开发版本里面涉及到软件工程学： 
   +  x.x.x
      *  0.0.1
      *  0.0.2
      *  1.1.5
      *  1.9.2
      *  2（新增功能比较多，甚至可能去除了某些功能）.5(加入了新功能).0（修复bug，提升性能）
      *  大版本
      *  一般是这些客户端软件、技术框架开发者比较理解的多
      *  做网站很少涉及到版本的概念，网站的目的就是快
-  art-template里面用的语法是jQuery吗， each什么的 我晕了 each,forEach, 遍历的全混了
   + art-template 和 jQuery 一毛钱关系都没有
   + each 是 art-template 的模板语法，专属的
   + {{each 数组}}
   + <li>{{ $value }}</li>
   + {{/each}} 这是 art-template 模板引擎支持的语法，只能在模板字符串中使用
   + $.each(数组, function)
   + $('div').each(function) 一般用于遍历 jQuery 选择器选择到的伪数组实例对象
   + forEach 是 EcmaScript 5 中的一个数组遍历函数，是 JavaScript 原生支持的遍历方法 可以遍历任何可以被遍历的成员
   + jQuery 的 each 方法和 forEach 几乎一致
   + 由于 forEach 是 EcmaScript 5 中的，所以低版本浏览器不支持
-  每一次的复习贼重要 老师很不错 我喜欢
-  在以后的工作中 用到node.js的地方多吗？ 在留言本的案例中 点击发表留言跳转页面的路径是url路径 和之前写的页面跳转写的文件路径还是有点分不清。
   + 技多不压身
   + Node 对于前端来讲是进阶高级前端开发工程师必备的技能
   + 屌丝最容易逆袭的职业
   + 见得东西多了你就不怕了
   + 为所欲为
-  老师讲的挺清晰的 可是第一节太困了 路径有点没转变过来
-  如果从a中调用b中的数据，又从b中调用a中的数据，执行a代码，为什么把b中的执行完后才会执行a，而不是在b调用a的时候a中的代码继续执行
   + a 加载了 b
     * 执行 b 中的代码
     * 同时得到 b 中导出的接口对象：exports
     * 执行 b 的过程中发现 b 也在 require a
     * b 就会反过来执行 a
     * a 中又加载 b
     * b 又反过来加载 a
     * 这就是循环加载
     * 如果你一旦出现了这种情况，说明你的思路有问题。
     * jQuery.js （可能不可能出现 jQuery 依赖了 main）
     * main.js 依赖了 jQuery
     * 这个问题是矛盾。
   + b 中也加载了 a
   + 
   + 网页中所有的路径其实都是 url 路径，不是文件路径
-  问题就是不知道问题是什么,写案例的时候似懂非懂
-  感觉思维有点跟不上,

## 复习

- 网站开发模型
  + 黑盒子、哑巴
  + 写代码让它变得更智能
  + 按照你设计好的套路供用户使用

- 在 Node 中使用 art-template 模板引擎
  + 安装
  + 加载
  + template.render()
- 客户端渲染和服务端渲染的区别
  + 最少两次请求，发起 ajax 在客户端使用模板引擎渲染
  + 客户端拿到的就是服务端已经渲染好的
- 处理留言本案例首页数据列表渲染展示
- 处理留言本案例发表留言功能
  + 路径
  + 设计好的请求路径
  + $GET 直接或查询字符串数据
  + Node 中需要咱们自己动手来解析
    * url.parse()
  + /pinglun?name=jack&message=hello
  + split('?')
  + name=jack&message=hello
  + split('&')
  + name=jack message=hello
  + forEach()
  + name=jack.split('=')
  + 0 key
  + 1 value
- 掌握如何解析请求路径中的查询字符串
  + url.parse()
- 如何在 Node 中实现服务器重定向
  + header('location')
    * 301 永久重定向 浏览器会记住
      - a.com b.com
      - a 浏览器不会请求 a 了
      - 直接去跳到 b 了
    * 302 临时重定向 浏览器不记忆
      - a.com b.com
      - a.com 还会请求 a
      - a 告诉浏览器你往 b
- Node 中的 Console（REPL）使用

## 上午总结

- jQuery 的 each 和 原生的 JavaScript 方法 forEach
  + EcmaScript 5 提供的
    * 不兼容 IE 8
  + jQuery 的 each 由 jQuery 这个第三方库提供
    * jQuery 2 以下的版本是兼容 IE 8 的
    * 它的 each 方法主要用来遍历 jQuery 实例对象（伪数组）
    * 同时它也可以作为低版本浏览器中 forEach 替代品
    * jQuery 的实例对象不能使用 forEach 方法，如果想要使用必须转为数组才可以使用
    * `[].slice.call(jQuery实例对象)`
- 模块中导出多个成员和导出单个成员
- 301 和 302 状态码区别
  + 301 永久重定向，浏览器会记住
  + 302 临时重定向
- exports 和 module.exports 的区别
  + 每个模块中都有一个 module 对象
  + module 对象中有一个 exports 对象
  + 我们可以把需要导出的成员都挂载到 module.exports 接口对象中
  + 也就是：`moudle.exports.xxx = xxx` 的方式
  + 但是每次都 `moudle.exports.xxx = xxx` 很麻烦，点儿的太多了
  + 所以 Node 为了你方便，同时在每一个模块中都提供了一个成员叫：`exports`
  + `exports === module.exports` 结果为  `true`s
  + 所以对于：`moudle.exports.xxx = xxx` 的方式 完全可以：`expots.xxx = xxx`
  + 当一个模块需要导出单个成员的时候，这个时候必须使用：`module.exports = xxx` 的方式
  + 不要使用 `exports = xxx` 不管用
  + 因为每个模块最终向外 `return` 的是 `module.exports`
  + 而 `exports` 只是 `module.exports` 的一个引用
  + 所以即便你为 `exports = xx` 重新赋值，也不会影响 `module.exports`
  + 但是有一种赋值方式比较特殊：`exports = module.exports` 这个用来重新建立引用关系的
  + 之所以让大家明白这个道理，是希望可以更灵活的去用它
- Node 是一个比肩 Java、PHP 的一个平台
  + JavaScript 既能写前端也能写服务端

```javascript
moudle.exports = {
  a: 123
}

// 重新建立 exports 和 module.exports 之间的引用关系
exports = module.exports

exports.foo = 'bar'
```

```javascript
Array.prototype.mySlice = function () {
  var start = 0
  var end = this.length
  if (arguments.length === 1) {
    start = arguments[0]
  } else if (arguments.length === 2) {
    start = arguments[0]
    end = arguments[1]
  }
  var tmp = []
  for (var i = start; i < end; i++) {
    // fakeArr[0]
    // fakeArr[1]
    // fakeArr[2]
    tmp.push(this[i])
  }
  return tmp
}

var fakeArr = {
  0: 'abc',
  1: 'efg',
  2: 'haha',
  length: 3
}

// 所以你就得到了真正的数组。 
[].mySlice.call(fakeArr)
```

## 下午总结

- jQuery 的 each 和 原生的 JavaScript 方法 forEach

- 301 和 302 的区别

- 模块中导出单个成员和导出多个成员的方式

- module.exports 和 exports 的区别

- require 方法加载规则

  + 优先从缓存加载
  + 核心模块
  + 路径形式的模块
  + 第三方模块
    * node_modules

- package.json 包描述文件

  + dependencies 选项的作用

- npm 常用命令

- Express 基本使用

- 使用 Express 把之前的留言本案例自己动手改造一下

- # Node.js 第4天课堂笔记

  ## 知识点

  - Express
  - 基于文件做一套 CRUD

  ## 反馈

  - 需要记忆的内容比较多，还是得多敲多练
  - 竟以为老师是理工男！！！老师每天来一波惊喜吧，魅力值up up up！
  - 老师很可爱，很喜欢，学习有动力，哈哈哈哈哈哈哈哈
    +  嘤嘤嘤
  - php什么的相关知识，老师可能大概也许说过，但是我清楚的知道，我是真的不知道，对我来说就是新知识。恩 所以，你没有重复
  - 给老师点赞
  - QAQ
    + @_@

  ## 复习

  - jQuery 的 each 和 原生的 JavaScript 方法 forEach
  - 301 和 302 的区别
  - 模块中导出单个成员和导出多个成员的方式
    + `module.exports = xxx`
    + 通过多次：`exports.xxx = xxx`
    + 导出多个也可以：`moudle.exports = {多个成员}`
  - module.exports 和 exports 的区别
    + exports 只是 module.exports 的一个引用而已，目的只是为了简化写法
    + 每个模块最终 return 的是 module.exports
  - require 方法加载规则
    + 优先从缓存加载
    + 核心模块
    + 路径形式的模块
      * `./xxx`
      * `../xxxx`
      * `/xxxx` / 在这里表示的是磁盘根路径
      * `c:/xxx`
    + 第三方模块
      * 第三方模块的标识就是第三方模块的名称（不可能有第三方模块和核心模块的名字一致）
      * npm
        - 开发人员可以把写好的框架、库发布到 npm 上
        - 使用者在使用的时候就可以很方便的通过 npm 来下载
      * 使用方式：`var 名字 = require('npm install 的那个包名')`
      * node_modules
      * node_modules/express
      * node_modules/express/package.json
      * node_modules/express/package.json main
      * 如果 package.json 或者 package.json main 不成立，则查找备选项：index.js
      * 如果以上条件都不成立，则继续进入上一级目录中的 node_modules 按照上面的规则继续查找
      * 如果直到当前文件模块所属磁盘根目录都找不到，最后报错：`can not find module xxx`
  - package.json 包描述文件
    + 就是产品的说明书
    + `dependencies` 属性，用来保存项目的第三方包依赖项信息
    + 所以建议每个项目都要有且只有一个 package.json (存放在项目的根目录)
    + 我们可以通过 `npm init [--yes]` 来生成 package.json 文件
    + 同样的，为了保存依赖项信息，我们每次安装第三方包的时候都要加上：`--save` 选项。
  - npm 常用命令
    + install
    + uninstall
  - Express 基本使用
  - 使用 Express 把之前的留言本案例自己动手改造一下

  ### 模块标识中的 `/` 和文件操作路径中的 `/`

  ## 上午总结

  ### 演讲

  > 说服
  > PPT
  > 脑图
  > markdown
  > 结构思维

  - 找痛点 why 为什么
  - 解决方案 what 是什么
  - 怎么去使用 how 怎么用
  - where 在哪儿用
  - when  什么时候用

  - 文件路径中的 `/` 和模块标识中的 `/`
  - nodemon
  - Express
    + art-template 模板引擎的配置
    + body-parser 解析表单 POST 请求体
  - 技术只是一种解决问题的手段、工具而已
    + 第三方的东西，不要纠结
    + 先以解决问题为主
  - 详解了 express 静态服务 API
    + app.use('/public/', express.static('./public'))
  - crud

  ## 下午总结

  ## 目标

  - 文件路径中的 `/` 和模块标识中的 `/`

  - Express 中配置使用 art-template 模板引擎

  - Express 中配置使用 body-parser

  - Express 中配置处理静态资源

  - CRUD 案例中单独提取路由模块

  - # Node.js 第5天课堂笔记

    ## 知识点

    - Express
    - MongoDB
    - 项目
      + 一天半的时间

    ## 反馈

    -  新版sublime 怎么格式化 怎么一起选中长度不等的内容 怎么改颜色 有的写对了也没颜色 仍然是白色
       +  HTML-CSS-JS Prettify
    -  代码量好多
       +  真正的开发是咱们这个小案例的无数倍
    -  callback是不是相当于函数自调用?
       +  很简单，函数也是一种数据类型，既可以当作参数进行传递，也可以当作方法的返回值
    -  我们现在用的模块化是CMD吧 老师能不能给我们扩展一下AMD
       + PHP 中为什么就可以直接 `require`、`include` 因为 PHP 当初在设计的时候就加入了这个功能
       + PHP 这门语言天生就支持
       + 模块作用域
       + 可以使用 API 来进行文件与文件之间的依赖加载
       + 在 Node 这个环境中对 JavaScript 进行了特殊的模块化支持 CommonJS
       + JavaScript 天生不支持模块化
         * require
         * exports
         * Node.js 才有的
       + 在浏览器中也可以像在 Node 中的模块一样来进行编程
         * `<script>` 标签来引用加载，而且你还必须考虑加载的顺序问题
         * require.js 第三方库 AMD
         * sea.js     第三方库 CMD
       + 无论是 CommonJS、AMD、CMD、UMD、EcmaScript 6 Modules 官方规范
         * 都是为了解决 JavaScript 的模块化问题
         * CommonJS、AMD、CMD 都是民间搞出来的
         * EcmaScript 是官方规范定义
         * 官方看民间都在乱搞，开发人员为了在不同的环境使用不同的 JavaScript 模块化解决方案
         * 所以 EcmaScript 在 2015 年发布了 EcmaScript 2016 官方标准
         * 其中就包含了官方对 JavaScript 模块化的支持
         * 也就是说语言天生就支持了
         * 但是虽然标准已经发布了，但是很多 JavaScript 运行换将还不支持
         * Node 也是只在 8.5 版本之后才对 EcmaScript 6 module 进行了支持
         * 后面学 Vue 的时候会去学习
         * less 编译器 > css
         * EcmaScript 6 -> 编译器 -> EcmaScript 5
         * 目前的前端情况都是使用很多新技术，然后利用编译器工具打包可以在低版本浏览器运行。
         * 使用新技术的目的就是为了提高效率，增加可维护性
    -  内心极度脆弱。。。有心杀敌 无力回天，总感觉时间不够用。
       +  不要猥琐发育，就得浪
    -  虽然比较多 但是因为老师讲的很清晰 还是愿意去写的 对于 node.js 的奥义 封装异步的API 就是需要多练
    -  老师讲的很清晰 讲课也很洒脱 老师是不是被夸的已经习惯了 后面讲的回掉函数有点懵了
    -  老师讲的很好，思路清晰，项目跟着老师的笔记一步一步敲，so easy
    -  觉得老师讲课真的超级棒啊 传智的实力担当 双击没毛病 老铁666
    -  都坐下 基本操作 哇,老师,一般敢说这句话的都是大神,我还是个菜鸟,学的那是一脸懵逼
    -  有点懵，看着老师的思路做，可是还是不知道从何入手，唉。。。
       +  本着达芬奇画鸡蛋的精神
       +  《使徒行者》三哥
       +  《反黑》陈小春
          *  卧底 8年卧底
          *  文职工作
          *  报了电脑版
          *  吃饭都在看书
          *  学习 -》吃饭也是看书
          *  边角余料
    -  var router = require('./router') 这一步不是加载router.js并执行该文件吗 为什么还要执行app.use(router) app.use 不是开放静态资源吗 app.use(router)在这里是什么意思，挂载到 app 服务中的意思是？ module.exports = app 也不懂
       + 这里涉及到一个中间件的概念
       + app.use 不仅仅是用来处理静态资源的
       + 还可以做很多工作
       + 配置 body-parse 也是通过 app.use 来配置的
       + 这叫中间件，其中有一套规则
    -  npm init --yes 生成一个package.json 文件 npm --save 文件名 又生成一个package-lock.json文件,又生成的文件和初始化生成的文件有区别吗?
       + 当你安装包的时候，新版的 npm 还会自动生成一个文件：package-lock.json
    -  早上听的还可以，下午感觉一头蒙，还好老师讲了晚上自己做案例的具体步骤，不然感觉无从下手，还是反馈多一点好，还可以回顾回顾，不然感觉老师一天讲的知识太多了，消化不了，嘤嘤嘤~~~
    -  其实拖堂的效率也不高啊。。可能是我天资愚笨
       + 对自己有信息
       + 撸起袖子加油干、一张蓝图绘到底
    -  老师你好，每节课的事件有点长，上课时间长注意力就容易模糊。听课效率确实有问题，有时候同桌都快憋不住了，为了不丢下知识点，依旧在憋着，好担心...
    -  为什么模板引擎在app.js中引入之后在router.js中不引入可以直接使用，而express还需要在router.js中再引入一次 app.js中路由器挂载不是很懂 router.js中为什么要创建一个路由器容器，不知道作用是干什么的 es6中的find方法不是很懂
       + 中间件
       + EcmaScript 6 的 find 方法
    -  老师，你后来讲的回调函数那里，关于增删改查案例一个已经够呛了，你竟然在最后都讲完了； 虽然增删改查文件的操作在php之前讲过，但是真的忘了，而且php学的也不好； 还有：对于php是世界上最好的语言，我持怀疑态度，觉得它是世界上最难理解的语言； 诶！苦恼！又来了一个node，知道后边的boss都很难应付，比如什么angular、react和vue，现在其实也做好了心理准备！ 来者不拒吧！看来这一个月注定是一个煎熬的日子！
       + PHP 是世界上最好的语言（贬义）
       + 一切我抗
    -  在express框架中怎么判断访问页面不存在的情况？

    ## 复习

    - 文件路径中的 `/` 和模块标识中的 `/`
    - Express 中配置使用 art-template 模板引擎
    - Express 中配置使用 body-parser
    - Express 中配置处理静态资源
    - CRUD 案例中单独提取路由模块

    ## 上午总结

    - 回调函数
      + 异步编程
      + 如果需要得到一个函数内部异步操作的结果，这是时候必须通过回调函数来获取
      + 在调用的位置传递一个函数进来
      + 在封装的函数内部调用传递进来的函数
    - find、findIndex、forEach
      + 数组的遍历方法，都是对函数作为参数一种运用
        + every
      + some
      + includes
      + map
      + reduce
    - package-lock.json 文件的作用
      + 下载速度快了
      + 锁定版本
    - JavaScript 模块化
      + Node 中的 CommonJS
      + 浏览器中的
        * AMD require.js
        * CMD sea.js
      + EcmaScript 官方在 EcmaScript 6 中增加了官方支持
      + EcmaScript 6
      + 后面我们会学，编译工具
    - MongoDB 数据库
      + MongoDB 的数据存储结构
        * 数据库
        * 集合（表）
        * 文档（表记录）
    - MongoDB 官方有一个 mongodb 的包可以用来操作 MongoDB 数据库
      + 这个确实和强大，但是比较原始，麻烦，所以咱们不使用它
    - mongoose
      + 真正在公司进行开发，使用的是 mongoose 这个第三方包
      + 它是基于 MongoDB 官方的 mongodb 包进一步做了封装
      + 可以提高开发效率
      + 让你操作 MongoDB 数据库更方便
    - 掌握使用 mongoose 对数据集合进行基本的 CRUD
    - 把之前的 crud 案例改为了 MongoDB 数据库版本
    - 使用 Node 操作 mysql 数据库

    ## 下午总结

    ## 目标

﻿# Node.js 第6天课堂笔记

## 知识点

- 多人社区案例

## 反馈

## 复习

- MongoDB 数据库
  + 灵活
  + 不用设计数据表
  + 业务的改动不需要关心数据表结构
  + DBA 架构师 级别的工程师都需要掌握这项技能
    * 设计
    * 维护
    * 分布式计算
- mongoose
  + mongodb 官方包也可以操作 MongoDB 数据库
  + 第三方包：WordPress 项目开发团队
  + 设计 Schema
  + 发布 Model（得到模型构造函数）
    * 查询
    * 增加
    * 修改
    * 删除
- Promise
  + http://es6.ruanyifeng.com/#docs/promise
  + callback hell 回调地狱
  + 回调函数中套了回调函数
  + Promise(EcmaScript 6 中新增了一个语法 API)
  + 容器
    * 异步任务（pending）
    * resolve
    * reject
  + then 方法获取容器的结果（成功的，失败的）
  + then 方法支持链式调用
  + 可以在 then 方法中返回一个 promise 对象，然后在后面的 then 方法中获取上一个 then 返回的 promise 对象的状态结果

## 上午总结

## 下午总结

## 总结

- path 模块
- __dirname 和 __filename
  + **动态的** 获取当前文件或者文件所处目录的绝对路径
  + 用来解决文件操作路劲的相对路径问题
  + 因为在文件操作中，相对路径相对于执行 `node` 命令所处的目录
  + 所以为了尽量避免这个问题，都建议文件操作的相对路劲都转为：**动态的绝对路径**
  + 方式：`path.join(__dirname, '文件名')`
- art-template 模板引擎(include、block、extend)
  + include
  + extend
  + block
- 表单同步提交和异步提交区别
  + 以前没有 ajax 都是这么干的，甚至有些直接就是渲染了提示信息出来了
  + 异步提交页面不会刷新，交互方式更灵活
- Express 中配置使用 express-session 插件
- 概述案例中注册-登陆-退出的前后端交互实现流程

## 目标

# Node.js 第7天课堂笔记

## 知识点

- 上午
  + 多人社区案例
  + Express 中间件
- 下午
  + Vue

---

## 反馈

```javascript
funtion extend (source, target) {
  for (var key in source) {
    target[key] = source[key]
  }
}

var obj1 = {
  foo: 'bar'
}

var obj2 = {
  name: 'Jack'
}

// obj2 就拥有了 obj1 的所有成员了
extend(obj1, obj2)
```

-  唉............
-  老师我想问下 那个操作文件路径不受打开命令执行node命令所属路径影响什么意思，是可以在任意窗口打开都可以访问到吗。。。。。
-  慢点 心急吃不了热豆腐
-  老师讲的很好 很清晰 希望老师下午第一节上课时间短点 第一节很困 上课时间太长听课效率有点低
-  老师 写案例的时候 一个文件的代码量多了 可不可以把字体稍微调小点便于看全局结构 有时候感觉自己连不上
-  extend还不是很理解
   + 模板继承
   + extend 把复制过来
   + layout
   + index （extend layout）
   + index 就具有了 layout 的内容
   + index 还可以有自己的自定义内容
-  能不能把命令系统地罗列一下,@ 0 @
-  听得时候都差不多听懂了，可是自己做的时候发现不知道从何入手，即使是看着老师的需求与代码，也根本不懂怎么写了，感觉自己听完了就全都忘光了，很郁闷！
-  我现在学习的感觉就像 你是个俄国人，教我了一句外语，你已经重复 了很多遍，我也努力再听，但是当你说完的那一刻，我就完全不知道你说了什么。就是仅仅过了一耳朵，再加上内容太多，我已经感觉完全跟不上了，怎么办，我有点崩溃。怎么破
   + 上帝撒了一把智慧，可惜我打了一把伞
   + 多花时间、废寝忘食
-  老师你是不是喜欢Anglebaby?我同桌问的，她是个女的
   + Angelababy
-  如何在浏览器中模拟所谓的art-template高级技术？关于浏览器操作cookie的插件如何使用，需要注意些什么？还可以安装一些什么谷歌浏览器插件，有助于提高开发效率或模拟项目、测试的实用插件！
   + 只是一个工具
   + https://github.com/js-cookie/js-cookie
   + EditThisCookie Chrome 浏览器插件
-  文件引入有规则吗，像router.js中，需要重新引入第三方模块express，但是body-parser在routre页面也使用了呀，但是怎么不用引入
   +  这主要是中间件的原因
-  req.session对象不清楚 希望老师再讲讲
   + req.session.xxx = xxx
   + req.session.xxx
   + Session 是基于 Cookie 实现的
-  session 那块还是不怎么明白
-  课间下课尽量要准时，特别是上午第一节课比较困，听课效率低，反正下课次数固定，也不会让上课时间减少。 下午5点半增加上课时间多多益善
-  思路有点乱，有些小地方不明确，总的来说练得太少
-  mongoose中的Schema用的不熟练
   + 多写写
-  如果先启动node服务，再开启数据库，数据库服务开启了，但是数据库并没有连接，这样会出现所有的操作都会失效的情况，必须打开新的命令行使用mongo命令手动连接数据库 反过来，如果先开启数据库，再开启node服务，就不会出现这样的问题，因为user.js代码中mongoose.connect('mongodb://localhost/test', { useMongoClient: true })自动连接了数据库，刚开始以为数据库竟然和node产生了依赖，原来并不是！ 希望老师控制每节课的上课时间，一节课集中精力的时间最多20分钟，接下来的20分钟基本只有一半的效率，后面的时间效率只会指数减小，所以希望老师能在45分钟左右就休息一次，也能提高效率； 老师讲的很细，很认真，也很负责，希望能在最后一个月的时间学好最重要的内容，就像你说的，因为刚好遇见你！
   +  你说的对，加油。
-  一到数据库就蒙。数据库始终连接不上去。我觉得不知道我数据库都学了什么?_?
-  nice！

---

## 复习

- path 模块
- __dirname 和 __filename
  + **动态的** 获取当前文件或者文件所处目录的绝对路径
  + 用来解决文件操作路劲的相对路径问题
  + 因为在文件操作中，相对路径相对于执行 `node` 命令所处的目录
  + 所以为了尽量避免这个问题，都建议文件操作的相对路劲都转为：**动态的绝对路径**
  + 方式：`path.join(__dirname, '文件名')`
- art-template 模板引擎(include、block、extend)
  + include
  + extend
  + block
  + 动手写一写
- 表单同步提交和异步提交区别
  + 字符串交互
  + 请求（报文、具有一定格式的字符串）
  + HTTP 就是 Web 中的沟通语言
  + 服务器响应（字符串）
  + 01
  + 服务器端重定向针对异步请求无效
- Express 中配置使用 express-session 插件
  + 插件也是工具
  + 你只需要明确你的目标就可以了
  + 我们最终的目标就是使用 Session 来帮我们管理一些敏感信息数据状态，例如保存登陆状态
  + 写 Session
    * req.session.xxx = xx
  + 读 Session
    * req.session.xxx
  + 删除 Session
    * req.session.xxx = null
    * 更严谨的做法是 `delete` 语法
    * delete req.session.xxx
- 概述案例中注册-登陆-退出的前后端交互实现流程

---

## 上午总结

---

## 下午总结

---

## 总结

---

## 目标