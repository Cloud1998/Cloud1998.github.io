---
title: Golang语法基础3-常量、变量与基本数据类型
date: 2023-02-05
tags: [Golang语法基础]
categories: [编程技术]
slug: golang basic grammar 3 constants variables and elementary types
---

本文介绍 Golang 中的常量、变量、基本数据类型和常用的类型转换。

<!--more-->

## 1. 常量

常量用于存储程序运行过程中恒定不变的数据，因此我们无法在程序运行过程中修改它的值，如果你在代码中试图修改常量的值则会引发编译错误。Go语言中使用关键字 `const` 定义常量，格式如下：

```go
const identifier [type] = value
```

常量的值必须在编译时能够确定，因此只能是基本数据类型和表达式。一个常量定义的例子如下：

```go
const Pi float64 = 3.1415926
```

存储在常量中的数据类型只可以是：布尔型、数字型（整数型、浮点型和复数）和字符串型。

另外，由于 Go 的特性，常量的定义具有一些不同的形式：

1. 在 Go 语言中，你可以省略类型说明符 `[type]`，因为编译器可以根据变量的值来推断其类型。

   ```go
   const Pi 3.1415926
   ```

2. Go 支持在同一行同时定义多个值，称为并行赋值。

   ```go
   const Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday = 1, 2, 3, 4, 5, 6, 7
   ```

3. Go 支持批量声明，这种定义形式叫做因式分解关键字。

   ```go
   const (
     Monday, Tuesday, Wednesday = 1, 2, 3
     Thursday, Friday, Saturday = 4, 5, 6
   	Sunday = 7
   )
   ```

未定义类型的常量会在必要时刻根据上下文来获得相关类型。例如：

```go
var n int
f(n + 5)	// 无类型的数字型常量 “5” 它的类型在这里变成了 int
```

常量的值必须是能够在编译时就能够确定的；你可以在其赋值表达式中涉及计算过程，但是所有用于计算的值必须在编译期间就能获得。

- 正确的做法：`const c1 = 2 / 3`
- 错误的做法：`const c2 = getNumber()` // 引发构建错误: `getNumber() used as value`

**因为在编译期间自定义函数均属于未知，因此无法用于常量的赋值，但内置函数可以使用，如：`len()`。**

数字型的常量是没有大小和符号的，并且可以使用任何精度而不会导致溢出：

```go
const Ln2 = 0.693147180559945309417232121458\
			176568075500134360255254120680009
const Log2E = 1 / Ln2 // this is a precise reciprocal
const Billion = 1e9 // float constant
const hardEight = (1 << 100) >> 97
```

根据上面的例子我们可以看到，反斜杠 `\` 可以在常量表达式中作为多行的连接符使用。

此外，Go 还提供关键字 `iota`，用作常量计数器，只能在常量定义时使用。`iota` 在 `const` 关键字出现时被重置为 0，每新增一行常量声明新增一个计数，能极大的简化定义。一个例子如下：

```go
const a = iota	// a = 0
const (
	b = iota	// b = 0
  c	= iota	// c = 1
)
```

可以使用空白标识符跳过不想要的值：

```go
const (
  a = iota	// a = 0
  _ = iota	// _ = 1	空白标识符可以简单地理解为一个“只写”的变量
  c = iota	// c = 2
)
```

赋值一个常量时，之后没赋值的常量都会应用上一行的赋值表达式：

```go
const (
	a = iota	// a = 0
  b					// b = 1
  c					// c = 2
  d = 5			// d = 5
  e					// e = 5
)
```

同一行有多个常量不产生影响，中间有数值插队也不产生影响，因为 iota 的值是每新增一行声明增加 1：

```go
// 赋值两个常量，iota 只会增长一次，而不会因为使用了两次就增长两次
const (
	Apple, Banana = iota + 1, iota + 2	// Apple = 1 Banana = 2
  Cherimoya, Durian                  // Cherimoya = 2 Durian = 3
  Elderberry, Fig                    // Elderberry = 3, Fig = 4
)
```

`iota` 也可以用在表达式中参与运算操作，如：

```go
// 使用 iota 结合位运算表示资源状态
const (
	Open = 1 << iota	// 0001
  Close							// 0010
  Pending						// 0100
)

