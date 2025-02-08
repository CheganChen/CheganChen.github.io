---
title: lua基础
date: 2025-02-07 18:45:45
tags: 脚本语言
---

# 简介

Lua的设计目的是为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。  

作为一门过程型动态语言，Lua有着如下的特性：

- 变量名没有类型，值才有类型，变量名在运行时可与任何类型的值绑定;
- 语言只提供唯一一种数据结构，称为表(table)，它混合了数组、哈希，可以用任何类型的值作为 key 和 value。提供了一致且富有表达力的表构造语法，使得 Lua 很适合描述复杂的数据;
- 函数是一等类型，支持匿名函数和正则尾递归(proper tail recursion);
- 支持词法定界(lexical scoping)和闭包(closure);
- 提供 thread 类型和结构化的协程(coroutine)机制，在此基础上可方便实现协作式多任务;
- 运行期能编译字符串形式的程序文本并载入虚拟机执行;
- 通过元表(metatable)和元方法(metamethod)提供动态元机制(dynamic metamechanism)，从而允许程序运行时根据需要改变或扩充语法设施的内定语义;
- 能方便地利用表和动态元机制实现基于原型(prototype-based)的面向对象模型;
- 从 5.1 版开始提供了完善的模块机制，从而更好地支持开发大型的应用程序;


Lua应用场景：

- 游戏开发
- 独立应用脚本
- Web 应用脚本
- 扩展和数据库插件如：MySQL Proxy 和 MySQL WorkBench
- 安全系统，如入侵检测系统

## 安装

Lua最新版本是Lua 5.3.4（截止到2018-3-18）。  

官网：http://www.lua.org/

### Linux 系统上安装

Linux & Mac上安装 Lua 安装非常简单，只需要下载源码包并在终端解压编译即可，本文使用了5.3.0版本进行安装：

```
curl -R -O http://www.lua.org/ftp/lua-5.3.0.tar.gz
tar zxf lua-5.3.0.tar.gz
cd lua-5.3.0
make linux test
make install
```

### Mac OS X 系统上安装

```
curl -R -O http://www.lua.org/ftp/lua-5.3.0.tar.gz
tar zxf lua-5.3.0.tar.gz
cd lua-5.3.0
make macosx test
make install
```

Mac 上也可以通过 homebrew 安装:

```
brew install lua
```

### Window 系统上安装 Lua

window下你可以使用一个叫"SciTE"的IDE环境来执行lua程序，下载地址为：
Github 下载地址：https://github.com/rjpcomputing/luaforwindows/releases
Google Code下载地址 : https://code.google.com/p/luaforwindows/downloads/list
双击安装后即可在该环境下编写 Lua 程序并运行。

你也可以使用 Lua 官方推荐的方法使用 LuaDist：http://luadist.org/

安装好后查看版本：

```
$ lua -v
Lua 5.1.4  Copyright (C) 1994-2008 Lua.org, PUC-Rio
```

### Lua 和 LuaJIT 的区别

Lua 非常高效，它运行得比许多其它脚本(如 Perl、Python、Ruby)都快，这点在第三方的独立测评中得到了证实。尽管如此，仍然会有人不满足，他们总觉得还不够快。LuaJIT 就是一个为了再榨出一些速度的尝试，它利用即时编译（Just-in Time）技术把 Lua 代码编译成本地机器码后交由 CPU 直接执行。  

LuaJIT 2 的测评报告表明，在数值运算、循环与函数调用、协程切换、字符串操作等许多方面它的加速效果都很显著。  

凭借着 FFI 特性，LuaJIT 2 在那些需要频繁地调用外部 C/C++ 代码的场景，也要比标准 Lua 解释器快很多。  

目前 LuaJIT 2 已经支持包括 i386、x86_64、ARM、PowerPC 以及 MIPS 等多种不同的体系结构。

LuaJIT 是采用 C 和汇编语言编写的 Lua 解释器与即时编译器。LuaJIT 被设计成全兼容标准的 Lua 5.1 语言，同时可选地支持 Lua 5.2 和 Lua 5.3 中的一些不破坏向后兼容性的有用特性。因此，标准 Lua 语言的代码可以不加修改地运行在 LuaJIT 之上。  

LuaJIT 和标准 Lua 解释器的一大区别是，LuaJIT 的执行速度，即使是其汇编编写的 Lua 解释器，也要比标准 Lua 5.1 解释器快很多，可以说是一个高效的 Lua 实现。另一个区别是，LuaJIT 支持比标准 Lua 5.1 语言更多的基本原语和特性，因此功能上也要更加强大。

LuaJIT 官网链接：http://luajit.org

### OpenResty 与 Lua、LuaJIT

自从 OpenResty 1.5.8.1 版本之后，默认捆绑的 Lua 解释器就被替换成了 LuaJIT，而不再是标准 Lua。也就是，我们安装了OpenResty也会包含Lua解释器。

OpenResty 官网链接：http://openresty.org/cn/

##  基础语法

### Hello World

我们创建一个 HelloWorld.lua 文件，代码如下:

``` lua
print("Hello World!")
```

执行以下命令:

```
$ lua HelloWorld.lua
```

输出结果为：
Hello World!

Lua也提供了交互式编程。打开命令行输入`lua`就会进入交互式编程模式:

```
$ lua
Lua 5.1.4  Copyright (C) 1994-2008 Lua.org, PUC-Rio
> print("Hello World!")
Hello World!
>
```

### 注释

两个减号是单行注释:

``` lua
--
```

多行注释:

``` lua
--[[
 多行注释
 多行注释
 --]]
```

### 标示符

Lua标示符以一个字母 A 到 Z 或 a 到 z 或下划线`_`开头后加上0个或多个字母，下划线，数字（0到9）。  
最好不要使用下划线加大写字母的标示符，因为Lua的保留字也是这样的。   
Lua 不允许使用特殊字符如 `@`, `$`, 和 `%` 来定义标示符。 
Lua区分大小写。

### 关键词

以下列出了 Lua 的保留关键字。保留关键字不能作为常量或变量或其他用户自定义标示符：

```
and	break	do	else
elseif	end	false	for
function	if	in	local
nil	not	or	repeat
return	then	true	until
while		
```

一般约定，以下划线开头连接一串大写字母的名字（比如 `_VERSION` ）被保留用于 Lua 内部全局变量。  


### 变量定义

变量在使用前，必须在代码中进行声明，即创建该变量。  

Lua是动态类型语言，变量不要类型定义,只需要为变量赋值：

``` lua
name = "yjc"
year = 2018
a = true
score = 98.01
```

