# TS 高级类型

ts 的高级类型有很多，重点学习一下高级类型：

1. class 类
2. 类型兼容性
3. 交叉类型
4. 泛型 和 keyof
5. 索引签名类型 和 左营查询类型
6. 映射类型

## 1. class 类

ts 全面支持 es2015 中引入的 class 关键字，并为其添加了类型注解和其他语法（比如，可见性修饰符等）

class 基本使用：

```ts
class Person {}

const p: Person;
const p = new Person();
```

解释：

1. 根据 TS 中的类型推论，可以知道 Person 类的实例对象 p 的类型是 Person
2. TS 中的 class，`不仅提供了class的语法功能，也作为另一种类型的存在`

**1.1 实例属性初始化：**

```ts
class Person {
  age: number;
  gender = "男"; //gender: string = '男'
}

const p = new Person();

p.age;
p.gender;
```

解释：

1. 声明成员 age，类型为 number（没有初始值）
2. 声明成员 gender，并设置初始值，此时，刻省略雷静注解（TS 类型推论为 string 类型）

**1.2 构造函数**

用来实例函数的初始化

```ts
class Person {
  age: number;
  gender: string;

  constructor(age: number, gender: string) {
    this.age = age;
    this.gender = gender;
  }
}

new Person(18, "男");
```

解释：

1. 成员初始化（比如，age：number）后，才可以通过 this.age 来访问实例成员
2. 需要为构造函数指定类型注解，否则会被隐式推断为 any，构造函数不需要返回值类型

**1.3 实例方法**

```ts
class Point {
  x = 10;
  y = 20;

  scale(n: number): void {
    this.x *= n;
    this.y *= n;
  }
}

const p = new Point();

p.scale(10);

console.log(p.x, p.y); //100  200
```

解释：方法的类型注解（参数和返回值）与函数用法相同。

`类继承`的两种方式：

1. extends（继承父类）
2. implements（实现接口）

说明：JS 中只有 extends，而 implements 是 TS 提供的

- extends 继承

```ts
class Animal {
  move() {
    consle.log("Moving along!");
  }
}
class Dog extends Animal {
  name = "二哈";
  bark() {
    console.log("汪！");
  }
}
const dog = new Dog();
dog.move(); //Moving along!
dog.bark(); //汪！
console.log(dog.name); // 二哈
```

解释：

1. 通过 `extends` 关键字实现继承
2. 子类 Dog 继承父类 Animal，则 Dog 的实例对象就同时具有了父类 Animal 和子类 Dog 的所有属性和方法

- implements 实现接口

```ts
interface Singale {
  sing(): void;
}

class Person implements Singale {
  name = "jack";
  sing() {
    console.log("雨下整夜");
  }
}
```

解释：

1. 通过 `implements` 关键字让 class 实现接口
2. Person 类实现接口 Singale 意味着，Person 类中必须提供 Singale 接口中指定的所有哦方法和属性

### `类成员可见性：`

可以使用 TS 来`控制class的方法或属性对于class外的代码是否可见`.

可见性修饰符包括：

1. public（公有的）
2. protected（受保护的）
3. private（私有的）

### public：公开的、公有的，`公有成员可以被任何地方访问`，默认可见性。

```ts
class Animal {
  public move() {
    console.log("Moving along!");
  }
}
```

解释：

1. 在类属性或方法前面添加 public 关键字，来修饰该属性或方法是公有的
2. 因为 public 是默认可见性，所以可以`直接省略`

### protected: 受保护的，仅对其声明所在类和子类中（非实例对象）可见。

```ts
class Animal {
  protected move() { console.log('Moving!')}
}
class Dog extends Animal {
  console.log('汪！')
  this.move()
}
```

解释：

1. 在类属性或方法前面添加 protected 关键字，来修饰该属性或方法是受保护的
2. 在子类的方法内部可以通过 this 哎访问父类中受保护的成员，但是`对实例不可见！`

### private：私有的，`只在当前类中可见`，对实例对象以及子类也是不可见的。

```ts
class Animal {
  privata move() { console.log('Moving along!')}
  walk() {
    this.move()
  }
}
```

解释：

1. 在类属性或方法前面添加 private 关键字，来修饰该属性或方法是私有的
2. 私有的属性或方法只在当前类中可见，对子类和实例对象也哦度是不可见的

