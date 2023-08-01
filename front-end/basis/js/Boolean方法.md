## JS Boolean 方法

> Boolean为JS的标准内置类，Boolean()作为其构造函数，可以将非boolean值转为boolean值，其直接调用的效果等同于!!。

#### 1. 转boolean规则
```javascript
Boolean(0); // false
Boolean(true); // true
Boolean(1); // true
Boolean(""); // false
Boolean("false"); // true
```

#### 2. Boolean() 与 new Boolean()区别
Boolean()的效果相当于!!，而new Boolean()返回一个Boolean类型的对象，其原理是将执行Boolean()后得到的boolean值作为Boolean类型对象的初始值。
```javascript
if(Boolean(expression)){ /* 是否执行视expression转boolean后的值而定 */ }
if(!!expression){ /* 是否执行视expression转boolean后的值而定 */ }
if(new Boolean(expression)){ /* 条件语句恒成立，始终执行 */ }
```
```javascript
const myFalse = new Boolean(false); // myFalse初始值为false
const myTrue = Boolean(myFalse); // myTrue值为true
```