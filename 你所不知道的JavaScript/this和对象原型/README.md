## this 并不指向函数自身。在任何情况下，它也不指向词法作用域

## this 到底是什么

当一个函数被调用时，会创建一个活动记录（有时候也称为执行上下文）。

这个记录会包含函数在哪里被调用（调用栈）、函数的调用方式、传入的参数等信息。

this就是这个记录的一个属性，会在函数执行的过程中用到

> this实际上是在函数被调用时发生的绑定，它指向什么完全取决于函数在哪里被调用。

# new 做了什么

1. 创建一个空对象，并把该对象的原型关联到构造函数的prototype上。
2. 把this绑定到第一步创建的对象上。
3. 执行构造函数，如果构造函数返回一个对象，则返回该非空对象。反之，则返回第一步的对象。

# this绑定的优先级

1. new 绑定
2. call、apply(显示绑定)或bind(硬绑定)
3. 隐式绑定(通过上下文调用)
4. 默认绑定

使用`call`、`apply`、`bind`时，如果传入一个`null`或`undefined`时，实际应用的是默认绑定。

    如果要判断一个运行中函数的this绑定，就需要找到这个函数的直接调用位置。
    找到之后就可以顺序应用下面这四条规则来判断this的绑定对象。
      1．由new调用？绑定到新创建的对象。
      2．由call或者apply（或者bind）调用？绑定到指定的对象。
      3．由上下文对象调用？绑定到那个上下文对象。
      4．默认：在严格模式下绑定到undefined，否则绑定到全局对象。

# 属性描述符
es5后所有属性都具有`writable(可写)`、`enumerable(可枚举)` 和 `configurable可配置)`。

结合 `writable:false` 和 `configurable:false` 就可以创建一个真正的常量属性(不可修改、重定义或者删除)

## 禁止扩展
使用 `Object.preventExtensions(..)` 会禁止添加新属性，但保留原属性

## 密封
`Object.seal(..)` 会创建一个“密封”的对象，这个方法实际上会在一个现有对象上调用`Object.preventExtensions(..)`并把所有现有属性标记为`configurable:false`。

返回的对象不允许修改、添加属性

## 冻结
`Object.freeze(..)` 会创建一个冻结对象，这个方法实际上会在一个现有对象上调用`Object.seal(..)`并把所有“数据访问”属性标记为`writable:false`，这样就无法修改它们的值。

## [GET]
属性访问，看起来是在查找某个属性。
其实本质是在调用对象的getter函数，判断是否可以找到同名属性，如果找到了就返回，找不到再去原型链上查找。
如果原型链上查找到了就返回原型链上到属性，否则返回undefined

## Getter和Setter
对象默认的[Put]和[Get]操作分别可以控制属性值的设置和获取。

> 最好只使用 for...in 遍历对象, 遍历数组使用 for...of 或者传统的 for

> in和 hasOwnProperty(..) 的区别在于是否查找 [Prototype] 链

# 遍历

**for..in循环可以用来遍历对象的可枚举属性列表 (包括[[Prototype]]链)**