除了可见性修饰符之外，还有一个常见的修饰符就是：`readonly(只读修饰符)`

### readonly：表示只读，`用来防止构造函数之外对属性进行赋值`

```ts
class Person {
  readonly age: number = 18;
  constructor(age: number) {
    this.age = age;
  }
  // setAge() {
  //   this.age = 20; //无法分配到age，因为是只读属性
  // }
}
```

解释：

1. 使用`readonly`关键字修饰该属性是只读的，注意`只能修饰属性不能修饰方法`
2. 注意：属性 age 后面的类型注解（比如，此处的 number）如果不加，则 age 的类型为 18（字面量类型）
3. `接口或者{}表示的对象类型，也可以使用 readonly`
4. 只要是 readonly 来修饰的属性，必须手动提供明确的类型

```ts
interface IPerson {
  readonly name: string;
}

let obj: IPerson = {
  name: "jack",
};

obj.name = "rose"; //无法分配到name，因为是只读的
```

或者

```ts
let obj: { readonly name: string } = {
  name: "luoluo",
};

obj.name = "rose"; //无法分配到name，因为是只读的
```

## 2. 类型兼容性

两种类型系统：

1. Structural Type System(结构化类型系统)
2. Nominal Type Syetem(标明类型系统)

`TS 采用的是结构化类型系统`，也叫作 duck typing（鸭子类型），`类型检查关注的是值所具有的形状`。

也就是说，在结构类型系统中，如果两个对象具有相同的形状，则认为它们鼠疫同一类型。

```ts
//两个类的兼容性演示
class Point { x: number, y: number }
class Point2D { x: number, y: number }

const p: Point = new Point2D()
```

解释：

1. Point 和 Point2D 是两个名称不同的类
2. 变量 p 的类型被显示标注为 Point 类型，但是，它的值却是 Point2D 的实例，并且没有类型错误
3. 因为 TS 是结构化类型系统，只检查 Point 和 Point2D 的结构是否相同（相同，都具有 x 和 y 两个属性，属性类型也相同）
4. 但是，如果在 Nominal Type Syetem 中（比如，c#，Java 等），他们是不同的类，类型无法兼容

```ts
//演示类型兼容性
let arr = ["a", "b", "c"];

arr.forEach((item) => {});
arr.forEach((item, index) => {});
arr.forEach((item, index, array) => {});
```

注意：在结构化类型系统中，如果两个对象具有相同的形状，则认为他们属于同一类型，这种说法并不准确。

`更准确的说法：对于对象类型来说，y的成员至少与x相同，则x兼容y（成员多的可以赋值给少的）`

```ts
class Point {
  x: number;
  y: number;
}
class Point3D {
  x: number;
  y: number;
  z: number;
}
const p: Point = new Point3D();
```

解释：

1. Point3D 的成员`至少`与 point 相同，则 point 兼容 point3D
2. 所以，成员多的 point3D 可以赋值给成员少的 point

除了 class 之外，TS 中的其他类型也存在相互兼容的情况，包括：

1. 接口兼容性
2. 函数兼容性等

- `接口之间的兼容性，类似于class`，并且，class 和 interface 之间也可以兼容

```ts
interface Point {
  x: number;
  y: number;
}
interface Point2D {
  x: number;
  y: number;
}
let p1: Point;
let p2: Point2D = p1;

interface Point3D {
  x: number;
  y: number;
  z: number;
}
let p3: Point3D;
p2 = p3;

//类和接口之间也是兼容的
class Point4D {
  x: number;
  y: number;
}
let p4: Point2D = new Point4D();
```

- `函数之间的兼容性比较复杂，`需要考虑：

1. 参数个数
2. 参数类型
3. 返回值类型

### **1.参数个数**：参数多的兼容参数少的（`参数少的可以赋值给多的`）

```ts
type F1 = (a: number) => void;
type F2 = (a: number, b: number) => void;
let f1: F1;
let f2: F2 = f1;
```

```ts
const arr = ["a", "b", "c"];
arr.forEach(() => {});
arr.forEach((item) => {});
```

解释：

1. 参数少的可以赋值给参数多的，所以 f1 可以赋值给 f2
2. 数组中 forEach 方法的第一个参数是回调函数，该示例类型中类型为：(value:string, index:number, array:string[]) => void
3. `在JS中省略用不到的函数参数实际上是常见的，这样的使用方式，促成了TS中函数类型之间的兼容性`
4. 并且因为回调函数是有类型的，所以，TS 会自动推导出参数 item、index、array 的类型

