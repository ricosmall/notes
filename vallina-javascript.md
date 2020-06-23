# JavaScript 各种方法手写实现

## 实现 `new` 的过程

```javascript
function objectFactory() {
  const obj = new Object()
  const Constuctor = [].shift.call(arguments)
  const res = Constructor.apply(obj, arguments)
  return typeof res === 'object' ? res : obj
}
```

## 实现 `Function.prototype.bind`

```javascript
Function.prototype.bind = function () {
  const self = this
  const context = [].shift.call(arguments)
  const prevArgs = [].slice.call(arguments)
  return function () {
    const args = [].concat.call(prevArgs, arguments)
    return self.apply(context, args)
  }
}
```

## 实现 `Function.prototype.call`

```javascript
Function.prototype.call = function (context, args) {
  const self = this
  if (context === undefined || context === null) {
    return self(...args)
  }
  context.__fn__ = self
  const result = context.__fn__(...args)
  delete context.__fn__
  return result
}
```

## 实现 `Function.prototype.apply`

```javascript
Function.prototype.apply = function (context, args) {
  const self = this
  if (context === undefined || context === null) {
    return self(args)
  }
  context.__fn__ = self
  const result = context.__fn__(args)
  delete context.__fn__
  return result
}
```

## 实现 `Object.create`

```javascript
Object.create = function (obj) {
  const F = function () {}
  F.prototype = obj
  return new F()
}
```

## 实现 `Array.isArray`

```javascript
Array.isArray = function (arr) {
  return Object.prototype.toString.call(arr) === '[object Array]'
}
```

## 实现 `Array.prototype.reduce`

```javascript
Array.prototype.reduce = function (fn, initial) {
  const arr = this
  const acc = initial || arr[0]
  const start = initial ? 0 : 1

  for (let i = start; i < arr.length; i++) {
    acc = fn(acc, arr[i], i, arr)
  }

  return acc
}
```

## 实现 `Array.prototype.map`

```javascript
Array.prototype.map = function (fn) {
  const arr = this
  const result = []
  for (let i = 0; i < arr.length; i++) {
    result[i] = fn(arr[i])
  }
  return result
}
```

## 实现 `Array.prototype.flat`

```javascript
Array.prototype.flat = function (depth) {
  const source = this
  const sourceLen = source.length
  const depthNum = depth > 0 ? Number(depth) : 1
  const flatten = (target, source, sourceLen, start, depth) => {
    let targetIndex = start
    let sourceIndex = 0
    while (sourceIndex < sourceLen) {
      const element = source[sourceIndex]
      let shouldFlatten = false
      if (depth > 0) shouldFlatten = Array.isArray(element)
      if (shouldFlatten) {
        let elementLen = element.length
        targetIndex = flatten(
          target,
          element,
          elementLen,
          targetIndex,
          depth - 1
        )
      } else {
        target[targetIndex] = element
        targetIndex++
      }
      sourceIndex++
    }
    return targetIndex
  }
  const result = []
  flatten(result, source, sourceLen, 0, depthNum)
  return result
}
```

## 实现一个 `EventEmitter`

```javascript
class EventEmitter {
  constructor() {
    this.events = {}
  }
  on(type, handler) {
    if (this.events[type] && !this.events[type].includes(handler)) {
      this.events[type].push(handler)
    }
    {
      this.events[type] = [handler]
    }
  }
  once(type, handler) {
    const newHandler = () => {
      const args = Array.prototype.slice.call(arguments)
      handler.call(this, ...args)
      this.off(type, handler)
    }
    this.on(type, newHandler)
  }
  off(type, handler) {
    if (this.events[type] && this.events[type].includes(handler)) {
      const index = this.events[type].indexOf(handler)
      this.events[type].splice(index, 1)
    }
  }
  emit(type) {
    if (this.events[type]) {
      const args = Array.prototype.slice.call(arguments)
      args.shift()
      this.events[type].forEach((handler) => {
        handler.call(this, ...args)
      })
    }
  }
}
```

## 实现 `currying`

```javascript
function currying(fn) {
  const args = []
  return function () {
    if (arguments.length === 0) {
      return fn.apply(this, args)
    } else {
      ;[].push.call(args, arguments)
      return arguments.callee
    }
  }
}
```

## 实现 `uncurrying`

```javascript
function uncurrying() {
  const self = this
  return function () {
    const obj = [].shift.call(arguments)
    return self.apply(obj, arguments)
  }
}
```

## 实现 `throttle`

```javascript
function throttle(fn, delay = 500) {
  let flag = true
  return () => {
    if (!flag) return
    flag = false
    setTimeout(() => {
      fn.apply(this, arguments)
      flag = true
    }, delay)
  }
}
```

## 实现 `debounce`

```javascript
function debounce(fn, delay = 500) {
  let timer = null
  return (...args) => {
    clearTimeout(timer)
    timer = setTimeout(() => {
      fn.apply(this, args)
    }, delay)
  }
}
```

## 实现 `deepClone`

## 实现 `instance of`

```javascript
function instanceOf(L, R) {
  const O = R.prototype
  L = L.__proto__
  while (true) {
    if (L === null) return false
    if (L === O) return true
    L = L.__proto__
  }
}
```

## 实现 `async/await`