变量的默认值均为 `nil`。访问一个没有初始化的全局变量不会出错，只不过返回结果是：nil。当然如果你想删除一个全局变量，只需要将变量赋值为nil即可：

``` lua
score = 98.01
score = nil
```

Lua 变量作用域:

``` lua
a = 10	--全局变量
local b = 10 	--局部变量
```

Lua 中的局部变量要用`local`关键字来显式定义，不使用 local 显式定义的变量就是全局变量。就算在`if`等语句块中，只要没使用`local`关键字来显式定义，也是全局变量，这一点和别的语言不同。  

实际编程中尽量使用局部变量。


## 拓展阅读

1、http://notebook.kulchenko.com/programming/lua-good-different-bad-and-ugly-parts



# 数据类型



---

Lua中有8个基本类型分别为：nil、boolean、number、string、table、function、userdata、thread。  

函数 `type` 能够返回一个值或一个变量所属的类型：

``` lua
print(type("hello world")) -->output:string
print(type(print)) -->output:function
print(type(true)) -->output:boolean
print(type(360.0)) -->output:number
print(type(nil)) -->output:nil
```

## nil

只有值`nil`属于该类，表示一个无效值（在条件表达式中相当于false）。一个变量在第一次赋值前的默认值是 `nil`，将
nil 赋予给一个全局变量就等同于删除它。

``` lua
local num
print(num) -->output:nil

num = 100
print(num) -->output:100
```

## boolean

boolean 类型只有两个可选值：`true`和`false`，Lua 把 `false` 和 `nil` 看作是"假"，其他的都为"真"，比如 0 和空字符串就是"真"，这和C、PHP等语言不一样。

``` lua
local a = true
local b = 0
local c = nil

if a then
	print("a") -->output:a
else
	print("not a") --这个没有执行
end

if b then
	print("b") -->output:b
else
	print("not b") --这个没有执行
end

if c then
	print("c") --这个没有执行
else
	print("not c") -->output:not c
end
```

输出：

```
a
b
not c
```

## number

Lua 默认只有一种 number 类型: double（双精度）类型，以下几种写法都被看作是 `number` 类型：

``` lua
print(type(2))
print(type(2.2))
print(type(0.2))
print(type(2e+1))
print(type(0.2e-1))
print(type(7.8263692594256e-06))
```

## string

字符串由一对双引号或单引号来表示:

``` lua
string1 = "this is string1\n"
string2 = 'this is string2\n'
print(string1)
print(string2)
```

输出：

```
this is string1

this is string2

```

可以看出单引号里面转义字符也生效。说明Lua不区分单引号、双引号。    

也可以用 2 个方括号 "[[]]" 来表示"一块"字符串。我们把两个正的方括号（即`[[`）间插入 n 个等号定义为第 n 级正长括号:

``` lua
string3 = [[this is string3\n]] -- 0 级正的长括号
string4 = [=[this is string4\n]=] -- 1 级正的长括号
string5 = [==[this is string5\n]==] -- 2 级正的长括号
string6 = [====[ this is string6\n[===[]===] ]====] -- 4 级正的长括号，可以包含除了本级别的反长括号外的所有内容
print(string3)
print(string4)
print(string5)
print(string6)
```

输出：

```
this is string3\n
this is string4\n
this is string5\n
 this is string6\n[===[]===]
```

注意：由方括号包含的字符串，整个词法分析过程将不受分行限制，不处理任何转义符，并且忽略掉任何不同级别的长括号。    

另外，需要注意的就是：Lua的字符串是不可改变的值，不能像在 c 语言中那样直接修改字符串的某个字符，而是根据修改要求来创建一个新的字符串。Lua 也不能通过下标来访问字符串的某个字符。  

字符串使用`..`拼接：

``` lua
string7 = string3..string4
print(string7)
```

输出：

```
this is string3\nthis is string4\n
```

在对一个数字字符串上进行算术操作时，Lua 会尝试将这个数字字符串转成一个数字:

``` bash
> print("2" + 6)
8.0
```

使用 `#` 来计算字符串的长度，放在字符串前面，如下实例：

``` lua
local string8 = "this is string8"
print(#string8) -- 输出：15
```

## table

Table 类型实现了一种抽象的“关联数组”。“关联数组”是一种具有特殊索引方式的数组，索引通常是`string`或者`number`类型，但也可以是除 `nil` 以外的任意类型的值。  

PHP程序员对此会很熟悉，因为PHP里的数组(`array`)和Table非常类似。示例：

``` lua
local tmp = {
	name = "lua",
	-- "name2" = "lua2", -- 错误的表示
	["name3"] = "lua",
	year = 2018,
	pi = 3.14159,
	lang = {"c", "java", "lua"},
	100,  -- 相当于[1] = 100，此时索引为数字。lua里数字索引是从1开始的，不是0
	-- 10 = 11, -- 错误的表示
	[10] = 11, -- 相当于[10] = 11，此时索引为数字
}

print(tmp.name)  -- 等同于 print(tmp["name"])
print(tmp["name3"])  -- 等同于 print(tmp.name3)
print(tmp.year)
print(tmp.pi)
print(tmp.lang[1])
print(tmp[1])
print(tmp[10])
```

输出：

```
lua
lua
2018
3.14159
c
100
11
```

在 Lua 里表的默认初始索引一般以 1 开始，而不是0，这点需要注意。   

也可以先创建一个空表，再添加数据：

``` lua
a = {}
a.key = "value"
```


在内部实现上，table 通常实现为一个哈希表、一个数组、或者两者的混合。具体的实现为何种形式，动态依赖于具体的 table 的键分布特点。


## function

在 Lua 中，`函数` 也是一种数据类型，函数可以存储在变量中，可以通过参数传递给其他函数，还可以作为其他函数的返回值。

``` lua
function maxNumber(a, b)
	if a > b then
		return a
	else
		return b
	end
end

local testFunc = maxNumber

print(testFunc(10,100));
```

输出：  

```
100  
```

有名函数的定义本质上是匿名函数对变量的赋值：

``` lua
function foo()
end
```

等价于：

``` lua
foo = function ()
end
```

## userdata

userdata 是一种用户自定义数据，用于表示一种由应用程序或 C/C++ 语言库所创建的类型，可以将任意 C/C++ 的任意数据类型的数据（通常是 struct 和 指针）存储到 Lua 变量中调用。


## thread

在 Lua 里，最主要的线程是协同程序（coroutine）。它跟线程（thread）差不多，拥有自己独立的栈、局部变量和指令指针，可以跟其他协同程序共享全局变量和其他大部分东西。
线程跟协程的区别：线程可以同时多个运行，而协程任意时刻只能运行一个，并且处于运行状态的协程只有被挂起（suspend）时才会暂停。