### **2.参数类型**：相同位置的参数类型要相同（原始类型）或兼容（对象类型）

```ts
// 原始类型
type F1 = (a: number) => string;
type F2 = (a: number) => string;
let f1: F1;
let f2: F2 = f1;
```

解释：函数类型 F2 兼容函数类型 F1，因为 F1 和 F2 的第一个参数类型相同

```ts
//对象类型
interface Point2D {
  x: number;
  y: number;
}
interface Point3D {
  x: number;
  y: number;
  z: number;
}
type F2 = (p: Point2D) => void;
type F3 = (p: POint3D) => void;
let f2: F2;
let f3: F3 = f2;
```

解释：

1. 注意：此处与前面讲到的接口兼容性冲突
2. 技巧：`将对象拆开，把每个属性看做一个个参数`，则，参数少的 f2 可以赋值给参数多的 f3

### **3.返回值类型**：只关注返回值类型本身即可

```ts
//返回值类型是原始类型
type F5 = () => string;
type F6 = () => string;
let f5: F5;
let f6: F6 = f5;

//返回值是对象类型
type F7 = () => { name: string };
type F8 = () => { name: string; age: number };
let f7: F7;
let f8: F8;
f7 = f8;
```

解释：

1. 如果返回值类型是原始类型，此时两个类型要相同，比如，F5 和 F6
2. 如果返回值类型是对象类型，此时成员多的可以赋值给成员少的，比如，F7 和 F8

## 3. 交叉类型 &

功能类似于接口继承 extends，`用于组合多个类型为一个类型`（常用于对象类型）

比如：

```ts
interface Person { name: string }
interface Contact { phone: string }
type PersonDetail = Person & Contact
let obj: PersonDetail = {
  name: 'jack'
  phone: '13552.....'
}
```

解释：使用交叉类型后，新的类型 PersonDetail 就`同时具备`了 Person 和 Contact 的所有属性类型

相当于：

```ts
type PersonDetail = { name: string; phone: string };
```

交叉类型（&）和接口继承（extends）的对比：

- 相同点：都可以实现对象类型的组合
- 不同点：两种方式实现类型组合时，对于`同名属性之间，处理类型冲突的方式不同`

```ts
interface A {
  fn: (value: number) => string;
}
interface B extends A {
  fn: (value: string) => string;
}
//报错，类型不兼容
```

```ts
interface A {
  fn: (value: number) => string;
}
interface B {
  fn: (value: string) => string;
}
type C = A & B;
// 没有错误
// 简单理解为：fn: (value: string | number) => string
```

## 4. 泛型 \*

泛型是可以`在保证类型安全`前提下，`让函数等与多种类型一起工作`，从而`实现复用`，常用于：`函数、接口、class`中

需求：创建一个 id 函数，传入什么数据就返回该数据本身（也即是说，参数和返回值类型相同）

```ts
function id(value: number): number { retrn valeue }
```

比如，id(10)调用以上函数就会直接返回 10 本身。但是，该函数只接收数值类型，无法用于其他类型。

为了能让函数接受任意类型，可以将参数类型修改为 any。但是，这样就失去了 TS 的类型保护，类型不安全。

```ts
function id(value: any): any {
  return value;
}
```

**泛型在保证类型安全**（不丢失信息）的同时，可以**让函数等与多种不同的类型一起工作**，灵活可**复用**。

实际上，在 c#和 Java 等编程语言中，泛型都是用来实现可复用组件功能的主要工具之一。

**`1.创建泛型函数：`**

```ts
function id<Type>(value: Type): Type {
  return value;
}
```

解释：

1. 语法：在函数名称的后面添加 `<>` (尖括号)，`尖括号中添加类型变量`，比如此处的 type
2. `类型变量` Type，`是一种特殊类型的变量，它处理类型`而不是值
3. 该类型变量相当于一个类型容器，能过捕获用户提供的类型（具体是什么类型由用户调用该函数时指定）
4. 因为 Type 是类型，因此可以将其作为函数参数和返回值的类型，表示参数和返回值具有相同的类型。
5. 类型变量 Type，可以是任意合法的变量名称。

**`2.调用泛型函数：`**

