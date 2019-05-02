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

因为可能会造成同名函数的覆盖，所以使用原型链和匿名函数来避免函数覆盖

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



## 第三版

使用命名空间来调用函数，调用太麻烦了，使用原型链配合this来简化

```javascript
Node.prototype.getSiblings = function () {
  let allChildren = this.parentNode.children
  let array = { length: 0 }
  for (let i = 0; i < allChildren.length; i++) {
    if (allChildren[i] !== this) {
      array[array.length] = allChildren[i]
      array.length += 1
    }
  }
  return array
}

Node.prototype.addClass = function (classes) {
  for (let key in classes) {
    let value = classes[key]
    let methodName = value ? 'add' : 'remove'
    this.classList[methodName](key)
  }
}

item3.getSiblings()
item3.addClass({ 'a': true, 'b': false, 'c': true })
```



## 第四版

直接在原型链添加，会将原型链该乱，而且可能会有冲突，所以自己创一个对象最好

```javascript
window.soa = function (node) {
  return {
    getSiblings: function () {
      let allChildren = node.parentNode.children
      let array = { length: 0 }
      for (let i = 0; i < allChildren.length; i++) {
        if (allChildren[i] !== node) {
          array[array.length] = allChildren[i]
          array.length += 1
        }
      }
      return array
    },
    addClass: function (classes) {
      for (let key in classes) {
        let value = classes[key]
        let methodName = value ? 'add' : 'remove'
        node.classList[methodName](key)
      }
    }
  }
}

let newdom = soa(item3)
newdom.getSiblings()
newdom.addClass({ 'a': true, 'b': false, 'c': true })
```



## 第五版

增加使用选择器来选择，现在可以使用id和选择器两种方法来选择元素

```javascript
window.soa = function (nodeOrSelector) {
  let node
  if (typeof nodeOrSelector == 'string') {
    node = document.querySelector(nodeOrSelector)
  } else {
    node = nodeOrSelector
  }
  return {
    getSiblings: function () {
      /*code...*/
    },
    addClass: function (classes) {
      /*code...*/
    }
  }
}

let newdom = soa('ul > li:nth-child(3)')
newdom.getSiblings()
newdom.addClass({ 'red': true, 'b': false, 'c': true })
```



## 第六版

如果选择器选出是一个集合，也可以添加属性

```javascript
window.soa = function (nodeOrSelector) {
  let nodes = {}
  if (typeof nodeOrSelector == 'string') {
    let temp = document.querySelectorAll(nodeOrSelector)
    for (let i = 0; i < temp.length; i++) { // 使用object原型链
      nodes[i] = temp[i]
    }
    nodes.length = temp.length
  } else if (nodeOrSelector instanceof Node) {
    nodes = { 0: nodeOrSelector, length: 1 }
  }

  nodes.addClass = function (classes) {
    for (let key in classes) {
      let value = classes[key]
      let methodName = value ? 'add' : 'remove'
      for (let i = 0; i < nodes.length; i++) {
        nodes[i].classList[methodName](key)
      }
    }
  }

  return nodes
}

let newdom = soa('ul > li')
newdom.addClass({ 'red': true, 'b': false, 'c': true })
```



## 总结

jQuery是真的强，就算不用但是它的思想将会一直陪伴这我们。这次小小的尝试，也让我获益匪浅！