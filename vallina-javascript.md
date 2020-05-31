# Vallina JavaScript

## Implement of `new`

```javascript
function objectFactory() {
  const obj = new Object()
  const Constuctor = [].shift.call(arguments)
  const res = Constructor.apply(obj, arguments)
  return typeof res === 'object' ? res : obj
}
```

## Implement of `Function.prototype.bind`

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

## Implement of `Function.prototype.call`

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

## Implement of `Function.prototype.apply`

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

## Implement of `Object.create`

```javascript
Object.create = function (obj) {
  const F = function () {}
  F.prototype = obj
  return new F()
}
```

## Implement of `Array.isArray`

```javascript
Array.isArray = function (arr) {
  return Object.prototype.toString.call(arr) === '[object Array]'
}
```

## Implement of `Array.prototype.reduce`

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

## Implement of `Array.prototype.map`

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

## Implement of `Array.prototype.flat`

## Implement of EventEmitter

## Implement of `currying`

## Implement of `throttle`

## Implement of `debounce`

## Implement of deep copy

## Implement of `instance of`

## Implement of `async/await`