# 运算符

---


Lua支持下列主要的运算符：

- 算术运算符
- 关系运算符
- 逻辑运算符
- 赋值运算符

还支持`..`、`#`特殊运算符。其中赋值运算符仅支持`=`，不支持C语言的`+=`、`++`等运算符。

## 算术运算符

```
+ 加法
- 减法或者负号
* 乘法
/ 除法
^ 指数
% 取模
```

``` lua
print(1 + 2) -->打印 3
print(5 / 10) -->打印 0.5。 这是Lua不同于c语言的
print(5.0 / 10) -->打印 0.5。 浮点数相除的结果是浮点数
-- print(10 / 0) -->注意除数不能为0，计算的结果会出错
print(2 ^ 10) -->打印 1024。 求2的10次方
```

## 关系运算符

```
< 小于
> 大于
<= 小于等于
>= 大于等于
== 等于
~= 不等于
```

注意：Lua 语言中“不等于”运算符的写法为：`~=`。  

由于 Lua 字符串总是会被“内化”，即相同内容的字符串只会被保存一份，因此 Lua 字符串之间的相等性比较可以简化为其内部存储地址的比较。其它语言一般需要逐个字节（或按
若干个连续字节）进行比较。  

## 逻辑运算符

```
and 逻辑与
or 逻辑或
not 逻辑非
```

Lua 中的 `and` 和 `or` 是不同于 c 语言的。在 c 语言中，`and` 和 `or` 只得到两个值 1 和 0，其中 1 表示真，0 表示假。而 Lua 中 `and` 的执行过程是这样的：

- a and b 如果 a 为 nil，则返回 a，否则返回 b;
- a or b 如果 a 为 nil，则返回 b，否则返回 a。

``` lua
a = 10
b = nil

print(a and b)
print(b and a)
print(a or b)
print(b or a)
```

输出：

```
nil
nil
10
10
```

## 赋值运算符

```
= 简单的赋值运算符，把右边操作数的值赋给左边操作数
```

## 其它运算符

```
..	连接两个字符串
#	一元运算符，返回字符串或表的长度。
```

``` lua
print("hello".." world") -- 输出：hello world
print(#"hello") -- 输出：5
```

# 控制语句

---

Lua 语言提供的控制结构有 `if-else`，`while`，`repeat`，`for`，并提供 `break`、`return` 关键字来满足更丰富的需求。不支持`switch`、`continue`。 

Lua 提供的控制语句部分特征类似Shell和Python：

- 语句都以`end`结束
- `if`后面都有`then`
- 没有花括号`{}`
- 循环结构`while`、`for`表达式后面都有关键字`do`; python里是用的`:`

##  if-else

单个 if 分支 型:

``` lua
a = 10
if a > 0 then
	print(a)
end
```

两个分支 if-else 型：

``` lua
a = 10
b = 11
if a > b then
	print(a)
else
	print(b)
end
```

多个分支 if-elseif-else 型：

``` lua
a = 10
b = 11
if a > b then
	print(a)
elseif a < b then
	print(b)
else
	print(a)
end
```

与 C 语言的不同之处是 `else` 与 `if` 是连在一起的，若将 `else` 与 `if` 写成 `else if` 则相当于在`else` 里嵌套另一个 `if`语句。


## while

Lua 跟其他常见语言一样，提供了 `while` 控制结构，语法上也没有什么特别的。但是没有提供 `do-while` 型的控制结构，但是提供了功能相当的 `repeat`。 

``` lua
sum = 0
i = 0
while i<=100 do
	sum = sum + i
	i = i + 1;
end
print(sum) -- 5050
```

>注意：Lua 并没有像许多其他语言那样提供类似 `continue` 这样的控制语句用来跳过当前循环。

再看看 `repeat` 的用法：

``` lua
sum = 0
i = 0
repeat
	sum = sum + i
	i = i + 1;
until i>100
print(sum) -- 5050
```

## for

for有两种结构：数字 for（numeric for） 和范型 for（generic for）。

数字 for 类似C语言的用法；范型 for 类似Python里的`for...in`用法。

### for 数字型

数字型 for 的语法如下：

``` lua
for var = begin, finish, step do
	--body
end
```

需要关注以下几点： 

- `var` 从 `begin` 变化到 `finish`，每次变化都以 `step` 作为步长递增 `var`
- `begin`、`finish`、`step` 三个表达式只会在循环开始时执行一次 
- 第三个表达式 `step`是可选的，默认为 `1`
- 控制变量 `var` 的作用域仅在 `for` 循环内，需要在外面控制，则需将值赋给一个新的变量 
- 循环过程中不要改变控制变量的值，那样会带来不可预知的影响

示例：

``` lua
sum = 0
for i=0,100,1 do
	sum = sum + i
end
print(sum) -- 5050
```

### for 泛型

Lua 编程语言中泛型for循环语法格式:

``` lua
-- 打印数组a的所有值  
for i,v in ipairs(a) do 
	print(v) 
end  
```

`i`是数组索引值，`v`是对应索引的数组元素值。`ipairs`是`Lua`提供的一个迭代器函数，用来迭代数组。

示例：

``` lua
days = {
	"Monday",
	"Tuesday",
	"Wednesday",
	"Thursday",
	"Friday",
}

for k,v in ipairs(days) do
	print(k,v)
end
```

输出：

```
1       Monday
2       Tuesday
3       Wednesday
4       Thursday
5       Friday
```

>ipairs以及pairs 的不同:
>pairs可以遍历表中所有的key，并且除了迭代器本身以及遍历表本身还可以返回nil; 但是ipairs则不能返回nil，只能返回数字0，如果值遇到nil则直接跳出循环退出。

示例：

``` lua
local tabFiles = {  
	[1] = "test1",  
	[6] = "test2",  
	[4] = "test3"  
}  

for k,v in ipairs(tabFiles) do
	print(k,v)
end
```

输出：

```
1       test1
```

`ipairs`遍历时，当key=2时候value就是nil，所以直接跳出循环。    

如果换成`pairs`，则全部输出:

```
1       test1
6       test2
4       test3
```

>值得一提的是，在 LuaJIT 2.1 中， `ipairs()` 内建函数是可以被 `JIT` 编译的，而 `pairs()` 则只能被解释执行。


## break

语句 `break` 用来终止 `while` 、 repeat 和 for 三种循环的执行，并跳出当前循环体， 继续执行当前循环之后的语句。

## return

`return` 主要用于从函数中返回结果，或者用于简单的结束一个函数的执行。  

需要注意的是： return 只能写在语句块的最后，一旦执行了 return 语句，该语句之后的所有语句都不会再执行。  