```ts
function id<Type>(value: Type): Type {
  return value;
}

// 以 number 类型调用泛型函数
const num: number;
const num = id<number>(10);

// 以 string 类型调用泛型函数
const str: string;
const str = id<string>("a");
```

解释：

1. 语法：在函数名称的后面天添加 `<>`(尖括号)，`尖括号中指定具体的类型`，比如，此处的 number
2. 当传入类型 number 后，这个类型就会被函数声明时指定的类型变量 Type 捕获到
3. 此时，Type 的类型就是 number，所以，函数 id 参数和返回值的类型也都是 number

同样，如果传入类型 string，函数 id 参数和返回值的类型就都是 string。

这样通过泛型就做到了让 id 函数与多种不同的类型一起工作，`实现了复用的同时保证了类型安全`。

**`3.简化调用泛型函数：`**

```ts
function id<Type>(value: Type): Type {
  return value;
}

let str = id("abc");
let num = id(10);
```

解释：

1. 在调用泛型函数时，`可以省略<类型>来建华泛型函数的调用`
2. 此时，TS 内部会采用一种叫做 类型参数推断 的机制，来根据传入的实参自动推断出类型变量 Type 的类型
3. 比如，传入实参 10，TS 会自动推断出变量 num 的类型 number，并作为 Type 的类型

推荐：使用这种简化的方式调用泛型函数，使代码更短，更易于阅读

说明：当编译器无法推断类型或者推断的类型不准确时，就需要显式的传入类型参数

### 4.1 泛型约束

默认情况下，泛型函数的类型变量 Type 可以代表多个类型，这导致无法访问任何属性。

比如，id('a')调用函数时获取参数的长度：

```ts
function id<Type>(value: Type): Type {
  console.log(value.length); //无法访问任何属性
  return value;
}
```

解释：Type 可以代表任意类型，无法确定一定存在 length 属性，比如 number 类型就没有 length。

此时就需要为泛型`添加约束`来**收缩类型**（缩窄类型取值范围）。

主要方式：

1. `指定更加具体的类型`

```ts
function id<Type>(value: Type[]): Type[] {
  console.log(value.length);
  return value;
}
```

比如，将类型修改为 Type[]（Type 类型的数组），因为只要是数组就一定存在 length 属性，因此就可以访问了。

2. `添加约束`

```ts
interface ILength {
  length: number;
}
function id<Type extends ILength>(value: Type): Type {
  console.log(value.length);
  return value;
}

id(["a", "b"]);
id("abc");
id({ length: 10, name: "jack" });
```

解释：

1. 创建描述约束的接口 ILength，该接口要求提供 length 属性
2. 通过`extends`关键字使用该接口，为泛型（类型变量）添加约束
3. 该约束表示：`传入的类型必须具有length属性`

注意：传入的实参（比如，数组）只要有 length 属性即可，也符合接口的类型兼容性

**`keyof`**

泛型的类型变量可以有多个，并且`类型变量之间还可以约束`（比如，第二个类型变量受第一个类型变量约束）

比如，创建一个函数来获取对象中属性的值：

```TS
function getProp<Type, key extends keyof Type>(obj: Type, key: Key) {
  return obj[key]
}

getProp({ name: 'jack', age: 18 }, 'age')
getProp({ name: 'jack', age: 18 }, 'name')

// 几乎不常用
getProp(18, 'toFixed')
getProp('abc', 'split')
getProp('abc', 1) // 1 表示索引
getProp(['a'], 'length')
getProp(['a'], 1000)

console.log('object'[1])  // b
```

解释：

1. 添加了第二个类型变量 Key，两个类型变量之间使用（,）逗号分隔
2. `keyof`关键字`接收一个对象类型，生成其键名称（可能是字符串或数字）的联合类型`
3. 本示例中 keyof Type 实际上获取的是 person 对象所有键的联合类型，也就是'name'|'age'
4. 类型变量 Key 受 Type 约束，可以理解为：Key 只能是 Type 所有键中的任意一个，或者说只能访问对象中存在的属性

**`泛型接口：`**

接口也可以配合泛型来使用，以增加其灵活性，增强其复用性。

```ts
interface IdFunc<Type> {
  id: (value: Type) => Type;
  ids: () => Type[];
}

let obj: IdFunc<number> = {
  id(value) {
    return value;
  },
  ids() {
    return [1, 3, 5];
  },
};
```

