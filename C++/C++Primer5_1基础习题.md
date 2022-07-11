# 第一章 开始

## 练习1.1

查阅你使用的编译器的文档，确定它所使用的文件名约定。编译并运行第2页的main程序。

解：

- ``g++ --std=c++11 ch1.cpp -o main``
- ``./main``

## 练习1.2

改写程序，让它返回-1。返回值-1通常被当做程序错误的标识。重新编译并运行你的程序，观察你的系统如何处理main返回的错误标识。

解：

- 在ubuntu下，使用g++，返回-1，``./main``没有发现任何异常。
- ``echo $?``，返回255。

## 练习1.3

编写程序，在标准输出上打印Hello, World。

解：

```cpp
#include <iostream>

int main()
{
	std::cout << "Hello, World" << std::endl;
	return 0;
}
```

## 练习1.4

我们的程序使用加法运算符`+`来将两个数相加。编写程序使用乘法运算符`*`，来打印两个数的积。

解：

```cpp
#include <iostream>

int main()
{
    std::cout << "Enter two numbers:" << std::endl;
    int v1 = 0, v2 = 0;
    std::cin >> v1 >> v2;
    std::cout << "The product of " << v1 << " and " << v2
              << " is " << v1 * v2 << std::endl;
}        
```

## 练习1.5

我们将所有的输出操作放在一条很长的语句中，重写程序，将每个运算对象的打印操作放在一条独立的语句中。

解：

```cpp
#include <iostream>

int main()
{
    std::cout << "Enter two numbers:" << std::endl;
        int v1 = 0, v2 = 0;
        std::cin >> v1 >> v2;
        std::cout << "The product of ";
        std::cout << v1;
        std::cout << " and ";
        std::cout << v2;
        std::cout << " is ";
        std::cout << v1 * v2;
        std::cout << std::endl;
}    
```

## 练习1.6

解释下面程序片段是否合法。

```cpp
std::cout << "The sum of " << v1;
          << " and " << v2;
          << " is " << v1 + v2 << std::endl;
```

如果程序是合法的，它的输出是什么？如果程序不合法，原因何在？应该如何修正？

解：

程序不合法，有多余的分号，修改如下：

```cpp
std::cout << "The sum of " << v1
          << " and " << v2
          << " is " << v1 + v2 << std::endl;
```

## 练习1.7

编译一个包含不正确的嵌套注释的程序，观察编译器返回的错误信息。

解：

```cpp
/* 正常注释 /* 嵌套注释 */ 正常注释*/
```

错误信息：

```
  /* 正常注释 /* 嵌套注释 */ 正常注释*/
                                     ^
ch1.cpp:97:37: error: stray ‘\255’ in program
ch1.cpp:97:37: error: stray ‘\243’ in program
ch1.cpp:97:37: error: stray ‘\345’ in program
ch1.cpp:97:37: error: stray ‘\270’ in program
ch1.cpp:97:37: error: stray ‘\270’ in program
ch1.cpp:97:37: error: stray ‘\346’ in program
ch1.cpp:97:37: error: stray ‘\263’ in program
ch1.cpp:97:37: error: stray ‘\250’ in program
ch1.cpp:97:37: error: stray ‘\351’ in program
ch1.cpp:97:37: error: stray ‘\207’ in program
ch1.cpp:97:37: error: stray ‘\212’ in program
ch1.cpp: In function ‘int main()’:
ch1.cpp:97:50: error: expected primary-expression before ‘/’ token
  /* 正常注释 /* 嵌套注释 */ 正常注释*/
                                                  ^
ch1.cpp:98:5: error: expected primary-expression before ‘return’
     return 0;
     ^
```

## 练习1.8

指出下列哪些输出语句是合法的（如果有的话）：

```cpp
std::cout << "/*";
std::cout << "*/";
std::cout << /* "*/" */;
std::cout << /* "*/" /* "/*" */;
```

预测编译这些语句会产生什么样的结果，实际编译这些语句来验证你的答案(编写一个小程序，每次将上述一条语句作为其主体)，改正每个编译错误。

解：

只有第三句编译出错，改成如下即可：

```cpp
std::cout << /* "*/" */";
```

第四句等价于输出 `" /* "`。

## 练习1.9

编写程序，使用`while`循环将50到100整数相加。

解：

```cpp
#include <iostream>int main(){    int sum = 0, val = 50;    while (val <= 100){        sum += val;        val += 1;    }    std::cout << "Sum of 50 to 100 inclusive is "              << sum << std::endl;}    
```

## 练习1.10

除了`++`运算符将运算对象的值增加1之外，还有一个递减运算符`--`实现将值减少1.编写程序与，使用递减运算符在循环中按递减顺序打印出10到0之间的整数。

解：

```cpp
#include <iostream>int main(){    int val = 10;    while (val >= 0){        std::cout << val << " ";        val -= 1;    }    std::cout << std::endl;}  
```

## 练习1.11

编写程序，提示用户输入两个整数，打印出这两个整数所指定的范围内的所有整数。

解：

```cpp
#include <iostream>int main(){    int start = 0, end = 0;    std::cout << "Please input two num: ";    std::cin >> start >> end;    if (start <= end) {        while (start <= end){            std::cout << start << " ";            ++start;        }        std::cout << std::endl;    }    else{        std::cout << "start should be smaller than end !!!";    }}  
```

## 练习1.12

下面的for循环完成了什么功能？sum的终值是多少？

```cpp
int sum = 0;for (int i = -100; i <= 100; ++i)	sum += i;
```

解：

从-100加到100，sum的终值是0。

## 练习1.13

使用for循环重做1.4.1节中的所有练习（练习1.9到1.11）。

解：

### 练习1.9

```cpp
#include <iostream>int main(){    int sum = 0;    for (int val = 50; val <= 100; ++val){        sum += val;    }    std::cout << "Sum of 50 to 100 inclusive is "              << sum << std::endl;}    
```

### 练习1.10

```cpp
#include <iostream>int main(){    for (int val = 10; val >=0; --val){        std::cout << val << " ";    }    std::cout << std::endl;}  
```

### 练习1.11

```cpp
#include <iostream>int main(){    int start = 0, end = 0;    std::cout << "Please input two num: ";    std::cin >> start >> end;    if (start <= end) {        for (; start <= end; ++start){            std::cout << start << " ";        }        std::cout << std::endl;    }    else{        std::cout << "start should be smaller than end !!!";    }}  
```

## 练习1.14

对比for循环和while循环，两种形式的优缺点各是什么？

解：

```
The main difference between the `for`'s and the `while`'s is a matter of pragmatics: we usually use `for` when there is a known number of iterations, and use `while` constructs when the number of iterations in not known in advance. The `while` vs `do ... while` issue is also of pragmatics, the second executes the instructions once at start, and afterwards it behaves just like the simple `while`.
```

## 练习1.15

编写程序，包含第14页“再探编译”中讨论的常见错误。熟悉编译器生成的错误信息。

解：

编译器可以检查出的错误有：

- 语法错误
- 类型错误 
- 声明错误

## 练习1.16

编写程序，从cin读取一组数，输出其和。

解：

```cpp
#include <iostream>int main(){    int sum = 0;    for (int value = 0; std::cin >> value; )        sum += value;    std::cout << sum << std::endl;    return 0;}
```

## 练习1.17

如果输入的所有值都是相等的，本节的程序会输出什么？如果没有重复值，输出又会是怎样的？

## 练习1.18

编译并运行本节的程序，给它输入全都相等的值。再次运行程序，输入没有重复的值。

解：

全部重复：

```
1 1 1 1 1 1 occurs 5 times 
```

没有重复：

```
1 2 3 4 51 occurs 1 times 2 occurs 1 times 3 occurs 1 times 4 occurs 1 times 5 occurs 1 times 
```

## 练习1.19

修改你为1.4.1节练习1.11（第11页）所编写的程序（打印一个范围内的数），使其能处理用户输入的第一个数比第二个数小的情况。

解：

```cpp
#include <iostream>int main(){    int start = 0, end = 0;    std::cout << "Please input two num: ";    std::cin >> start >> end;    if (start <= end) {        while (start <= end){            std::cout << start << " ";            ++start;        }        std::cout << std::endl;    }    else{        std::cout << "start should be smaller than end !!!";    }}  
```

## 练习1.20

在网站http://www.informit.com/title/032174113 上，第1章的代码目录包含了头文件 Sales_item.h。将它拷贝到你自己的工作目录中。用它编写一个程序，读取一组书籍销售记录，将每条记录打印到标准输出上。

解：

```cpp
#include <iostream>#include "Sales_item.h"int main(){	for (Sales_item item; std::cin >> item; std::cout << item << std::endl);	return 0;}
```

命令：

```
./main < data/add_item
```

输出：

```
0-201-78345-X 3 60 200-201-78345-X 2 50 25
```

## 练习1.21

编写程序，读取两个 ISBN 相同的 Sales_item 对象，输出他们的和。

解：

```cpp
#include <iostream>#include "Sales_item.h"int main(){    Sales_item item_1;    Sales_item item_2;    std::cin >> item_1;    std::cout << item_1 << std::endl;    std::cin >> item_2;    std::cout << item_2 << std::endl;    std::cout << "sum of sale items: " << item_1 + item_2 << std::endl;	return 0;}
```

命令：

```
./main < data/add_item
```

输出：

```
0-201-78345-X 3 60 200-201-78345-X 2 50 25sum of sale items: 0-201-78345-X 5 110 22
```

## 练习1.22

编写程序，读取多个具有相同 ISBN 的销售记录，输出所有记录的和。

解：

```cpp
#include <iostream>#include "Sales_item.h"int main(){    Sales_item sum_item;    std::cin >> sum_item;    std::cout << sum_item << std::endl;    for (Sales_item item; std::cin >> item; std::cout << item << std::endl){        sum_item += item;    }    std::cout << "sum of sale items: " << sum_item << std::endl;	return 0;}
```

命令：

```
./main < data/add_item
```

输出：

```
0-201-78345-X 3 60 200-201-78345-X 2 50 25sum of sale items: 0-201-78345-X 5 110 22
```

## 练习1.23

编写程序，读取多条销售记录，并统计每个 ISBN（每本书）有几条销售记录。

## 练习1.24

输入表示多个 ISBN 的多条销售记录来测试上一个程序，每个 ISBN 的记录应该聚在一起。

解：

```cpp
#include <iostream>#include "Sales_item.h"int main(){    Sales_item total;    if (std::cin >> total){        Sales_item trans;        while (std::cin >> trans){            if (total.isbn() == trans.isbn()) {                total += trans;            }            else {                std::cout << total << std::endl;                total = trans;            }        }        std::cout << total << std::endl;    }    else {        std::cerr << "No data?!" << std::endl;        return -1;    }    return 0;}
```

命令：

```
./main < data/book_sales
```

输出：

```
0-201-70353-X 4 99.96 24.990-201-82470-1 4 181.56 45.390-201-88954-4 16 198 12.3750-399-82477-1 5 226.95 45.390-201-78345-X 5 110 22
```

## 练习1.25

借助网站上的`Sales_item.h`头文件，编译并运行本节给出的书店程序。



# 第二章 变量和基本类型 

## 练习2.1

类型 int、long、long long 和 short 的区别是什么？无符号类型和带符号类型的区别是什么？float 和 double的区别是什么？

解：

C++ 规定 short 和 int 至少16位，long 至少32位，long long 至少64位。 带符号类型能够表示正数、负数和 0 ，而无符号类型只能够表示 0 和正整数。

## 练习2.2

计算按揭贷款时，对于利率、本金和付款分别应选择何种数据类型？说明你的理由。

解：

使用`double`。需要进行浮点计算。

## 练习2.3

读程序写结果。

```cpp
unsigned u = 10, u2 = 42;
std::cout << u2 - u << std::endl;
std::cout << u - u2 << std::endl;
int i = 10, i2 = 42;
std::cout << i2 - i << std::endl;
std::cout << i - i2 << std::endl;
std::cout << i - u << std::endl;
std::cout << u - i << std::endl;
```

解：

输出：

```
32
4294967264
32
-32
0
0
```

## 练习2.4

编写程序检查你的估计是否正确，如果不正确，请仔细研读本节直到弄明白问题所在。

## 练习2.5

指出下述字面值的数据类型并说明每一组内几种字面值的区别：

```
(a) 'a', L'a', "a", L"a"
(b) 10, 10u, 10L, 10uL, 012, 0xC
(c) 3.14, 3.14f, 3.14L
(d) 10, 10u, 10., 10e-2
```

解：

- (a): 字符字面值，宽字符字面值，字符串字面值，宽字符串字面值。
- (b): 十进制整型，十进制无符号整型，十进制长整型，八进制整型，十六进制整型。
- (c): double, float, long double
- (d): 十进制整型，十进制无符号整型，double, double

## 练习2.6

下面两组定义是否有区别，如果有，请叙述之：

```cpp
int month = 9, day = 7;
int month = 09, day = 07;
```

解：

第一行定义的是十进制的整型，第二行定义的是八进制的整型。但是month变量有误，八进制不能直接写9。

## 练习2.7

下述字面值表示何种含义？它们各自的数据类型是什么？

```cpp
(a) "Who goes with F\145rgus?\012"
(b) 3.14e1L
(c) 1024f
(d) 3.14L
```

解：

- (a) Who goes with Fergus?(换行)，string 类型
- (b) long double
- (c) 无效，因为后缀`f`只能用于浮点字面量，而1024是整型。
- (d) long double

## 练习2.8

请利用转义序列编写一段程序，要求先输出 2M，然后转到新一行。修改程序使其先输出 2，然后输出制表符，再输出 M，最后转到新一行。

解：

```cpp
#include <iostream>
int main()
{
   std::cout << 2 << "\115\012";
   std::cout << 2 << "\t\115\012";
   return 0;
}
```

## 练习2.9

解释下列定义的含义，对于非法的定义，请说明错在何处并将其改正。

- (a) std::cin >> int input_value;
- (b) int i = { 3.14 };
- (c) double salary = wage = 9999.99;
- (d) int i = 3.14;

解： 

(a): 应该先定义再使用。

```cpp
int input_value = 0;
std::cin >> input_value;
```

(b): 用列表初始化内置类型的变量时，如果存在丢失信息的风险，则编译器将报错。

```cpp
double i = { 3.14 };
```

(c): 在这里`wage`是未定义的，应该在此之前将其定义。

```cpp
double wage;double salary = wage = 9999.99;
```

(d): 不报错，但是小数部分会被截断。

```cpp
double i = 3.14;
```

## 练习2.10

下列变量的初值分别是什么？

```cpp
std::string global_str;int global_int;int main(){    int local_int;    std::string local_str;}
```

解：

`global_str`和`global_int`是全局变量，所以初值分别为空字符串和0。
`local_int`是局部变量并且没有初始化，它的初值是未定义的。 
`local_str` 是 `string` 类的对象，它的值由类确定，为空字符串。

## 练习2.11

指出下面的语句是声明还是定义：

- (a) extern int ix = 1024;
- (b) int iy;
- (c) extern int iz;

解：

(a): 定义
(b): 定义
(c): 声明

## 练习2.12

请指出下面的名字中哪些是非法的？

- (a) int double = 3.14;
- (b) int _;
- (c) int catch-22;
- (d) int 1_or_2 = 1;
- (e) double Double = 3.14;

解：

(a), (c), (d) 非法。

## 练习2.13

下面程序中`j`的值是多少？

```cpp
int i = 42;int main(){    int i = 100;    int j = i;}
```

解：

`j`的值是100，局部变量`i`覆盖了全局变量`i`。

## 练习2.14

下面的程序合法吗？如果合法，它将输出什么？

```cpp
int i = 100, sum = 0;for (int i = 0; i != 10; ++i)    sum += i;std::cout << i << " " << sum << std::endl;
```

解：

合法。输出是 100 45 。


## 练习2.15

下面的哪个定义是不合法的？为什么？

- (a) int ival = 1.01;
- (b) int &rval1 = 1.01;
- (c) int &rval2 = ival;
- (d) int &rval3;

解：

(b)和(d)不合法，(b)引用必须绑定在对象上，(d)引用必须初始化。

## 练习2.16

考察下面的所有赋值然后回答：哪些赋值是不合法的？为什么？哪些赋值是合法的？它们执行了哪些操作？

```cpp
int i = 0, &r1 = i; double d = 0, &r2 = d;
```

- (a) r2 = 3.14159;
- (b) r2 = r1;
- (c) i = r2;
- (d) r1 = d;

解：

- (a): 合法。给 d 赋值为 3.14159。
- (b): 合法。会执行自动转换（int->double）。
- (c): 合法。会发生小数截取。
- (d): 合法。会发生小数截取。

## 练习2.17

执行下面的代码段将输出什么结果？

```cpp
int i, &ri = i;i = 5; ri = 10;std::cout << i << " " << ri << std::endl;
```

解：

输出：10, 10

## 练习2.18

编写代码分别改变指针的值以及指针所指对象的值。

解：

```cpp
int a = 0, b = 1;int *p1 = &a, *p2 = p1;// change the value of a pointer.p1 = &b;// change the value to which the pointer points*p2 = b;
```

## 练习2.19

说明指针和引用的主要区别

解：

引用是另一个对象的别名，而指针本身就是一个对象。
引用必须初始化，并且一旦定义了引用就无法再绑定到其他对象。而指针无须在定义时赋初值，也可以重新赋值让其指向其他对象。

## 练习2.20

请叙述下面这段代码的作用。

