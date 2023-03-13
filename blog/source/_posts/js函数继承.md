---
title: js函数继承
tags: 学习
categories: 前端
typora-copy-images-to: upload
top: 122
---

### 原型链继承
<!--more-->
```javascript
function Parent(){
  this.name = 'parent'
}

Parent.prototype.getName = function(){
  return this.name
}

function Child(){
  thia.age = '18'
}

Child.prototype = new Parent

var child = new Child

console.log(child.getName)//parent
console.log(child.age)//18


```

### 构造函数继承（只能继承实例的属性和方法，不能继承原型对象上的属性和方法）

```javascript
function Parent(){
  this.name = 'parent'
}

function Child(){
  Parent.call(this)
  this.age = '18'
}

var child = new Child
console.log(child.name)//parrent
sonsole.log(child.age)//18
```

### 组合继承

```javascript
function Parent(){
  this.name = 'parent'
}

Parent.prototype.getName = function(){
  return this.name
}

function Child(){
  Parent.call(this)
  this.age = '18'
}

Child.prototype = Object.create(Parent.prototype)
Child.prototype.construtor = Child

var child = new Child
console.log(child.name)//parent
console.log(child.age)//age
console.log(child.gatName)//parent
```