// 使用 iota 结合乘法运算表示单位
const (
	_ = iota							// 使用 _ 忽略不需要的 iota
  KB = 1 << (10 * iota)	// 1 << (10*1)
  MB										// 1 << (10*2)
  GB										// 1 << (10*3)
  TB										// 1 << (10*4)
  PB										// 1 << (10*5)
  EB										// 1 << (10*6)
  ZB										// 1 << (10*7)
  YB										// 1 << (10*8)
)
```

{{< admonition tip "iota小结" false >}}

1. 只能在定义常量时使用
2. 遇到`const`关键字重置为0
3. 每次换行声明，iota的值就会增加1

{{< /admonition >}}

关于 `iota` 的使用涉及到非常复杂多样的情况，这里解释的并不清晰，因为很难对 `iota` 的用法进行直观的文字描述。如希望进一步了解，请观看视频教程 [《Go编程基础》](https://github.com/Unknwon/go-fundamental-programming) [第四课：常量与运算符](https://github.com/Unknwon/go-fundamental-programming/blob/master/lectures/lecture4.md)。

## 2. 变量

声明变量的一般形式是使用 `var` 关键字

```go
var identifier type
```

当一个变量被声明后，系统会自动赋予它该类型的零值：`int` 为 `0`，`float` 为 `0.0`，`bool` 为 `false`，`string` 为空字符串，指针为 `nil`。

也可以在声明的同时初始化：

```go
var a int = 15
```

最后，由于常量部分提到的三个 Go 的特性，变量的声明和使用也有一些不同的形式：

1. 类型推断，从而可以省略类型定义

   ```go
   var a = 15
   ```

2. 并行赋值

   ```go
   var a, b, c = 5, 6, 7
   ```

3. 因式分解关键字（一般用于声明全局变量）

   ```go
   var (
    a = 15
    b = false
    str = "Go says hello to the world!"
    numShips = 50
    city string
   )
   ```

上面提到的所有方式可以用于全局变量，也可以用于局部变量，但还有一种更加简短的声明与定义方式，**仅能用于局部变量**（函数体内，包括 main 函数），这是我们使用非常多的一种写法：

```go
a := 15
```

这里详细介绍一下常量部分提到过的空白标识符，空白标识符指的是下划线 `_`，也叫做匿名变量，只允许写入，任何类型都可以赋值给它，但无法使用它的值。我们在使用 `iota` 关键字时可以使用空白标识符跳过不想要的值，另外一种常见的使用场景是在多重赋值中抛弃不需要的变量。

```go
_, b = 15, 7
```

匿名变量不会被分配内存，因此不占用内存空间，多次声明也不会引起冲突。

Go 还提供了一种非常友好的功能，如果想要交换两个变量的值，可以直接使用 `a, b = b, a` 这种形式，不需要再使用临时变量，为程序编写带来了极大的便利。

**变量的命名规则**遵循骆驼命名法，即**首个单词小写，每个新单词的首字母大写**，例如：`numShips`。但如果你的全局变量希望能够被外部包所使用，则需要将首个单词的首字母也大写（可见性规则）。

**变量作用域**的规则同 C 语言相同，关于值类型和引用类型的理解也和 C 语言相同。

Go 中值类型包括`int`、`float`、`bool`、`string`、数组、结构体（struct），**引用类型**包括指针、切片（slices）、映射（maps）和通道（channel）。值类型存储在栈中，引用类型存储在堆中，以便进行垃圾回收。

{{< admonition warning "注意" >}}

1. `:=` 是声明语句。如果在相同的代码块中，我们不可以再次对于相同名称的变量使用初始化声明，例如：`a := 20` 就是不被允许的，编译器会提示错误 `no new variables on left side of :=`，但是 `a = 20` 是可以的，因为这是给相同的变量赋予一个新的值。

2. 如果你在定义变量 `a` 之前使用它，则会得到编译错误 `undefined: a`。如果你声明了一个局部变量却没有在相同的代码块中使用它，同样会得到编译错误，例如下面这个例子当中的变量 `a`：

   ```go
   func main() {
     var a string = "abc"
     fmt.Println("hello, world")
   }
   ```

   尝试编译这段代码将得到错误 `a declared and not used`。

   此外，单纯地给 `a` 赋值也是不够的，这个值必须被使用，所以使用 `fmt.Println("hello, world", a)` 会移除错误。

3. **全局变量允许被声明但是不使用**。

{{< /admonition >}}

{{< admonition question "为什么将变量的类型放在变量的名称之后？" false >}}

首先，它是为了避免像 C 语言中那样含糊不清的声明形式，例如：`int* a, b;`。在这个例子中，只有 `a` 是指针而 `b` 不是。如果你想要这两个变量都是指针，则需要将它们分开书写（你可以在 [Go 语言的声明语法](http://blog.golang.org/2010/07/gos-declaration-syntax.html) 页面找到有关于这个话题的更多讨论）。

而在 Go 中，则可以很轻松地将它们都声明为指针类型：`var a, b *int`

其次，这种语法能够按照从左至右的顺序阅读，使得代码更加容易理解。

{{< /admonition >}}

## 3. 基本数据类型 

Go 拥有 4 大类共7种基本数据类型

- 布尔类型 bool
- 数字类型：
  - 整型 int，根据位数的不同包括 int8, int16, int32, int64 四种以及相对应的 uint
  - 浮点型 float，包括 float32 和 float64 两种
  - 复数 complex，包括 complex32 和 complex64 两种，复数类型并不常用
- 字符类型：
  - byte，uint8 的别名，完全等同
  - rune，int32 是别名，完全等同
- 字符串类型 string

Go 语言对于值之间的比较有非常严格的限制，只有两个类型相同的值才可以进行比较。我们将 Go 中不可比较类型总结如下，除此之外，其它类型都是可比较的：

1. 切片类型
2. 映射类型
3. 函数类型
4. 任何字段为不可比较类型的结构体类型，以及任何元素类型为不可比较类型的数组类型

对于接口而言，情况更加复杂一点，如果值的类型是接口，那么它们必须实现了相同的接口。如果条件不满足，则必须事先进行类型转换才可以比较。

### 3.1 布尔类型

使用`bool`关键字声明，值只可以是常量 `true` 或 `false`。

```go
var b bool = true
```

两个**类型相同**的值可以使用关系运算符（`==`和`!=`）来获得一个布尔类型的值。布尔类型的值之间也可以使用逻辑运算符（`!`、`&&`、`||`）来产生另一个布尔值，运算规则与其它语言相同。

{{< admonition tip "提示" >}}

Go 中的布尔值并不等于数字 1 和 0，因此不能直接进行运算。

{{< /admonition >}}

### 3.2 数字类型

Go 中数字类型分为三种，整型、浮点型和复数类型，其中位的运算采用补码。

#### 整型

整型提供有符号和无符号两种，每一种又分别提供对应 8、16、32、64bit 大小的四种类型，总计八种，列表如下：

|                             整型                             |                无符号整型                 |
| :----------------------------------------------------------: | :---------------------------------------: |
|                     int8（-128 -> 127）                      |             uint8（0 -> 255）             |
|                   int16（-32768 -> 32767）                   |           uint16（0 -> 65,535）           |
|           int32（-2,147,483,648 -> 2,147,483,647）           |       uint32（0 -> 4,294,967,295）        |
| int64（-9,223,372,036,854,775,808 -> 9,223,372,036,854,775,807） | uint64（0 -> 18,446,744,073,709,551,615） |

除此之外还提供两种不带位数的类型声明：`int` 和 `uint`。这两种类型的大小取决于所运行的平台处理器支持的字长，例如，在 32 位操作系统上，它们均使用 32 位（4 个字节），在 64 位操作系统上，它们均使用 64 位（8 个字节）。

{{< admonition tip "提示" >}}

你可以使用 `a := uint64(0)` 来同时完成类型转换和赋值操作，这样 `a` 的类型就是 `uint64`。

{{< /admonition >}}

尽管 int 有可能是 32 位，但在需要时 int 和 int32 之间也必须显式进行类型转换。

最后还有一种无符号整型 `uintptr`，它没有指定具体的 bit 大小但被设定为足够容纳一个指针。`uintptr` 类型只有在底层编程时才需要，特别是 Go 语言和 C 语言函数库或操作系统接口相交互的地方，一般用于指针计算。

`int` 型是计算最快的类型，也是最常使用的类型。

{{< admonition tip "提示" >}}

你可以通过增加前缀 0 来表示 8 进制数（如：`077`），增加前缀 0x 来表示 16 进制数（如：`0xFF`），以及使用 `e` 来表示 10 的连乘（如： `1e3` = 1000，或者 `6.022e23` = 6.022 x 1e23）。

{{< /admonition >}}

#### 浮点型

Go 语言中没有 float 类型，没有 double 类型，只有 `float32` 和 `float64`。它们的算术规范由 IEEE-754 标准定义，该标准被所有现代的 CPU 支持。

`float32` 精确到小数点后 7 位，`float64` 精确到小数点后 15 位。通常应该优先使用 float64 类型，因为 `math` 包中所有有关数学运算的函数都会要求接收这个类型。

#### 复数类型

Go 语言拥有两种复数类型，分别是 `complex64`（32 位实数和虚数）和 `complex128`（64 位实数和虚数）。

```go
var c1 complex64 = 5 + 10i	// 5 是实部 10i 是虚部
```

内置的 complex 函数用于构建复数，内置的 `real` 和 `imag` 函数分别返回复数的实部和虚部：

```go
var cl complex128 = complex(1, 2) // 1+2i
fmt.Println(real(cl))           // "1"
fmt.Println(imag(cl))           // "2"
```

`cmath` 包中包含了一些操作复数的公共方法。如果对内存的要求不是特别高，最好使用 complex128 作为计算类型，因为相关函数都使用这个类型的参数。

### 3.3 字符类型

Go语言的字符有两种：

- `byte` 型，代表了 ASCII 码的一个字符。
- `rune` 类型，代表一个 UTF-8 字符，当需要处理中文、日文或者其他复合字符时，则需要用到 rune 类型。

严格来说，字符并不是 Go 语言的一种类型，字符只是整数的别名，因此其零值也是 0。在声明时应使用**单引号**括起来。

`byte` 类型是 `uint8` 的别名，刚好一个字节，足以表示传统 ASCII 编码的字符。例如：`var ch byte = 'A'`；

`rune`是`int32`的别名，四个字节，足以表示最长的 UTF-8 字符。例如：`var ch rune = '\u0041'`。

{{< admonition tip "提示" >}}

在书写 Unicode 字符时，需要在 16 进制数之前加上前缀 `\u` 或者 `\U`。因为 Unicode 至少占用 2 个字节，所以我们使用 `int16` 或者 `int` 类型来表示。如果需要使用到 4 字节，则会加上 `\U` 前缀；前缀 `\u` 则总是紧跟着长度为 4 的 16 进制数，前缀 `\U` 紧跟着长度为 8 的 16 进制数。

{{< /admonition >}}

包 `unicode` 包含了一些针对测试字符的非常有用的函数（其中 `ch` 代表字符）：

- 判断是否为字母：`unicode.IsLetter(ch)`
- 判断是否为数字：`unicode.IsDigit(ch)`
- 判断是否为空白符号：`unicode.IsSpace(ch)`

这些函数返回一个布尔值。包 `utf8` 拥有更多与 rune 相关的函数。

### 3.4 字符串类型

字符串底层约定是字节的一个序列，编码方式建议是 UTF-8，但不是必须遵守，通常是 ASCII。因此取字符串单个字符的类型通常是 byte，只有遇到中文等语言是才是 rune。另外，使用 for-range 结构遍历时字符串的单个字符类型是 rune。

字符串是值类型，且值不可变，即创建一个字符串后无法再次修改它的内容

Go 支持以下 2 种形式的字符串：

- 解释字符串：该类字符串使用**双引号**括起来，其中的转义字符将被替换，这些转义字符包括：

  - `\n`：换行符
  - `\r`：回车符
  - `\t`：tab 键
  - `\u` 或 `\U`：Unicode 字符
  - `\\`：反斜杠自身

- 非解释字符串：该类字符串使用**反引号**括起来，当使用多行字符串时使用这种形式。

  ```go
  a := `abc
  def`
  fmt.Println(a)
  // Output:
  abc
  def
  ```

`string` 类型的零值为长度为零的字符串，即空字符串 `""`。

Go 中的字符串是根据长度限定的，而非特殊字符`\0`，其长度可以使用内置函数`len()`来获取，长度的基本含义是字符串在内存中所占字节的个数，所以下面的例子虽然是两个中文，但长度是6

```go
a := "中国"
fmt.Println(len(a))	// 6
```

可以将字符串看作数组而索引其内的单个字符，如第 i 个字节表示为`str[i-1]`。

{{< admonition warning "注意" >}}
获取字符串中某个字节的地址的行为是非法的，例如：`&str[i]`。
{{< /admonition >}}

使用拼接符`+`可以拼接两个字符串，以下是一个多行字符串拼接的例子

```go
str := "Beginning of the string " +
	"second part of the string"
