---
title: Flutter之Dart基础知识学习与使用
categories: 技术笔记
tags:
  - Flutter
  - Dart
excerpt: Flutter之Dart基础知识学习与使用
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/google-dart-generic-hero.png'
articleLink: 699f40be
date: 2020-12-16 11:10:11
---

## Dart

因为在安装Flutter SDK的时候，它已经内置了Dart了，我们完全可以直接使用Flutter去进行Dart的编写并且运行。

但是，如果你想单独学习Dart，并且运行自己的Dart代码，最好去安装一个Dart SDK。

### Hello Dart

接下来，就可以步入正题了。学习编程语言，从祖传的Hello World开始。

新建一个*helloWorld.dart*文件

```
// 俗称入口文件
main(List<String> args) {
	// 每行语句必须使用分号结尾
  print('Hello World');
}
```

然后在终端执行`dart helloWorld.dart`，就能看到*Hello World*的结果了。

`List<String> args` 是接受命令行传递的参数

### 定义变量

#### 明确声明(Explicit)

这种就是我们能明确这个变量的类型是什么 从而规定好 类型进行声明

```
变量类型 变量名称 = 赋值;
```

示例代码:

```
String name = 'qiao';
int age = 20;
double height = 2.2;
print('${name}, ${age}, ${height}'); // 这里的拼接方式和 js 模版字符串很相似
```

#### 类型推导(Type Inference)

就是我们声明变量类型由系统直接推断出来是什么类型 （也就是初始化值的类型）

常用的 推断类型声明字符为 `var,dynamic,const,final `

##### var的使用

```
var name = "qiao"
```

这样这个 `name` 的类型就是 `String` 类型

```
var age = 18; // 这推断的是 int 类型
age = 'why'; // 不可以将String赋值给一个int类型
```

##### dynamic的使用

我们使用 `dynamic` 声明了变量 在后期赋值不是初始化值的类型也是可以的

通常情况下不使用dynamic, 因为类型的变量会带来潜在的危险

```
dynamic name = 'coderwhy';
print(name.runtimeType); // String
name = 18; // 可以再次改变类型
print(name.runtimeType); // int
```

##### final&const的使用

final和const都是用于定义常量的, 也就是定义之后值都不可以修改

```
final name = 'coderwhy';
name = 'kobe'; // 错误做法

const age = 18;
age = 20; // 错误做法
```

final和const有什么区别呢?

- const在赋值时, 赋值的内容必须是在编译期间就确定下来的
- final在赋值时, 可以动态获取, 比如赋值一个函数

```
String getName() {
  return 'coderwhy';
}

main(List<String> args) {
  const name = getName(); // 错误的做法, 因为要执行函数才能获取到值
  final name = getName(); // 正确的做法
}
```

### 数据类型

#### 数字类型

对于数值来说，我们也不用关心它是否有符号，以及数据的宽度和精度等问题。

只要记着整数用`int`，浮点数用`double`就行了。

不过，要说明一下的是Dart中的`int`和`double`可表示的范围并不是固定的，它取决于运行Dart的平台。

```
// 1.整数类型int
int age = 18;
int hexAge = 0x12;
print(age);
print(hexAge);

// 2.浮点类型double
double height = 1.88;
print(height);
```

#### 布尔类型

布尔类型中,Dart提供了一个bool的类型, 取值为true和false

```
// 布尔类型
var isFlag = true;
print('$isFlag ${isFlag.runtimeType}');
```

> 注意: Dart中不能判断非0即真, 或者非空即真

#### 字符串类型

```
// 1.定义字符串的方式
var s1 = 'Hello World';
var s2 = "Hello Dart";
var s3 = 'Hello\'Fullter';
var s4 = "Hello'Fullter";
```

我们可以创建多行字符串

```
// 2.表示多行字符串的方式
var message1 = '''
哈哈哈
呵呵呵
嘿嘿嘿''';
```

字符串和其他变量或表达式拼接: 使用${expression}, 如果表达式是一个标识符, 那么{}可以省略

```
// 3.拼接其他变量
var name = 'coderwhy';
var age = 18;
var height = 1.88;
print('my name is ${name}, age is $age, height is $height');
```

### 集合类型

对于集合类型，Dart则内置了最常用的三种：`List / Set / Map`。

其中，`List`可以这样来定义：

