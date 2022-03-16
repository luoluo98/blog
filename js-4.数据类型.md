# 数据类型

## 1.1 为什么需要数据类型？

在计算机中，不同的数据所需占用的存储空间是不同的，为了便于把数据分成所需内存大小不同的数据，充分利用存储空间，于是定义了不同的数据类型。

简单来说，数据类型就是数据的类别型号。比如姓名张三，年龄 18，这些数据类型是不一样的。

## 1.2 变量的数据类型

变量是用来存储值的所在处，它们有名字和数据类型，变量的数据类型决定了如何将代表这些值的位存储到计算机的内存中。

**JavaScript 是一种弱类型或者说动态语言。**

这意味着不用提前声明变量的类型。在程序运行过程中，类型会被自动确定。

```
var age = 10;   这是一个数字型

var areYouOk = '是的';  这是一个字符串
```

在代码运行时，变量的数据类型是由 JS 引擎根据=右边变量值的数据类型来判断的，运行完毕之后，变量就确定了数据类型。

**JavaScript 拥有动态类型，同时也意味着相同的变量可用作不同的类型。**

```
var x = 6;         x为数字

var x = 'bill';    x为字符串
```

## 1.3 数据类型的分类

- 简单数据类型（Number、String、Boolean、Undefined、Null）
- 复杂数据类型（object）

## 1.3.1 简单数据类型（基本数据类型）

| 简单数据类型 | 说明                                                 | 默认值    |
| ------------ | ---------------------------------------------------- | --------- |
| Number       | 数字型，包含整值型和浮点值型，如 21、0.21            | 0         |
| Boolean      | 布尔值型，如 true、false，等价于 1 和 0              | false     |
| String       | 字符串类型，如“张三”，注意 js 里面，字符串都带引号   | ""        |
| Undefined    | var a; 声明了变量 a 但是没有给值，此时 a = undefined | Undefined |
| Null         | var a = null; 声明了变量 a 为空值                    | null      |

1. **数字型 Number**

```
var age = 21; 整数

var Age = 21.3174; 小数
```

`1.1 数字型进制：`二进制、八进制、十进制、十六进制。

```
1. 八进制数字序列范围：0~7
var num1 = 07;        对应十进制的7；
var num2 = 019;       对应十进制的19；
var num3 = 08;        对应十进制的8；

2. 十六进制数字序列范围：0~9以及A~F
var num = 0xA;
```

**在 js 中八进制前面加 0，十六进制前面加 0x**

`1.2 数字型范围：`JavaScript 中数值的最大和最小值。

```
alert(Number.MAX_VALUE);   1.7976931348623157e+308；

alert(Number.MIN_VALUE);   5e-324
```

`1.3 数字型的三个特殊值：`

```
alert(Infinity);

alert(-Infinity);

alert(Naw);
```

- Infinity，代表无穷大，大于任何数值
- -Infinity，代表无穷小，小于任何数值
- NaN， not a number，代表一个非数值

`1.4 isNaN()`--练习 12

用来判断一个变量是否为非数字的类型，是数字返回 false，不是数字返回 true。

2. **字符串型 String**

字符串型可以是引号中的任意文本，其语法为双引号“”和单引号''

`2.1 字符串引号嵌套`

JS 可以用单引号嵌套双引号，或者用双引号嵌套单引号。（外双内单，外单内双）

`2.2 字符串转义符`

转义符都是以\开头的。

| 转义符 | 解释说明                    |
| ------ | --------------------------- |
| \n     | 换行符，n 是 newline 的意思 |
| \ \    | 斜杠\                       |
| \\'    | 单引号'                     |
| \\"    | 双引号"                     |
| \t     | tab 缩进                    |
| \b     | 空格，blank 的意思          |

`2.3 字符串长度`

字符串长度是由若干字符组成的，这些字符的数量就是字符串的长度。通过字符串的 length 属性可以获取整个字符串的长度。

`2.4 字符串拼接`

- 多个字符串之间可以使用+进行拼接，其拼接方式为 **字符串+任何类型=拼接之后的新字符串**
- 拼接前会把字符串相加的任何类型转成字符串，再拼接成一个新的字符串
- 只要有字符串和其他类型相拼接，最终结果都是字符串类型

**数值相加，字符相连**

`2.5 字符串拼接加强`

- 经常会使用字符串和变量来拼接，因为变量可以很方便的修改里面的值。
- 变量是不能添加引号的
- 如果变量两侧都有字符串拼接，口诀“引引加加”
  `alert("你今年已经" + age + "岁了");`

3. **布尔型 Boolean**

布尔型有两个值：true-真、对 和 false-假、错。

布尔型数字型相加的时候，true 的值为 1，false 的值为 0.

