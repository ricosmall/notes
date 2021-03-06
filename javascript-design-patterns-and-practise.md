# JavaScript 设计模式和开发实践

[书籍介绍](https://book.douban.com/subject/26382780/)

## 进度

- [ ] 1.面向对象的 JavaScript
- [ ] 2.this、call 和 apply
- [ ] 3.闭包和高阶函数
- [ ] 4.单例模式
- [ ] 5.策略模式
- [ ] 6.代理模式
- [ ] 7.迭代器模式
- [ ] 8.发布-订阅模式
- [ ] 9.命令模式
- [ ] 10.组合模式
- [ ] 11.模板方法模式
- [ ] 12.享元模式
- [ ] 13.职责链模式
- [ ] 14.中介者模式
- [ ] 15.装饰者模式
- [ ] 16.状态模式
- [ ] 17.适配器模式
- [ ] 18.单一职责原则
- [ ] 19.最少知识原则
- [ ] 20.开放-封闭原则
- [ ] 21.接口和面向接口编程
- [ ] 22.代码重构

## 1.面向对象的 JavaScript

JavaScript 没有提供传统面向对象语言中的类式继承，而是通过原型委托的方式来实现对象与对象之间的继承。

### 1.1 动态类型语言和鸭子类型

| 语言类型     | 优点                                                                       | 缺点                                                                                                                           |
| ------------ | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| 静态类型语言 | 编译时就能发现类型不匹配的错误，避免运行时可能发生的一些错误。             | 迫使程序员依照强契约来编写程序，为每个变量规定数据类型，归根结底只是辅助我们编写可靠性高程序的一种手段，而不是编写程序的目的。 |
| 动态类型语言 | 编写的代码数量更少，看起来更简洁，程序员可以把精力更多地放在业务逻辑上面。 | 无法保证变量的类型，从而在程序运行时可能发生跟类型相关的错误。                                                                 |

动态类型语言对变量类型的宽容给实际编码带来了很大的灵活性。

### 1.2 多态

多态的实际含义是：同一操作作用于不同的对象上面，可以产生不同的解释和不同的执行结果。换句话说，给不同的对象发送同一个消息的时候，这些对象会根据这个消息分别给出不同的反馈。

示例代码：

```javascript
var makeSound = function (animal) {
  if (animal instanceof Duck) {
    console.log('嘎嘎嘎')
  } else if (animal instanceof Chicken) {
    console.log('咯咯咯')
  }
}

var Duck = function () {}
var Chicken = function () {}

makeSound(new Duck()) // 嘎嘎嘎
makeSound(new Chicken()) // 咯咯咯
```

多态背后的思路是将「做什么」 和「谁去做以及怎样去做」分离开来，也就是将「不变的事物」与「可能改变的事物」分离开来。以上示例中，动物都会叫，这是不变的，但是不同类型的动物具体怎么叫是可变的。

把不变的部分隔离出来，把可变的部分封装起来，这给予我们扩展程序的能力，程序看起来是可生长的，也是符合开放-封闭原则的，相对于修改代码来说，仅仅增加代码就能完成同样的功能，这显然优雅和安全得多。

#### 对象的多态性

改写上面的示例代码如下：

```javascript
var makeSound = function (animal) {
  animal.sound()
}

var Duck = function () {}

Duck.prototype.sound = function () {
  console.log('嘎嘎嘎')
}

var Chicken = function () {}

Chicken.prototype.sound = function () {
  console.log('咯咯咯')
}

makeSound(new Duck())
makeSound(new Chicken())
```

如果有一天动物世界里又增加了一只狗，我们只需要追加一些代码就可以了，而不用改动以前的 `makeSound` 函数。

```javascript
var Dog = function () {}

Dog.prototype.sound = function () {
  console.log('汪汪汪')
}

makeSound(new Dog())
```

#### 多态在面向对象程序设计中的作用

有许多人认为，多态是面向对象编程语言中最重要的技术。但是我们目前还很难看出这一点，毕竟大部分人都不关心鸡是怎么叫的，也不想知道鸭是怎么叫的。

Martin Fowler 在《重构：改善既有代码的设计》里写到：

> 多态的最根本的好处在于，你不必再向对象询问「你是什么类型」而后根据得到的答案调用对象的某个行为 -- 你只管调用该行为就是了，其他的一切多态机制都会为你安排妥当。

换句话说，多态最根本的作用就是通过把过程化的条件分支语句转化为对象的多态性，从而消除这些条件分支语句。

### 1.3 封装

封装的目的是将信息隐藏。一般而言，我们讨论的封装是封装数据和封装实现。实际上封装还包括封装类型和封装变化。

#### 封装数据

在许多语言的对象系统中，封装数据是由语法解析来实现的，这些语言也许提供了 `private`、`public`、`protected` 等关键字来提供不同的访问权限。

但 JavaScript 并没有提供对这些关键字的支持，我们只能依赖变量的作用域来实现封装特性，而且只能模拟出 public 和 private 这两种封装性。

一般我们通过函数来创建作用域：

```javascript
var myObject = (function () {
  var __name = 'sven' // 私有（private）变量
  return {
    // 公开（public）方法
    getName: function () {
      return __name
    },
  }
})()
console.log(myObject.getName()) // sven
console.log(myObject.__name) // undefined
```

#### 封装实现

封装的目的是将信息隐藏，封装应该被视为「任何形式的封装」，也就是说，封装不仅仅是隐藏数据，还包括隐藏实现细节、设计细节以及隐藏对象的类型等。

从封装实现细节来讲，封装使得对象内部的变化对其他对象而言是透明的，也就是不可见的。对象对他自己的行为负责。其他对象或者用户都不关心它的内部实现。封装使得对象之间的耦合变松散，对象之间只通过暴露的 API 接口通信。当我们修改一个对象时，可以随意地修改它的内部实现，只要对外的接口没有变化，就不会影响到程序的其他功能。

#### 封装类型

封装类型是静态类型语言中一种重要的封装方式。一般而言，封装类型是通过抽象类和接口来进行的。把对象的真正类型隐藏在抽象类或者接口之后，相比对象的类型，客户更关心对象的行为。在许多静态语言的设计模式中，想方设法地隐藏对象的类型，也是促使这些模式诞生的原因之一。

当然在 JavaScript 中，并没有对抽象类和接口的支持。在封装类型方面，JavaScript 没有能力也没有必要做得更多。对于 JavaScript 的设计模式实现来说，不区分类型是一种失色，也可以说是一种解脱。

#### 封装变化

从设计模式的角度出发，封装在更重要的层面体现为封装变化。

《设计模式》一书中共归纳总结了 23 种设计模式。从意图上区分，这 23 中模式分别被划分为创建型模式、结构型模式和行为型模式。创建型模式的目的就是封装创建对象的变化，而结构型模式封装的是对象之间的组合关系，行为型模式封装的是对象的行为变化。

通过封装变化的方式，把系统中稳定不变的部分和容易变化的部分隔离开来，在系统的演变过程中，我们只需要替换那些容易变化的部分，如果这些部分是已经封装好的，替换起来也相对容易。这可以最大程度的保证程序的稳定性和可扩展性。

### 1.4 原型模式和基于原型继承的 JavaScript 对象系统

在以类为中心的面向对象编程语言中，类和对象的关系可以想象成铸模和铸件的关系，对象总是从类中创建而来。而在原型编程的思想中，类并不是必需的，对象未必需要从类中创建而来，一个对象是通过克隆另外一个对象所得到的的。

原型模式不单是一种设计模式，也被称为一种编程范型。

#### 使用克隆的原型模式

从设计模式的角度讲，原型模式是用于创建对象的一种模式，如果我们想要创建一个对象，一种方法是先指定它的类型，然后通过类来创建这个对象。原型模式选择了另外一种方式，我们不再关心对象的具体类型，而是找到一个对象，然后通过克隆来创建一个一模一样的对象。

原型模式的实现关键，是语言本身是否提供了 clone 的方法。ECMAScript 5 提供了 `Object.create` 方法，可以用来克隆对象。

在不支持 `Object.create` 方法的浏览器中，则可以使用以下代码：

```javascript
Object.create =
  Object.create ||
  function (obj) {
    var F = function () {}
    F.prototype = obj

    return new F()
  }
```
