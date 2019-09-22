# Dissecting RxJS

中文名：[《深入浅出 RxJS》](https://book.douban.com/subject/30217949/)

## 进度

- [x] 第 1 章 函数响应式编程
- [x] 第 2 章 RxJS 入门
- [x] 第 3 章 操作符基础
- [x] 第 4 章 创建数据流
- [ ] 第 5 章 合并数据流
- [ ] 第 6 章 辅助类操作符
- [ ] 第 7 章 过滤数据流
- [ ] 第 8 章 转化数据流
- [ ] 第 9 章 异常错误处理
- [ ] 第 10 章 多播
- [ ] 第 11 章 掌握时间的 Scheduler
- [ ] 第 12 章 RxJS 的调试和测试
- [ ] 第 13 章 用 RxJS 驱动 React
- [ ] 第 14 章 Redux 和 RxJS 结合
- [ ] 第 15 章 RxJS 游戏开发

## 第 1 章 函数响应式编程

软件开发中有什么老问题？

技术发展迅速，用户的需求增加更快，软件的代码库也会随需求增长而快速膨胀，在这种情况下，如何保证代码质量？如何控制代码的复杂度？如何保证代码的可维护性？就成了软件开发的大问题。

业界的同仁们为了解决这些老问题做了各种尝试，函数式编程和响应式编程就是在实践中被证明行之有效的两种方法。

### 什么是函数式编程

函数式编程就是非常强调使用函数来解决问题的一种编程方式。函数式编程对函数的使用有一些特殊的要求，这些要求包括以下几点：

- 声明式（Declarative）
- 纯函数（Pure Function）
- 数据不可变性（Immutability）

### 函数响应式编程的优势

- 数据流抽象了很多现实的问题
- 擅长处理异步操作
- 把复杂问题分解成简单问题的组合

现实应用中，很多问题都可以抽象为数据流的问题来解决。以网页应用的前端领域为例：

- 网页 DOM 的事件，可以看作为数据流；
- 通过 WebSocket 获得的服务器端推送消息可以看做是数据流；
- 通过 AJAX 获得服务器端的数据资源也可以看作是数据流；
- 网页的动画显示可以看作是一个数据流。

RxJS 擅长处理异步操作，因为它对数据采用「推」的处理方式，当一个数据产生的时候，被推送给对应的处理函数，这个处理函数不用关心数据是同步产生的还是异步产生的，这样就把开发者从命令式异步处理的枷锁中解放了出来。

RxJS 中的数据流可能包含复杂的功能，但是可以分解成很多小的部分来实现，实现某一个小功能的函数就是操作符，可以说，学习 RxJS 就是学习如何组合操作符来解决复杂问题。

## 第 2 章 RxJS 入门

### RxJS 的版本和运行环境

目前被广泛使用的 RxJS 版本有两个：v4 和 v5，这两个版本的 API 很相似，但是也有巨大的差别。

安装 RxJS v5 的 npm 包：

```sh
npm install rxjs
```

安装 RxJS v4 的 npm 包：

```sh
npm install rx
```

为什么不同版本的 RxJS 的 npm 包名都会不同呢？这是因为在开发 v5 的时候，考虑到架构的巨大差别，另起炉灶重写的，原本 RxJS 的库是 [https://github.com/Reactive-Extensions/RxJS](https://github.com/Reactive-Extensions/RxJS) ，包含了 v4 的代码，而开发 v5 版本的 RxJS，使用的是另外一个代码库 [https://github.com/ReactiveX/rxjs](https://github.com/ReactiveX/rxjs) ，这种代码库的分离，也就导致了发布的 npm 包名不同。

### Observable 和 Observer

要理解 RxJS，先要理解两个最重要的概念：Observable 和 Observer，可以说 RxJS 的运行就是 Observable 和 Observer 之间的互动游戏。

Observable 就是「可以被观察的对象」即「可被观察者」，而 Observer 就是「观察者」，连接两者的桥梁就是 Observable 对象的函数 subscribe。

RxJS 中的数据流就是 Observable 对象，Observable 实现了下面两种设计模式：

- 观察者模式
- 迭代器模式

（1）**观察者模式**

观察者模式要解决的问题，就是在一个持续产生事件的系统中，如何分割功能，让不同模块只需要处理一部分逻辑，这种分而治之的思想是基本的系统设计概念。

观察者模式将逻辑分为发布者（Publisher）和观察者（Observer），其中发布者只管负责生产事件，它会通知所有注册挂上号的观察者，而不关心这些观察者如何处理这些事件，相对的，观察者可以被注册上某个发布者，只管接收到事件之后就处理，而不关心这些数据是如何产生的。

观察者模式将复杂的问题分解成三个小问题：

- 如何产生事件，这就是发布者的责任，在 RxJS 中是 Observable 对象的工作
- 如何响应事件，这是观察者的责任，在 RxJS 中是 subscribe 的参数来决定的
- 什么样的发布者关联什么样的观察者，也就是何时调用 subscribe

（2）**迭代器模式**

迭代器（Iterator）指的是能够遍历一个数据集合的对象，因为数据集合的实现方式很多，可以是一个数组，也可以是一个树形结构，也可以是一个单向链表……迭代器的作用就是提供一个通用的接口，让使用者完全不关心这个数据集合的具体实现方式。

迭代器另一个容易理解的名字叫游标（cursor），就像是一个移动的指针一样，从集合的一个元素移动到另一个元素，完成对整个集合的遍历。

迭代器的实现通常应该包含以下几个函数：

- getCurrent，获取当前被游标所指向的元素
- moveToNext，将游标移动到下一个元素，调用这个函数之后，getCurrent 获得的元素就会不同
- isDone，判断是否已经遍历完所有的元素

RxJS 实现的是「推」式迭代器。

> 在编程的世界中，所谓「拉」（pull）或者「推」（push），都是从数据消费者角度的描述，比如，在网页应用中，如果是网页主动通过 AJAX 请求从服务器获取数据，这是「拉」，如果网页和服务器建立了 websocket 通道，然后，不需要网页主动请求，服务器都可以通过 websocket 通道推送数据到网页中，这是「推」。

在 RxJS 中，作为迭代器的使用者，并不需要主动去从 Observable 中「拉」数据，而是只要 subscribe 上 Observable 对象之后，自然就能够收到消息的推送，这就是观察者模式和迭代器模式结合的强大之处。

### Observable

每个 Observable 对象，代表的就是在一段时间范围内发生的一系列事件。

RxJS 结合了观察者模式和迭代器模式，其中的 Observable 可以用下面的这种公式表示：

```javascript
Observable = Publisher + Iterator
```

（1）**创建 Observable**

```javascript
import { Observable } from 'rxjs/Observable'

const onSubscribe = observer => {
  observer.next(1)
  observer.error('Something Wrong')
  observer.complete()
}

const srouce$ = new Observable(onSubscribe)

const theObserver = {
  next: item => console.log(item),
  error: err => console.log(err),
  complete: () => console.log('No more data.')
}

source$.subscribe(theObserver)
```

在 RxJS 中，一个 Observable 对象只有一种终结状态，要么是完结（complete），要么是出错（error），一旦进入出错状态，这个 Observable 对象也就终结了，再不会调用对应 Observer 的 next 函数，也不会调用 Observer 的 complete 函数；同样，如果一个 Observable 对象进入了完结状态，也不能在调用 Observer 的 next 和 error。

![status flow](https://user-images.githubusercontent.com/18362949/65381515-a697b780-dd25-11e9-9df6-67bf20463f79.png)

*Observable* 对象的状态流转图

（2）**Observable 的简单形式**

Observer 是一个可以包含 next、complete、error 三个方法的对象，用于接受 Observable 对象的三种不同的事件，如果我们根本不关心某种事件的话，也可以不实现对应的方法。

为了让代码更加简洁，可以不用创造 Observer 对象，subscribe 可以直接接受函数作为参数，第一个参数如果是函数类型，就被认为是 next，第二函数参数被认为是 error，第三个函数参数被认为是 complete，下面是代码示例：

```javascript
source$.subscribe(item => console.log(item), err => console.log(err), () => console.log('No more data'))
```

（3）**退订 Observable**

Observable 产生的事件，只有 Observer 通过 subscribe 订阅之后才会收到，在 unsubscribe 之后就不会再收到。

```javascript
const onSubscribe = observer => {
  let number = 1
  const handle = setInterval(() => {
    observer.next(number++)
  }, 1000)
  return {
    unsubscribe: () => clearInterval(handle)
  }
}

const source$ = new Observable(onSubscribe)

const subscription = source$.subscribe(item => console.log(item))

setTimeout(() => {
  subscription.unsubscribe()
}, 3000)
```

（4）**Hot Observable 和 Cold Observable**

一个 Observable 是 Hot 还是 Cold，都是相对于生产者而言的，如果每次订阅的时候，已经有一个 Hot 的「生产者」准备好了，那就是 Hot Observable，相反，如果每次订阅都要产生一个新的生产者，新的生产者就像汽车引擎一样刚启动时肯定是冷的，所以叫 Cold Observable。

非常现实的例子，电视台的任何一个频道的节目如果看做是一个 Observable 对象，那么每一台电视机就是 Observer，当你打开电视切换到一个频道的时候，相当于 subscribe 上了对应频道的 Observable，毫无疑问，切换到某个频道，你所看到的节目内容就是从那一刻开始的，不包含之前的内容，这个场景就属于 Hot Observable。我们平时会访问一些视频点播网站，如果把可供点播的每一个剧集看做一个 Observable 对象，那么你观看剧集的终端（浏览器或者手机 APP）就是 Observer，当你打开某一个剧集的时候，就是从这一集的第一秒钟开始播放，另一个用户在另一个时间另一个终端上打开同样的剧集，也是从第一秒开始播放，互相没有影响，这个场景就属于 Cold Observable。

### 操作符简介

对于现实中复杂的问题，并不会创造一个数据流之后就直接通过 subscribe 接上一个 Observer，往往需要对这个数据留作一系列处理，然后才交给 Observer。就像一个管道，数据从管道的一端流入，途经管道各个环节，当数据到达 Observer 的时候，已经被管道操作过，有的数据已经被中途过滤抛弃掉了，有的数据已经被改变了原来的形态，而且最后的数据可能来自多个数据源，最后 Observer 只需要处理能够走到终点的数据。

![operators pipe](https://user-images.githubusercontent.com/18362949/65381627-068f5d80-dd28-11e9-849b-d19ee10e3453.png)

*数据管道示意图*

在 RxJS 中，组成数据管道的元素就是**操作符**。

在数据管道里流淌的数据就像是水，从上游流向下游。对一个操作符来说，上游可能是一个数据源，也可能是其他操作符，下游可能是最终的观察者，也有可能是另一个操作符，每一个操作符之间都是独立的，正因为如此，所以可以对操作符进行任意组合，从而产生各种功能的数据管道。

在 RxJS 中，有一系列用于产生 Observable 对象的函数，这些函数有的凭空创造 Observable 对象，有的根据外部数据源产生 Observable 对象，更多的是根据其他的 Observable 中的数据来产生新的 Observable 对象，也就是把上游数据转化为下游数据，所有这些函数统称为**操作符**。

下面看一个示例：

```javascript
import { Observable } from 'rxjs/Observable'
import { map } from 'rxjs/operators'

const onSubscribe = observer => {
  observer.next(1)
  observer.next(2)
  observer.next(3)
}

const source$ = Observable.create(onSubscribe)

source$.map(x => x * x).subscribe(console.log)
```

在以上示例中，第一个操作符是 Observable 自带的 create，它做的事情就是创造一个新的 Observable 对象。第二个使用的操作符是 map，这种操作符要求存在上游 Observable，而且 Observable 类的实例函数存在。

在 RxJS 中，作为操作符的 map 接受一个函数参数，对 Observable 对象中的每一个数据映射为一个新的值，产生一个新的 Observable 对象。

通过代码可以看得更清楚一些：

```javascript
const source$ = Observable.create(onSubscribe)

const mapped$ = source$.map(x => x * x)

mapped$.subscribe(console.log)
```

这个 `mapped$` 就是一个全新的 Observable 对象。每一个操作符都是创造一个新的 Observable 对象，不会对上游的 Observable 对象做任何修改。

### 弹珠图（Marble Diagram）

RxJS 中的 Observable 代表一个数据流，这个概念需要一点想象力，简单的数据流还是可以靠大脑来想象，对于复杂一点的场景，大脑可能就不够用了，所以需要其他形象且具体的方式来描述数据流，这种方式就是「弹珠图」。

![marble diagram](https://user-images.githubusercontent.com/18362949/65381867-612ab880-dd2c-11e9-8316-39ebeaec7c22.png)

> *弹珠图*

![marble diagram](https://user-images.githubusercontent.com/18362949/65381860-3d677280-dd2c-11e9-94f7-9cb324b52ea9.png)

> *产生异常的弹珠图*

在弹珠图中，每个弹珠之间的间隔，代表的是吐出数据之间的时间间隔，用这种形式，能够很形象地看清楚一个 Observable 对象中数据的分布。

根据弹珠图的传统，竖杠符号「|」代表的是数据流的完结，对应调用下游的 complete 函数；符号「x」代表数据流中的异常，对应于调用下游的 error 函数。

我们知道一个 Observable 对象只能有一种完结形式，complete 或者 error，所以在一个 Observable 对象的弹珠图上，不可能既有符号「|」也有符号「x」。

（1）**弹珠图工具**

- [http://rxmarbles.com/](http://rxmarbles.com)
- [https://rxviz.com](https://rxviz.com)

## 第 3 章 操作符基础

使用和组合操作符是 RxJS 编程的重要部分，毫不夸张地说，对操作符使用的熟练程度决定对 RxJS 的掌握程度。

操作符其实就是解决某个具体应用问题的模式。

- 当我们要用 RxJS 解决问题时，首先需要创建 Observable 对象，于是需要创建类操作符；
- 当需要将多个数据流中的数据汇合到一起处理时，需要合并类操作符；
- 当需要筛选去除一些数据时，需要过滤类操作符；
- 当希望把数据流中的数据变化为其他数据时，需要转化类操作符；
- 而对数据流的处理可能引起异常，所以我们需要异常处理类操作符；
- 要让一个数据流的数据可以提供给多个观察者，我们需要多播类操作符。

### filter 和 map 操作符

JavaScript 的数组对象就有同名的 filter 和 map 方法，这个两个方法课一链式调用，有以下特点：

- filter 和 map 都是数组对象的成员函数
- filter 和 map 的返回结果依然是数组对象
- filter 和 map 不会改变原来的数组对象

同样的，对 Observable 对象能够链式调用 filter 和 map 操作符，且有以下特点：

- filter 和 map 都是 Observable 对象的成员函数
- filter 和 map 的返回结果依然是 Observable 对象
- filter 和 map 不会改变原本的 Observable 对象

> 如果你惊叹于数组对象和 Observable 对象的相似性，那是因为这两个东西在数学概念上都是 Functor， Functor 属于数学范畴论，满足 Functor 的类型就会具备一些共同的特质，比如链式调用。

### 操作符分类

（1）**功能分类**

根据功能，操作符可以分为以下类别：

- 创建类（creation）
- 转化类（transformation）
- 过滤类（filtering）
- 合并类（combination）
- 多播类（multicasting）
- 错误处理类（error handling）
- 辅助工具类（utility）
- 条件分支类（conditional & boolean）
- 数学和合计类（mathmatical & aggregate）

RxJS 自带的任何操作符都属于上面某一种分类。比如 map 属于「转化类」，filter 属于「过滤类」。

（2）**静态和实例分类**

所有的操作符都是函数，不过有的操作符是 Observable 类的静态函数，也就是不需要 Observable 实例就可以执行的函数，所以称为「静态操作符」；另一类操作符是 Observable 的实例函数，前提是要有一个创建好的 Observable 对象，这一类称为「实例操作符」。

对一个静态操作符，比如名为 of，可以认为它是这样添加给 Observable 类的：

```javascript
Observable.of = functionToImplementOf
```

对于一个实例操作符，比如名为 map，可以认为是通过下面这样添加给 Observable 类的 prototype 的：

```javascript
Observable.prototype.map = implementationOfMap
```

无论是静态操作符还是实例操作符，它们都会返回一个 Observable 对象。在链式调用中，静态操作符只能出现在首位，实例操作符则可以出现在任何位置，因为链式调用中各级之间靠 Observable 对象来关联，一个静态函数在链式调用的中间位置是不可能有容身之处的。

### 如何实现操作符

（1）**操作符的实现**

每一个操作符都是一个函数，不管实现什么功能，都必须考虑下面这些功能要点：

- 返回一个全新的 Observable 对象
- 对上游和下游的订阅及退订处理
- 处理异常情况
- 及时释放资源

map 操作符实现示例：

```javascript
function map(project) {
  return new Observable(observer => {
    const sub = this.subscribe({
      next: value => {
        try {
          observer.next(project(value))
        } catch (err) {
          observer.error(err)
        }
      },
      error: err => observer.error(err),
      complete: () => observer.complete()
    })
    return {
      unsubscribe: () => {
        sub.unsubscribe()
      }
    }
  })
}
```

（2）**lettable 和 pipeable 操作符**

```javascript
import { of } from 'rxjs/observable/of'
import { map, filter } from 'rxjs/operators'

const source$ = of(1, 2, 3)

const result$ = source$.pipe(
  filter(x => x % 2 === 0),
  map(x => x * 2)
)

result$.subscribe(console.log)
```

## 第 4 章 创建数据流

对应不同功能需求的操作符如下：

- create：直接操作观察者
- of：根据有限的数据产生同步数据流
- range：产生一个数值范围内的数据
- generate：以循环方式产生数据
- repeat|repeatWhen：重复产生数据流中的数据
- empty: 产生空数据流
- throw：产生直接出错的数据流
- never：产生永不完结的数据流
- interval|timer：间隔给定时间持续产生数据
- from：从数组等枚举类型数据产生数据流
- fromPromise：从 Promise 对象产生数据流
- fromEvent|fromEventPattern：从外部事件对象产生数据流
- ajax：从 AJAX 请求结果产生数据流
- defer：延迟产生数据流

### 创建同步数据流

同步数据流，或者说同步 Observable 对象，需要关注的是：

- 产生哪些数据
- 数据之间的先后顺序如何

对于同步数据流，数据之间的时间间隔不存在，所以不需要考虑时间方面的问题。

- `create`：

### 创建异步数据的 Observable 对象

异步数据流，或者说异步 Observable 对象，不光要考虑产生什么数据，还要考虑这些数据之间的时间问题，RxJS 提供的操作符就是要让开发者在日常尽量不要考虑时间因素。

## 第 5 章 合并数据流

在 RxJS 的世界中，为了满足复杂的需求，往往需要把不同来源的数据汇聚在一起，把来自多个 Observable 对象的数据合并到一个 Observable 对象中。RxJS 提供了众多操作符支持数据流合并，具体使用哪种操作符，要根据待解决的问题决定，下面列举了各种场景下使用的合并类操作符：

- concat|concatAll：把多个数据流以首尾相连方式合并
- merge|mergeAll：把多个数据流中数据以先到先得方式合并
- zip|zipAll：把多个数据流中数据以一一对应方式合并
- combineLatest|combineAll|withLatestFrom：持续合并多个数据流中最新产生的数据
- race：从多个数据流中选取第一个产生内容的数据流
- startWith：在数据流前面添加一个指定数据
- forkJoin：只获取多个数据流最后产生的那个数据
- switch|exhaust：从高阶数据流中切换数据源

### 合并类操作符

RxJS 提供了一系列可以完成 Observable 组合操作的操作符，这一类操作符称为合并类操作符，这类操作符都有多个 Observable 对象作为数据来源，把不同来源的数据根据不同的规则合并到一个 Observable 对象中。