解释：

1. 在接口名称后面添加 `<类型变量>` ，那么在这个接口就变成了泛型接口
2. 接口的类型变量，对接口中所有其他成员可见，也就是`接口中所有成员都可以使用类型变量`
3. 使用接口泛型时，`需要显式指定具体的类型`（比如，此处的 IdFunc`<number>`）
4. 此时，id 方法的参数和返回值类型都是 number；ids 方法的返回值类型是 number[]

实际上，JS 中的数组在 TS 中就是一个`泛型接口`。

在使用数组的时候，TS 会根据数组的不同类型，来自动将类型变量设置为相应的类型。

技巧：可以通过 Ctrl + 鼠标左键（Mac：option + 鼠标左键）来查看具体的类型信息。

**`泛型类`**

class 也可以配合泛型来使用。

比如，React 的 class 组件的基类 component 就是泛型类，不同的组件有不同的 props 和 state。

```ts
interface IState {
  count: number;
}
interface IProps {
  maxLength: number;
}
class InputCount extends React.Component<IProps, IState> {
  state: IState = {
    count: 0,
  };
  render() {
    return <div>{this.props.maxLength}</div>;
  }
}
```

解释：React.Component 泛型类两个类型变量，分别指定 props 和 state 类型。

**创建泛型类**

```ts
class GenericNumber<NumType> {
  defaultValue: NumType;
  add: (x: NumType, y: NumType) => NumType;
}

const myNum = new GenericNumber<number>();
myNum.defaultValue = 10;
```

解释：

1. 类似于泛型接口，在 class 名称后面添加`<类型变量>`，这个类就变成了泛型类
2. 此处的 add 方法，采用的是箭头函数形式的类型书写方式
3. 类似于泛型接口，在创建 class 实例时，在类名后面通过 <类型> 来指定明确的类型。

**`泛型工具类型`**
TS 内置了一些常用的工具类型，来简化 TS 中的一些常见操作。

说明：它们都是`基于泛型实现的`（泛型适用于多种类型，更加通用），并且是内置的，可以直接在代码中使用。

这些工具类型有很多，主要学习：

- `Partial<Type>`
- `Readonly<Type>`
- Pick<Type,keys>
- Record<Keys,Type>

**`1.Partial<Type>用来构造（创建）一个类型，将Type的所有属性设置为可选。`**

```ts
interface Props {
  id: string;
  children: number[];
}
type PartialProps = Partial<Props>;

let p1: Props = {
  id: "",
  children: [1],
};
let p2: PartialProps = {};
```

解释：构造出来的新类型 PartialProps 结构和 Props 相同，但所有属性都变为可选的。

**`2.Readonly<Type>用来构造一个类型，将Type的所有属性都设置为只读`**

```ts
interface Props {
  id: string;
  children: number[];
}
type ReadonlyProps = Readonly<Props>;

let props: ReadonlyProps = { id: "1", children: [] };
props.id = "2"; // 报错，无法分配到id，因为是只读的
```

解释：构造出来的新类型 ReadonlyProps 结构和 Props 相同，但所有属性都变为只读的。

当想重新给 id 属性赋值时，会报错。

**`3.Pick<Type,Keys>从Type中选择一组属性来构造新类型`**

```ts
interface Props {
  id: string;
  title: string;
  children: number[];
}
type PickProps = Pick<Props, "id" | "title">;
```

解释：

1. Pick 工具类型有两个类型变量：1-表示选择的谁的属性，2-表示选择按几个属性
2. 其中第二个类型变量，如果只选择一个则只传入该属性名即可。
3. `第二个类型变量传入的属性只能是第一个类型变量中存在的属性`
4. 构造出来的新类型 PickProps，只有 id 和 title 两个属性类型

**`4.Record<Keys,Type>构造一个对象类型，属性键为Keys，属性类型为Type`**

```ts
type RecordObj = Record<'a' | 'b' | 'c', string[]>
let obj: RecordObj = {
  a: ['1']
  b: ['2']
  c: ['3']
}
```

## 5. 索引签名类型

绝大多数情况下，都可以在使用对象前就确定对象的结构，并为对象添加准确的类型。

使用场景：`当无法确定对象中有哪些属性`（或者是对象中出现任意多个属性），此时，用到索引签名类型。

