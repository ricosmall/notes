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

