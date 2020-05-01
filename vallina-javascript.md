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