```cpp
int i = 42;int *p1 = &i; *p1 = *p1 * *p1;
```

解：

让指针 pi 指向 i，然后将 i 的值重新赋值为 42 * 42 (1764)。

## 练习2.21

请解释下述定义。在这些定义中有非法的吗？如果有，为什么？

`int i = 0;`

- (a) double* dp = &i;
- (b) int *ip = i;
- (c) int *p = &i;

解：

- (a): 非法。不能将一个指向 `double` 的指针指向 `int` 。
- (b): 非法。不能将 `int` 变量赋给指针。
- (c): 合法。

## 练习2.22

假设 p 是一个 int 型指针，请说明下述代码的含义。

```cpp
if (p) // ...if (*p) // ...
```

解：

第一句判断 p 是不是一个空指针, 
第二句判断 p 所指向的对象的值是不是为0


## 练习2.23

给定指针 p，你能知道它是否指向了一个合法的对象吗？如果能，叙述判断的思路；如果不能，也请说明原因。

解：

不能，因为首先要确定这个指针是不是合法的，才能判断它所指向的对象是不是合法的。

## 练习2.24

在下面这段代码中为什么 p 合法而 lp 非法？

```cpp
int i = 42;void *p = &i;long *lp = &i;
```

解：

`void *`是从C语言那里继承过来的，可以指向任何类型的对象。
而其他指针类型必须要与所指对象严格匹配。

## 练习2.25

说明下列变量的类型和值。

```cpp
(a) int* ip, i, &r = i;(b) int i, *ip = 0;(c) int* ip, ip2;
```

解：

- (a): ip 是一个指向 int 的指针, i 是一个 int, r 是 i 的引用。
- (b): i 是 int , ip 是一个空指针。
- (c): ip 是一个指向 int 的指针, ip2 是一个 int。

## 练习2.26

下面哪些语句是合法的？如果不合法，请说明为什么？

解：

```cpp
const int buf;      // 不合法, const 对象必须初始化int cnt = 0;        // 合法const int sz = cnt; // 合法++cnt; ++sz;        // 不合法, const 对象不能被改变
```

## 练习2.27

下面的哪些初始化是合法的？请说明原因。

解：

```cpp
int i = -1, &r = 0;         // 不合法, r 必须引用一个对象int *const p2 = &i2;        // 合法，常量指针const int i = -1, &r = 0;   // 合法const int *const p3 = &i2;  // 合法const int *p1 = &i2;        // 合法const int &const r2;        // 不合法, r2 是引用，引用没有顶层 constconst int i2 = i, &r = i;   // 合法
```

## 练习2.28

说明下面的这些定义是什么意思，挑出其中不合法的。

解：

```cpp
int i, *const cp;       // 不合法, const 指针必须初始化int *p1, *const p2;     // 不合法, const 指针必须初始化const int ic, &r = ic;  // 不合法, const int 必须初始化const int *const p3;    // 不合法, const 指针必须初始化const int *p;           // 合法. 一个指针，指向 const int
```

## 练习2.29

假设已有上一个练习中定义的那些变量，则下面的哪些语句是合法的？请说明原因。

解：

```cpp
i = ic;     // 合法, 常量赋值给普通变量p1 = p3;    // 不合法, p3 是const指针不能赋值给普通指针p1 = &ic;   // 不合法, 普通指针不能指向常量p3 = &ic;   // 合法, p3 是常量指针且指向常量p2 = p1;    // 合法, 可以将普通指针赋值给常量指针ic = *p3;   // 合法, 对 p3 取值后是一个 int 然后赋值给 ic
```

## 练习2.30

对于下面的这些语句，请说明对象被声明成了顶层const还是底层const？

```cpp
const int v2 = 0; int v1 = v2;int *p1 = &v1, &r1 = v1;const int *p2 = &v2, *const p3 = &i, &r2 = v2;
```

解：

v2 是顶层const，p2 是底层const，p3 既是顶层const又是底层const，r2 是底层const。

## 练习2.31

假设已有上一个练习中所做的那些声明，则下面的哪些语句是合法的？请说明顶层const和底层const在每个例子中有何体现。

解：

```cpp
r1 = v2; // 合法, 顶层const在拷贝时不受影响p1 = p2; // 不合法, p2 是底层const，如果要拷贝必须要求 p1 也是底层constp2 = p1; // 合法, int* 可以转换成const int*p1 = p3; // 不合法, p3 是一个底层const，p1 不是p2 = p3; // 合法, p2 和 p3 都是底层const，拷贝时忽略掉顶层const
```

## 练习2.32

下面的代码是否合法？如果非法，请设法将其修改正确。

```cpp
int null = 0, *p = null;
```

解：

合法。指针可以初始化为 0 表示为空指针。

## 练习2.33

利用本节定义的变量，判断下列语句的运行结果。

解：

```cpp
a=42; // a 是 intb=42; // b 是一个 int,(ci的顶层const在拷贝时被忽略掉了)c=42; // c 也是一个intd=42; // d 是一个 int *,所以语句非法e=42; // e 是一个 const int *, 所以语句非法g=42; // g 是一个 const int 的引用，引用都是底层const，所以不能被赋值
```

## 练习2.34

基于上一个练习中的变量和语句编写一段程序，输出赋值前后变量的内容，你刚才的推断正确吗？如果不对，请反复研读本节的示例直到你明白错在何处为止。

## 练习2.35

判断下列定义推断出的类型是什么，然后编写程序进行验证。

```cpp
const int i = 42;auto j = i; const auto &k = i; auto *p = &i; const auto j2 = i, &k2 = i;
```

解：

j 是 int，k 是 const int的引用，p 是const int *，j2 是const int，k2 是 const int 的引用。

## 练习2.36

关于下面的代码，请指出每一个变量的类型以及程序结束时它们各自的值。

```cpp
int a = 3, b = 4;decltype(a) c = a;decltype((b)) d = a;++c;++d;
```

解：

c 是 int 类型，值为 4。d 是 int & 类型，绑定到 a，a 的值为 4 。

## 练习2.37

赋值是会产生引用的一类典型表达式，引用的类型就是左值的类型。也就是说，如果 i 是 int，则表达式 i=x 的类型是 int&。根据这一特点，请指出下面的代码中每一个变量的类型和值。

```cpp
int a = 3, b = 4;decltype(a) c = a;decltype(a = b) d = a;
```

解：

c 是 int 类型，值为 3。d 是 int& 类型，绑定到 a。

## 练习2.38

说明由decltype 指定类型和由auto指定类型有何区别。请举一个例子，decltype指定的类型与auto指定的类型一样；再举一个例子，decltype指定的类型与auto指定的类型不一样。

解：

decltype 处理顶层const和引用的方式与 auto不同，decltype会将顶层const和引用保留起来。

```cpp
int i = 0, &r = i;//相同auto a = i;decltype(i) b = i;//不同 d 是一个 int&auto c = r;decltype(r) d = r;
```

## 练习2.39

编译下面的程序观察其运行结果，注意，如果忘记写类定义体后面的分号会发生什么情况？记录下相关的信息，以后可能会有用。

```cpp
struct Foo { /* 此处为空  */ } // 注意：没有分号int main(){    return 0;}。
```

解：

提示应输入分号。

## 练习2.40

根据自己的理解写出 Sales_data 类，最好与书中的例子有所区别。

```cpp
struct Sale_data{    std::string bookNo;    std::string bookName;    unsigned units_sold = 0;    double revenue = 0.0;    double price = 0.0;    //...}
```

## 练习2.41

使用你自己的Sale_data类重写1.5.1节（第20页）、1.5.2节（第21页）和1.6节（第22页）的练习。眼下先把Sales_data类的定义和main函数放在一个文件里。

```cpp
// 1.5.1#include <iostream>#include <string>struct Sale_data{    std::string bookNo;    unsigned units_sold = 0;    double revenue = 0.0;};int main(){    Sale_data book;    double price;    std::cin >> book.bookNo >> book.units_sold >> price;    book.revenue = book.units_sold * price;    std::cout << book.bookNo << " " << book.units_sold << " " << book.revenue << " " << price;    return 0;}
```

```cpp
// 1.5.2#include <iostream>#include <string>struct Sale_data{    std::string bookNo;    unsigned units_sold = 0;    double revenue = 0.0;};int main(){    Sale_data book1, book2;    double price1, price2;    std::cin >> book1.bookNo >> book1.units_sold >> price1;    std::cin >> book2.bookNo >> book2.units_sold >> price2;    book1.revenue = book1.units_sold * price1;    book2.revenue = book2.units_sold * price2;    if (book1.bookNo == book2.bookNo)    {        unsigned totalCnt = book1.units_sold + book2.units_sold;        double totalRevenue = book1.revenue + book2.revenue;        std::cout << book1.bookNo << " " << totalCnt << " " << totalRevenue << " ";        if (totalCnt != 0)            std::cout << totalRevenue / totalCnt << std::endl;        else            std::cout << "(no sales)" << std::endl;        return 0;    }    else    {        std::cerr << "Data must refer to same ISBN" << std::endl;        return -1;  // indicate failure    }}
```

```cpp
// 1.6#include <iostream>#include <string>struct Sale_data{    std::string bookNo;    unsigned units_sold = 0;    double revenue = 0.0;};int main(){    Sale_data total;    double totalPrice;    if (std::cin >> total.bookNo >> total.units_sold >> totalPrice)    {        total.revenue = total.units_sold * totalPrice;        Sale_data trans;        double transPrice;        while (std::cin >> trans.bookNo >> trans.units_sold >> transPrice)        {            trans.revenue = trans.units_sold * transPrice;            if (total.bookNo == trans.bookNo)            {                total.units_sold += trans.units_sold;                total.revenue += trans.revenue;            }            else            {                std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << " ";                if (total.units_sold != 0)                    std::cout << total.revenue / total.units_sold << std::endl;                else                    std::cout << "(no sales)" << std::endl;                total.bookNo = trans.bookNo;                total.units_sold = trans.units_sold;                total.revenue = trans.revenue;            }        }        std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << " ";        if (total.units_sold != 0)            std::cout << total.revenue / total.units_sold << std::endl;        else            std::cout << "(no sales)" << std::endl;        return 0;    }    else    {        std::cerr << "No data?!" << std::endl;        return -1;  // indicate failure    }}
```

## 练习2.42

根据你自己的理解重写一个Sales_data.h头文件，并以此为基础重做2.6.2节（第67页）的练习。



# 第三章 字符串、向量和数组

## 练习3.1

使用恰当的using 声明重做 1.4.1节和2.6.2节的练习。

解：

1.4.1

```cpp
#include <iostream>

using std::cin;
using std::cout;
using std::endl;

int main()
{
	int sum = 0;
	for (int val = 1; val <= 10; ++val) sum += val;

	cout << "Sum of 1 to 10 inclusive is " << sum << endl;
	
	return 0;
}
```

2.6.2 类似

## 练习3.2

编写一段程序从标准输入中一次读入一行，然后修改该程序使其一次读入一个词。

解：

一次读入一行：

```cpp
#include <iostream>
#include <string>

using std::string;
using std::cin;
using std::cout;
using std::endl;
using std::getline;

int main()
{
	string s;
	while (getline(cin,s))
	{
		cout << s << endl;
	}
	return 0;
}
```

一次读入一个词

```cpp
#include <iostream>
#include <string>

using std::string;
using std::cin;
using std::cout;
using std::endl;
using std::getline;

int main()
{
	string s;
	while (cin >> s)
	{
		cout << s << endl;
	}
	return 0;
}
```


## 练习3.3

请说明string类的输入运算符和getline函数分别是如何处理空白字符的。

解：

- 类似`is >> s`的读取：string对象会忽略开头的空白并从第一个真正的字符开始，直到遇见下一**空白**为止。
- 类似`getline(is, s)`的读取：string对象会从输入流中读取字符，直到遇见**换行符**为止。


## 练习3.4

编写一段程序读取两个字符串，比较其是否相等并输出结果。如果不相等，输出比较大的那个字符串。改写上述程序，比较输入的两个字符串是否等长，如果不等长，输出长度较大的那个字符串。

解：

比较大的

```cpp
#include <iostream>
#include <string>
using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
	string str1, str2;
	while (cin >> str1 >> str2)
	{
		if (str1 == str2)
			cout << "The two strings are equal." << endl;
		else
			cout << "The larger string is " << ((str1 > str2) ? str1 : str2);
	}

	return 0;
}
```

长度大的

```cpp
#include <iostream>
#include <string>
using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
	string str1, str2;
	while (cin >> str1 >> str2)
	{
		if (str1.size() == str2.size())
			cout << "The two strings have the same length." << endl;
		else
			cout << "The longer string is " << ((str1.size() > str2.size()) ? str1 : str2) << endl;
	}

	return 0;
}
```

## 练习3.5

编写一段程序从标准输入中读入多个字符串并将他们连接起来，输出连接成的大字符串。然后修改上述程序，用空格把输入的多个字符串分割开来。

解：

未隔开的：

```cpp
#include <iostream>
#include <string>

using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
	string result, s;
	while (cin >> s)
	{
		result += s;
	}
	cout << result << endl;

	return 0;
}
```

隔开的：

```cpp
#include <iostream>
#include <string>

using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
	string result, s;
	while (cin >> s)
	{
		result += s + " ";
	}
	cout << result << endl;

	return 0;
}
```

## 练习3.6

编写一段程序，使用范围for语句将字符串内所有字符用X代替。

解：

```cpp
#include <iostream>
#include <string>
#include <cctype>

using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
	string s = "this is a string";

	for (auto &x : s)
	{
		x = 'X';
	}

	cout << s << endl;
	return 0;
}
```

## 练习3.7

就上一题完成的程序而言，如果将循环控制的变量设置为char将发生什么？先估计一下结果，然后实际编程进行验证。

解：

如果设置为char，那么原来的字符串不会发生改变。

## 练习3.8

分别用while循环和传统for循环重写第一题的程序，你觉得哪种形式更好呢？为什么？

解：

```cpp
#include <iostream>#include <string>#include <cctype>using std::string;using std::cin;using std::cout;using std::endl;int main(){	string s = "this is a string";	decltype(s.size()) i = 0;	while (i != s.size())	{		s[i] = 'X';		++i;	}	cout << s << endl;	for (i = 0; i != s.size(); ++i)	{		s[i] = 'Y';	}	cout << s << endl;	return 0;}
```

范围for语句更好，不直接操作索引，更简洁。

## 练习3.9

下面的程序有何作用？它合法吗？如果不合法？为什么？

```cpp
string s;cout << s[0] << endl;
```

解：

不合法。使用下标访问空字符串是非法的行为。

## 练习3.10

编写一段程序，读入一个包含标点符号的字符串，将标点符号去除后输出字符串剩余的部分。

解：

```cpp
#include <iostream>#include <string>#include <cctype>using std::string;using std::cin;using std::cout;using std::endl;int main(){	string s = "this, is. a :string!";	string result;	for (auto x : s)	{		if (!ispunct(x))		{			result += x;		}	}		cout << result << endl;	return 0;}
```

## 练习3.11

下面的范围for语句合法吗？如果合法，c的类型是什么？

```cpp
const string s = "Keep out!";for(auto &c : s){ /* ... */ }
```

解：

要根据for循环中的代码来看是否合法，c是string 对象中字符的引用，s是常量。因此如果for循环中的代码重新给c赋值就会非法，如果不改变c的值，那么合法。

## 练习3.12

下列vector对象的定义有不正确的吗？如果有，请指出来。对于正确的，描述其执行结果；对于不正确的，说明其错误的原因。

```cpp
vector<vector<int>> ivec;         // 在C++11当中合法vector<string> svec = ivec;       // 不合法，类型不一样vector<string> svec(10, "null");  // 合法
```

## 练习3.13

下列的vector对象各包含多少个元素？这些元素的值分别是多少？

```cpp
vector<int> v1;         // size:0,  no values.vector<int> v2(10);     // size:10, value:0vector<int> v3(10, 42); // size:10, value:42vector<int> v4{ 10 };     // size:1,  value:10vector<int> v5{ 10, 42 }; // size:2,  value:10, 42vector<string> v6{ 10 };  // size:10, value:""vector<string> v7{ 10, "hi" };  // size:10, value:"hi"
```

## 练习3.14

编写一段程序，用cin读入一组整数并把它们存入一个vector对象。

解：

```cpp
#include <iostream>#include <string>#include <cctype>#include <vector>using std::cin;using std::cout;using std::endl;using std::vector;int main(){	vector<int> v;	int i;	while (cin >> i)	{		v.push_back(i);	}	return 0;}
```

## 练习3.15

改写上题程序，不过这次读入的是字符串。

解：

```cpp
#include <iostream>#include <string>#include <cctype>#include <vector>using std::cin;using std::cout;using std::endl;using std::vector;using std::string;int main(){	vector<string> v;	string i;	while (cin >> i)	{		v.push_back(i);	}	return 0;}
```

## 练习3.16

编写一段程序，把练习3.13中vector对象的容量和具体内容输出出来

解：