```

`+`必须放在第一行末尾，因为编译器会在行尾自动补全分号。当然，`+=`一样可用于字符串

```go
s := "hel" + "lo,"
s += "world!"
fmt.Println(s)	// 输出 “hello, world!”
```

在循环中使用`+`拼接字符串并不是最高效的做法，更好的办法是使用`string.join()`，或者使用字节缓冲`bytes.Buffer`。

### 3.5 类型别名

使用某个类型时可以给它起个别名在程序中使用，用于简化名称或解决名称冲突。

在 `type TZ int` 中，TZ 就是 int 类型的新名称（用于表示程序中的时区），然后就可以使用 TZ 来操作 int 类型的数据。

```go
package main
import "fmt"

type TZ int

func main() {
	var a, b TZ = 3, 4
	c := a + b
	fmt.Printf("c has the value: %d", c) // 输出：c has the value: 7
}
```

实际上，类型别名得到的新类型并非和原类型完全相同，**新类型不会拥有原类型所附带的方法**；TZ 可以自定义一个方法用来输出更加人性化的时区信息。

## 4. 类型转换

Go语言中类型转换是可行的，但是不存在隐式类型转换，**所有的转换都必须显式说明**。类型转换的基本格式为：

```go
valueOfTypeB = typeB(valueOfTypeA)
```

两条转换原则如下：

1. 只有相同底层类型的变量间可以进行相互转换（如 int16 和 int32），不同底层类型的变量相互转换会引发编译错误。
2. 类型转换只有从取值范围小的类型转换到取值范围大的类型才能成功，反过来会发生精度丢失（截断）。

```go
var a, b int16
var c int32
A := int32(a)	// 标准转换
B := bool(b)	// 类型不匹配，引发编译错误
C := int16(c)	// 取值范围变小，精度丢失
```

浮点型可以转换为整型，转换时会将小数部分去掉，只保留整数部分：

```go
a := 12.54
fmt.Println(int(a))	// 输出12
```

精度丢失可以使用专门的函数保证安全，如 int 型到 int8：

```go
func Uint8FromInt(n int) (uint8, error) {
	if 0 <= n && n <= math.MaxUint8 { // conversion is safe
		return uint8(n), nil
	}
	return 0, fmt.Errorf("%d is out of the uint8 range", n)
}
```

其它的类型转换则需要使用一些库函数。

### 4.1 bool 与 string

Go 语言中 bool 类型值与数字 1 和 0 不等同，因此不能和数字类型相互转换（可以简单的使用 if-else 结构完成这一功能）。但借助 strconv 包，可以和 string 类型转换。

```go
// string -> bool
b, err := strconv.ParseBool("true")
// bool -> string
s := strconv.FormatBool(true)
```

两个函数的原型如下：

```go
// ParseBool返回字符串代表的bool值，接受 1, t, T, TRUE, true, True, 0, f, F, FALSE, false, False作为传入参数，其他参数均返回error
func ParseBool(str string) (bool, error) {
	switch str {
	case "1", "t", "T", "true", "TRUE", "True":
		return true, nil
	case "0", "f", "F", "false", "FALSE", "False":
		return false, nil
	}
	return false, syntaxError("ParseBool", str)
}