若要写在函数中间，则只能写在一个显式的语句块内，否则会报错：

``` lua
function test1(x, y)
	return x+y;
	-- print(x+y)
	-- 后面的print如果不注释，会报错
end

function test2(x, y)
	if x > y then
		return x
	else
		return y
	end
	print("end") -- 此处的print不注释不会报错，因为return只出现在前面显式的语句块
end

function test3(x, y)
	print(x+y)
	do return end
	print(x) -- 此处的print不注释不会报错，因为return由do...end语句块包含。这一行语句永远不会执行到
end

test1(10,11)
test2(10,11)
test3(10,11)
```

所以，有时候为了调试方便，我们可以想在某个函数的中间提前 return ，以进行控制流的短路，此时我们可以将 return 放在一个 `do...end` 代码块中。

# 函数

---

在 Lua 中，函数 也是一种数据类型，函数可以存储在变量中，可以通过参数传递给其他函数，还可以作为其他函数的返回值。

## 函数定义

函数定义格式：

``` lua
function function_name (arc) -- arc 表示参数列表，函数的参数列表可以为空
	-- body
end
```

支持使用`local`定义为局部作用域的函数。  

>由于函数定义本质上就是变量赋值，而变量的定义总是应放置在变量使用之前，所以函数的定义也需要放置在函数调用之前。  


由于函数定义等价于变量赋值，我们也可以把函数名替换为某个 Lua 表的某个字段，例如：

``` lua
function foo.bar(a, b, c)
	-- body ...
end
```

对于此种形式的函数定义，不能再使用 `local` 修饰符了，因为不存在定义新的局部变量了。  

## 函数参数

支持固定参数和变长参数。固定参数很好理解，变长参数则是使用`...`定义的：

``` lua
function function_name (...)
	local args = {...} or {} 
end
```

注意：在调用函数的时候，如果实参和形参个数不一样的时候：

- 实参缺少，则使用nil代替
- 实参大于形参，则忽略


>LuaJIT 2 尚不能 JIT 编译这种变长参数的用法，只能解释执行。

Lua函数的参数大部分是按值传递的。值传递就是调用函数时，实参把它的值通过赋值运算传递给形参，然后形参的改变和实参就没有关系了。  

当函数参数是 `table` 类型时，传递进来的是实际参数的引用，此时在函数内部对该 table 所做的修改，会直接对调用者所传递的实际参数生效。

## 函数返回值

Lua可以返回多个值，这点和Python、Go类似，不同于C、PHP等语言。返回多个值时，值之间用“,”隔开。

``` lua
local function swap(a, b) 
	return b, a -- 按相反顺序返回变量的值
end
print(swap(1,2))
```

输出：

```
2	1
```

注意：在调用函数的时候，如果返回值的个数和接收返回值的变量的个数不一致时：

- 返回值缺少，则使用nil代替
- 返回值个数大于接收变量的个数，则忽略  

当一个方法返回多个值时，有些返回值有时候用不到，要是声明很多变量来一一接收，显然不太合适 。Lua 提供了一个虚变量(dummy variable)，以单个下划线（“_”） 来命名，用它来丢弃不需要的数值，仅仅起到占位的作用。这点和Go用法一致。

``` lua
local function test_var()
	return 1,2,3
end

local x,_,y = test_var()
print(x,y) -- 1	3
```

## 函数动态调用

函数动态调用是指：调用回调函数，并把一个数组参数作为回调函数的参数。  

主要用到了`unpack`函数和可变参数。  

示例：

``` lua
function do_action(func, ...)
	local args = {...} or {} -- 防止为nil
	func(unpack(args, 1, table.maxn(args))) -- 如果实参中确定没有nil空洞（nil值被夹在非空值之间），可以只写第一个参数 
end

local function add(x, y)
	print(x+y)
end

local function add2(x, y, z)
	print(x+y+z)
end

do_action(add, 1, 1)
do_action(add2, 1, 1, 1)
```

输出：

```
2
3
```

>`unpack` 内建函数还不能为 LuaJIT 所 JIT 编译，因此这种用法总是会被解释执行。

# 模块

---

## 模块

从Lua5.1开始，Lua添加了对模块和包的支持。  

Lua 的模块是由变量、函数等已知元素组成的 table，因此创建一个模块很简单，就是创建一个table，然后把需要导出的常量、函数放入其中，最后返回这个 table 就行。   

示例：  

mymodule.lua

``` lua
local _M = {}

_M.VERSION = "1.0"

_M.getName = function()
	return "get"
end

return _M
```

使用`require`即可引入模块：

``` lua
local m = require "mymodule"
print(m.VERSION)
print(m.getName())
```

输出：

```
1.0
get
```

## 点号与冒号操作符的区别

看下面示例代码：

``` lua
local str = "abcde"
print("case 1:", str:sub(1, 2))
print("case 2:", str.sub(str, 1, 2))
```

输出：

```
case 1: ab
case 2: ab
```

冒号操作会带入一个 `self` 参数，用来代表 `自己` 。而点号操作，只是内容的展开，需要手动传入`self` 参数。  

在函数定义时，使用冒号将默认接收一个 `self` 参数，而使用点号则需要显式传入 `self` 参数。  

示例：

``` lua
mytable = {}
mytable.func1 = function(self, name)
	self.name = name
end

mytable:func2 = function(name)
	self.name = name
end
```

这里的`func1`和`func2`是等价的。

# 常用库介绍

---

## String 库

- `..` 链接两个字符串
- `string.upper(argument)` 字符串全部转为大写字母。
- `string.lower(argument)` 字符串全部转为小写字母。
- `string.len(arg)` 计算字符串长度
- `string.reverse(arg)` 字符串反转
- `string.format(...)` 返回一个类似printf的格式化字符串
- `string.byte(s [, i [, j ]])` 转换字符为整数值(可以指定某个字符，默认第一个字符)
- `string.char(arg)` 将整型数字转成字符并连接
- `string.rep(string, n)` 返回字符串string的n个拷贝
- `string.gsub(mainString, findString, replaceString, num)`
  在字符串中替换,mainString为要替换的字符串， findString 为被替换的字符，replaceString 要替换的字符，num 替换次数（可以忽略，则全部替换）
- `string.find (str, substr, [init, [end]])`
  在一个指定的目标字符串中搜索指定的内容(第三个参数为索引),返回其具体位置。不存在则返回 nil。
- `string.gmatch(str, pattern)`
  返回一个迭代器函数，每一次调用这个函数，返回一个在字符串 str 找到的下一个符合 pattern 描述的子串。如果参数 pattern 描述的字符串没有找到，迭代函数返回nil。