```cpp
#include <iostream>#include <string>#include <cctype>#include <vector>using std::cin;using std::cout;using std::endl;using std::vector;using std::string;int main(){	vector<int> v1;         // size:0,  no values.	vector<int> v2(10);     // size:10, value:0	vector<int> v3(10, 42); // size:10, value:42	vector<int> v4{ 10 };     // size:1,  value:10	vector<int> v5{ 10, 42 }; // size:2,  value:10, 42	vector<string> v6{ 10 };  // size:10, value:""	vector<string> v7{ 10, "hi" };  // size:10, value:"hi"	cout << "v1 size :" << v1.size() << endl;	cout << "v2 size :" << v2.size() << endl;	cout << "v3 size :" << v3.size() << endl;	cout << "v4 size :" << v4.size() << endl;	cout << "v5 size :" << v5.size() << endl;	cout << "v6 size :" << v6.size() << endl;	cout << "v7 size :" << v7.size() << endl;	cout << "v1 content: ";	for (auto i : v1)	{		cout << i << " , ";	}	cout << endl;	cout << "v2 content: ";	for (auto i : v2)	{		cout << i << " , ";	}	cout << endl;	cout << "v3 content: ";	for (auto i : v3)	{		cout << i << " , ";	}	cout << endl;	cout << "v4 content: ";	for (auto i : v4)	{		cout << i << " , ";	}	cout << endl;	cout << "v5 content: ";	for (auto i : v5)	{		cout << i << " , ";	}	cout << endl;	cout << "v6 content: ";	for (auto i : v6)	{		cout << i << " , ";	}	cout << endl;	cout << "v7 content: ";	for (auto i : v7)	{		cout << i << " , ";	}	cout << endl;	return 0;}
```

## 练习3.17

从cin读入一组词并把它们存入一个vector对象，然后设法把所有词都改为大写形式。输出改变后的结果，每个词占一行。

解：

```cpp
#include <iostream>#include <string>#include <cctype>#include <vector>using std::cin;using std::cout;using std::endl;using std::vector;using std::string;int main(){	vector<string> v;	string s;	while (cin >> s)	{		v.push_back(s);	}	for (auto &str : v)	{		for (auto &c : str)		{			c = toupper(c);		}	}	for (auto i : v)	{		cout << i << endl;	}	return 0;}
```

## 练习3.18

下面的程序合法吗？如果不合法，你准备如何修改？

```cpp
vector<int> ivec;ivec[0] = 42;
```

解：

不合法。应改为：

```cpp
ivec.push_back(42);
```

## 练习3.19

如果想定义一个含有10个元素的vector对象，所有元素的值都是42，请例举三种不同的实现方法，哪种方式更好呢？

如下三种：

```cpp
vector<int> ivec1(10, 42);vector<int> ivec2{ 42, 42, 42, 42, 42, 42, 42, 42, 42, 42 };vector<int> ivec3;for (int i = 0; i < 10; ++i)	ivec3.push_back(42);
```

第一种方式最好。

## 练习3.20

读入一组整数并把他们存入一个vector对象，将每对相邻整数的和输出出来。改写你的程序，这次要求先输出第一个和最后一个元素的和，接着输出第二个和倒数第二个元素的和，以此类推。

解：

```cpp
#include <iostream>#include <string>#include <cctype>#include <vector>using std::cin;using std::cout;using std::endl;using std::vector;using std::string;int main(){	vector<int> ivec;	int i;	while (cin >> i)	{		ivec.push_back(i);	}	for (int i = 0; i < ivec.size() - 1; ++i)	{		cout << ivec[i] + ivec[i + 1] << endl;	}		//---------------------------------	cout << "---------------------------------" << endl;		int m = 0;	int n = ivec.size() - 1;	while (m < n)	{		cout << ivec[m] + ivec[n] << endl;		++m;		--n;	}	return 0;}
```

## 练习3.21

请使用迭代器重做3.3.3节的第一个练习。

解：

```cpp
#include <vector>#include <iterator>#include <string>#include <iostream>using std::vector;using std::string;using std::cout;using std::endl;void check_and_print(const vector<int>& vec){	cout << "size: " << vec.size() << "  content: [";	for (auto it = vec.begin(); it != vec.end(); ++it)		cout << *it << (it != vec.end() - 1 ? "," : "");	cout << "]\n" << endl;}void check_and_print(const vector<string>& vec){	cout << "size: " << vec.size() << "  content: [";	for (auto it = vec.begin(); it != vec.end(); ++it)		cout << *it << (it != vec.end() - 1 ? "," : "");	cout << "]\n" << endl;}int main(){	vector<int> v1;	vector<int> v2(10);	vector<int> v3(10, 42);	vector<int> v4{ 10 };	vector<int> v5{ 10, 42 };	vector<string> v6{ 10 };	vector<string> v7{ 10, "hi" };	check_and_print(v1);	check_and_print(v2);	check_and_print(v3);	check_and_print(v4);	check_and_print(v5);	check_and_print(v6);	check_and_print(v7);	return 0;}
```

## 练习3.22

修改之前那个输出text第一段的程序，首先把text的第一段全部改成大写形式，然后输出它。

解： 略   

## 练习3.23

编写一段程序，创建一个含有10个整数的vector对象，然后使用迭代器将所有元素的值都变成原来的两倍。输出vector对象的内容，检验程序是否正确。

解：

```cpp
#include <iostream>#include <vector>using namespace std;int main(){	vector<int> v(10, 1);    for (auto it=v.begin(); it!=v.end(); it++){        *it *= 2;    }    for (auto one : v){        cout << one <<endl;    }	return 0;}
```

## 练习3.24

请使用迭代器重做3.3.3节的最后一个练习。

解：

```cpp
#include <iostream>#include <string>#include <cctype>#include <vector>using std::cin;using std::cout;using std::endl;using std::vector;using std::string;int main(){	vector<int> ivec;	int i;	while (cin >> i)	{		ivec.push_back(i);	}	for (auto it = ivec.begin(); it != ivec.end() - 1; ++it)	{		cout << *it + *(it + 1) << endl;	}	//---------------------------------	cout << "---------------------------------" << endl;	auto it1 = ivec.begin();	auto it2 = ivec.end() - 1;	while (it1 < it2)	{		cout << *it1 + *it2 << endl;		++it1;		--it2;	}	return 0;}
```

## 练习3.25

3.3.3节划分分数段的程序是使用下标运算符实现的，请利用迭代器改写该程序实现完全相同的功能。

解：

```cpp
#include <vector>#include <iostream>using std::vector; using std::cout; using std::cin; using std::endl;int main(){	vector<unsigned> scores(11, 0);	unsigned grade;	while (cin >> grade)	{		if (grade <= 100)			++*(scores.begin() + grade / 10);	}	for (auto s : scores)		cout << s << " ";	cout << endl;	return 0;}
```

## 练习3.26

在100页的二分搜索程序中，为什么用的是 `mid = beg + (end - beg) / 2`, 而非 `mid = (beg + end) / 2 ;` ?

解：

因为两个迭代器相互之间支持的运算只有 `-` ，而没有 `+` 。
但是迭代器和迭代器差值（整数值）之间支持 `+`。

## 练习3.27

假设`txt_size`是一个无参函数，它的返回值是`int`。请回答下列哪个定义是非法的，为什么？

```cpp
unsigned buf_size = 1024;(a) int ia[buf_size];(b) int ia[4 * 7 - 14];(c) int ia[txt_size()];(d) char st[11] = "fundamental";
```

解：

- (a) 非法。维度必须是一个常量表达式。
- (b) 合法。
- (c) 非法。txt_size() 的值必须要到运行时才能得到。
- (d) 非法。数组的大小应该是12。

## 练习3.28

下列数组中元素的值是什么？

```cpp
string sa[10];int ia[10];int main() {	string sa2[10];	int ia2[10];}
```

解：

数组的元素会被默认初始化。
`sa`的元素值全部为空字符串，`ia` 的元素值全部为0。
`sa2`的元素值全部为空字符串，`ia2`的元素值全部未定义。

## 练习3.29

相比于vector 来说，数组有哪些缺点，请例举一些。

解：

- 数组的大小是确定的。
- 不能随意增加元素。
- 不允许拷贝和赋值。

## 练习3.30

指出下面代码中的索引错误。

```cpp
constexpr size_t array_size = 10;int ia[array_size];for (size_t ix = 1; ix <= array_size; ++ix)	ia[ix] = ix;
```

解：

当`ix`增长到 10 的时候，`ia[ix]`的下标越界。

## 练习3.31

编写一段程序，定义一个含有10个int的数组，令每个元素的值就是其下标值。

```cpp
#include <iostream>using std::cout; using std::endl;int main(){    int arr[10];    for (auto i = 0; i < 10; ++i) arr[i] = i;    for (auto i : arr) cout << i << " ";    cout << endl;    return 0;}
```

## 练习3.32

将上一题刚刚创建的数组拷贝给另一数组。利用vector重写程序，实现类似的功能。

```cpp
#include <iostream>#include <vector>using std::cout; using std::endl; using std::vector;int main(){    // array    int arr[10];    for (int i = 0; i < 10; ++i) arr[i] = i;    int arr2[10];    for (int i = 0; i < 10; ++i) arr2[i] = arr[i];    // vector    vector<int> v(10);    for (int i = 0; i != 10; ++i) v[i] = arr[i];    vector<int> v2(v);    for (auto i : v2) cout << i << " ";    cout << endl;    return 0;}
```

## 练习3.33

对于104页的程序来说，如果不初始化scores将会发生什么？

解：

数组中所有元素的值将会未定义。

## 练习3.34

假定`p1` 和 `p2` 都指向同一个数组中的元素，则下面程序的功能是什么？什么情况下该程序是非法的？

```cpp
p1 += p2 - p1;
```

解：

将 `p1` 移动到 `p2` 的位置。任何情况下都合法。

## 练习3.35

编写一段程序，利用指针将数组中的元素置为0。

解：

```cpp
#include <iostream>using std::cout; using std::endl;int main(){    const int size = 10;    int arr[size];    for (auto ptr = arr; ptr != arr + size; ++ptr) *ptr = 0;    for (auto i : arr) cout << i << " ";    cout << endl;    return 0;}
```

## 练习3.36

编写一段程序，比较两个数组是否相等。再写一段程序，比较两个vector对象是否相等。

解：

```cpp
#include <iostream>#include <vector>#include <iterator>using std::begin; using std::end; using std::cout; using std::endl; using std::vector;// pb point to begin of the array, pe point to end of the array.bool compare(int* const pb1, int* const pe1, int* const pb2, int* const pe2){    if ((pe1 - pb1) != (pe2 - pb2)) // have different size.        return false;    else    {        for (int* i = pb1, *j = pb2; (i != pe1) && (j != pe2); ++i, ++j)            if (*i != *j) return false;    }    return true;}int main(){    int arr1[3] = { 0, 1, 2 };    int arr2[3] = { 0, 2, 4 };    if (compare(begin(arr1), end(arr1), begin(arr2), end(arr2)))        cout << "The two arrays are equal." << endl;    else        cout << "The two arrays are not equal." << endl;    cout << "==========" << endl;    vector<int> vec1 = { 0, 1, 2 };    vector<int> vec2 = { 0, 1, 2 };    if (vec1 == vec2)        cout << "The two vectors are equal." << endl;    else        cout << "The two vectors are not equal." << endl;    return 0;}
```

## 练习3.37

下面的程序是何含义，程序的输出结果是什么？

```cpp
const char ca[] = { 'h', 'e', 'l', 'l', 'o' };const char *cp = ca;while (*cp) {    cout << *cp << endl;    ++cp;}
```

解：

会将ca 字符数组中的元素打印出来。但是因为没有空字符的存在，程序不会退出循环。

## 练习3.38

在本节中我们提到，将两个指针相加不但是非法的，而且也没有什么意义。请问为什么两个指针相加没有意义？

解：

将两个指针相减可以表示两个指针(在同一数组中)相距的距离，将指针加上一个整数也可以表示移动这个指针到某一位置。但是两个指针相加并没有逻辑上的意义，因此两个指针不能相加。

## 练习3.39

编写一段程序，比较两个 `string` 对象。再编写一段程序，比较两个C风格字符串的内容。

解：

```cpp
#include <iostream>#include <string>#include <cstring>using std::cout; using std::endl; using std::string;int main(){    // use string.    string s1("Mooophy"), s2("Pezy");    if (s1 == s2)        cout << "same string." << endl;    else if (s1 > s2)        cout << "Mooophy > Pezy" << endl;    else        cout << "Mooophy < Pezy" << endl;    cout << "=========" << endl;    // use C-Style character strings.    const char* cs1 = "Wangyue";    const char* cs2 = "Pezy";    auto result = strcmp(cs1, cs2);    if (result == 0)        cout << "same string." << endl;    else if (result < 0)        cout << "Wangyue < Pezy" << endl;    else        cout << "Wangyue > Pezy" << endl;    return 0;}
```

## 练习3.40

编写一段程序，定义两个字符数组并用字符串字面值初始化它们；接着再定义一个字符数组存放前面两个数组连接后的结果。使用`strcpy`和`strcat`把前两个数组的内容拷贝到第三个数组当中。

解：

```cpp
#include <iostream>#include <cstring>const char cstr1[]="Hello";const char cstr2[]="world!";int main(){    constexpr size_t new_size = strlen(cstr1) + strlen(" ") + strlen(cstr2) +1;    char cstr3[new_size];        strcpy(cstr3, cstr1);    strcat(cstr3, " ");    strcat(cstr3, cstr2);        std::cout << cstr3 << std::endl;}
```

## 练习3.41

编写一段程序，用整型数组初始化一个vector对象。

```cpp
#include <iostream>#include <vector>using std::vector; using std::cout; using std::endl; using std::begin; using std::end;int main(){    int arr[] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };    vector<int> v(begin(arr), end(arr));    for (auto i : v) cout << i << " ";    cout << endl;    return 0;}
```

## 练习3.42

编写一段程序，将含有整数元素的 `vector` 对象拷贝给一个整型数组。

解：

```cpp
#include <iostream>#include <vector>using std::vector; using std::cout; using std::endl; using std::begin; using std::end;int main(){    vector<int> v{ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };    int arr[10];    for (int i = 0; i != v.size(); ++i) arr[i] = v[i];    for (auto i : arr) cout << i << " ";    cout << endl;    return 0;}
```

## 练习3.43

编写3个不同版本的程序，令其均能输出`ia`的元素。
版本1使用范围`for`语句管理迭代过程；版本2和版本3都使用普通`for`语句，其中版本2要求使用下标运算符，版本3要求使用指针。
此外，在所有3个版本的程序中都要直接写出数据类型，而不能使用类型别名、`auto`关键字和`decltype`关键字。

解：

```cpp
#include <iostream>using std::cout; using std::endl;int main(){    int arr[3][4] =     {         { 0, 1, 2, 3 },        { 4, 5, 6, 7 },        { 8, 9, 10, 11 }    };    // range for    for (const int(&row)[4] : arr)        for (int col : row) cout << col << " ";    cout << endl;    // for loop    for (size_t i = 0; i != 3; ++i)        for (size_t j = 0; j != 4; ++j) cout << arr[i][j] << " ";    cout << endl;    // using pointers.    for (int(*row)[4] = arr; row != arr + 3; ++row)        for (int *col = *row; col != *row + 4; ++col) cout << *col << " ";    cout << endl;    return 0;}
```

## 练习3.44

改写上一个练习中的程序，使用类型别名来代替循环控制变量的类型。

解：

```cpp
#include <iostream>using std::cout; using std::endl;int main(){    int ia[3][4] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 };    // a range for to manage the iteration    // use type alias    using int_array = int[4];    for (int_array& p : ia)        for (int q : p)            cout << q << " ";    cout << endl;    // ordinary for loop using subscripts    for (size_t i = 0; i != 3; ++i)        for (size_t j = 0; j != 4; ++j)            cout << ia[i][j] << " ";    cout << endl;    // using pointers.    // use type alias    for (int_array* p = ia; p != ia + 3; ++p)        for (int *q = *p; q != *p + 4; ++q)            cout << *q << " ";    cout << endl;    return 0;}
```

## 练习3.45

再一次改写程序，这次使用 `auto` 关键字。

解：

```cpp
#include <iostream>
using std::cout; using std::endl;

int main()
{
    int ia[3][4] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 };

    // a range for to manage the iteration
    for (auto& p : ia)
        for (int q : p)
            cout << q << " ";
    cout << endl;

    // ordinary for loop using subscripts
    for (size_t i = 0; i != 3; ++i)
        for (size_t j = 0; j != 4; ++j)
            cout << ia[i][j] << " ";
    cout << endl;

    // using pointers.
    for (auto p = ia; p != ia + 3; ++p)
        for (int *q = *p; q != *p + 4; ++q)
            cout << *q << " ";
    cout << endl;

    return 0;
}
```



# 第四章 表达式

## 练习4.1

表达式`5 + 10 * 20 / 2`的求值结果是多少？

解：

等价于`5 + ((10 * 20) / 2) = 105`

## 练习4.2

根据4.12节中的表，在下述表达式的合理位置添加括号，使得添加括号后运算对象的组合顺序与添加括号前一致。
(a) `*vec.begin()`
(b) `*vec.begin() + 1`

解：

```cpp
*(vec.begin())
(*(vec.begin())) + 1
```

## 练习4.3

C++语言没有明确规定大多数二元运算符的求值顺序，给编译器优化留下了余地。这种策略实际上是在代码生成效率和程序潜在缺陷之间进行了权衡，你认为这可以接受吗？请说出你的理由。

解：

可以接受。C++的设计思想是尽可能地“相信”程序员，将效率最大化。然而这种思想却有着潜在的危害，就是无法控制程序员自身引发的错误。因此 Java 的诞生也是必然，Java的思想就是尽可能地“不相信”程序员。

## 练习4.4