```
// List定义
// 1.使用类型推导定义
var letters = ['a', 'b', 'c', 'd'];
print('$letters ${letters.runtimeType}');

// 2.明确指定类型
List<int> numbers = [1, 2, 3, 4];
print('$numbers ${numbers.runtimeType}');
```

其中，`set`可以这样来定义：

- 其实，也就是把`[]`换成`{}`就好了。
- `Set`和`List`最大的两个不同就是：`Set`是无序的，并且元素是不重复的。

```
// Set的定义
// 1.使用类型推导定义
var lettersSet = {'a', 'b', 'c', 'd'};
print('$lettersSet ${lettersSet.runtimeType}');

// 2.明确指定类型
Set<int> numbersSet = {1, 2, 3, 4};
print('$numbersSet ${numbersSet.runtimeType}');
```

最后，`Map`是我们常说的字典类型，它的定义是这样的：

```
// Map的定义
// 1.使用类型推导定义
var infoMap1 = {'name': 'why', 'age': 18};
print('$infoMap1 ${infoMap1.runtimeType}');

// 2.明确指定类型
Map<String, Object> infoMap2 = {'height': 1.88, 'address': '北京市'};
print('$infoMap2 ${infoMap2.runtimeType}');
```

#### `Map`的操作

由于它有key和value，因此无论是读取值，还是操作，都要明确是基于key的，还是基于value的，或者是基于key/value对的。

```
// Map的操作
// 1.根据key获取value
print(infoMap1['name']); // why

// 2.获取所有的entries
print('${infoMap1.entries} ${infoMap1.entries.runtimeType}'); // (MapEntry(name: why), MapEntry(age: 18)) MappedIterable<String, MapEntry<String, Object>>

// 3.获取所有的keys
print('${infoMap1.keys} ${infoMap1.keys.runtimeType}'); // (name, age) _CompactIterable<String>

// 4.获取所有的values
print('${infoMap1.values} ${infoMap1.values.runtimeType}'); // (why, 18) _CompactIterable<Object>

// 5.判断是否包含某个key或者value
print('${infoMap1.containsKey('age')} ${infoMap1.containsValue(18)}'); // true true

// 6.根据key删除元素
infoMap1.remove('age');
print('${infoMap1}'); // {name: why}
```

### 函数

#### 函数的基本定义

Dart是一种真正的面向对象语言，所以即使函数也是对象，所有也有类型, 类型就是Function。

这也就意味着函数可以作为变量定义或者作为其他函数的参数或者返回值.

函数的定义方式:

```
返回值类型 函数的名称(参数列表) {
  函数体
  return 返回值
}
```

按照上面的定义方式, 我们定义一个完整的函数:

```
int sum(num num1, num num2) {
  return num1 + num2;
}
```

另外, 如果函数中只有一个表达式, 那么可以使用箭头语法(*arrow* syntax)

```
sum(num1, num2) => num1 + num2;
```

> 注意:这里面只能是一个表达式, 不能是一个语句

#### 函数的参数问题

可选参数可以分为 **命名可选参数** 和 **位置可选参数**

```
命名可选参数: {param1, param2, ...}
位置可选参数: [param1, param2, ...]
```

```
// 命名可选参数
printInfo1(String name, {int age, double height}) {
  print('name=$name age=$age height=$height');
}
```

我们在使用命名参数的时候我们想进行调用必须加上名字

```
// 命名可选参数的函数调用
printInfo1('why', age: 18); // name=why age=18 height=null
```

而我们使用 可选参数的时候 就按照这个去写就行 不想传递的参数可以不去传递

```
printInfo2('why', 18); // name=why age=18 height=null// 定义位置可选参数
printInfo2(String name, [int age, double height]) {
  print('name=$name age=$age height=$height');
}

// 调用可选参数函数
printInfo2('why', 18); // name=why age=18 height=null
```

#### 默认参数

只有可选参数才可以有默认值, 必须参数不能有默认值

```
// 参数的默认值
printInfo4(String name, {int age = 18, double height=1.88}) {
  print('name=$name age=$age height=$height');
}
```

#### 作用域

dart中的词法有自己明确的作用域范围，它是根据代码的结构({})来决定作用域范围的

优先使用自己作用域中的变量，如果没有找到，则一层层向外查找。

```
var name = 'global';
main(List<String> args) {
  // var name = 'main';
  void foo() {
    // var name = 'foo';
    print(name);
  }

  foo();
}
```

