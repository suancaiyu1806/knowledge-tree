## SASS @at-root

> @at-root可以完全突破嵌套结构，到达嵌套树的“root”


#### 1. 基本用法
```scss
.grand-parent {
  .parent {
    @at-root .child {}
  }
}
// 编译后
.child {}
```

#### 2. 实际使用
```scss
.grand-parent {
  .parent {
    @at-root body.page-about .child {}
  }
}
// 编译后
body.page-about .child {}
```