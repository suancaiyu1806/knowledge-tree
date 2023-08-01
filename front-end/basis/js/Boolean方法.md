## JS Boolean 方法

> Boolean 为 JS 的标准内置类，Boolean()作为其构造函数，可以将非 boolean 值转为 boolean 值，其直接调用的效果等同于!!。

#### 1. 转 boolean 规则

```javascript
Boolean(0); // false
Boolean(true); // true
Boolean(1); // true
Boolean(""); // false
Boolean("false"); // true
```

#### 2. Boolean() 与 new Boolean()区别

Boolean()的效果相当于!!，而 new Boolean()返回一个 Boolean 类型的对象，其原理是将执行 Boolean()后得到的 boolean 值作为 Boolean 类型对象的初始值。

```javascript
if (Boolean(expression)) {
  /* 是否执行视expression转boolean后的值而定 */
}
if (!!expression) {
  /* 是否执行视expression转boolean后的值而定 */
}
if (new Boolean(expression)) {
  /* 条件语句恒成立，始终执行 */
}
```

```javascript
const myFalse = new Boolean(false); // myFalse初始值为false
const myTrue = Boolean(myFalse); // myTrue值为true
```
