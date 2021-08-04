---
title: JavaScript 对象、原型和类
desc: JavaScript 对象、原型和类
date: 2020-04-13 10:25:21
tags:
  - 开发
  - JS
---

<!--more-->

## 创建对象

### 对象字面量

```javascript
let person = {
  firstName: 'Jack',
  lastName: 'Sparrow',
  age: 30,
  isAdult: function () {
    return this.age > 18
  }
}

// shortand syntax
let firstName = 'Jack'
let lastName = 'Sparrow'

let person = {
  firstName,
  lastName
}

// inspect property names
let propertyNames = Object.keys(person)

for (let propertyName in person) {
  console.log(propertyName)
}
```

### 构造函数

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
  this.age = 0
  this.isAdult = function () {
    return this.age > 18
  }

  let jack = new Person('Jack', 'Sparrow')
}
```

### 对象是否相同

避免使用 `==`

`===` vs `Object.is()`:

- `NaN`
- `-0`

### 属性合并

### 不可变对象

## 对象属性

## 原型

### 类

```javascript

```

## 内建对象