- `string.match(str, pattern, init)` 
  寻找源字串str中的第一个配对. 参数init可选, 指定搜寻过程的起点, 默认为1。 在成功配对时, 函数将返回配对表达式中的所有捕获结果; 如果没有设置捕获标记, 则返回整个配对字符串. 当没有成功的配对时, 返回nil。


### 字符串格式化

`string.format()`类似c里的`printf()`。示例：

``` lua
print(string.format("name %s, age %s", "yjc", 20)) -- name yjc, age 20
print(string.format("%.4f",1/3)) -- 0.3333
```

格式字符串可能包含以下的转义码:

- `%c` - 接受一个数字, 并将其转化为ASCII码表中对应的字符
- `%d`, `%i` - 接受一个数字并将其转化为有符号的整数格式
- `%o` - 接受一个数字并将其转化为八进制数格式
- `%u` - 接受一个数字并将其转化为无符号整数格式
- `%x` - 接受一个数字并将其转化为十六进制数格式, 使用小写字母
- `%X` - 接受一个数字并将其转化为十六进制数格式, 使用大写字母
- `%e` - 接受一个数字并将其转化为科学记数法格式, 使用小写字母e
- `%E` - 接受一个数字并将其转化为科学记数法格式, 使用大写字母E
- `%f` - 接受一个数字并将其转化为浮点数格式
- `%g(%G)` - 接受一个数字并将其转化为%e(%E, 对应%G)及%f中较短的一种格式
- `%q` - 接受一个字符串并将其转化为可安全被Lua编译器读入的格式
- `%s` - 接受一个字符串并按照给定的参数格式化该字符串

为进一步细化格式, 可以在`%`号后添加参数. 参数将以如下的顺序读入:

- 符号: 一个+号表示其后的数字转义符将让正数显示正号. 默认情况下只有负数显示符号.
- 占位符: 一个0, 在后面指定了字串宽度时占位用. 不填时的默认占位符是空格.
- 对齐标识: 在指定了字串宽度时, 默认为右对齐, 增加-号可以改为左对齐.
- 宽度数值
- 小数位数/字串裁切: 在宽度数值后增加的小数部分n, 若后接f(浮点数转义符, 如`%6.3f`)则设定该浮点数的小数只保留n位, 若后接s(字符串转义符, 如`%5.3s`)则设定该字符串只显示前n位.


> 注：`string.match()`、`string.gmatch()` 目前并不能被 JIT 编译，OpenResty 里应尽量使用 `ngx_lua` 模块提供的 `ngx.re.match` 等API。

## Table 库

- `table.concat (table [, sep [, start [, end]]])`
  concat是concatenate(连锁, 连接)的缩写. table.concat()函数列出参数中指定table的数组部分从start位置到end位置的所有元素, 元素间以指定的分隔符(sep)隔开。
- `table.insert (table, [pos,] value)`
  在table的数组部分指定位置(pos)插入值为value的一个元素. pos参数可选, 默认为数组部分末尾.
- `table.maxn (table)`
  指定table中所有正数key值中最大的key值. 如果不存在key值为正数的元素, 则返回0。(**Lua5.2之后该方法已经不存在了,本文使用了自定义函数实现**)
- `table.remove (table [, pos])`
  返回table数组部分位于pos位置的元素. 其后的元素会被前移. pos参数可选, 默认为table长度, 即从最后一个元素删起。
- `table.sort (table [, comp])`
  对给定的table进行升序排序。

## 日期时间库

- `os.time ([table])` 如果不使用参数 table 调用 time 函数，它会返回当前的时间和日期（它表示从某一时刻到现在的秒数）。如果用 table 参数，它会返回一个数字，表示该 table 中 所描述的日期和时间（它表示从某一时刻到 table 中描述日期和时间的秒数）。
- `os.difftime (t2, t1)` 返回 t1 到 t2 的时间差，单位为秒。
- `os.date ([format [, time]])` 把一个表示日期和时间的数值，转换成更高级的表现形式。其第一个参数 format 是一个格式化字符串，描述了要返回的时间形式。第二个参数 time 就是日期和时间的数字表示，缺省时默认为当前的时间。

> 如果使用OpenResty，不建议使用Lua的标准时间函数，因为这些函数通常会引发不止一个昂贵的系统调用，同时无法为 LuaJIT JIT 编译，对性能造成较大影响。推荐使用 ngx_lua 模块提供的带缓存的时间接口，如 `ngx.today`, `ngx.time`, `ngx.utctime`, `ngx.localtime`, `ngx.now`, `ngx.http_time`，以及 `ngx.cookie_time` 等。

## 数学库

常用数学函数：  

- `math.rad(x)`  角度x转换成弧度
- `math.deg(x)`  弧度x转换成角度
- `math.max(x, ...)`  返回参数中值最大的那个数，参数必须是number型
- `math.min(x, ...)`  返回参数中值最小的那个数，参数必须是number型
- `math.random ([m [, n]])`  不传入参数时，返回 一个在区间[0,1)内均匀分布的伪随机实数；只使用一个整数参数m时，返回一个在区间[1, m]内均匀分布的伪随机整数；使用两个整数参数时，返回一个在区间[m, n]内均匀分布的伪随机整数
- `math.randomseed (x)`  为伪随机数生成器设置一个种子x，相同的种子将会生成相同的数字序列
- `math.abs(x)`  返回x的绝对值
- `math.fmod(x, y)`  返回 x对y取余数
- `math.pow(x, y)`  返回x的y次方
- `math.sqrt(x)`  返回x的算术平方根
- `math.exp(x)`  返回自然数e的x次方
- `math.log(x)`  返回x的自然对数
- `math.log10(x)`  返回以10为底，x的对数
- `math.floor(x)`  返回最大且不大于x的整数
- `math.ceil(x)`  返回最小且不小于x的整数
- `math.pi`	圆周率
- `math.sin(x)`  求弧度x的正弦值
- `math.cos(x)`  求弧度x的余弦值
- `math.tan(x)`  求弧度x的正切值
- `math.asin(x)`  求x的反正弦值
- `math.acos(x)`  求x的反余弦值
- `math.atan(x)`  求x的反正切值


示例：

``` lua
-- src/test_math.lua
print(math.pi) -- 3.1415926535898
print(math.pow(2,3))  -- 8
print(math.max(-1, 2, 0, 3.6, 9.1))     --  9.1
print(math.floor(3.14159))  -- 3
print(math.ceil(7.9988))    -- 8
```

注意：使用 `math.random()` 函数获得伪随机数时，如果不使用 `math.randomseed()` 设置伪随机数生成种子或者设置相同的伪随机数生成种子，那么得得到的伪随机数序列是一样的。示例：