在下面的表达式中添加括号，说明其求值过程及最终结果。编写程序编译该（不加括号的）表达式并输出结果验证之前的推断。

`12 / 3 * 4 + 5 * 15 + 24 % 4 / 2`

解：

`((12 / 3) * 4) + (5 * 15) + ((24 % 4) / 2) = 16 + 75 + 0 = 91`

## 练习4.5

写出下列表达式的求值结果。

```cpp
-30 * 3 + 21 / 5  // -90+4 = -86
-30 + 3 * 21 / 5  // -30+63/5 = -30+12 = -18
30 / 3 * 21 % 5   // 10*21%5 = 210%5 = 0
-30 / 3 * 21 % 4  // -10*21%4 = -210%4 = -2
```

## 练习4.6

写出一条表达式用于确定一个整数是奇数还是偶数。

解：

```cpp
if (i % 2 == 0) /* ... */
```

或者

```cpp
if (i & 0x1) /* ... */
```

## 练习4.7

溢出是何含义？写出三条将导致溢出的表达式。

解：

当计算的结果超出该类型所能表示的范围时就会产生溢出。

```cpp
short svalue = 32767; ++svalue; // -32768
unsigned uivalue = 0; --uivalue;  // 4294967295
unsigned short usvalue = 65535; ++usvalue;  // 0
```

## 练习4.8

说明在逻辑与、逻辑或及相等性运算符中运算对象的求值顺序。

解：

- 逻辑与运算符和逻辑或运算符都是先求左侧运算对象的值再求右侧运算对象的值，当且仅当左侧运算对象无法确定表达式的结果时才会计算右侧运算对象的值。这种策略称为 **短路求值**。
- 相等性运算符未定义求值顺序。

## 练习4.9

解释在下面的`if`语句中条件部分的判断过程。

```cpp
const char *cp = "Hello World";
if (cp && *cp)
```

解：

首先判断`cp`，`cp` 不是一个空指针，因此`cp`为真。然后判断`*cp`，`*cp` 的值是字符`'H'`，非0。因此最后的结果为真。

## 练习4.10

为`while`循环写一个条件，使其从标准输入中读取整数，遇到`42`时停止。

解：

```cpp
int i;
while(cin >> i && i != 42)
```

## 练习4.11

书写一条表达式用于测试4个值a、b、c、d的关系，确保a大于b、b大于c、c大于d。

解：

```cpp
a>b && b>c && c>d
```

## 练习4.12

假设`i`、`j`和`k`是三个整数，说明表达式`i != j < k`的含义。

解：

这个表达式等于`i != (j < k)`。首先得到`j < k`的结果为`true`或`false`，转换为整数值是`1`或`0`，然后判断`i`不等于`1`或`0` ，最终的结果为`bool`值。

## 练习4.13

在下述语句中，当赋值完成后 i 和 d 的值分别是多少？

```cpp
int i;   double d;d = i = 3.5; // i = 3, d = 3.0i = d = 3.5; // d = 3.5, i = 3
```

## 练习4.14

执行下述 if 语句后将发生什么情况？

```cpp
if (42 = i)   // 编译错误。赋值运算符左侧必须是一个可修改的左值。而字面值是右值。if (i = 42)   // true.
```

## 练习4.15

下面的赋值是非法的，为什么？应该如何修改？

```cpp
double dval; int ival; int *pi;dval = ival = pi = 0;
```

解：
`p`是指针，不能赋值给`int`，应该改为：

```cpp
dval = ival = 0;pi = 0;
```

## 练习4.16

尽管下面的语句合法，但它们实际执行的行为可能和预期并不一样，为什么？应该如何修改？

```cpp
if (p = getPtr() != 0)if (i = 1024)
```

解：

```cpp
if ((p=getPtr()) != 0)if (i == 1024)
```

## 练习4.17

说明前置递增运算符和后置递增运算符的区别。

解：

前置递增运算符将对象本身作为左值返回，而后置递增运算符将对象原始值的副本作为右值返回。

## 练习4.18

如果132页那个输出`vector`对象元素的`while`循环使用前置递增运算符，将得到什么结果？

解：

将会从第二个元素开始取值，并且最后对`v.end()`进行取值，结果是未定义的。

## 练习4.19

假设`ptr`的类型是指向`int`的指针、`vec`的类型是`vector`、`ival`的类型是`int`，说明下面的表达式是何含义？如果有表达式不正确，为什么？应该如何修改？

```cpp
(a) ptr != 0 && *ptr++  (b) ival++ && ival(c) vec[ival++] <= vec[ival] 
```

解：

- (a) 判断`ptr`不是一个空指针，并且`ptr`当前指向的元素的值也为真，然后将`ptr`指向下一个元素
- (b) 判断`ival`的值为真，并且`(ival + 1)`的值也为真
- (c) 表达式有误。C++并没有规定`<=`运算符两边的求值顺序，应该改为`vec[ival] <= vec[ival+1]`

## 练习4.20

假设`iter`的类型是`vector::iterator`, 说明下面的表达式是否合法。如果合法，表达式的含义是什么？如果不合法，错在何处？

```cpp
(a) *iter++;(b) (*iter)++;(c) *iter.empty();(d) iter->empty();(e) ++*iter;(f) iter++->empty();
```

解：

- (a)合法。返回迭代器所指向的元素，然后迭代器递增。
- (b)不合法。因为`vector`元素类型是`string`，没有`++`操作。
- (c)不合法。这里应该加括号。
- (d)合法。判断迭代器当前的元素是否为空。
- (e)不合法。`string`类型没有`++`操作。
- (f)合法。判断迭代器当前元素是否为空，然后迭代器递增。

## 练习4.21

编写一段程序，使用条件运算符从`vector`中找到哪些元素的值是奇数，然后将这些奇数值翻倍。

解：

```cpp
#include <iostream>#include <vector>using std::cout;using std::endl;using std::vector;int main(){	vector<int> ivec{ 1, 2, 3, 4, 5, 6, 7, 8, 9 };	for (auto i : ivec)	{		cout << ((i & 0x1) ? i * 2 : i) << " ";	}	cout << endl;	return 0;}
```

## 练习4.22

本节的示例程序将成绩划分为`high pass`、`pass` 和 `fail` 三种，扩展该程序使其进一步将 60 分到 75 分之间的成绩设定为`low pass`。要求程序包含两个版本：一个版本只使用条件运算符；另一个版本使用1个或多个`if`语句。哪个版本的程序更容易理解呢？为什么？

解：

```cpp
#include <iostream>using std::cout; using std::cin; using std::endl;int main(){	for (unsigned g; cin >> g;)	{		auto result = g > 90 ? "high pass" : g < 60 ? "fail" : g < 75 ? "low pass" : "pass";		cout << result << endl;		// -------------------------		if (g > 90)         cout << "high pass";		else if (g < 60)    cout << "fail";		else if (g < 75)    cout << "low pass";		else                cout << "pass";		cout << endl;	}	return 0;}
```

第二个版本容易理解。当条件运算符嵌套层数变多之后，代码的可读性急剧下降。而`if else`的逻辑很清晰。

## 练习4.23

因为运算符的优先级问题，下面这条表达式无法通过编译。根据4.12节中的表指出它的问题在哪里？应该如何修改？

```cpp
string s = "word";string pl = s + s[s.size() - 1] == 's' ? "" : "s" ;
```

解：

加法运算符的优先级高于条件运算符。因此要改为：

```cpp
string pl = s + (s[s.size() - 1] == 's' ? "" : "s") ;
```

## 练习4.24

本节的示例程序将成绩划分为`high pass`、`pass`、和`fail`三种，它的依据是条件运算符满足右结合律。假如条件运算符满足的是左结合律，求值的过程将是怎样的？

解：

如果条件运算符满足的是左结合律。那么

`finalgrade = (grade > 90) ? "high pass" : (grade < 60) ? "fail" : "pass";`
等同于
`finalgrade = ((grade > 90) ? "high pass" : (grade < 60)) ? "fail" : "pass";`
假如此时 `grade > 90` ，第一个条件表达式的结果是 `"high pass"` ，而字符串字面值的类型是 `const char *`，非空所以为真。因此第二个条件表达式的结果是 `"fail"`。这样就出现了自相矛盾的逻辑。

## 练习4.25

如果一台机器上`int`占32位、`char`占8位，用的是`Latin-1`字符集，其中字符`'q'` 的二进制形式是`01110001`，那么表达式`~'q' << 6`的值是什么？

解：

首先将`char`类型提升为`int`类型，即`00000000 00000000 00000000 01110001`，然后取反，再左移6位，结果是-7296。

## 练习4.26

在本节关于测验成绩的例子中，如果使用`unsigned int` 作为`quiz1` 的类型会发生什么情况？

解：

在有的机器上，`unsigned int` 类型可能只有 16 位，因此结果是未定义的。

## 练习4.27

下列表达式的结果是什么？

```cpp
unsigned long ul1 = 3, ul2 = 7;(a) ul1 & ul2 (b) ul1 | ul2 (c) ul1 && ul2(d) ul1 || ul2 
```

解：

- (a) 3
- (b) 7
- (c) true
- (d) ture

## 练习4.28

编写一段程序，输出每一种内置类型所占空间的大小。

解：

```cpp
#include <iostream> using namespace std;int main(){	cout << "bool:\t\t" << sizeof(bool) << " bytes" << endl << endl;	cout << "char:\t\t" << sizeof(char) << " bytes" << endl;	cout << "wchar_t:\t" << sizeof(wchar_t) << " bytes" << endl;	cout << "char16_t:\t" << sizeof(char16_t) << " bytes" << endl;	cout << "char32_t:\t" << sizeof(char32_t) << " bytes" << endl << endl;	cout << "short:\t\t" << sizeof(short) << " bytes" << endl;	cout << "int:\t\t" << sizeof(int) << " bytes" << endl;	cout << "long:\t\t" << sizeof(long) << " bytes" << endl;	cout << "long long:\t" << sizeof(long long) << " bytes" << endl << endl;	cout << "float:\t\t" << sizeof(float) << " bytes" << endl;	cout << "double:\t\t" << sizeof(double) << " bytes" << endl;	cout << "long double:\t" << sizeof(long double) << " bytes" << endl << endl;	return 0;}
```

输出：

```
bool:           1 byteschar:           1 byteswchar_t:        4 byteschar16_t:       2 byteschar32_t:       4 bytesshort:          2 bytesint:            4 byteslong:           8 byteslong long:      8 bytesfloat:          4 bytesdouble:         8 byteslong double:    16 bytes
```

## 练习4.29

推断下面代码的输出结果并说明理由。实际运行这段程序，结果和你想象的一样吗？如不一样，为什么？

```cpp
int x[10];   int *p = x;cout << sizeof(x)/sizeof(*x) << endl;cout << sizeof(p)/sizeof(*p) << endl;
```

解：

第一个输出结果是 10。第二个结果1,此处用法不合理不是未定义，参考https://www.geeksforgeeks.org/using-sizof-operator-with-array-paratmeters/。

## 练习4.30

根据4.12节中的表，在下述表达式的适当位置加上括号，使得加上括号之后的表达式的含义与原来的含义相同。

```cpp
(a) sizeof x + y      (b) sizeof p->mem[i]  (c) sizeof a < b     (d) sizeof f() 
```

解：

```cpp
(a) (sizeof x) + y(b) sizeof(p->mem[i])(c) sizeof(a) < b(d) sizeof(f())
```

## 练习4.31

本节的程序使用了前置版本的递增运算符和递减运算符，解释为什么要用前置版本而不用后置版本。要想使用后置版本的递增递减运算符需要做哪些改动？使用后置版本重写本节的程序。

解：

在4.5节（132页）已经说过了，除非必须，否则不用递增递减运算符的后置版本。在这里要使用后者版本的递增递减运算符不需要任何改动。

## 练习4.32

解释下面这个循环的含义。

```cpp
constexpr int size = 5;int ia[size] = { 1, 2, 3, 4, 5 };for (int *ptr = ia, ix = 0;    ix != size && ptr != ia+size;    ++ix, ++ptr) { /* ... */ }
```

解：

这个循环在遍历数组`ia`，指针`ptr`和整型`ix`都是起到一个循环计数的功能。

## 练习4.33

根据4.12节中的表说明下面这条表达式的含义。

```cpp
someValue ? ++x, ++y : --x, --y
```

解：

逗号表达式的优先级是最低的。因此这条表达式也等于：

```cpp
(someValue ? ++x, ++y : --x), --y
```

如果`someValue`的值为真，`x` 和 `y` 的值都自增并返回 `y` 值，然后丢弃`y`值，`y`递减并返回`y`值。如果`someValue`的值为假，`x` 递减并返回`x` 值，然后丢弃`x`值，`y`递减并返回`y`值。



## 练习4.34

根据本节给出的变量定义，说明在下面的表达式中将发生什么样的类型转换：

```cpp
(a) if (fval)(b) dval = fval + ival;(c) dval + ival * cval;
```

需要注意每种运算符遵循的是左结合律还是右结合律。

解：

```cpp
(a) fval 转换为 bool 类型(b) ival 转换为 float ，相加的结果转换为 double(c) cval 转换为 int，然后相乘的结果转换为 double
```

## 练习4.35

假设有如下的定义：

```cpp
char cval;int ival;unsigned int ui;float fval;double dval;
```

请回答在下面的表达式中发生了隐式类型转换吗？如果有，指出来。

```cpp
(a) cval = 'a' + 3;(b) fval = ui - ival * 1.0;(c) dval = ui * fval;(d) cval = ival + fval + dval;
```

解：

- (a) `'a'` 转换为 `int` ，然后与 `3` 相加的结果转换为 `char`
- (b) `ival` 转换为 `double`，`ui` 转换为 `double`，结果转换为 `float`
- (c) `ui` 转换为 `float`，结果转换为 `double`
- (d) `ival` 转换为 `float`，与`fval`相加后的结果转换为 `double`，最后的结果转换为`char`

## 练习4.36

假设 `i` 是`int`类型，`d` 是`double`类型，书写表达式 `i*=d` 使其执行整数类型的乘法而非浮点类型的乘法。

解：

```cpp
i *= static_cast<int>(d);
```

## 练习4.37

练习4.37
用命名的强制类型转换改写下列旧式的转换语句。

```cpp
int i; double d; const string *ps; char *pc; void *pv;(a) pv = (void*)ps;(b) i = int(*pc);(c) pv = &d;(d) pc = (char*)pv;
```

解：

```cpp
(a) pv = static_cast<void*>(const_cast<string*>(ps));(b) i = static_cast<int>(*pc);(c) pv = static_cast<void*>(&d);(d) pc = static_cast<char*>(pv);
```

## 练习4.38

说明下面这条表达式的含义。

```cpp
double slope = static_cast<double>(j/i);
```

解：

将`j/i`的结果值转换为`double`，然后赋值给`slope`。



# 第五章 语句

## 练习5.1

什么是空语句？什么时候会用到空语句？

解：

只含义一个单独的分号的语句是空语句。如：`;`。

如果在程序的某个地方，语法上需要一条语句但是逻辑上不需要，此时应该使用空语句。

```cpp
while (cin >> s && s != sought)
	;
```

## 练习5.2

什么是块？什么时候会用到块？

解：

用花括号括起来的语句和声明的序列就是块。

```cpp
{
	// ...
}
```

如果在程序的某个地方，语法上需要一条语句，而逻辑上需要多条语句，此时应该使用块

```cpp
while (val <= 10) {
	sum += val;
	++val;
}
```

## 练习5.3

使用逗号运算符重写1.4.1节的`while`循环，使它不再需要块，观察改写之后的代码可读性提高了还是降低了。

```cpp
while (val <= 10)
    sum += val, ++val;
```

代码的可读性反而降低了。

## 练习5.4

说明下列例子的含义，如果存在问题，试着修改它。

```cpp
(a) while (string::iterator iter != s.end()) { /* . . . */ }

(b) while (bool status = find(word)) { /* . . . */ }
		if (!status) { /* . . . */ }
```

解：

- (a) 这个循环试图用迭代器遍历`string`，但是变量的定义应该放在循环的外面，目前每次循环都会重新定义一个变量，明显是错误的。
- (b) 这个循环的`while`和`if`是两个独立的语句，`if`语句中无法访问`status`变量，正确的做法是应该将`if`语句包含在`while`里面。

## 练习5.5

写一段自己的程序，使用`if else`语句实现把数字转换为字母成绩的要求。

```cpp
#include <iostream>
#include <vector>
#include <string>
using std::vector; using std::string; using std::cout; using std::endl; using std::cin;

int main()
{
    vector<string> scores = { "F", "D", "C", "B", "A", "A++" };
    for (int g; cin >> g;)
    {
        string letter;
        if (g < 60)
        {
            letter = scores[0];
        }
        else
        {
            letter = scores[(g - 50) / 10];
            if (g != 100)
                letter += g % 10 > 7 ? "+" : g % 10 < 3 ? "-" : "";
        }
        cout << letter << endl;
    }

    return 0;
}
```

## 练习5.6

改写上一题的程序，使用条件运算符代替`if else`语句。

```cpp
#include <iostream>
#include <vector>
#include <string>
using std::vector; using std::string; using std::cout; using std::endl; using std::cin;

int main()
{
    vector<string> scores = { "F", "D", "C", "B", "A", "A++" };

    int grade = 0;
    while (cin >> grade)
    {
        string lettergrade = grade < 60 ? scores[0] : scores[(grade - 50) / 10];
        lettergrade += (grade == 100 || grade < 60) ? "" : (grade % 10 > 7) ? "+" : (grade % 10 < 3) ? "-" : "";
        cout << lettergrade << endl;
    }

    return 0;
}
```