```
console.log(true + 1); //结果是2

console.log(false + 1);  //结果是1
```

4. **Undefined 和 Null**

- 一个声明没有被赋值的变量会有一个默认值 Undefined（如果进行相连或者相加时，注意结果）。

```
var variable;

console.log(variable);  //undefined；

console.log('你好' + variable); //你好undefined；

console.log(11 + variable); //NaN；

console.log(true + variable); //NaN；
```

- 一个声明变量给 null 值，里面存储的值为空

```
var vari = null;

console.log('你好' + vari); //你好null；

console.log(11 + vari); //11；

console.log(true + vari); //1
```

## 1.4 获取变量数据类型

**typeof** 可以用来获取检测变量的数据类型

### 1.4.1 字面量

字面量是在源代码中一个固定值的表示法，通俗来说就是字面量如何表达这个值。

- 数字字面量：8,9,10
- 字符串字面量：'黑马程序员',"大前端"
- 布尔字面量：true、false

## 1.5 数据类型转换

使用表单、prompt 获取过来的数据默认是字符串类型的，此时就不能直接简单的进行加法运算，而需要转换变量数据的类型。通俗来说，就是把一种数据类型的变量转换成另一种数据类型。

- 转换为字符串类型
- 转换为数字型
- 转换为布尔型

### 1.5.1 转换为字符串

| 方式             | 说明                         | 案例                                 |
| ---------------- | ---------------------------- | ------------------------------------ |
| 加号拼接字符串   | 和字符串拼接的结果都是字符串 | var num = 1; alert(num+"我是字符串") |
| toString()       | 转换成字符串                 | var num= 1; alert(num.toString());   |
| String()强制转换 | 转换成字符串                 | var num = 1; alert(String(num));     |

- toString() 和 String() 使用方式不一样
- 三种转换方式，更喜欢用加号拼接字符串转换方式，也称之为隐式转换

### 1.5.2 转换为数字型（重点）

| 方式                   | 说明                             | 案例                |
| ---------------------- | -------------------------------- | ------------------- |
| parseInt(string)函数   | 将 String 类型转换成整数数值型   | parseInt('78')      |
| parseFloat(string)函数 | 将 string 类型转换成浮点数数值型 | parseFloat('78.12') |
| Number()强制转换函数   | 将 string 类型转换为数值型       | Number('12')        |
| js 隐式转换(- \* /)    | 利用算数运算隐式转换为数值型     | '12'-0              |

- 注意：parseInt 和 parseFloat 单词的大小写
- 隐式转换是我们在进行算数运算的时候，JS 自动转换了数据类型

### 1.5.3 转换为布尔型 Boolean

| 方式          | 说明                   | 案例            |
| ------------- | ---------------------- | --------------- |
| Boolean()函数 | 数其他类型转换为布尔型 | Boolean('true') |

- 代表空、否定 值会被转换为 false，如''、0、NaN、null、undefined
- 其余值都会被转换为 true

```
console.log(Boolean(''));//false

console.log(Boolean(0));//false

console.log(Boolean(NaN));//false

console.log(Boolean(null));//false

console.log(Boolean(undefined));//false

console.log(Boolean('小白'));//true

console.log(Boolean(12));//true
```

## 1.6 扩展阅读之编译和解释语言的区别

计算机不能直接理解任何除机器语言外的语言，所以必须要把程序员所写的程序语言翻译成机器语言才能执行程序。
程序语言翻译成机器语言的工具，被称为翻译器。

- 翻译器翻译的方式有两种：一个是**编译**，另外一个是**解释**。两种方式之间的区别在于**翻译的时间点不同**。
- 翻译器是在**代码执行之前进行编译**，生成中间代码文件。
- 解释器是在**运行时及时进行解释，并立即执行**。（当编译器以解释方式运行的时候，也称为解释器）。

## 1.7 扩展阅读之标识符、关键字、保留字

1. 标识符：就是指开发人员为变量、属性、函数、参数取的名字。

   `标识符不能是关键字和保留字`

2. 关键字：是指 JS 本身已经使用了的名字，不能再用它充当变量名、方法名。

   `包括：break\case\catch\continu\default\delete\do\else\finally\for\function\if\in\instanceof\new\return\switch\this\throw\try\typeof\var\void\while\with等`

3. 保留字：实际上就是预留的关键字，意思是现在虽然还不是关键字，但未来可能成为关键字，统一不能使用它们当变量名或方法名。

   `包括：Boolean\byte\char\class\const\debugger\enum\export\extends\fimal\float\goto\implements\import\int\interface\long\mative\package\private\protected\public\short\static\super\synchonized\throws\transient\volatile等`