``` lua
-- math.randomseed(os.time())  -- 设置随机种子
print(math.random()) -- 0.79420629243124
print(math.random(10)) -- 7
print(math.random(10,20)) -- 16
```

上面的例子里同一机器运行多次的结果是一样的，只有设置了随机的伪随机数生成种子，才能保证每次生成的随机数是不相同的。

## 参考

1、OpenResty最佳实践   
https://moonbingbing.gitbooks.io/openresty-best-practices/  
2、Lua 5.3 参考手册 - 目录  
http://www.runoob.com/manual/lua53doc/contents.html  
3、Lua 字符串 | 菜鸟教程  
http://www.runoob.com/lua/lua-strings.html  

# 文件操作

---

Lua I/O 库用于读取和处理文件。分为简单模式、完全模式。

- 简单模式（simple model）
  拥有一个当前输入文件和一个当前输出文件，并且提供针对这些文件相关的操作。
- 完全模式（complete model） 
  使用外部的文件句柄来实现。它以一种面对对象的形式，将所有的文件操作定义为文件句柄的方法。

对文件进行简单的读写操作时可以使用简单模式，但是对文件进行一些高级的操作简单模式则处理不了了。

## 简单模式

简单模式由`io`模块提供，主要有：

- `io.open(filename [, mode])`: 以mode模式打开一个文件
- `io.input(file)`: 设置默认输入文件为file
- `io.output(file)`: 设置默认输出文件为file
- `io.write(content)`: 在文件最后一行添加content内容
- `io.read()`:  读取文件的一行。  
  参数可以是下列中的一个：

``` lua
"*n"	读取一个数字并返回它。例：file.read("*n")
"*a"	从当前位置读取整个文件。例：file.read("*a")
"*l"   （默认）读取下一行，在文件尾 (EOF) 处返回 nil。例：file.read("*l")
number	返回一个指定字符个数的字符串，或在 EOF 时返回 nil。例：file.read(5)
```

- `io.close(file)`: 关闭打开的文件file
- `io.tmpfile()`: 返回一个临时文件句柄，该文件以更新模式打开，程序结束时自动删除
- `io.type(file)`: 检测obj是否一个可用的文件句柄
- `io.flush()`: 向文件写入缓冲中的所有数据
- `io.lines(optional file name)`: 返回一个迭代函数,每次调用将获得文件中的一行内容,当到文件尾时，将返回nil,但不关闭文件

mode模式：

| 模式 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| r    | 以只读方式打开文件，该文件必须存在。                         |
| w    | 打开只写文件，若文件存在则文件长度清为0，即该文件内容会消失。若文件不存在则建立该文件。 |
| a    | 以附加的方式打开只写文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾，即文件原先的内容会被保留。（EOF符保留） |
| r+   | 以可读写方式打开文件，该文件必须存在。                       |
| w+   | 打开可读写文件，若文件存在则文件长度清为零，即该文件内容会消失。若文件不存在则建立该文件。 |
| a+   | 与a类似，但此文件可读可写                                    |
| b    | 二进制模式，如果文件是二进制文件，可以加上                   |
| b+   | 二进制模式，表示对文件既可以读也可以写                       |

示例：

``` lua
-- write
-- src/file_io_write.lua

local file = io.open("test_file.txt", 'w+')
io.output(file) -- 设置默认输出文件
io.write("hello lua!\nhahah") -- 把内容写到文件
io.close(file)
```

运行后查看生成的文件：

``` bash
$ cat test_file.txt
hello lua!
nhahah
```

``` lua
-- read
-- src/file_io_read.lua

local file = io.open("test_file.txt", 'r')
io.input(file) -- 设置默认输入文件

-- while true do
--     line = io.read()
--     if line == nil then
--         break;
--     end
--     print(line)
-- end

for line in io.lines() do 
    print(line)
end
io.close(file)
```

运行：

``` bash
$ luajit file_io_read.lua
hello lua!
hahah
```

## 完全模式

简单模式由`file`模块提供，主要有：

- `file:write(content)`: 在文件最后一行添加content内容
- `file:read()`:  读取文件的一行
- `file:close()`: 关闭打开的文件
- `file:seek(optional where, optional offset)`: 设置和获取当前文件位置,成功则返回最终的文件位置(按字节),失败则返回nil加错误信息。  

参数 where 值可以是:

``` lua
"set": 从文件头开始
"cur": 从当前位置开始[默认]
"end": 从文件尾开始
offset:默认为0
```

- `file:flush()`: 向文件写入缓冲中的所有数据

其中 `file` 为 `io.open()` 返回的文件句柄。

示例：

``` lua
-- read
-- src/file_read.lua

local file = io.open("test_file.txt", 'r')

file:seek("end", -5) -- 定位到文件倒数第 5 个位置
print(file:read("*a")) -- 从当前位置读取整个文件

file:close()  -- 关闭打开的文件
```

输出：

```
$ luajit src/file_read.lua
hahah
```

## 参考

1、文件操作 · OpenResty最佳实践  
https://moonbingbing.gitbooks.io/openresty-best-practices/content/lua/file.html  

# 元表

---

在Lua5.1语言中，元表 (metatable) 的表现行为类似于 C++ 语言中的操作符重载，类似PHP的魔术方法。Python里也有`元类(metaclass)`一说。  

通过元表，Lua有了更多的扩展特性。Lua的面向对象特性就是基于元表实现的。      

Lua 提供了两个十分重要的用来处理元表的方法，如下：

- `setmetatable(table, metatable)`：此方法用于为一个表设置元表。
- `getmetatable(table)`：此方法用于获取表的元表对象。


设置元表的方法很简单，如下：

``` lua
local mytable = {}
local mymetatable = {}
setmetatable(mytable, mymetatable)
```

上面的代码可以简写成如下的一行代码：

``` lua
local mytable = setmetatable({}, {})
```


例如我们可以重载 `__add` 元方法 (metamethod)，实现重载`+`操作符，来计算两个 Lua 数组的并集：

``` lua
-- 计算集合的并集实例
set1 = {10,40}
set2 = {10,20,30}

setmetatable(set1, {
	__add = function(self, another)
		local res = {}
		local set = {}
		
		for k,v in pairs(self) do set[v] = true end -- 防止集合元素重复
		for k,v in pairs(another) do set[v] = true end -- 防止集合元素重复
		
		for k,v in pairs(set) do table.insert(res, k) end
		
		return res
	end
})

local set3 = set1 + set2
for k,v in pairs(set3) do print(v) end 
```

输出：

```
30
20
10
40
```

类似的元方法还有：