## 练习5.7

改写下列代码段中的错误。

```cpp
(a) if (ival1 != ival2) 
		ival1 = ival2
    else 
    	ival1 = ival2 = 0;
(b) if (ival < minval) 
		minval = ival;
    	occurs = 1;
(c) if (int ival = get_value())
    	cout << "ival = " << ival << endl;
    if (!ival)
    	cout << "ival = 0\n";
(d) if (ival = 0)
    	ival = get_value();
```

解：

- (a) `ival1 = ival2` 后面少了分号。
- (b) 应该用花括号括起来。
- (c) `if (!ival)` 应该改为 `else`。
- (d) `if (ival = 0)` 应该改为 `if (ival == 0)`。

## 练习5.8

什么是“悬垂else”？C++语言是如何处理else子句的？

解：

用来描述在嵌套的`if else`语句中，如果`if`比`else`多时如何处理的问题。C++使用的方法是`else`匹配最近没有配对的`if`。

## 练习5.9

编写一段程序，使用一系列`if`语句统计从`cin`读入的文本中有多少元音字母。

解：

```cpp
#include <iostream>using std::cout; using std::endl; using std::cin;int main(){	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0;	char ch;	while (cin >> ch)	{		if (ch == 'a') ++aCnt;		else if (ch == 'e') ++eCnt;		else if (ch == 'i') ++iCnt;		else if (ch == 'o') ++oCnt;		else if (ch == 'u') ++uCnt;	}	cout << "Number of vowel a: \t" << aCnt << '\n'		<< "Number of vowel e: \t" << eCnt << '\n'		<< "Number of vowel i: \t" << iCnt << '\n'		<< "Number of vowel o: \t" << oCnt << '\n'		<< "Number of vowel u: \t" << uCnt << endl;	return 0;}
```

## 练习5.10

我们之前实现的统计元音字母的程序存在一个问题：如果元音字母以大写形式出现，不会被统计在内。编写一段程序，既统计元音字母的小写形式，也统计元音字母的大写形式，也就是说，新程序遇到'a'和'A'都应该递增`aCnt`的值，以此类推。

解：

```cpp
#include <iostream>using std::cin; using std::cout; using std::endl;int main(){	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0;	char ch;	while (cin >> ch)		switch (ch)	{		case 'a':		case 'A':			++aCnt;			break;		case 'e':		case 'E':			++eCnt;			break;		case 'i':		case 'I':			++iCnt;			break;		case 'o':		case 'O':			++oCnt;			break;		case 'u':		case 'U':			++uCnt;			break;	}	cout << "Number of vowel a(A): \t" << aCnt << '\n'		<< "Number of vowel e(E): \t" << eCnt << '\n'		<< "Number of vowel i(I): \t" << iCnt << '\n'		<< "Number of vowel o(O): \t" << oCnt << '\n'		<< "Number of vowel u(U): \t" << uCnt << endl;	return 0;}
```

## 练习5.11

修改统计元音字母的程序，使其也能统计空格、制表符、和换行符的数量。

解：

```cpp
#include <iostream>using std::cin; using std::cout; using std::endl;int main(){	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0, spaceCnt = 0, tabCnt = 0, newLineCnt = 0;	char ch;	while (cin >> std::noskipws >> ch)  //noskipws(no skip whitespce)		switch (ch)	{		case 'a':		case 'A':			++aCnt;			break;		case 'e':		case 'E':			++eCnt;			break;		case 'i':		case 'I':			++iCnt;			break;		case 'o':		case 'O':			++oCnt;			break;		case 'u':		case 'U':			++uCnt;			break;		case ' ':			++spaceCnt;			break;		case '\t':			++tabCnt;			break;		case '\n':			++newLineCnt;			break;	}	cout << "Number of vowel a(A): \t" << aCnt << '\n'		<< "Number of vowel e(E): \t" << eCnt << '\n'		<< "Number of vowel i(I): \t" << iCnt << '\n'		<< "Number of vowel o(O): \t" << oCnt << '\n'		<< "Number of vowel u(U): \t" << uCnt << '\n'		<< "Number of space: \t" << spaceCnt << '\n'		<< "Number of tab char: \t" << tabCnt << '\n'		<< "Number of new line: \t" << newLineCnt << endl;	return 0;}
```

其中，使用 `std::noskipws`可以保留默认跳过的空格。

## 练习5.12

修改统计元音字母的程序，使其能统计含以下两个字符的字符序列的数量：`ff`、`fl`和`fi`。

解：

```cpp
#include <iostream>using std::cin; using std::cout; using std::endl;int main(){	unsigned aCnt = 0, eCnt = 0, iCnt = 0, oCnt = 0, uCnt = 0, spaceCnt = 0, tabCnt = 0, newLineCnt = 0, ffCnt = 0, flCnt = 0, fiCnt = 0;	char ch, prech = '\0';	while (cin >> std::noskipws >> ch)	{		switch (ch)		{		case 'a':		case 'A':			++aCnt;			break;		case 'e':		case 'E':			++eCnt;			break;		case 'i':			if (prech == 'f') ++fiCnt;		case 'I':			++iCnt;			break;		case 'o':		case 'O':			++oCnt;			break;		case 'u':		case 'U':			++uCnt;			break;		case ' ':			++spaceCnt;			break;		case '\t':			++tabCnt;			break;		case '\n':			++newLineCnt;			break;		case 'f':			if (prech == 'f') ++ffCnt;			break;		case 'l':			if (prech == 'f') ++flCnt;			break;		}		prech = ch;	}	cout << "Number of vowel a(A): \t" << aCnt << '\n'		<< "Number of vowel e(E): \t" << eCnt << '\n'		<< "Number of vowel i(I): \t" << iCnt << '\n'		<< "Number of vowel o(O): \t" << oCnt << '\n'		<< "Number of vowel u(U): \t" << uCnt << '\n'		<< "Number of space: \t" << spaceCnt << '\n'		<< "Number of tab char: \t" << tabCnt << '\n'		<< "Number of new line: \t" << newLineCnt << '\n'		<< "Number of ff: \t" << ffCnt << '\n'		<< "Number of fl: \t" << flCnt << '\n'		<< "Number of fi: \t" << fiCnt << endl;	return 0;}
```

## 练习5.13

下面显示的每个程序都含有一个常见的编码错误，指出错误在哪里，然后修改它们。

```cpp
(a) unsigned aCnt = 0, eCnt = 0, iouCnt = 0;    char ch = next_text();    switch (ch) {        case 'a': aCnt++;        case 'e': eCnt++;        default: iouCnt++;    }(b) unsigned index = some_value();    switch (index) {        case 1:            int ix = get_value();            ivec[ ix ] = index;            break;        default:            ix = ivec.size()-1;            ivec[ ix ] = index;    }(c) unsigned evenCnt = 0, oddCnt = 0;    int digit = get_num() % 10;    switch (digit) {        case 1, 3, 5, 7, 9:            oddcnt++;            break;        case 2, 4, 6, 8, 10:            evencnt++;            break;    }(d) unsigned ival=512, jval=1024, kval=4096;    unsigned bufsize;    unsigned swt = get_bufCnt();    switch(swt) {        case ival:            bufsize = ival * sizeof(int);            break;        case jval:            bufsize = jval * sizeof(int);            break;        case kval:            bufsize = kval * sizeof(int);            break;    }
```

解：

(a) 少了`break`语句。应该为：

```cpp
unsigned aCnt = 0, eCnt = 0, iouCnt = 0;    char ch = next_text();    switch (ch) {    	case 'a': aCnt++; break;    	case 'e': eCnt++; break;    	default: iouCnt++; break;    }
```

(b) 在`default`分支当中，`ix`未定义。应该在外部定义`ix`。

```cpp
unsigned index = some_value();    int ix;    switch (index) {        case 1:            ix = get_value();            ivec[ ix ] = index;            break;        default:            ix = static_cast<int>(ivec.size())-1;            ivec[ ix ] = index;    }
```

(c) `case`后面应该用冒号而不是逗号。

```cpp
unsigned evenCnt = 0, oddCnt = 0;    int digit = get_num() % 10;    switch (digit) {        case 1: case 3: case 5: case 7: case 9:            oddcnt++;            break;        case 2: case 4: case 6: case 8: case 0:            evencnt++;            break;    }
```

(d) `case`标签必须是整型常量表达式。

```cpp
const unsigned ival=512, jval=1024, kval=4096;    unsigned bufsize;    unsigned swt = get_bufCnt();    switch(swt) {        case ival:            bufsize = ival * sizeof(int);            break;        case jval:            bufsize = jval * sizeof(int);            break;        case kval:            bufsize = kval * sizeof(int);            break;    }
```

## 练习5.14

编写一段程序，从标准输入中读取若干`string`对象并查找连续重复出现的单词，所谓连续重复出现的意思是：一个单词后面紧跟着这个单词本身。要求记录连续重复出现的最大次数以及对应的单词。如果这样的单词存在，输出重复出现的最大次数；如果不存在，输出一条信息说明任何单词都没有连续出现过。
例如：如果输入是：

```
how now now now brown cow cow
```

那么输出应该表明单词now连续出现了3次。

解：

```cpp
#include <iostream>#include <string>using std::cout; using std::cin; using std::endl; using std::string; using std::pair;int main(){     pair<string, int> max_duplicated;    int count = 0;    for (string str, prestr; cin >> str; prestr = str)    {        if (str == prestr) ++count;        else count = 0;         if (count > max_duplicated.second) max_duplicated = { prestr, count };    }        if (max_duplicated.first.empty()) cout << "There's no duplicated string." << endl;    else cout << "the word " << max_duplicated.first << " occurred " << max_duplicated.second + 1 << " times. " << endl;        return 0;}
```

## 练习5.15

说明下列循环的含义并改正其中的错误。

```cpp
(a) for (int ix = 0; ix != sz; ++ix) { /* ... */ }    if (ix != sz)    	// . . .(b) int ix;    for (ix != sz; ++ix) { /* ... */ }(c) for (int ix = 0; ix != sz; ++ix, ++sz) { /*...*/ }
```

解：

应该改为下面这样：

```cpp
(a) int ix;    for (ix = 0; ix != sz; ++ix)  { /* ... */ }    if (ix != sz)    // . . .(b) int ix;    for (; ix != sz; ++ix) { /* ... */ }(c) for (int ix = 0; ix != sz; ++ix) { /*...*/ }
```

## 练习5.16

`while`循环特别适用于那种条件不变、反复执行操作的情况，例如，当未达到文件末尾时不断读取下一个值。
`for`循环更像是在按步骤迭代，它的索引值在某个范围内一次变化。根据每种循环的习惯各自编写一段程序，然后分别用另一种循环改写。
如果只能使用一种循环，你倾向于哪种？为什么？

解：

```cpp
int i;while ( cin >> i )    // ...for (int i = 0; cin >> i;)    // ...for (int i = 0; i != size; ++i)    // ...int i = 0;while (i != size){    // ...    ++i;}
```

如果只能用一种循环，我会更倾向使用`while`，因为`while`显得简洁，代码可读性强。

## 练习5.17

假设有两个包含整数的`vector`对象，编写一段程序，检验其中一个`vector`对象是否是另一个的前缀。
为了实现这一目标，对于两个不等长的`vector`对象，只需挑出长度较短的那个，把它的所有元素和另一个`vector`对象比较即可。
例如，如果两个`vector`对象的元素分别是0、1、1、2 和 0、1、1、2、3、5、8，则程序的返回结果为真。

解：

```cpp
#include <iostream>#include <vector>using std::cout; using std::vector;bool is_prefix(vector<int> const& lhs, vector<int> const& rhs){    if(lhs.size() > rhs.size())        return is_prefix(rhs, lhs);    for(unsigned i = 0; i != lhs.size(); ++i)        if(lhs[i] != rhs[i]) return false;    return true;}int main(){    vector<int> l{ 0, 1, 1, 2 };    vector<int> r{ 0, 1, 1, 2, 3, 5, 8 };    cout << (is_prefix(r, l) ? "yes\n" : "no\n");    return 0;}
```

## 练习5.18

说明下列循环的含义并改正其中的错误。

```cpp
(a) do { // 应该添加花括号        int v1, v2;        cout << "Please enter two numbers to sum:" ;        if (cin >> v1 >> v2)            cout << "Sum is: " << v1 + v2 << endl;    }while (cin);(b) int ival;    do {        // . . .    } while (ival = get_response()); // 应该将ival 定义在循环外(c) int ival = get_response();    do {        ival = get_response();    } while (ival); // 应该将ival 定义在循环外
```

## 练习5.19

编写一段程序，使用`do while`循环重复地执行下述任务：
首先提示用户输入两个`string`对象，然后挑出较短的那个并输出它。

解：

```cpp
#include <iostream>#include <string>using std::cout; using std::cin; using std::endl; using std::string;int main(){    string rsp;    do {        cout << "Input two strings: ";        string str1, str2;        cin >> str1 >> str2;        cout << (str1 <= str2 ? str1 : str2)              << " is less than the other. " << "\n\n"             << "More? Enter yes or no: ";        cin >> rsp;    } while (!rsp.empty() && tolower(rsp[0]) == 'y');    return 0;}
```

## 练习5.20

编写一段程序，从标准输入中读取`string`对象的序列直到连续出现两个相同的单词或者所有的单词都读完为止。
使用`while`循环一次读取一个单词，当一个单词连续出现两次时使用`break`语句终止循环。
输出连续重复出现的单词，或者输出一个消息说明没有任何单词是连续重复出现的。

解：

```cpp
#include <iostream>#include <string>using std::cout; using std::cin; using std::endl; using std::string;int main(){    string read, tmp;    while (cin >> read)        if (read == tmp) break; else tmp = read;    if (cin.eof())  cout << "no word was repeated." << endl; //eof(end of file)判断输入是否结束,或者文件结束符,等同于 CTRL+Z    else            cout << read << " occurs twice in succession." << endl;    return 0;}
```

## 练习5.21

修改5.5.1节练习题的程序，使其找到的重复单词必须以大写字母开头。

解：

```cpp
#include <iostream>using std::cin; using std::cout; using std::endl;#include <string>using std::string;int main(){    string curr, prev;    bool no_twice = true;    while (cin >> curr)     {        if (isupper(curr[0]) && prev == curr)        {            cout << curr << ": occurs twice in succession." << endl;            no_twice = false;            break;        }        prev = curr;    }    if (no_twice)        cout << "no word was repeated." << endl;    return 0;}
```

## 练习5.22

本节的最后一个例子跳回到`begin`，其实使用循环能更好的完成该任务，重写这段代码，注意不再使用`goto`语句。

```cpp
// 向后跳过一个带初始化的变量定义是合法的begin:    int sz = get_size();    if (sz <= 0) {        goto begin;    }
```

解：

用 for 循环修改的话就是这样

```cpp
for (int sz = get_size(); sz <=0; sz = get_size())    ;
```

## 练习5.23

编写一段程序，从标准输入读取两个整数，输出第一个数除以第二个数的结果。

解：

```cpp
#include <iostream>using std::cin;using std::cout;using std::endl;int main() {    int i, j;     cin >> i >> j;    cout << i / j << endl;     return 0;}
```

## 练习5.24

修改你的程序，使得当第二个数是0时抛出异常。先不要设定`catch`子句，运行程序并真的为除数输入0，看看会发生什么？

解：

```cpp
#include <iostream>#include <stdexcept>int main(void){    int i, j;    std::cin >> i >> j;    if (j == 0)        throw std::runtime_error("divisor is 0");    std::cout << i / j << std::endl;    return 0;}
```

## 练习5.25

修改上一题的程序，使用`try`语句块去捕获异常。`catch`子句应该为用户输出一条提示信息，询问其是否输入新数并重新执行`try`语句块的内容。

解：

```cpp
#include <iostream>
#include <stdexcept>
using std::cin; using std::cout; using std::endl; using std::runtime_error;

int main(void)
{
    for (int i, j; cout << "Input two integers:\n", cin >> i >> j; )
    {
        try 
        {
            if (j == 0) 
                throw runtime_error("divisor is 0");
            cout << i / j << endl;
        }
        catch (runtime_error err) 
        {
            cout << err.what() << "\nTry again? Enter y or n" << endl;
            char c;
            cin >> c;
            if (!cin || c == 'n')
                break;
        }
    }

    return 0;
}
```



# 第六章 函数

## 练习6.1

实参和形参的区别的什么？

解：

实参是函数调用的实际值，是形参的初始值。

## 练习6.2

请指出下列函数哪个有错误，为什么？应该如何修改这些错误呢？

```cpp
(a) int f() {
         string s;
         // ...
         return s;
   }
(b) f2(int i) { /* ... */ }
(c) int calc(int v1, int v1) { /* ... */ }
(d) double square (double x)  return x * x; 
```

解：

应该改为下面这样：

```cpp
(a) string f() {
         string s;
         // ...
         return s;
   }
(b) void f2(int i) { /* ... */ }
(c) int calc(int v1, int v2) { /* ... */ return ; }
(d) double square (double x) { return x * x; }
```

## 练习6.3

编写你自己的`fact`函数，上机检查是否正确。注：阶乘。

解：

```cpp
#include <iostream>

int fact(int i)
{
    if(i<0)
    {
        std::runtime_error err("Input cannot be a negative number");
        std::cout << err.what() << std::endl;
    }
    return i > 1 ? i * fact( i - 1 ) : 1;
}

int main()
{
    std::cout << std::boolalpha << (120 == fact(5)) << std::endl;
    return 0;
}
```