// FormatBool returns "true" or "false" according to the value of b.
func FormatBool(b bool) string {
	if b {
		return "true"
	}
	return "false"
}
```

### 4.2 int / float 与 string

与字符串相关的类型转换都是通过 strconv 包实现的。最常用的是 Atoi(string to int) 和 Itoa(int to string) 函数：

```go
i, err := strconv.Atoi("-42")
s := strconv.Itoa(-42)
```

函数原型如下

```go
func Atoi(s string) (int, error)
func Itoa(i int) string
```

#### 字符串->数字类型

ParseFloat, ParseInt, 和 ParseUint 可以将字符串转化为对应的值：

```go
f, err := strconv.ParseFloat("3.1415", 64)
i, err := strconv.ParseInt("-42", 10, 64)
u, err := strconv.ParseUint("42", 10, 64)
```

浮点型函数原型如下：

```go
func ParseFloat(s string, bitSize int) (float64, error)
```

bitSize指定了返回值的类型，当 bitSize=32，返回 float32 类型（结果仍是 float64，但会转换为 float32)；当 bitSize=64，返回 float64 类型。只有这两种情况。

整型函数原型如下：

```go
func ParseInt(s string, base int, bitSize int) (i int64, err error)
func ParseUint(s string, base int, bitSize int) (uint64, error)
```

如果 base 为 0 那么实际上 base 由 string 的前缀指定，`0x`意味着 base=16，`0`意味着 base=8，否则 base=10，本质上是进制的前缀。若base等于1，小于0或超过36，返回一个error。

bitSize 仍然指定返回值类型. 其值为 0, 8, 16, 32, 和 64 分别对应 int, int8, int16, int32 和 int64. bitSize 值小于 0 或大于 64 ，返回一个 error。

#### 数字类型->字符串

FormatFloat, FormatInt, 和 FormatUint 可以将值转换为字符串：

```go
s := strconv.FormatFloat(3.1415, 'E', -1, 64)
s := strconv.FormatInt(-42, 16)
s := strconv.FormatUint(42, 16)
```

浮点型的函数原型如下：

```go
func FormatFloat(f float64, fmt byte, prec, bitSize int) string
```

bitSize 表示 f 的来源类型（32：float32、64：float64），会据此进行舍入。

fmt 表示格式：‘b’ (-ddddp±ddd, 二进制指数), ‘e’ (-d.dddde±dd,十进制指数), ‘E’ (-d.ddddE±dd, 十进制指数), ‘f’ (-ddd.dddd, 没有指数), ‘g’ (指数很大时用‘e’, 否则用 ‘f’ ), ‘G’ (指数很大时用‘E’, 否则用 ‘f’ ).

精度 prec 控制数字的个数 (排除指数)。对’e’, ‘E’, 和 ‘f’ ，表示小数点后的数字位数. 对 ‘g’ 和 ‘G’ 是有效数字位数 (trailing zeros are removed)。prec 等于 -1 时则使用最少数量但又必须的数字来表示 f。

整型的函数原型如下：

```go
func FormatInt(i int64, base int) string
func FormatUint(i uint64, base int) string
```

返回 base 指定进制的整数 i 的字符串形式，2 <= base <= 36。使用小写字母 ‘a’ 到 ‘z’ 表示大于10的数字。

忽略可能出现的转换错误，可以给出如下例子：

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	var orig string = "666"
	var an int
	var newS string  

	an, _ = strconv.Atoi(orig)
	fmt.Printf("The integer is: %d\n", an) 
	an = an + 5
	newS = strconv.Itoa(an)
	fmt.Printf("The new string is: %s\n", newS)
}
//输出：
The integer is: 666
The new string is: 671
```