- `__add` `+`操作
- `__sub` `-`操作 其行为类似于 `add` 操作
- `__mul` `*`操作 其行为类似于 `add` 操作
- `__div` `/`操作 其行为类似于 `add` 操作
- `__mod` `%`操作 其行为类似于 `add` 操作
- `__pow` `^`（幂）操作 其行为类似于 `add` 操作
- `__unm` 一元 `-` 操作
- `__concat` `..`（字符串连接） 操作
- `__len` `#`操作
- `__eq` `==`操作 函数 getcomphandler 定义了 Lua 怎样选择一个处理器来作比较操作，仅在两个对象类型相同且有对应操作相同的元方法时才起效
- `__lt` `<`操作
- `__le` `<=`操作

- `__index` 取下标操作用于访问 `table[key]`
- `__newindex` 赋值给指定下标 `table[key] = value`
- `__tostring` 转换成字符串
- `__call` 当 Lua 调用一个值时调用
- `__mode` 用于弱表(week table)
- `__metatable` 用于保护`metatable`不被访问

## __index 元方法

该方法实现了在表中查找键不存在时转而在元表中查找该键的功能。有两种写法：  

第一种是给 `__index` 元方法一个函数：

``` lua
local mytable = setmetatable({}, {
	__index = function(self, key)
		return "__index"
	end
})
print(mytable.key1) -- __index 
```

另一种方法是给 `__index` 元方法一个表：

``` lua
local _M = {
	add = function(x,y) return x+y end,
	mul = function(x,y) return x*y end,
	ver = "1.0",
}
local mytable = setmetatable({}, {
	__index = _M
})
print(mytable.ver) -- 1.0 
print(mytable.add(1,3)) -- 4 
```

Lua查找一个表元素时的规则，其实就是如下3个步骤:

1. 在表中查找，如果找到，返回该元素，找不到则继续
2. 判断该表是否有元表，如果没有元表，返回`nil`，有元表则继续。
3. 判断元表有没有`__index`方法，如果`__index`方法为`nil`，则返回`nil`；如果`__index`方法是一个表，则重复1、2、3；如果`__index`方法是一个函数，则返回该函数的返回值。  


通过`__index`这个方法，我们可以实现继承的特性。下节再详细讲述。

## __newindex 元方法

如果说`__index`具有PHP里`__get`的一些特性，那么`__newindex`则类似`__set`。  

以下实例使用了 rawset 函数来更新表：

``` lua
mytable = setmetatable({key1 = "value1"}, {
  __newindex = function(self, key, value)
        rawset(self, key, "\""..value.."\"")

  end
})

mytable.key1 = "new value"
mytable.key2 = 4

print(mytable.key1,mytable.key2)
```

以上实例执行输出结果为：

```
new value    "4"
```

## __tostring 元方法

如果设置了__tostring 元方法，当直接输出表时会自动调用该方法。示例：

``` lua
local mytable = setmetatable({ 10, 20, 30 }, {
  __tostring = function(mytable)
    sum = 0
    for k, v in pairs(mytable) do
        sum = sum + v
    end
    return sum
  end
})
print(mytable) -- 60
```

## __call 元方法

__call 元方法的功能类似于 C++ 中的仿函数，使得普通的表也可以被调用。

``` lua
local mytable = setmetatable({}, {
  __call = function(self, arg)
	local sum = 0
	for _,v in pairs(arg) do
		sum = sum + v
	end
    print(sum)
  end
})
mytable({10,20,30}) -- 60
```

示例里我们调用自定义的表，并给该表传了参数，最终算出了参数的和。

## __metatable 元方法

如果给表设置了 __metatable 元方法的值，getmetatable 将返回这个域的值，而调用 setmetatable将会被禁止，会直接报错。

``` lua
local mytable = setmetatable({}, {
  __metatable = "no access"
})
print(getmetatable(mytable)) -- no access
setmetatable(mytable, {}) -- 引发编译器报错
```

# 面向对象编程

---

在Lua 中，我们可以使用表和函数实现面向对象。将函数和相关的数据放置于同一个表中就形成了一个对象。  

示例：

``` lua
-- module/cache.lua

local _M = {}

_M.mt = {}

_M.set = function(self, key ,value)
	self.mt[key] = value
end

_M.get = function(self, key)
	return self.mt[key]
end

_M.getall = function(self)
	return self.mt
end

_M.new = function(self)
	return setmetatable({}, {__index = _M })
end

return _M
```

调用：

``` lua
local cache = require "module/cache"
local c = cache:new()
c:set("name", "lua")
c:set("year", 2018)
print(c:get("name"))
print(c:get("year"))

for k,v in pairs(c:getall()) do
	print(k,v)
end
```

输出：

```
lua
2018
name    lua
year    2018
```

`setmetatable` 将 `_M` 作为新建表的原型，所以在自己的表内找不到所调用方法和变量的时候，便会到 `__index` 所指定的 `_M` 类型中去寻找。

## 继承

`__index`元方法实现了在父类中查找存在的方法和变量的机制。借助这个，可以实现继承。  

还是利用上面的例子：

``` lua
 -- module/mycache
local cache = require "module/cache"

local _M = {}

_M.del = function(self, key)
	_M.mt[key] = nil
end

_M.new = function(self)
	return setmetatable(_M, {__index = cache }) --该方法需要覆写父类的，确保此处的setmetatable先执行
end

return _M
```

此处实现了删除操作。调用：

``` lua
local cache = require "module/mycache"
local c = cache:new()
c:set("name", "lua")
c:del("name")
print(c:get("name"))
```

输出：

```
nil
```



# Redis里使用Lua

---

版本：自2.6.0起可用。
时间复杂度：取决于执行的脚本。

使用Lua脚本的好处：

- 减少网络开销。可以将多个请求通过脚本的形式一次发送，减少网络时延。
- 原子操作。redis会将整个脚本作为一个整体执行，中间不会被其他命令插入。因此在编写脚本的过程中无需担心会出现竞态条件，无需使用事务。
- 复用。客户端发送的脚本会永久存在redis中，这样，其他客户端可以复用这一脚本而不需要使用代码完成相同的逻辑。

## 如何使用

### 基本使用

命令格式：

``` bash
EVAL script numkeys key [key ...] arg [arg ...]
```

说明：

- `script`是第一个参数，为Lua 5.1脚本。该脚本不需要定义Lua函数（也不应该）。
- 第二个参数`numkeys`指定后续参数有几个key。
- `key [key ...]`，是要操作的键，可以指定多个，在lua脚本中通过`KEYS[1]`, `KEYS[2]`获取
- `arg [arg ...]`，参数，在lua脚本中通过`ARGV[1]`, `ARGV[2]`获取。

简单实例：

