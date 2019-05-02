# ideaJQuery
做一个非常简单的jQuery，学习其思想
```html
<ul>
    <li id="item1">选项1</li>
    <li id="item2">选项2</li>
    <li id="item3">选项3</li>
    <li id="item4">选项4</li>
    <li id="item5">选项5</li>
</ul>
```
## 第一版
```javascript
if (value) {
  node.classList.add(key)
} else {
  node.classList.remove(key)
}
// 优化
let methodName = value ? 'add' : 'remove'
node.classList[methodName](key)
```
```javascript
function getSiblings (node) {
  let allChildren = node.parentNode.children
  let array = { length: 0 }
  for (let i = 0; i < allChildren.length; i++) {
    if (allChildren[i] !== node) {
      array[array.length] = allChildren[i]
      array.length += 1
    }
  }
  return array
}

function addClass (node, classes) {
  for (let key in classes) {
    let value = classes[key]
    let methodName = value ? 'add' : 'remove'
    node.classList[methodName](key)
  }
}

window.soadom = {} // 使用命名空间
soadom.getSiblings = getSiblings
soadom.addClass = addClass

soadom.getSiblings(item3)
soadom.addClass(item3, { 'a': true, 'b': false, 'c': true }) 
```



## 第二版

```javascript
window.soadom = {} /*使用命名空间，避免覆盖同名方法*/
soadom.getSiblings = function (node) {
    /*code...*/
}

soadom.addClass = function (node, classes) {
    /*code...*/
}

soadom.getSiblings(item3)
soadom.addClass(item3, { 'a': true, 'b': false, 'c': true })
```