### 4.3 []rune 与 string

用 for range 遍历字符串可以返回每个字符，返回的字符类型是 rune。但 []rune 类型也经常需要转换为 string，和单个 rune 类型相似，都可以直接进行显示类型转换，如下：

```go
a := []rune{'a', 'b', 'c'}
b := 'g'
c := string(a)
d := string(b)
fmt.Println(reflect.TypeOf(a), reflect.TypeOf(b), reflect.TypeOf(c), reflect.TypeOf(d))
fmt.Println(a, b, c, d)
//Output
[]int32 int32 string string
[97 98 99] 103 abc g
```

## 5. init函数

变量除了可以在全局声明中初始化，也可以在 `init()` 函数中初始化。这是一类非常特殊的函数，它不能够被人为调用，而是在每个包完成初始化后自动执行，并且执行优先级比 `main()` 函数高。

每个源文件可以包含多个 `init()` 函数，同一个源文件中的 `init()` 函数会按照从上到下的顺序执行，如果一个包有多个源文件包含 `init()` 函数的话，则官方鼓励但不保证以文件名的顺序调用。初始化总是以单线程并且按照包的依赖关系顺序执行。

一个可能的用途是在开始执行程序之前对数据进行检验或修复，以保证程序状态的正确性。

```go
// init.go
package trans

import "math"

var Pi float64

func init() {
  Pi = 4 * math.Atan(1)	//init() function computes Pi
}
```

在它的 `init()` 函数中计算变量 `Pi` 的初始值。

下面的代码导入上文所创建包 `trans`（需要 `init.go` 目录为 `./trans/init.go` ）并且使用到了变量 `Pi`：

```go
package main

import (
	"fmt"
  "./trans"
)

var twoPi = 2 * trans.Pi

func main() {
  fmt.Printf("2*Pi = %g\n", twoPi)	// 2*Pi = 6.283185307179586
}
```

`init()` 函数也经常被用在当一个程序开始之前调用后台执行的 goroutine，如下面这个例子当中的 `backend()`：

```go
func init() {
  // setup preparations
  go backend()
}
```