``` bash
127.0.0.1:6379> eval "return ARGV[1]" 0 100 
"100"
127.0.0.1:6379> eval "return {ARGV[1],ARGV[2]}" 0 100 101
1) "100"
2) "101"
127.0.0.1:6379> eval "return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}" 2 key1 key2 first second
1) "key1"
2) "key2"
3) "first"
4) "second"

127.0.0.1:6379> eval "redis.call('SET', KEYS[1], ARGV[1]);redis.call('EXPIRE', KEYS[1], ARGV[2]); return 1;" 1 test 10 60
(integer) 1
127.0.0.1:6379> ttl test
(integer) 59
127.0.0.1:6379> get test
"10"
```

注：

- `{}`在lua里是指数据类型`table`，类似数组。
- `redis.call()`可以调用redis命令。

### 命令行里使用

如果直接使用`redis-cli`命令，格式会有点不一样：

``` bash
redis-cli --eval lua_file key1 key2 , arg1 arg2 arg3
```

注意的地方：

- eval 后面参数是lua脚本文件,`.lua`后缀
- 不用写`numkeys`，而是使用`,`隔开。注意`,`前后有空格。

示例：

incrbymul.lua

``` lua
local num = redis.call('GET', KEYS[1]);  

if not num then
	return 0;
else
	local res = num * ARGV[1]; 
	redis.call('SET',KEYS[1], res); 
	return res;
end
```

命令行运行：

``` bash
$ redis-cli --eval incrbymul.lua lua:incrbymul , 8
(integer) 0
$ redis-cli incr lua:incrbymul 
(integer) 1
$ redis-cli --eval incrbymul.lua lua:incrbymul , 8
(integer) 8
$ redis-cli --eval incrbymul.lua lua:incrbymul , 8
(integer) 64
$ redis-cli --eval incrbymul.lua lua:incrbymul , 2
(integer) 128
```

由于redis没有提供命令可以实现将一个数原子性的乘以N倍，这里我们就用Lua脚本实现了，运行过程中确保不会被其它客户端打断。

### phpredis里使用

接着上面的例子：  

incrbymul.php

``` php
<?php 

$lua = <<<EOF
local num = redis.call('GET', KEYS[1]);  

if not num then
	return 0;
else
	local res = num * ARGV[1]; 
	redis.call('SET',KEYS[1], res); 
	return res;
end

EOF;

$redis = new Redis();
$redis->connect('127.0.0.1', 6379);
$ret = $redis->eval($lua, array("lua:incrbymul", 2), 1);
echo $ret;
```

运行：

``` bash
$ redis-cli set lua:incrbymul 0
OK
$ redis-cli incr lua:incrbymul
(integer) 1
$ php incrbymul.php 
2
$ php incrbymul.php 
4
```

eval原型：

``` c
Redis::eval(string script, [array keys, long num_keys])
```

eval函数的第3个参数为KEYS个数，phpredis依据此值将KEYS和ARGV做区分。


## 参考

1、在redis中使用lua脚本让你的灵活性提高5个逼格 - 一线码农 - 博客园  
https://www.cnblogs.com/huangxincheng/p/6230129.html  
2、Redis执行Lua脚本示例 - yanghuahui - 博客园  
https://www.cnblogs.com/yanghuahui/p/3697996.html  
3、EVAL - Redis  
https://redis.io/commands/eval  
4、phpredis 执行LUA脚本的例子 - jingtan的专栏 - CSDN博客  
https://blog.csdn.net/jingtan/article/details/53392309  

# Ngx_lua

---

## 简介

ngx_lua 指的是 [`lua-nginx-module`](https://github.com/openresty/lua-nginx-module)模块：通过将 `LuaJIT` 的虚拟机嵌入到 Nginx 的 worker 中，这样既保持高性能，又能不失去lua开发的简单特性。  

`OpenResty` 就是一个基于 Nginx 与 Lua 的高性能 Web 平台，其内部集成了大量精良的 Lua 库、第三方模块以及大多数的依赖项。OpenResty 基于`Nginx`开发，可以简单认为是 `Nginx` + `lua-nginx-module`的组合版。

官网：https://openresty.org/cn/  
官方文档：https://github.com/openresty/lua-nginx-module  

## Hello World 

### OpenResty 安装

以 CentOS 为例：

``` bash
mkdir /opt && cd /opt

# download openresty
wget https://openresty.org/download/openresty-1.13.6.2.tar.gz

tar zxvf openresty-1.13.6.2.tar.gz
cd openresty-1.13.6.2

# configure
./configure --prefix=/usr/local/openresty -j4

make -j4 && make install
```

其中 源码包可以到 https://openresty.org/cn/download.html 该页面获取。
`-j4`表示使用4核。`configure`那一步还可以指定各种参数：

``` bash
./configure --prefix=/usr/local/openresty \
            --with-luajit \
            --without-http_redis2_module \
            --with-http_iconv_module \
            --with-http_postgres_module
```

使用 `./configure --help` 查看更多的选项。

其它系统环境上安装可以参考 https://openresty.org/cn/installation.html 。

其实安装 OpenResty 和安装 Nginx 是类似的，因为 OpenResty 是基于 Nginx 开发的。

如果已经安装了 Nginx，又想使用 OpenResty 的功能，可以参考  《Nginx编译安装Lua》：https://www.cnblogs.com/52fhy/p/10164553.html 一文安装`lua-nginx-module`模块即可。

### 第一个程序

修改 `/usr/local/openresty/nginx/conf/nginx.conf`:

``` conf
worker_processes  1;
error_log logs/error.log;
events {
    worker_connections 1024;
}
http {
    server {
        listen 8080;
        location /hello {
            default_type text/html;
            content_by_lua '
                ngx.say("<p>hello, world</p>")
            ';
        }
    }
}
```

把默认的`80`端口改为`8080`，新增`/hello`部分。

其中`content_by_lua`便是 OpenResty 提供的指令，在官方文档可以搜索到：

![](http://img2018.cnblogs.com/blog/663847/201903/663847-20190324174956312-276962073.png)


现在我们启动OpenResty：

``` bash
/usr/local/openresty/nginx/sbin/nginx
```

启动成功后，查看效果：

``` bash
curl http://127.0.0.1:8080/hello
<p>hello, world</p>
```

说明成功运行了。

## 参考

1、OpenResty® - 中文官方站  
https://openresty.org/cn/  
2、openresty/lua-nginx-module: Embed the Power of Lua into NGINX HTTP servers  
https://github.com/openresty/lua-nginx-module#version  
3、环境搭建 · OpenResty最佳实践  
https://moonbingbing.gitbooks.io/openresty-best-practices/content/openresty/install.html  