```ts
interface AnyObject {
  [key: string]: number;
}

let obj: AnyObject = {
  a: 1,
  b: 2,
};
```

解释：

1. 使用`[key: string]`来约束接口中允许出现的属性名称。表示只要是 string 类型的属性名称，都可以出现在对象中。
2. 这样，对象 obj 中皆可以出现任意多个属性（比如 a、b 等）
3. `key只是一个占位符`，可以换成任意合法的变量名称。
4. 隐藏的前置知识：`JS中对象（{}）的键是string类型的`。

在 JS 中数组是一类特殊的对象，特殊在`数组的键（索引）是数值类型`。

并且，数组也可以出现任意多个元素，所以，在数组对应的泛型接口中，也用到了索引签名类型。

```ts
interface MyArray<T> {
  [n: number]: T;
}

let arr: MyArray<number> = [1, 3, 5];
arr[0];
```

解释：

1. MyArray 接口模拟原生的数组接口，并使用[n: number]来作为索引签名类型
2. 该索引签名类型表示：只要是 number 类型的键（索引）都可以出现在数组中，或者是数组中可以有任意多个元素
3. 同时也符合数组索引是 number 类型这一前提

## 6. 映射类型

`基于旧类型创建新类型（对象类型）`，减少重复，提升开发效率。

比如，类型 PropKeys 有 x/y/z，另一个类型 Type1 中也有 x/y/z，并且 Type1 中的 x/y/z 的类型相同：

```ts
type PropKeys = "x" | "y" | "z";
type Type1 = { x: number; y: number; z: number };

//这这样书写没错，但是x/y/z重复书写了两次。使用映射类型来简化：
type PropKeys = "x" | "y" | "z";
type Type2 = { [Key in PropKeys]: number };
```

解释：

1. 映射类型是基于索引签名类型的，所以，该语法类似于索引签名类型，也使用了`[]`。
2. `Key in PropKeys`表示 Key 可以是 PropKeys 联合类型中的任意一个，类似于 forin（let k in obj）
3. 使用映射类型创建的新对象类型 Type2 和类型 Type1 结构完全相同
4. 注意：`映射类型只能在类型别名中使用，不能在接口中使用`

映射类型除了根据联合类型创建新的类型外，还可以`根据对象类型来创建`：

```ts
type Props = { a: number; b: string; c: boolean };
type Type3 = { [Key in keyof Props]: number };
```

解释：

1. 首先，执行`keyof Props`获取到对象类型 Props 中所有键的联合类型，即'a'|'b'|'c'
2. 然后，Key in...就表示 Key 可以是 Props 中所有的键名称中的任意一个

实际上，`泛型工具类型（比如，Partial<Type>)都是基于映射类型实现的。`

比如，Partial< Type>的实现：

```ts
type Partial<T> = {
  [P in keyof T]?: T[P];
};

type Props = { a: number; b: string; c: boolean };
type PartialProps = Partial<Props>;
```

解释：

1. `keyof T` 即 keyof Props 表示获取 Props 的所有键，也就是'a'|'b'|'c'
2. 在 [] 后面添加`?`问号，表示将这些属性变为`可选`的，以此来实现 Partial 的功能
3. 冒号后面的 `T[P]表示获取T中每个键对应的类型`。比如，如果是'a'则类型是 number，如果是'b'则类型是 string
4. 最终，新类型 PartialProps 和旧类型 Props 结构完全相同，只是让所有类型都变为可选了。

上面用到的`T[P]`语法，在 TS 类型中叫做`索引查询（访问）类型`。

作用：`用来查询属性的类型`

```ts
type Props = { a: number; b: string; c: boolean };
// type TypeA = number;
type TypeA = Props["a"];
```

解释：Props['a']表示查询类型 Props 中属性'a'对应的类型 number。所以，TypeA 的类型为 number。

注意：`[] 中的属性必须存在于被查询类型中，`否则会报错。

索引查询类型的其他使用方式：

`同时查询多个索引的类型：`

```ts
type Props = { a: number; b: string; c: boolean };

type TypeA = Props["a" | "b"]; //string | number
// 解释：使用字符串面量的联合类型，获取属性a和b对应的类型，结果为string|number

type TypeA = Props[keyof Props];
// 解释： 使用keyof操作符获取Props中所有键对应的类型，结果为：string|number|Boolean
```
