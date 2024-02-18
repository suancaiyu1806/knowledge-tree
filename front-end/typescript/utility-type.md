## TypeScript Utility Types

#### 1. Awaited\<Type\>

```typescript
type A = Awaited<Promise<string>>;

type A = string;

type B = Awaited<Promise<Promise<number>>>;

type B = number; // 多级Promise会得到最终类型

type C = Awaited<boolean | Promise<number>>;

type C = number | boolean;
```

#### 2. Partial\<Type\>

构造一个类型，将源类型的所有属性都设置为可选。

```typescript
interface Todo {
  title: string;
  description: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}

const todo1 = {
  title: "organize desk",
  description: "clear clutter",
};

const todo2 = updateTodo(todo1, {
  description: "throw out trash",
});
```

#### 3. Require\<Type\>

与 Partial 相反，将源类型的所有属性都设置为必须。

```typescript
interface Props {
  a?: number;
  b?: string;
}

const obj: Props = { a: 5 };

const obj2: Required<Props> = { a: 5 };
// Error: Property 'b' is missing in type '{ a: number; }' but required in type 'Required<Props>'.
```

#### 4. Readonly\<Type\>

将源类型的属性设置为只读属性，不可更改。

```typescript
interface Todo {
  title: string;
}

const todo: Readonly<Todo> = {
  title: "Delete inactive users",
};

todo.title = "Hello";
// Error: Cannot assign to 'title' because it is a read-only property.
```

#### 5. Pick\<Type, Keys\>

从源类型中选取部分属性。

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```

#### 6. Omit\<Type, Keys\>

与 Pick 相反，从源类型中移除部分属性。

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
  createdAt: number;
}

type TodoPreview = Omit<Todo, "description">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
  createdAt: 1615544252770,
};

type TodoInfo = Omit<Todo, "completed" | "createdAt">;

const todoInfo: TodoInfo = {
  title: "Pick up kids",
  description: "Kindergarten closes at 5pm",
};
```

#### 7. Exclude\<UnionType, ExcludedMembers\>

类似 Omit，从源类型中排除部分属性，区别是 Exclude 的排除支持“模糊匹配”。

```typescript
type T0 = Exclude<"a" | "b" | "c", "a">;

type T0 = "b" | "c";

type T1 = Exclude<"a" | "b" | "c", "a" | "b">;

type T1 = "c";

type T2 = Exclude<string | number | (() => void), Function>;

type T2 = string | number;

type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; x: number }
  | { kind: "triangle"; x: number; y: number };

type T3 = Exclude<Shape, { kind: "circle" }>;

type T3 =
  | { kind: "square"; x: number }
  | { kind: "triangle"; x: number; y: number };
```

#### 8. Extract\<Type, Union\>

与 Exclude 相反，找出源类型中符合条件的类型，支持“模糊匹配”。

```typescript
type T0 = Extract<"a" | "b" | "c", "a" | "f">;

type T0 = "a";

type T1 = Extract<string | number | (() => void), Function>;

type T1 = () => void;

type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; x: number }
  | { kind: "triangle"; x: number; y: number };

type T2 = Extract<Shape, { kind: "circle" }>;

type T2 = {
  kind: "circle";
  radius: number;
};
```

#### 9. NonNullable\<Type\>

移除源类型中的空项(含 null 与 undefined)。

```typescript
type T0 = NonNullable<string | number | undefined>;

type T0 = string | number;

type T1 = NonNullable<string[] | null | undefined>;

type T1 = string[];
```

#### 10. Parameters\<Type\>

获取函数参数参数数组类型，传入非函数会报错。

```typescript
type T0 = Parameters<() => string>;

type T0 = [];

type T1 = Parameters<(s: string) => void>;

type T1 = [s: string];

type T2 = Parameters<<T>(arg: T) => T>;

type T2 = [arg: unknown];

declare function f1(arg: { a: number; b: string }): void;
type T3 = Parameters<typeof f1>;

type T3 = [arg: { a: number; b: string }];

type T4 = Parameters<any>;

type T4 = unknown[];

type T5 = Parameters<never>;

type T5 = never;

type T6 = Parameters<string>;
// Error: Type 'string' does not satisfy the constraint '(...args: any) => any'.

type T6 = never;

type T7 = Parameters<Function>;
// Error: Type 'Function' does not satisfy the constraint '(...args: any) => any'.
// Error: Type 'Function' provides no match for the signature '(...args: any): any'.

type T7 = never;
```

#### 11. ConstructorParameters

获取构造函数参数数组类型。

```typescript
type T0 = ConstructorParameters<ErrorConstructor>;

type T0 = [message?: string];

type T1 = ConstructorParameters<FunctionConstructor>;

type T1 = string[];

type T2 = ConstructorParameters<RegExpConstructor>;

type T2 = [pattern: string | RegExp, flags?: string];

class C {
  constructor(a: number, b: string) {}
}
type T3 = ConstructorParameters<typeof C>;

type T3 = [a: number, b: string];

type T4 = ConstructorParameters<any>;

type T4 = unknown[];

type T5 = ConstructorParameters<Function>;
// Error: Type 'Function' does not satisfy the constraint 'abstract new (...args: any) => any'.
// Error: Type 'Function' provides no match for the signature 'new (...args: any): any'.

type T5 = never;
```