启用`std::boolalpha`，可以输出 `"true"`或者 `"false"`。

## 练习6.4

编写一个与用户交互的函数，要求用户输入一个数字，计算生成该数字的阶乘。在main函数中调用该函数。

```cpp
#include <iostream>
#include <string>

int fact(int i)
{
    return i > 1 ? i * fact(i - 1) : 1;
}

void interactive_fact()
{
    std::string const prompt = "Enter a number within [1, 13) :\n";
    std::string const out_of_range = "Out of range, please try again.\n";
    for (int i; std::cout << prompt, std::cin >> i; )
    {
        if (i < 1 || i > 12)
        {
            std::cout << out_of_range; 
            continue;
        }
        std::cout << fact(i) << std::endl;
    }
}

int main()
{
    interactive_fact();
    return 0;
}
```

## 练习6.5

编写一个函数输出其实参的绝对值。

```cpp
#include <iostream>

int abs(int i)
{
   return i > 0 ? i : -i;
}

int main()
{
   std::cout << abs(-5) << std::endl;
   return 0;
}
```

## 练习6.6

说明形参、局部变量以及局部静态变量的区别。编写一个函数，同时达到这三种形式。

解：

形参定义在函数形参列表里面；局部变量定义在代码块里面；
局部静态变量在程序的执行路径第一次经过对象定义语句时初始化，并且直到程序终止时才被销毁。

```cpp
// 例子
int count_add(int n)       // n是形参
{
    static int ctr = 0;    // ctr 是局部静态变量
    ctr += n;
    return ctr;
}

int main()
{
    for (int i = 0; i != 10; ++i)  // i 是局部变量
      cout << count_add(i) << endl;

    return 0;
}
```

## 练习6.7

编写一个函数，当它第一次被调用时返回0，以后每次被调用返回值加1。

解：

```cpp
int generate()
{
    static int ctr = 0;
    return ctr++;
}
```

## 练习6.8

编写一个名为Chapter6.h 的头文件，令其包含6.1节练习中的函数声明。

解：

```cpp
int fact(int val);
int func();

template <typename T> //参考：https://blog.csdn.net/fightingforcv/article/details/51472586

T abs(T i)
{
    return i >= 0 ? i : -i;
}
```

## 练习6.9 : fact.cc | factMain.cc

编写你自己的fact.cc 和factMain.cc ，这两个文件都应该包含上一小节的练习中编写的 Chapter6.h 头文件。通过这些文件，理解你的编译器是如何支持分离式编译的。

解：

fact.cc：

```cpp
#include "Chapter6.h"#include <iostream>int fact(int val){    if (val == 0 || val == 1) return 1;    else return val * fact(val-1);}int func(){    int n, ret = 1;    std::cout << "input a number: ";    std::cin >> n;    while (n > 1) ret *= n--;    return ret;}
```

factMain.cc：

```cpp
#include "Chapter6.h"#include <iostream>int main(){    std::cout << "5! is " << fact(5) << std::endl;     std::cout << func() << std::endl;     std::cout << abs(-9.78) << std::endl;}
```

编译： `g++ factMain.cpp fact.cpp -o main`

## 练习6.10

编写一个函数，使用指针形参交换两个整数的值。
在代码中调用该函数并输出交换后的结果，以此验证函数的正确性。

解：

```cpp
#include <iostream>#include <string>void swap(int* lhs, int* rhs){	int tmp;	tmp = *lhs;	*lhs = *rhs;	*rhs = tmp;}int main(){	for (int lft, rht; std::cout << "Please Enter:\n", std::cin >> lft >> rht;)	{		swap(&lft, &rht);		std::cout << lft << " " << rht << std::endl;	}	return 0;}
```

## 练习6.11

编写并验证你自己的reset函数，使其作用于引用类型的参数。注：reset即置0。

解：

```cpp
#include <iostream>void reset(int &i){    i = 0;}int main(){    int i = 42;    reset(i);    std::cout << i  << std::endl;    return 0;}
```

## 练习6.12

改写6.2.1节练习中的程序，使其引用而非指针交换两个整数的值。你觉得哪种方法更易于使用呢？为什么？

```cpp
#include <iostream>#include <string>void swap(int& lhs, int& rhs){    int temp = lhs;    lhs = rhs;    rhs = temp;}int main(){    for (int left, right; std::cout << "Please Enter:\n", std::cin >> left >> right; )    {        swap(left, right);        std::cout << left << " " << right << std::endl;    }    return 0;}
```

很明显引用更好用。

## 练习6.13

假设`T`是某种类型的名字，说明以下两个函数声明的区别：
一个是`void f(T)`, 另一个是`void f(&T)`。

解：

`void f(T)`的参数通过值传递，在函数中`T`是实参的副本，改变`T`不会影响到原来的实参。
`void f(&T)`的参数通过引用传递，在函数中的`T`是实参的引用，`T`的改变也就是实参的改变。

## 练习6.14

举一个形参应该是引用类型的例子，再举一个形参不能是引用类型的例子。

解：

例如交换两个整数的函数，形参应该是引用

```cpp
void swap(int& lhs, int& rhs){	int temp = lhs;	lhs = rhs;	rhs = temp;}
```

当实参的值是右值时，形参不能为引用类型

```cpp
int add(int a, int b){	return a + b;}int main(){	int i = add(1,2);	return 0;}
```

## 练习6.15

说明`find_char`函数中的三个形参为什么是现在的类型，特别说明为什么`s`是常量引用而`occurs`是普通引用？
为什么`s`和`occurs`是引用类型而`c`不是？
如果令`s`是普通引用会发生什么情况？
如果令`occurs`是常量引用会发生什么情况？

解：

- 因为字符串可能很长，因此使用引用避免拷贝；
- 而在函数中我们不希望改变`s`的内容，所以令`s`为常量。
- `occurs`是要传到函数外部的变量，所以使用引用，`occurs`的值会改变，所以是普通引用。
- 因为我们只需要`c`的值，这个实参可能是右值(右值实参无法用于引用形参)，所以`c`不能用引用类型。
- 如果`s`是普通引用，也可能会意外改变原来字符串的内容。
- `occurs`如果是常量引用，那么意味着不能改变它的值，那也就失去意义了。

## 练习6.16

下面的这个函数虽然合法，但是不算特别有用。指出它的局限性并设法改善。

```cpp
bool is_empty(string& s) { return s.empty(); }
```

解：

局限性在于常量字符串和字符串字面值无法作为该函数的实参，如果下面这样调用是非法的：

```cpp
const string str;bool flag = is_empty(str); //非法bool flag = is_empty("hello"); //非法
```

所以要将这个函数的形参定义为常量引用：

```cpp
bool is_empty(const string& s) { return s.empty(); }
```

## 练习6.17

编写一个函数，判断`string`对象中是否含有大写字母。
编写另一个函数，把`string`对象全部改写成小写形式。
在这两个函数中你使用的形参类型相同吗？为什么？

解：

两个函数的形参不一样。第一个函数使用常量引用，第二个函数使用普通引用。

## 练习6.18

为下面的函数编写函数声明，从给定的名字中推测函数具备的功能。

- (a) 名为`compare`的函数，返回布尔值，两个参数都是`matrix`类的引用。
- (b) 名为`change_val`的函数，返回`vector`的迭代器，有两个参数：一个是`int`，另一个是`vector`的迭代器。

解：

```cpp
(a) bool compare(matrix &m1, matrix &m2);(b) vector<int>::iterator change_val(int, vector<int>::iterator);
```

## 练习6.19

假定有如下声明，判断哪个调用合法、哪个调用不合法。对于不合法的函数调用，说明原因。

```cpp
double calc(double);int count(const string &, char);int sum(vector<int>::iterator, vector<int>::iterator, int);vector<int> vec(10);(a) calc(23.4, 55.1);(b) count("abcda",'a');(c) calc(66);(d) sum(vec.begin(), vec.end(), 3.8);
```

解：

- (a) 不合法。`calc`只有一个参数。
- (b) 合法。
- (c) 合法。
- (d) 合法。

## 练习6.20

引用形参什么时候应该是常量引用？如果形参应该是常量引用，而我们将其设为了普通引用，会发生什么情况？

解：

应该尽量将引用形参设为常量引用，除非有明确的目的是为了改变这个引用变量。
如果形参应该是常量引用，而我们将其设为了普通引用，那么常量实参将无法作用于普通引用形参。

## 练习6.21

编写一个函数，令其接受两个参数：一个是`int`型的数，另一个是`int`指针。
函数比较`int`的值和指针所指的值，返回较大的那个。
在该函数中指针的类型应该是什么？

解：

```cpp
#include <iostream>using std::cout;int larger_one(const int i, const int *const p){    return (i > *p) ? i : *p;}int main(){    int i = 6;    cout << larger_one(7, &i);    return 0;}
```

应该是`const int *`类型。

## 练习6.22

编写一个函数，令其交换两个`int`指针。

解：

```cpp
#include <iostream>#include <string>void swap(int*& lft, int*& rht){    auto tmp = lft;    lft = rht;    rht = tmp;}int main(){    int i = 42, j = 99;    auto lft = &i;    auto rht = &j;    swap(lft, rht);    std::cout << *lft << " " << *rht << std::endl;    return 0;}
```

## 练习6.23

参考本节介绍的几个`print`函数，根据理解编写你自己的版本。
依次调用每个函数使其输入下面定义的`i`和`j`:

```cpp
int i = 0, j[2] = { 0, 1 };
```

解：

```cpp
#include <iostream>using std::cout; using std::endl; using std::begin; using std::end;void print(const int *pi){    if(pi)        cout << *pi << endl;}void print(const char *p){    if (p)        while (*p) cout << *p++;    cout << endl;}void print(const int *beg, const int *end){    while (beg != end)        cout << *beg++ << endl;}void print(const int ia[], size_t size){    for (size_t i = 0; i != size; ++i) {        cout << ia[i] << endl;    }}void print(int (&arr)[2]){    for (auto i : arr)        cout << i << endl;}int main(){    int i = 0, j[2] = { 0, 1 };    char ch[5] = "pezy";        print(ch);    print(begin(j), end(j));    print(&i);    print(j, end(j)-begin(j));    print(j);        return 0;}
```

## 练习6.24

描述下面这个函数的行为。如果代码中存在问题，请指出并改正。

```cpp
void print(const int ia[10]){	for (size_t i = 0; i != 10; ++i)		cout << ia[i] << endl;}
```

解：