#### 12. ReturnType\<Type\>

获取函数的返回类型。

```typescript
declare function f1(): { a: number; b: string };

type T0 = ReturnType<() => string>;

type T0 = string;

type T1 = ReturnType<(s: string) => void>;

type T1 = void;

type T2 = ReturnType<<T>() => T>;

type T2 = unknown;

type T3 = ReturnType<<T extends U, U extends number[]>() => T>;

type T3 = number[];

type T4 = ReturnType<typeof f1>;

type T4 = {
  a: number;
  b: string;
};

type T5 = ReturnType<any>;

type T5 = any;

type T6 = ReturnType<never>;

type T6 = never;

type T7 = ReturnType<string>;
// Error: Type 'string' does not satisfy the constraint '(...args: any) => any'.

type T7 = any;

type T8 = ReturnType<Function>;
// Error: Type 'Function' does not satisfy the constraint '(...args: any) => any'.
// Error: Type 'Function' provides no match for the signature '(...args: any): any'.

type T8 = any;
```

#### 13. InstanceType\<Type\>

获取对象实例对应类。

```typescript
class C {
  x = 0;
  y = 0;
}
type T0 = InstanceType<typeof C>;

type T0 = C;

type T1 = InstanceType<any>;

type T1 = any;

type T2 = InstanceType<never>;

type T2 = never;

type T3 = InstanceType<string>;
// Error: Type 'string' does not satisfy the constraint 'abstract new (...args: any) => any'.

type T3 = any;

type T4 = InstanceType<Function>;
// Error: Type 'Function' does not satisfy the constraint 'abstract new (...args: any) => any'.
// Error: Type 'Function' provides no match for the signature 'new (...args: any): any'.

type T4 = any;
```

#### 14. ThisParameterType\<Type\>

获取函数 this 参数的类型，无 this 参数则为 unknown。注意 ThisParameterType 的参数为类型，因此需要取函数的 type。

_注：apply 将函数中的 this 置为 apply 方法传入的参数，且会执行函数。_

```typescript
function toHex(this: Number) {
  return this.toString(16);
}

function numberToString(n: ThisParameterType<typeof toHex>) {
  return toHex.apply(n);
}
```

#### 15. OmitThisParameter\<Type\>

移除函数的 this 参数的类型。

_注：bind 即将函数中的 this 置为 apply 方法传入的参数，返回一个新的指定了 this 的函数。_

```typescript
function toHex(this: Number) {
  return this.toString(16);
}

const fiveToHex: OmitThisParameter<typeof toHex> = toHex.bind(5);

console.log(fiveToHex());
```

#### 16. ThisType\<Type\>

定义方法中 this 的类型。

```typescript
// 这里D为data的类型，M为methods的类型，M&ThisType<D & M>即将函数类型指定为M后同时指定了其this的类型。
type ObjectDescriptor<D, M> = {
  data?: D;
  methods?: M & ThisType<D & M>; // Type of 'this' in methods is D & M
};

function makeObject<D, M>(desc: ObjectDescriptor<D, M>): D & M {
  let data: object = desc.data || {};
  let methods: object = desc.methods || {};
  return { ...data, ...methods } as D & M;
}

let obj = makeObject({
  data: { x: 0, y: 0 },
  methods: {
    moveBy(dx: number, dy: number) {
      this.x += dx; // Strongly typed this
      this.y += dy; // Strongly typed this
    },
  },
});

obj.x = 10;
obj.y = 20;
obj.moveBy(5, 5);
```

#### 17. Intrinsic String Manipulation Types

针对字符串类型的操作。

- Uppercase<StringType> // 大写

```typescript
type Greeting = "Hello, world";
type ShoutyGreeting = Uppercase<Greeting>;

type ShoutyGreeting = "HELLO, WORLD";

type ASCIICacheKey<Str extends string> = `ID-${Uppercase<Str>}`;
type MainID = ASCIICacheKey<"my_app">;

type MainID = "ID-MY_APP";
```

- Lowercase<StringType> // 小写

```typescript
type Greeting = "Hello, world";
type QuietGreeting = Lowercase<Greeting>;

type QuietGreeting = "hello, world";

type ASCIICacheKey<Str extends string> = `id-${Lowercase<Str>}`;
type MainID = ASCIICacheKey<"MY_APP">;

type MainID = "id-my_app";
```

- Capitalize<StringType> // 首字母大写

```typescript
type LowercaseGreeting = "hello, world";
type Greeting = Capitalize<LowercaseGreeting>;

type Greeting = "Hello, world";
```

- Uncapitalize<StringType> // 首字母小写

```typescript
type UppercaseGreeting = "HELLO WORLD";
type UncomfortableGreeting = Uncapitalize<UppercaseGreeting>;

type UncomfortableGreeting = "hELLO WORLD";
```

#### _附_ $I$、示例解析

```typescript
type MenuItem = Required<MenuProps>["items"][number];
```

- 首先将 MenuProps 中属性均设置为必须，保证 items 是有的；
- 再取出 MenuProps 中的 items 属性（items 为数组）；
- 最后用 number 索引出数组中 item 的类型。