当数组作为实参的时候，会被自动转换为指向首元素的指针。
因此函数形参接受的是一个指针。
如果要让这个代码成功运行(不更改也可以运行），可以将形参改为数组的引用。

```cpp
void print(const int (&ia)[10]){	for (size_t i = 0; i != 10; ++i)		cout << ia[i] << endl;}
```

## 练习6.25

编写一个`main`函数，令其接受两个实参。把实参的内容连接成一个`string`对象并输出出来。

## 练习6.26

编写一个程序，使其接受本节所示的选项；输出传递给`main`函数的实参内容。

解：

包括6.25

```cpp
#include <iostream>#include <string>int main(int argc, char **argv){    std::string str;    for (int i = 1; i != argc; ++i)        str += std::string(argv[i]) + " ";    std::cout << str << std::endl;    return 0;}
```

## 练习6.27

编写一个函数，它的参数是`initializer_list`类型的对象，函数的功能是计算列表中所有元素的和。

解：

```cpp
#include <iostream>#include <initializer_list>int sum(std::initializer_list<int> const& il){    int sum = 0;    for (auto i : il) sum += i;    return sum;}int main(void){    auto il = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };    std::cout << sum(il) << std::endl;    return 0;}
```

## 练习6.28

在`error_msg`函数的第二个版本中包含`ErrCode`类型的参数，其中循环内的`elem`是什么类型？

解：

`elem`是`const string &`类型。

## 练习6.29

在范围`for`循环中使用`initializer_list`对象时，应该将循环控制变量声明成引用类型吗？为什么？

解：

应该使用常量引用类型。`initializer_list`对象中的元素都是常量，我们无法修改`initializer_list`对象中的元素的值。

## 练习6.30

编译第200页的`str_subrange`函数，看看你的编译器是如何处理函数中的错误的。

解：

编译器信息：

```
g++ (Ubuntu 5.4.0-6ubuntu1~16.04.10) 5.4.0 20160609
```

编译错误信息：

```
ch6.cpp:38:9: error: return-statement with no value, in function returning ‘bool’ [-fpermissive]
```

## 练习6.31

什么情况下返回的引用无效？什么情况下返回常量的引用无效？

解：

当返回的引用的对象是局部变量时，返回的引用无效；当我们希望返回的对象被修改时，返回常量的引用无效。

## 练习6.32

下面的函数合法吗？如果合法，说明其功能；如果不合法，修改其中的错误并解释原因。

```cpp
int &get(int *array, int index) { return array[index]; }int main(){    int ia[10];    for (int i = 0; i != 10; ++i)        get(ia, i) = i;}
```

解：

合法。`get`函数根据索引取得数组中的元素的引用。

## 练习6.33

编写一个递归函数，输出`vector`对象的内容。

解：

```cpp
#include <iostream>#include <vector>using std::vector; using std::cout;using Iter = vector<int>::const_iterator;void print(Iter first, Iter last){    if (first != last)    {        cout << *first << " ";        print(++first, last);    }}int main(){    vector<int> vec{ 1, 2, 3, 4, 5, 6, 7, 8, 9 };    print(vec.cbegin(), vec.cend());    return 0;}
```

## 练习6.34

如果`factorial`函数的停止条件如下所示，将发生什么？

```cpp
if (val != 0)
```

解：
如果`val`为正数，从结果上来说没有区别（多乘了个1）; 
如果`val`为负数，那么递归永远不会结束。

## 练习6.35

在调用`factorial`函数时，为什么我们传入的值是`val-1`而非`val--`？

解：

如果传入的值是`val--`，那么将会永远传入相同的值来调用该函数，递归将永远不会结束。

## 练习6.36

编写一个函数声明，使其返回数组的引用并且该数组包含10个`string`对象。
不用使用尾置返回类型、`decltype`或者类型别名。

解：

```cpp
string (&fun())[10];
```

## 练习6.37

为上一题的函数再写三个声明，一个使用类型别名，另一个使用尾置返回类型，最后一个使用`decltype`关键字。
你觉得哪种形式最好？为什么？

解：

```cpp
typedef string str_arr[10];str_arr& fun();auto fun()->string(&)[10];string s[10];decltype(s)& fun();
```

我觉得尾置返回类型最好，就一行代码。

## 练习6.38

修改`arrPtr`函数，使其返回数组的引用。

解：

```cpp
decltype(odd)& arrPtr(int i){    return (i % 2) ? odd : even;}
```

## 练习6.39

说明在下面的每组声明中第二条语句是何含义。
如果有非法的声明，请指出来。

```cpp
(a) int calc(int, int);	int calc(const int, const int);(b) int get();	double get();(c) int *reset(int *);	double *reset(double *);
```

解：

- (a) 非法。因为顶层const不影响传入函数的对象，所以第二个声明无法与第一个声明区分开来。
- (b) 非法。对于重载的函数来说，它们应该只有形参的数量和形参的类型不同。返回值与重载无关。
- (c) 合法。

## 练习6.40

下面的哪个声明是错误的？为什么？

```cpp
(a) int ff(int a, int b = 0, int c = 0);(b) char *init(int ht = 24, int wd, char bckgrnd);	
```

解：
	
(a) 正确。
(b) 错误。因为一旦某个形参被赋予了默认值，那么它之后的形参都必须要有默认值。

## 练习6.41

下面的哪个调用是非法的？为什么？哪个调用虽然合法但显然与程序员的初衷不符？为什么？

```cpp
char *init(int ht, int wd = 80, char bckgrnd = ' ');(a) init();(b) init(24,10);(c) init(14,'*');
```

解：

- (a) 非法。第一个参数不是默认参数，最少需要一个实参。
- (b) 合法。
- (c) 合法，但与初衷不符。字符`*`被解释成`int`传入到了第二个参数。而初衷是要传给第三个参数。

## 练习6.42

给`make_plural`函数的第二个形参赋予默认实参's', 利用新版本的函数输出单词success和failure的单数和复数形式。

解：

```cpp
#include <iostream>#include <string>using std::string;using std::cout;using std::endl;string make_plural(size_t ctr, const string& word, const string& ending = "s"){	return (ctr > 1) ? word + ending : word;}int main(){	cout << "single: " << make_plural(1, "success", "es") << " "		<< make_plural(1, "failure") << endl;	cout << "plural : " << make_plural(2, "success", "es") << " "		<< make_plural(2, "failure") << endl;	return 0;}
```

## 练习6.43

你会把下面的哪个声明和定义放在头文件中？哪个放在源文件中？为什么？

```cpp
(a) inline bool eq(const BigInt&, const BigInt&) {...}(b) void putValues(int *arr, int size);
```

解：

全部都放进头文件。(a) 是内联函数，(b) 是声明。

## 练习6.44

将6.2.2节的`isShorter`函数改写成内联函数。

解：

```cpp
inline bool is_shorter(const string &lft, const string &rht) {    return lft.size() < rht.size();}
```

## 练习6.45

回顾在前面的练习中你编写的那些函数，它们应该是内联函数吗？
如果是，将它们改写成内联函数；如果不是，说明原因。

解：

一般来说，内联机制用于优化规模小、流程直接、频繁调用的函数。

## 练习6.46

能把`isShorter`函数定义成`constexpr`函数吗？
如果能，将它改写成`constxpre`函数；如果不能，说明原因。

解：

不能。`constexpr`函数的返回值类型及所有形参都得是字面值类型。

## 练习6.47

改写6.3.2节练习中使用递归输出`vector`内容的程序，使其有条件地输出与执行过程有关的信息。
例如，每次调用时输出`vector`对象的大小。
分别在打开和关闭调试器的情况下编译并执行这个程序。

解：

```cpp
#include <iostream>#include <vector>using std::vector; using std::cout; using std::endl;void printVec(vector<int> &vec){#ifndef NDEBUG    cout << "vector size: " << vec.size() << endl;#endif    if (!vec.empty())    {        auto tmp = vec.back();        vec.pop_back();        printVec(vec);        cout << tmp << " ";    }}int main(){    vector<int> vec{ 1, 2, 3, 4, 5, 6, 7, 8, 9 };    printVec(vec);    cout << endl;    return 0;}
```

## 练习6.48

说明下面这个循环的含义，它对assert的使用合理吗？

```cpp
string s;while (cin >> s && s != sought) { } //空函数体assert(cin);
```

解：

不合理。从这个程序的意图来看，应该用

```cpp
assert(s == sought);
```

## 练习6.49

什么是候选函数？什么是可行函数？

解：

候选函数：与被调用函数同名，并且其声明在调用点可见。
可行函数：形参与实参的数量相等，并且每个实参类型与对应的形参类型相同或者能转换成形参的类型。

## 练习6.50

已知有第217页对函数`f`的声明，对于下面的每一个调用列出可行函数。
其中哪个函数是最佳匹配？
如果调用不合法，是因为没有可匹配的函数还是因为调用具有二义性？

```cpp
(a) f(2.56, 42)(b) f(42)(c) f(42, 0)(d) f(2.56, 3.14)
```

解：

- (a) `void f(int, int);`和`void f(double, double = 3.14);`是可行函数。
  该调用具有二义性而不合法。
- (b) `void f(int);` 是可行函数。调用合法。
- (c) `void f(int, int);`和`void f(double, double = 3.14);`是可行函数。
  `void f(int, int);`是最佳匹配。
- (d) `void f(int, int);`和`void f(double, double = 3.14);`是可行函数。
  `void f(double, double = 3.14);`是最佳匹配。

## 练习6.51

编写函数`f`的4版本，令其各输出一条可以区分的消息。
验证上一个练习的答案，如果你的回答错了，反复研究本节内容直到你弄清自己错在何处。

解：

```cpp
#include <iostream>using std::cout; using std::endl;void f(){    cout << "f()" << endl;}void f(int){    cout << "f(int)" << endl;}void f(int, int){    cout << "f(int, int)" << endl;}void f(double, double){    cout << "f(double, double)" << endl;}int main(){    //f(2.56, 42); // error: 'f' is ambiguous.    f(42);    f(42, 0);    f(2.56, 3.14);        return 0;}
```

## 练习6.52

已知有如下声明：

```cpp
void manip(int ,int);double dobj;
```

请指出下列调用中每个类型转换的等级。

```cpp
(a) manip('a', 'z');(b) manip(55.4, dobj);
```

解：

- (a) 第3级。类型提升实现的匹配。
- (b) 第4级。算术类型转换实现的匹配。

## 练习6.53

说明下列每组声明中的第二条语句会产生什么影响，并指出哪些不合法（如果有的话）。


```cpp
(a) int calc(int&, int&); 	int calc(const int&, const int&); (b) int calc(char*, char*);	int calc(const char*, const char*);(c) int calc(char*, char*);	int calc(char* const, char* const);
```

解：

(c) 不合法。顶层const不影响传入函数的对象。

## 练习6.54

编写函数的声明，令其接受两个`int`形参并返回类型也是`int`；然后声明一个`vector`对象，令其元素是指向该函数的指针。

解：

```cpp
int func(int, int);vector<decltype(func)*> v;
```

## 练习6.55

编写4个函数，分别对两个`int`值执行加、减、乘、除运算；在上一题创建的`vector`对象中保存指向这些函数的指针。

解：

```cpp
int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }
int multiply(int a, int b) { return a * b; }
int divide(int a, int b) { return b != 0 ? a / b : 0; }

v.push_back(add);
v.push_back(subtract);
v.push_back(multiply);
v.push_back(divide);
```

## 练习6.56

调用上述`vector`对象中的每个元素并输出结果。

解：

```cpp
std::vector<decltype(func) *> vec{ add, subtract, multiply, divide };
for (auto f : vec)
          std::cout << f(2, 2) << std::endl;
```



# 第七章 类

## 练习7.1

使用2.6.1节定义的`Sales_data`类为1.6节的交易处理程序编写一个新版本。

解：

```cpp
#include <iostream>
#include <string>
using std::cin; using std::cout; using std::endl; using std::string;

struct Sales_data
{
    string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

int main()
{
    Sales_data total;
    if (cin >> total.bookNo >> total.units_sold >> total.revenue)
    {
        Sales_data trans;
        while (cin >> trans.bookNo >> trans.units_sold >> trans.revenue) 
        {
            if (total.bookNo == trans.bookNo) 
            {
                total.units_sold += trans.units_sold;
                total.revenue += trans.revenue;
            }
            else
            {
                cout << total.bookNo << " " << total.units_sold << " " << total.revenue << endl;
                total = trans;
            }
        }
        cout << total.bookNo << " " << total.units_sold << " " << total.revenue << endl;
    }
    else
    {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```

## 练习7.2

曾在2.6.2节的练习中编写了一个`Sales_data`类，请向这个类添加`combine`函数和`isbn`成员。

解：

```cpp
#include <string>

struct Sales_data {
    std::string isbn() const { return bookNo; };
    Sales_data& combine(const Sales_data&);
    
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

Sales_data& Sales_data::combine(const Sales_data& rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}
```

## 练习7.3

修改7.1.1节的交易处理程序，令其使用这些成员。

解：

```cpp
#include <iostream>
using std::cin; using std::cout; using std::endl;

int main()
{
    Sales_data total;
    if (cin >> total.bookNo >> total.units_sold >> total.revenue)
    {
        Sales_data trans;
        while (cin >> trans.bookNo >> trans.units_sold >> trans.revenue) {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else {
                cout << total.bookNo << " " << total.units_sold << " " << total.revenue << endl;
                total = trans;
            }
        }
        cout << total.bookNo << " " << total.units_sold << " " << total.revenue << endl;
    }
    else
    {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    return 0;
}
```

## 练习7.4

编写一个名为`Person`的类，使其表示人员的姓名和地址。使用`string`对象存放这些元素，接下来的练习将不断充实这个类的其他特征。

解：

```cpp
#include <string>

class Person {
    std::string name;
    std::string address;
};
```

## 练习7.5

在你的`Person`类中提供一些操作使其能够返回姓名和地址。
这些函数是否应该是`const`的呢？解释原因。

解：

```cpp
#include <string>

class Person 
{
    std::string name;
    std::string address;
public:
    auto get_name() const -> std::string const& { return name; }
    auto get_addr() const -> std::string const& { return address; }
};
```

应该是`const`的。因为常量的`Person`对象也需要使用这些函数操作。

## 练习7.6

对于函数`add`、`read`和`print`，定义你自己的版本。

解：

```cpp
#include <string>
#include <iostream>

struct Sales_data {
    std::string const& isbn() const { return bookNo; };
    Sales_data& combine(const Sales_data&);

    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

// member functions.
Sales_data& Sales_data::combine(const Sales_data& rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

// nonmember functions
std::istream &read(std::istream &is, Sales_data &item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

std::ostream &print(std::ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " " << item.revenue;
    return os;
}

Sales_data add(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}
```

## 练习7.7

使用这些新函数重写7.1.2节练习中的程序。

```cpp
int main()
{
    Sales_data total;
    if (read(std::cin, total))
    {
        Sales_data trans;
        while (read(std::cin, trans)) {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else {
                print(std::cout, total) << std::endl;
                total = trans;
            }
        }
        print(std::cout, total) << std::endl;
    }
    else
    {
        std::cerr << "No data?!" << std::endl;
        return -1;
    }
    
    return 0;
}
```

## 练习7.8

为什么`read`函数将其`Sales_data`参数定义成普通的引用，而`print`函数将其参数定义成常量引用？

解：

因为`read`函数会改变对象的内容，而`print`函数不会。

## 练习7.9

对于7.1.2节练习中代码，添加读取和打印`Person`对象的操作。

解：

```cpp
#include <string>
#include <iostream>

struct Person 
{
    std::string const& getName()    const { return name; }
    std::string const& getAddress() const { return address; }
    
    std::string name;
    std::string address;
};

std::istream &read(std::istream &is, Person &person)
{
    return is >> person.name >> person.address;
}

std::ostream &print(std::ostream &os, const Person &person)
{
    return os << person.name << " " << person.address;
}
```

## 练习7.10

在下面这条`if`语句中，条件部分的作用是什么？

```cpp
if (read(read(cin, data1), data2)) //等价read(std::cin, data1);read(std::cin, data2);
```

解：

`read`函数的返回值是`istream`对象，
`if`语句中条件部分的作用是从输入流中读取数据给两个`data`对象。

## 练习7.11 : 

在你的`Sales_data`类中添加构造函数，
然后编写一段程序令其用到每个构造函数。

解：

头文件：

```cpp
#include <string>#include <iostream>struct Sales_data {    Sales_data() = default;    Sales_data(const std::string &s):bookNo(s) { }    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }    Sales_data(std::istream &is);        std::string isbn() const { return bookNo; };    Sales_data& combine(const Sales_data&);        std::string bookNo;    unsigned units_sold = 0;    double revenue = 0.0;};// nonmember functionsstd::istream &read(std::istream &is, Sales_data &item){    double price = 0;    is >> item.bookNo >> item.units_sold >> price;    item.revenue = price * item.units_sold;    return is;}std::ostream &print(std::ostream &os, const Sales_data &item){    os << item.isbn() << " " << item.units_sold << " " << item.revenue;    return os;}Sales_data add(const Sales_data &lhs, const Sales_data &rhs){    Sales_data sum = lhs;    sum.combine(rhs);    return sum;}// member functions.Sales_data::Sales_data(std::istream &is){    read(is, *this);}Sales_data& Sales_data::combine(const Sales_data& rhs){    units_sold += rhs.units_sold;    revenue += rhs.revenue;    return *this;}
```

主函数：

```cpp
int main(){    Sales_data item1;    print(std::cout, item1) << std::endl;        Sales_data item2("0-201-78345-X");    print(std::cout, item2) << std::endl;        Sales_data item3("0-201-78345-X", 3, 20.00);    print(std::cout, item3) << std::endl;        Sales_data item4(std::cin);    print(std::cout, item4) << std::endl;        return 0;}
```

## 练习7.12

把只接受一个`istream`作为参数的构造函数移到类的内部。

解：

```cpp
#include <string>#include <iostream>struct Sales_data;std::istream &read(std::istream&, Sales_data&);struct Sales_data {    Sales_data() = default;    Sales_data(const std::string &s):bookNo(s) { }    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }    Sales_data(std::istream &is) { read(is, *this); }        std::string isbn() const { return bookNo; };    Sales_data& combine(const Sales_data&);        std::string bookNo;    unsigned units_sold = 0;    double revenue = 0.0;};// member functions.Sales_data& Sales_data::combine(const Sales_data& rhs){    units_sold += rhs.units_sold;    revenue += rhs.revenue;    return *this;}// nonmember functionsstd::istream &read(std::istream &is, Sales_data &item){    double price = 0;    is >> item.bookNo >> item.units_sold >> price;    item.revenue = price * item.units_sold;    return is;}std::ostream &print(std::ostream &os, const Sales_data &item){    os << item.isbn() << " " << item.units_sold << " " << item.revenue;    return os;}Sales_data add(const Sales_data &lhs, const Sales_data &rhs){    Sales_data sum = lhs;    sum.combine(rhs);    return sum;}
```

## 练习7.13

使用`istream`构造函数重写第229页的程序。

解：

```cpp
int main(){    Sales_data total(std::cin);    if (!total.isbn().empty())    {        std::istream &is = std::cin;        while (is) {            Sales_data trans(is);            if (!is) break;            if (total.isbn() == trans.isbn())                total.combine(trans);            else {                print(std::cout, total) << std::endl;                total = trans;            }        }        print(std::cout, total) << std::endl;    }    else    {        std::cerr << "No data?!" << std::endl;        return -1;    }        return 0;}
```

## 练习7.14

编写一个构造函数，令其用我们提供的类内初始值显式地初始化成员。

```cpp
Sales_data() : units_sold(0) , revenue(0) { }
```

## 练习7.15

为你的`Person`类添加正确的构造函数。

解：

```cpp
#include <string>#include <iostream>struct Person;std::istream &read(std::istream&, Person&);struct Person{	Person() = default;	Person(const std::string& sname, const std::string& saddr) :name(sname), address(saddr) {}	Person(std::istream &is) { read(is, *this); }	std::string getName() const { return name; }	std::string getAddress() const { return address; }	std::string name;	std::string address;};std::istream &read(std::istream &is, Person &person){	is >> person.name >> person.address;	return is;}std::ostream &print(std::ostream &os, const Person &person){	os << person.name << " " << person.address;	return os;}
```

## 练习7.16

在类的定义中对于访问说明符出现的位置和次数有限定吗？
如果有，是什么？什么样的成员应该定义在`public`说明符之后？
什么样的成员应该定义在`private`说明符之后？

解：

在类的定义中对于访问说明符出现的位置和次数**没有限定**。

每个访问说明符指定了接下来的成员的访问级别，其有效范围直到出现下一个访问说明符或者达到类的结尾处为止。

如果某个成员能够在整个程序内都被访问，那么它应该定义为`public`; 
如果某个成员只能在类内部访问，那么它应该定义为`private`。

## 练习7.17

使用`class`和`struct`时有区别吗？如果有，是什么？

解：

`class`和`struct`的唯一区别是默认的访问级别不同。

## 练习7.18

封装是何含义？它有什么用处？

解：

将类内部分成员设置为外部不可见，而提供部分接口给外面，这样的行为叫做封装。

用处：

- 1.确保用户的代码不会无意间破坏封装对象的状态。
- 2.被封装的类的具体实现细节可以随时改变，而无需调整用户级别的代码。

## 练习7.19

在你的`Person`类中，你将把哪些成员声明成`public`的？
哪些声明成`private`的？
解释你这样做的原因。

构造函数、`getName()`、`getAddress()`函数将设为`public`。
`name`和 `address` 将设为`private`。
函数是暴露给外部的接口，因此要设为`public`；
而数据则应该隐藏让外部不可见。

## 练习7.20

友元在什么时候有用？请分别举出使用友元的利弊。

解：

当其他类或者函数想要访问当前类的私有变量时，这个时候应该用友元。

利：

与当前类有关的接口函数能直接访问类的私有变量。

弊：

牺牲了封装性与可维护性。

## 练习7.21

修改你的`Sales_data`类使其隐藏实现的细节。
你之前编写的关于`Sales_data`操作的程序应该继续使用，借助类的新定义重新编译该程序，确保其正常工作。

解：

```cpp
#include <string>#include <iostream>class Sales_data {    friend std::istream &read(std::istream &is, Sales_data &item);    friend std::ostream &print(std::ostream &os, const Sales_data &item);    friend Sales_data add(const Sales_data &lhs, const Sales_data &rhs);public:    Sales_data() = default;    Sales_data(const std::string &s):bookNo(s) { }    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }    Sales_data(std::istream &is) { read(is, *this); }    std::string isbn() const { return bookNo; };    Sales_data& combine(const Sales_data&);private:    std::string bookNo;    unsigned units_sold = 0;    double revenue = 0.0;};// member functions.Sales_data& Sales_data::combine(const Sales_data& rhs){    units_sold += rhs.units_sold;    revenue += rhs.revenue;    return *this;}// friend functionsstd::istream &read(std::istream &is, Sales_data &item){    double price = 0;    is >> item.bookNo >> item.units_sold >> price;    item.revenue = price * item.units_sold;    return is;}std::ostream &print(std::ostream &os, const Sales_data &item){    os << item.isbn() << " " << item.units_sold << " " << item.revenue;    return os;}Sales_data add(const Sales_data &lhs, const Sales_data &rhs){    Sales_data sum = lhs;    sum.combine(rhs);    return sum;}
```

## 练习7.22

修改你的`Person`类使其隐藏实现的细节。

解：

```cpp
#include <string>#include <iostream>class Person {    friend std::istream &read(std::istream &is, Person &person);    friend std::ostream &print(std::ostream &os, const Person &person);public:    Person() = default;    Person(const std::string sname, const std::string saddr):name(sname), address(saddr){ }    Person(std::istream &is){ read(is, *this); }    std::string getName() const { return name; }    std::string getAddress() const { return address; }private:    std::string name;    std::string address;};std::istream &read(std::istream &is, Person &person){    is >> person.name >> person.address;    return is;}std::ostream &print(std::ostream &os, const Person &person){    os << person.name << " " << person.address;    return os;}
```

## 练习7.23

编写你自己的`Screen`类型。

解：

```cpp
#include <string>class Screen {    public:        using pos = std::string::size_type;        Screen() = default;        Screen(pos ht, pos wd, char c):height(ht), width(wd), contents(ht*wd, c){ }        char get() const { return contents[cursor]; }        char get(pos r, pos c) const { return contents[r*width+c]; }    private:        pos cursor = 0;        pos height = 0, width = 0;        std::string contents;};
```

# 练习7.24

给你的`Screen`类添加三个构造函数：一个默认构造函数；另一个构造函数接受宽和高的值，然后将`contents`初始化成给定数量的空白；第三个构造函数接受宽和高的值以及一个字符，该字符作为初始化后屏幕的内容。

解：

```cpp
#include <string>class Screen {    public:        using pos = std::string::size_type;        Screen() = default; // 1        Screen(pos ht, pos wd):height(ht), width(wd), contents(ht*wd, ' '){ } // 2        Screen(pos ht, pos wd, char c):height(ht), width(wd), contents(ht*wd, c){ } // 3        char get() const { return contents[cursor]; }        char get(pos r, pos c) const { return contents[r*width+c]; }    private:        pos cursor = 0;        pos height = 0, width = 0;        std::string contents;};
```

## 练习7.25

`Screen`能安全地依赖于拷贝和赋值操作的默认版本吗？
如果能，为什么？如果不能？为什么？

解：

能。 `Screen`的成员只有内置类型和`string`，因此能安全地依赖于拷贝和赋值操作的默认版本。

管理动态内存的类则不能依赖于拷贝和赋值操作的默认版本，而且也应该尽量使用`string`和`vector`来避免动态管理内存的复杂性。

## 练习7.26

将`Sales_data::avg_price`定义成内联函数。

解：

在头文件中加入：

```cpp
inline double Sales_data::avg_price() const{    return units_sold ? revenue/units_sold : 0;}
```

## 练习7.27 

给你自己的`Screen`类添加`move`、`set` 和`display`函数，通过执行下面的代码检验你的类是否正确。

```cpp
Screen myScreen(5, 5, 'X');myScreen.move(4, 0).set('#').display(cout);cout << "\n";myScreen.display(cout);cout << "\n";
```

解：

增加代码：

```cpp
#include <string>#include <iostream>class Screen {public:    ... ...    inline Screen& move(pos r, pos c);    inline Screen& set(char c);    inline Screen& set(pos r, pos c, char ch);    const Screen& display(std::ostream &os) const { do_display(os); return *this; }    Screen& display(std::ostream &os) { do_display(os); return *this; }private:    void do_display(std::ostream &os) const { os << contents; }    ... ...};inline Screen& Screen::move(pos r, pos c){    cursor = r*width + c;    return *this;}inline Screen& Screen::set(char c){    contents[cursor] = c;    return *this;}inline Screen& Screen::set(pos r, pos c, char ch){    contents[r*width+c] = ch;    return *this;}
```

测试代码：

```cpp
int main(){    Screen myScreen(5, 5, 'X');    myScreen.move(4, 0).set('#').display(std::cout);    std::cout << "\n";    myScreen.display(std::cout);    std::cout << "\n";    return 0;}
```

## 练习7.28

如果`move`、`set`和`display`函数的返回类型不是`Screen&` 而是`Screen`，则在上一个练习中将会发生什么？

解：

如果返回类型是`Screen`，那么`move`返回的是`*this`的一个副本，因此`set`函数只能改变临时副本而不能改变`myScreen`的值。

## 练习7.29

修改你的`Screen`类，令`move`、`set`和`display`函数返回`Screen`并检查程序的运行结果，在上一个练习中你的推测正确吗？

解：

推测正确。

```
#with '&'XXXXXXXXXXXXXXXXXXXX#XXXXXXXXXXXXXXXXXXXXXXXX#XXXX                    ^# without '&'XXXXXXXXXXXXXXXXXXXX#XXXXXXXXXXXXXXXXXXXXXXXXXXXXX                    ^
```

## 练习7.30

通过`this`指针使用成员的做法虽然合法，但是有点多余。讨论显示使用指针访问成员的优缺点。

解：

优点：

程序的意图更明确

函数的参数可以与成员同名，如

```cpp
  void setAddr(const std::string &addr) { this->addr = addr; }
```

缺点：

有时候显得有点多余，如

```cpp
std::string getAddr() const { return this->addr; }
```

## 练习7.31

定义一对类`X`和`Y`，其中`X`包含一个指向`Y`的指针，而`Y`包含一个类型为`X`的对象。

解：

```cpp
class Y;class X{    Y* y = nullptr;	};class Y{    X x;};
```

## 练习7.32

定义你自己的`Screen`和`Window_mgr`，其中`clear`是`Window_mgr`的成员，是`Screen`的友元。

解：

```cpp
#include <vector>#include <iostream>#include <string>class Screen;class Window_mgr{public:	using ScreenIndex = std::vector<Screen>::size_type;	inline void clear(ScreenIndex);private:	std::vector<Screen> screens;};class Screen{	friend void Window_mgr::clear(ScreenIndex);public:	using pos = std::string::size_type;	Screen() = default;	Screen(pos ht, pos wd) :height(ht), width(wd), contents(ht*wd,' ') {}	Screen(pos ht, pos wd, char c) :height(ht), width(wd), contents(ht*wd, c) {}	char get() const { return contents[cursor]; }	char get(pos r, pos c) const { return contents[r*width + c]; }	inline Screen& move(pos r, pos c);	inline Screen& set(char c);	inline Screen& set(pos r, pos c, char ch);	const Screen& display(std::ostream& os) const { do_display(os); return *this; }	Screen& display(std::ostream& os) { do_display(os); return *this; }	private:	void do_display(std::ostream &os) const { os << contents; }private:	pos cursor = 0;	pos width = 0, height = 0;	std::string contents;};inline void Window_mgr::clear(ScreenIndex i){	Screen& s = screens[i];	s.contents = std::string(s.height*s.width,' ');}inline Screen& Screen::move(pos r, pos c){	cursor = r*width + c;	return *this;}inline Screen& Screen::set(char c){	contents[cursor] = c;	return *this;}inline Screen& Screen::set(pos r, pos c, char ch){	contents[r*width + c] = ch;	return *this;}
```

## 练习7.33

如果我们给`Screen`添加一个如下所示的`size`成员将发生什么情况？如果出现了问题，请尝试修改它。

```cpp
pos Screen::size() const{    return height * width;}
```

解：

纠正：错误为 error: extra qualification 'Screen::' on member 'size' [-fpermissive]
则应该去掉Screen::,改为

 ```cpp
pos size() const{           return height * width;       }
 ```

##  练习7.34

如果我们把第256页`Screen`类的`pos`的`typedef`放在类的最后一行会发生什么情况？

解：

在 dummy_fcn(pos height) 函数中会出现 未定义的标识符pos。

类型名的定义通常出现在类的开始处，这样就能确保所有使用该类型的成员都出现在类名的定义之后。

## 练习7.35

解释下面代码的含义，说明其中的`Type`和`initVal`分别使用了哪个定义。如果代码存在错误，尝试修改它。

```cpp
typedef string Type;Type initVal(); class Exercise {public:    typedef double Type;    Type setVal(Type);    Type initVal(); private:    int val;};Type Exercise::setVal(Type parm) {     val = parm + initVal();         return val;}
```

解：

书上255页中说：

```
然而在类中，如果成员使用了外层作用域中的某个名字，而该名字代表一种类型，则类不能在之后重新定义该名字。
```

因此重复定义`Type`是错误的行为。

虽然重复定义类型名字是错误的行为，但是编译器并不为此负责。所以我们要人为地遵守一些原则，在这里有一些讨论。

## 练习7.36

下面的初始值是错误的，请找出问题所在并尝试修改它。

```cpp
struct X {	X (int i, int j): base(i), rem(base % j) {}	int rem, base;};
```

解：

应该改为：

```cpp
struct X {	X (int i, int j): base(i), rem(base % j) {}	int base, rem;};
```

## 练习7.37

使用本节提供的`Sales_data`类，确定初始化下面的变量时分别使用了哪个构造函数，然后罗列出每个对象所有的数据成员的值。

解：

```cpp
Sales_data first_item(cin); // 使用 Sales_data(std::istream &is) ; 各成员值从输入流中读取int main() {    // 使用默认构造函数  bookNo = "", cnt = 0, revenue = 0.0    Sales_data next;    // 使用 Sales_data(std::string s = "");   bookNo = "9-999-99999-9", cnt = 0, revenue = 0.0    Sales_data last("9-999-99999-9"); }
```

## 练习7.38

有些情况下我们希望提供`cin`作为接受`istream&`参数的构造函数的默认实参，请声明这样的构造函数。

解：

```cpp
Sales_data(std::istream &is = std::cin) { read(is, *this); }
```

## 练习7.39

如果接受`string`的构造函数和接受`istream&`的构造函数都使用默认实参，这种行为合法吗？如果不，为什么？

解：

不合法。当你调用`Sales_data()`构造函数时，无法区分是哪个重载。

## 练习7.40

从下面的抽象概念中选择一个（或者你自己指定一个），思考这样的类需要哪些数据成员，提供一组合理的构造函数并阐明这样做的原因。

```
(a) Book(b) Data(c) Employee(d) Vehicle(e) Object(f) Tree
```

解：

(a) Book.

```cpp
class Book {public:    Book(unsigned isbn, std::string const& name, std::string const& author, std::string const& pubdate)        :isbn_(isbn), name_(name), author_(author), pubdate_(pubdate)    { }    explicit Book(std::istream &in)     {         in >> isbn_ >> name_ >> author_ >> pubdate_;    }private:    unsigned isbn_;    std::string name_;    std::string author_;    std::string pubdate_;};
```

## 练习7.41

使用委托构造函数重新编写你的`Sales_data`类，给每个构造函数体添加一条语句，令其一旦执行就打印一条信息。用各种可能的方式分别创建`Sales_data`对象，认真研究每次输出的信息直到你确实理解了委托构造函数的执行顺序。

解：

- [头文件](https://github.com/applenob/Cpp_Primer_Practice/tree/master/cpp_source/ch07/ex_7_41.h)
- [源文件](https://github.com/applenob/Cpp_Primer_Practice/tree/master/cpp_source/ch07/ex_7_41.cpp)
- [主函数](https://github.com/applenob/Cpp_Primer_Practice/tree/master/cpp_source/ch07/ex_7_41_main.cpp)

总结：使用委托构造函数，调用顺序是：

- 1.实际的构造函数的函数体。
- 2.委托构造函数的函数体。

## 练习7.42

对于你在练习7.40中编写的类，确定哪些构造函数可以使用委托。如果可以的话，编写委托构造函数。如果不可以，从抽象概念列表中重新选择一个你认为可以使用委托构造函数的，为挑选出的这个概念编写类定义。

解：

```cpp
class Book {public:    Book(unsigned isbn, std::string const& name, std::string const& author, std::string const& pubdate)        :isbn_(isbn), name_(name), author_(author), pubdate_(pubdate)    { }    Book(unsigned isbn) : Book(isbn, "", "", "") {}    explicit Book(std::istream &in)     {         in >> isbn_ >> name_ >> author_ >> pubdate_;    }private:    unsigned isbn_;    std::string name_;    std::string author_;    std::string pubdate_;};
```

## 练习7.43

假定有一个名为`NoDefault`的类，它有一个接受`int`的构造函数，但是没有默认构造函数。定义类`C`，`C`有一个 `NoDefault`类型的成员，定义`C`的默认构造函数。

```cpp
class NoDefault {public:    NoDefault(int i) { }};class C {public:    C() : def(0) { } private:    NoDefault def;};
```

## 练习7.44

下面这条声明合法吗？如果不，为什么？

```cpp
vector<NoDefault> vec(10);//vec初始化有10个元素
```

解：

不合法。因为`NoDefault`没有默认构造函数。

## 练习7.45

如果在上一个练习中定义的vector的元素类型是C，则声明合法吗？为什么？

合法。因为`C`有默认构造函数。

## 练习7.46

下面哪些论断是不正确的？为什么？

- (a) 一个类必须至少提供一个构造函数。
- (b) 默认构造函数是参数列表为空的构造函数。
- (c) 如果对于类来说不存在有意义的默认值，则类不应该提供默认构造函数。
- (d) 如果类没有定义默认构造函数，则编译器将为其生成一个并把每个数据成员初始化成相应类型的默认值。

解：

- (a) 不正确。如果我们的类没有显式地定义构造函数，那么编译器就会为我们隐式地定义一个默认构造函数，并称之为合成的默认构造函数。
- (b) 不完全正确。为每个参数都提供了默认值的构造函数也是默认构造函数。
- (c) 不正确。哪怕没有意义的值也需要初始化。
- (d) 不正确。只有当一个类没有定义**任何构造函数**的时候，编译器才会生成一个默认构造函数。

## 练习7.47

说明接受一个`string`参数的`Sales_data`构造函数是否应该是`explicit`的，并解释这样做的优缺点。

解：

是否需要从`string`到`Sales_data`的转换依赖于我们对用户使用该转换的看法。在此例中，这种转换可能是对的。`null_book`中的`string`可能表示了一个不存在的`ISBN`编号。

优点：

可以抑制构造函数定义的隐式转换

缺点：

为了转换要显式地使用构造函数

## 练习7.48

假定`Sales_data`的构造函数不是`explicit`的，则下述定义将执行什么样的操作？

解：

```cpp
string null_isbn("9-999-9999-9");Sales_data item1(null_isbn);Sales_data item2("9-999-99999-9");
```

这些定义和是不是`explicit`的无关。

## 练习7.49

对于`combine`函数的三种不同声明，当我们调用`i.combine(s)`时分别发生什么情况？其中`i`是一个`Sales_data`，而` s`是一个`string`对象。

解：

```cpp
(a) Sales_data &combine(Sales_data); // ok(b) Sales_data &combine(Sales_data&); // error C2664: 无法将参数 1 从“std::string”转换为“Sales_data &”	因为隐式转换只有一次(c) Sales_data &combine(const Sales_data&) const; // 该成员函数是const 的，意味着不能改变对象。而 combine函数的本意就是要改变对象
```

## 练习7.50

确定在你的`Person`类中是否有一些构造函数应该是`explicit` 的。

解：

```cpp
explicit Person(std::istream &is){ read(is, *this); }
```

## 练习7.51

`vector`将其单参数的构造函数定义成`explicit`的，而`string`则不是，你觉得原因何在？

假如我们有一个这样的函数：

```cpp
int getSize(const std::vector<int>&);
```

如果`vector`没有将单参数构造函数定义成`explicit`的，我们就可以这样调用：

```cpp
getSize(34);
```

很明显这样调用会让人困惑，函数实际上会初始化一个拥有34个元素的`vecto`r的临时量，然后返回34。但是这样没有任何意义。而`string`则不同，`string`的单参数构造函数的参数是`const char *`，因此凡是在需要用到`string`的地方都可以用` const char *`来代替（字面值就是`const char *`）。如：

```cpp
void print(std::string);print("hello world");
```

## 练习7.52

使用2.6.1节的 `Sales_data` 类，解释下面的初始化过程。如果存在问题，尝试修改它。

```cpp
Sales_data item = {"987-0590353403", 25, 15.99};
```

解：

`Sales_data` 类不是聚合类，应该修改成如下：

```cpp
struct Sales_data {    std::string bookNo;    unsigned units_sold;    double revenue;};
```

## 练习7.53

定义你自己的`Debug`。

解：

```cpp
class Debug {public:    constexpr Debug(bool b = true) : hw(b), io(b), other(b) { }    constexpr Debug(bool h, bool i, bool o) : hw(r), io(i), other(0) { }    constexpr bool any() { return hw || io || other; }    void set_hw(bool b) { hw = b; }    void set_io(bool b) { io = b; }    void set_other(bool b) { other = b; }    private:    bool hw;        // runtime error    bool io;        // I/O error    bool other;     // the others};
```

## 练习7.54

`Debug`中以 `set_` 开头的成员应该被声明成`constexpr` 吗？如果不，为什么？

解：

不能。`constexpr`函数必须包含一个返回语句。

## 练习7.55

7.5.5节的`Data`类是字面值常量类吗？请解释原因。

解：

不是。因为`std::string`不是字面值类型。

## 练习7.56

什么是类的静态成员？它有何优点？静态成员与普通成员有何区别？

解：

与类本身相关，而不是与类的各个对象相关的成员是静态成员。静态成员能用于某些场景，而普通成员不能。

## 练习7.57

编写你自己的`Account`类。

解：

```cpp
class Account {public:    void calculate() { amount += amount * interestRate; }    static double rate() { return interestRate; }    static void rate(double newRate) { interestRate = newRate; }    private:    std::string owner;    double amount;    static double interestRate;    static constexpr double todayRate = 42.42;    static double initRate() { return todayRate; }};double Account::interestRate = initRate();
```

## 练习7.58

下面的静态数据成员的声明和定义有错误吗？请解释原因。

```cpp
//example.h
class Example {
public:
	static double rate = 6.5;
	static const int vecSize = 20;
	static vector<double> vec(vecSize);
};

//example.c
#include "example.h"
double Example::rate;
vector<double> Example::vec;
```

解：

`rate`应该是一个**常量表达式**。而类内只能初始化整型类型的静态常量，所以不能在类内初始化`vec`。修改后如下：

```cpp
// example.h
class Example {
public:
    static constexpr double rate = 6.5;
    static const int vecSize = 20;
    static vector<double> vec;
};

// example.C
#include "example.h"
constexpr double Example::rate;
vector<double> Example::vec(Example::vecSize);
```

