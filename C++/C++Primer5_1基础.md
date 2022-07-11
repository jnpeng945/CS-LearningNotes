# 第一章 开始

## 熟悉编译器

**g++**：

- 编译：`g++ --std=c++11 ch01.cpp -o main`
- 运行：`./prog1`
- 查看运行状态：`echo $?`
- 编译多个文件:`g++ ch2.cpp Sales_item.cc -o main`

输入 `g++ --help`，查看编译器选项：

```cpp
Usage: g++ [options] file...
Options:
  -pass-exit-codes         Exit with highest error code from a phase
  --help                   Display this information
  --target-help            Display target specific command line options
  --help={common|optimizers|params|target|warnings|[^]{joined|separate|undocumented}}[,...]
                           Display specific types of command line options
  (Use '-v --help' to display command line options of sub-processes)
  --version                Display compiler version information
  -dumpspecs               Display all of the built in spec strings
  -dumpversion             Display the version of the compiler
  -dumpmachine             Display the compiler's target processor
  -print-search-dirs       Display the directories in the compiler's search path
  -print-libgcc-file-name  Display the name of the compiler's companion library
  -print-file-name=<lib>   Display the full path to library <lib>
  -print-prog-name=<prog>  Display the full path to compiler component <prog>
  -print-multiarch         Display the target's normalized GNU triplet, used as
                           a component in the library path
  -print-multi-directory   Display the root directory for versions of libgcc
  -print-multi-lib         Display the mapping between command line options and
                           multiple library search directories
  -print-multi-os-directory Display the relative path to OS libraries
  -print-sysroot           Display the target libraries directory
  -print-sysroot-headers-suffix Display the sysroot suffix used to find headers
  -Wa,<options>            Pass comma-separated <options> on to the assembler
  -Wp,<options>            Pass comma-separated <options> on to the preprocessor
  -Wl,<options>            Pass comma-separated <options> on to the linker
  -Xassembler <arg>        Pass <arg> on to the assembler
  -Xpreprocessor <arg>     Pass <arg> on to the preprocessor
  -Xlinker <arg>           Pass <arg> on to the linker
  -save-temps              Do not delete intermediate files
  -save-temps=<arg>        Do not delete intermediate files
  -no-canonical-prefixes   Do not canonicalize paths when building relative
                           prefixes to other gcc components
  -pipe                    Use pipes rather than intermediate files
  -time                    Time the execution of each subprocess
  -specs=<file>            Override built-in specs with the contents of <file>
  -std=<standard>          Assume that the input sources are for <standard>
  --sysroot=<directory>    Use <directory> as the root directory for headers
                           and libraries
  -B <directory>           Add <directory> to the compiler's search paths
  -v                       Display the programs invoked by the compiler
  -###                     Like -v but options quoted and commands not executed
  -E                       Preprocess only; do not compile, assemble or link
  -S                       Compile only; do not assemble or link
  -c                       Compile and assemble, but do not link
  -o <file>                Place the output into <file>
  -pie                     Create a position independent executable
  -shared                  Create a shared library
  -x <language>            Specify the language of the following input files
                           Permissible languages include: c c++ assembler none
                           'none' means revert to the default behavior of
                           guessing the language based on the file's extension

```

输入 `g++ -v --help`可以看到更完整的指令。
例如还有些常用的：

```cpp
-h FILENAME, -soname FILENAME: Set internal name of shared library
-I PROGRAM, --dynamic-linker PROGRAM: Set PROGRAM as the dynamic linker to use
-l LIBNAME, --library LIBNAME: Search for library LIBNAME
-L DIRECTORY, --library-path DIRECTORY: Add DIRECTORY to library search path
```

**获得程序状态**:

- windows: ``echo %ERRORLEVEL%``
- UNIX: ``echo $?``

## IO

- ```#include <iostream>```
- ```std::cout << "hello"```
- ```std::cin >> v1```

记住`>>`和`<<`返回的结果都是左操作数，也就是输入流和输出流本身。

**endl**：这是一个被称为**操纵符**（manipulator）的特殊值，效果是结束当前行，并将设备关联的缓冲区（buffer）中的内容刷到设备中。

UNIX和Mac下键盘输入文件结束符：`ctrl+d`，Windows下：`ctrl+z`

**头文件**：类的类型一般存储在头文件中，标准库的头文件使用`<>`，非标准库的头文件使用`""`。申明写在`.h`文件，定义实现写在`.cpp`文件。

**避免多次包含同一头文件**：

```cpp
#ifndef SALESITEM_H
#define SALESITEM_H
// Definition of Sales_itemclass and related functions goes here
#endif
```

**成员函数（类方法）**：使用`.`调用。

**命名空间（namespace）**：使用作用域运算符`::`调用。

## 注释

- 单行注释： `//`
- 多行注释： `/**/`。编译器将`/*`和`*/`之间的内容都作为注释内容忽略。注意不能嵌套。

## while语句

循环执行，（直到条件（condition）为假。

## for语句

循环头由三部分组成：

- 一个初始化语句（init-statement）
- 一个循环条件（condition）
- 一个表达式（expression）

## 使用文件重定向

``./main <infile >outfile``



# 第二章 变量和基本类型

### 基本内置类型

**基本算数类型**：

| 类型          | 含义           | 最小尺寸                       |
| ------------- | -------------- | ------------------------------ |
| `bool`        | 布尔类型       | 8bits                          |
| `char`        | 字符           | 8bits                          |
| `wchar_t`     | 宽字符         | 16bits                         |
| `char16_t`    | Unicode字符    | 16bits                         |
| `char32_t`    | Unicode字符    | 32bits                         |
| `short`       | 短整型         | 16bits                         |
| `int`         | 整型           | 16bits (在32位机器中是32bits)  |
| `long`        | 长整型         | 32bits                         |
| `long long`   | 长整型         | 64bits （是在C++11中新定义的） |
| `float`       | 单精度浮点数   | 6位有效数字                    |
| `double`      | 双精度浮点数   | 10位有效数字                   |
| `long double` | 扩展精度浮点数 | 10位有效数字                   |


### 如何选择类型

- 1.当明确知晓数值不可能是负数时，选用无符号类型；
- 2.使用`int`执行整数运算。一般`long`的大小和`int`一样，而`short`常常显得太小。除非超过了`int`的范围，选择`long long`。
- 3.算术表达式中不要使用`char`或`bool`。
- 4.浮点运算选用`double`。

### 类型转换

- 非布尔型赋给布尔型，初始值为0则结果为false，否则为true。
- 布尔型赋给非布尔型，初始值为false结果为0，初始值为true结果为1。

### 字面值常量

- 一个形如`42`的值被称作**字面值常量**（literal）。
  - 整型和浮点型字面值。
  - 字符和字符串字面值。
    - 使用空格连接，继承自C。
    - 字符字面值：单引号， `'a'`
    - 字符串字面值：双引号， `"Hello World""`
  - 转义序列。`\n`、`\t`等。
  - 布尔字面值。`true`，`false`。
  - 指针字面值。`nullptr`

## 变量

**变量**提供一个**具名**的、可供程序操作的存储空间。   `C++`中**变量**和**对象**一般可以互换使用。

### 变量定义（define）

- **定义形式**：类型说明符（type specifier） + 一个或多个变量名组成的列表。如`int sum = 0, value, units_sold = 0;`
- **初始化**（initialize）：对象在创建时获得了一个特定的值。
  - **初始化不是赋值！**：
  - 初始化 = 创建变量 + 赋予初始值
  - 赋值 = 擦除对象的当前值 + 用新值代替
  - **列表初始化**：使用花括号`{}`，如`int units_sold{0};`
  - 默认初始化：定义时没有指定初始值会被默认初始化；在函数体内部的内置类型变量将不会被初始化。
  - 建议初始化每一个内置类型的变量。

### 变量的**声明**（declaration） vs **定义**（define）

  - 为了支持分离式编译，`C++`将声明和定义区分开。**声明**使得名字为程序所知。**定义**负责创建与名字关联的实体。
  - **extern**：只是说明变量定义在其他地方。
  - 只声明而不定义： 在变量名前添加关键字 `extern`，如`extern int i;`。但如果包含了初始值，就变成了定义：`extern double pi = 3.14;`
  - 变量只能被定义一次，但是可以多次声明。
- 名字的**作用域**（namescope）

## 左值和右值

- **左值**（l-value）**可以**出现在赋值语句的左边或者右边，比如变量；
- **右值**（r-value）**只能**出现在赋值语句的右边，比如常量。


## 复合类型

### 引用

- **引用**：引用是一个对象的别名，引用类型引用（refer to）另外一种类型。如`int &refVal = val;`。
- 引用必须初始化。
- 引用和它的初始值是**绑定bind**在一起的，而**不是拷贝**。

### 指针

- 是一种 `"指向（point to）"`另外一种类型的复合类型。
- **定义**指针类型： `int *ip1;`，**从右向左读**，`ip1`是指向`int`类型的指针。
- 指针存放某个对象的**地址**。
- 获取对象的地址： `int i=42; int *p = &i;`。 `&`是**取地址符**。
- 指针的值的四种状态：
  - 1.指向一个对象；
  - 2.指向紧邻对象的下一个位置；
  - 3.空指针；
  - 4.无效指针。
- 指针访问对象： `cout << *p;`， `*`是**解引用符**。
- 空指针不指向任何对象。
- `void*`指针可以存放**任意**对象的地址。
- 其他指针类型必须要与所指对象**严格匹配**。
- 两个指针相减的类型是`ptrdiff_t`。
- 建议：初始化所有指针。

## const限定符

- 动机：希望定义一些不能被改变值的变量。

### 初始化和const

- const对象**必须初始化**，且**不能被改变**。
- const变量默认不能被其他文件访问，非要访问，必须在指定const前加extern。

### const的引用

- **reference to const**（对常量的引用）：指向const对象的引用，如 `const int ival=1; const int &refVal = ival;`，可以读取但不能修改`refVal`。
- **临时量**（temporary）对象：当编译器需要一个空间来暂存表达式的求值结果时，临时创建的一个未命名的对象。
- 对临时量的引用是非法行为。

### 指针和const

- **pointer to const**（指向常量的指针）：不能用于改变其所指对象的值, 如 `const double pi = 3.14; const double *cptr = &pi;`。
- **const pointer**：指针本身是常量，如 `int i = 0; int *const ptr = &i;`

### 顶层const

- `顶层const`：指针本身是个常量。
- `底层const`：指针指向的对象是个常量。拷贝时严格要求相同的底层const资格。

### `constexpr`和常量表达式

- 常量表达式：指值不会改变，且在编译过程中就能得到计算结果的表达式。
- `C++11`新标准规定，允许将变量声明为`constexpr`类型以便由编译器来验证变量的值是否是一个常量的表达式。

## 处理类型

### 类型别名

- 传统别名：使用**typedef**来定义类型的同义词。 `typedef double wages;`
- 新标准别名：别名声明（alias declaration）： `using SI = Sales_item;`（C++11）

### auto类型说明符

- **auto**类型说明符：让编译器**自动推断类型**。
- `int i = 0, &r = i; auto a = r;` 推断`a`的类型是`int`。
- 会忽略`顶层const`。
- `const int ci = 1; const auto f = ci;`推断类型是`int`，需要自己加`const`
- `C++11`

### decltype类型指示符

- 从表达式的类型推断出要定义的变量的类型。
- **decltype**：选择并返回操作数的**数据类型**。
- `decltype(f()) sum = x;` 推断`sum`的类型是函数`f`的返回类型。
- 不会忽略`顶层const`。
- `C++11`

## 自定义数据结构

### struct

- 类可以以关键字`struct`开始，紧跟类名和类体。
- 类数据成员：类体定义类的成员。
- `C++11`：可以为类数据成员提供一个**类内初始值**（in-class initializer）。

### 编写自己的头文件

- 头文件通常包含哪些只能被定义一次的实体：类、`const`和`constexpr`变量。

预处理器概述：

- **预处理器**（preprocessor）：确保头文件多次包含仍能安全工作。
- 当预处理器看到`#include`标记时，会用指定的头文件内容代替`#include`
- **头文件保护符**（header guard）：头文件保护符依赖于预处理变量的状态：已定义和未定义。

```cpp
#ifndef SALES_DATA_H
#define SALES_DATA_H
strct Sale_data{
    ...
}
#endif
```



# 第三章 字符串、向量和数组

## using声明

- 使用某个命名空间：例如 `using std::cin`表示使用命名空间`std`中的名字`cin`。
- 头文件中不应该包含`using`声明。这样使用了该头文件的源码也会使用这个声明，会带来风险。

## string

- 标准库类型`string`表示可变长的字符序列。
- `#include <string>`，然后 `using std::string;`
- **string对象**：注意，不同于字符串字面值。

### 定义和初始化string对象

初始化`string`对象的方式：

| 方式                  | 解释                                                    |
| --------------------- | ------------------------------------------------------- |
| `string s1`           | 默认初始化，`s1`是个空字符串                            |
| `string s2(s1)`       | `s2`是`s1`的副本                                        |
| `string s2 = s1`      | 等价于`s2(s1)`，`s2`是`s1`的副本                        |
| `string s3("value")`  | `s3`是字面值“value”的副本，除了字面值最后的那个空字符外 |
| `string s3 = "value"` | 等价于`s3("value")`，`s3`是字面值"value"的副本          |
| `string s4(n, 'c')`   | 把`s4`初始化为由连续`n`个字符`c`组成的串                |

- 拷贝初始化（copy initialization）：使用等号`=`将一个已有的对象拷贝到正在创建的对象。
- 直接初始化（direct initialization）：通过括号给对象赋值。

### string对象上的操作

`string`的操作：

| 操作                 | 解释                                                         |
| -------------------- | ------------------------------------------------------------ |
| `os << s`            | 将`s`写到输出流`os`当中，返回`os`                            |
| `is >> s`            | 从`is`中读取字符串赋给`s`，字符串以空白分割，返回`is`        |
| `getline(is, s)`     | 从`is`中读取一行赋给`s`，返回`is`                            |
| `s.empty()`          | `s`为空返回`true`，否则返回`false`                           |
| `s.size()`           | 返回`s`中字符的个数                                          |
| `s[n]`               | 返回`s`中第`n`个字符的引用，位置`n`从0计起                   |
| `s1+s2`              | 返回`s1`和`s2`连接后的结果                                   |
| `s1=s2`              | 用`s2`的副本代替`s1`中原来的字符                             |
| `s1==s2`             | 如果`s1`和`s2`中所含的字符完全一样，则它们相等；`string`对象的相等性判断对字母的大小写敏感 |
| `s1!=s2`             | 同上                                                         |
| `<`, `<=`, `>`, `>=` | 利用字符在字典中的顺序进行比较，且对字母的大小写敏感         |

- string io：
  - 执行读操作`>>`：忽略掉开头的空白（包括空格、换行符和制表符），直到遇到下一处空白为止。
  - `getline`：读取一整行，**包括空白符**。
- 字符串字面值和string是不同的类型。

### 处理string对象中的字符

- **ctype.h vs. cctype**：C++修改了c的标准库，名称为去掉`.h`，前面加`c`。

`cctype`头文件中定义了一组标准函数：

| 函数          | 解释                                                         |
| ------------- | ------------------------------------------------------------ |
| `isalnum(c)`  | 当`c`是字母或数字时为真                                      |
| `isalpha(c)`  | 当`c`是字母时为真                                            |
| `iscntrl(c)`  | 当`c`是控制字符时为真                                        |
| `isdigit(c)`  | 当`c`是数字时为真                                            |
| `isgraph(c)`  | 当`c`不是空格但可以打印时为真                                |
| `islower(c)`  | 当`c`是小写字母时为真                                        |
| `isprint(c)`  | 当`c`是可打印字符时为真                                      |
| `ispunct(c)`  | 当`c`是标点符号时为真                                        |
| `isspace(c)`  | 当`c`是空白时为真（空格、横向制表符、纵向制表符、回车符、换行符、进纸符） |
| `isupper(c)`  | 当`c`是大写字母时为真                                        |
| `isxdigit(c)` | 当`c`是十六进制数字时为真                                    |
| `tolower(c)`  | 当`c`是大写字母，输出对应的小写字母；否则原样输出`c`         |
| `toupper(c)`  | 当`c`是小写字母，输出对应的大写字母；否则原样输出`c`         |

- 遍历字符串：使用**范围for**（range for）语句： `for (auto c: str)`，或者 `for (auto &c: str)`使用引用直接改变字符串中的字符。 （C++11）

## vector

- vector是一个**容器**，也是一个类模板；
- `#include <vector>` 然后 `using std::vector;`
- 容器：包含其他对象。
- 类模板：本身不是类，但可以**实例化instantiation**出一个类。 `vector`是一个模板， `vector<int>`是一个类型。
- 通过将类型放在类模板名称后面的**尖括号**中来指定**类型**，如`vector<int> ivec`。

### 定义和初始化vector对象

初始化`vector`对象的方法

| 方法                        | 解释                                                         |
| --------------------------- | ------------------------------------------------------------ |
| `vector<T> v1`              | `v1`是一个空`vector`，它潜在的元素是`T`类型的，执行默认初始化 |
| `vector<T> v2(v1)`          | `v2`中包含有`v1`所有元素的副本                               |
| `vector<T> v2 = v1`         | 等价于`v2(v1)`，`v2`中包含`v1`所有元素的副本                 |
| `vector<T> v3(n, val)`      | `v3`包含了n个重复的元素，每个元素的值都是`val`               |
| `vector<T> v4(n)`           | `v4`包含了n个重复地执行了值初始化的对象                      |
| `vector<T> v5{a, b, c...}`  | `v5`包含了初始值个数的元素，每个元素被赋予相应的初始值       |
| `vector<T> v5={a, b, c...}` | 等价于`v5{a, b, c...}`                                       |

- 列表初始化： `vector<string> v{"a", "an", "the"};` （C++11）

### 向vector对象中添加元素

- `v.push_back(e)` 在尾部增加元素。

### 其他vector操作

`vector`支持的操作：

| 操作               | 解释                                                         |
| ------------------ | ------------------------------------------------------------ |
| `v.emtpy()`        | 如果`v`不含有任何元素，返回真；否则返回假                    |
| `v.size()`         | 返回`v`中元素的个数                                          |
| `v.push_back(t)`   | 向`v`的尾端添加一个值为`t`的元素                             |
| `v[n]`             | 返回`v`中第`n`个位置上元素的**引用**                         |
| `v1 = v2`          | 用`v2`中的元素拷贝替换`v1`中的元素                           |
| `v1 = {a,b,c...}`  | 用列表中元素的拷贝替换`v1`中的元素                           |
| `v1 == v2`         | `v1`和`v2`相等当且仅当它们的元素数量相同且对应位置的元素值都相同 |
| `v1 != v2`         | 同上                                                         |
| `<`,`<=`,`>`, `>=` | 以字典顺序进行比较                                           |

- 范围`for`语句内不应该改变其遍历序列的大小。
- `vector`对象（以及`string`对象）的下标运算符，只能对确知已存在的元素执行下标操作，不能用于添加元素。

## 迭代器iterator

- 所有标准库容器都可以使用迭代器。
- 类似于指针类型，迭代器也提供了对对象的间接访问。

### 使用迭代器

- `vector<int>::iterator iter`。
- `auto b = v.begin();`返回指向第一个元素的迭代器。
- `auto e = v.end();`返回指向最后一个元素的下一个（哨兵，尾后,one past the end）的迭代器（off the end）。
- 如果容器为空， `begin()`和 `end()`返回的是同一个迭代器，都是尾后迭代器。
- 使用解引用符`*`访问迭代器指向的元素。
- 养成使用迭代器和`!=`的习惯（泛型编程）。
- **容器**：可以包含其他对象；但所有的对象必须类型相同。
- **迭代器（iterator）**：每种标准容器都有自己的迭代器。`C++`倾向于用迭代器而不是下标遍历元素。
- **const_iterator**：只能读取容器内元素不能改变。
- **箭头运算符**： 解引用 + 成员访问，`it->mem`等价于 `(*it).mem`
- **谨记**：但凡是使用了迭代器的循环体，都不要向迭代器所属的容器添加元素。

标准容器迭代器的运算符:

| 运算符           | 解释                                   |
| ---------------- | -------------------------------------- |
| `*iter`          | 返回迭代器`iter`所指向的**元素的引用** |
| `iter->mem`      | 等价于`(*iter).mem`                    |
| `++iter`         | 令`iter`指示容器中的下一个元素         |
| `--iter`         | 令`iter`指示容器中的上一个元素         |
| `iter1 == iter2` | 判断两个迭代器是否相等                 |

### 迭代器运算

`vector`和`string`迭代器支持的运算：

| 运算符               | 解释                                                         |
| -------------------- | ------------------------------------------------------------ |
| `iter + n`           | 迭代器加上一个整数值仍得到一个迭代器，迭代器指示的新位置和原来相比向前移动了若干个元素。结果迭代器或者指示容器内的一个元素，或者指示容器尾元素的下一位置。 |
| `iter - n`           | 迭代器减去一个证书仍得到一个迭代器，迭代器指示的新位置比原来向后移动了若干个元素。结果迭代器或者指向容器内的一个元素，或者指示容器尾元素的下一位置。 |
| `iter1 += n`         | 迭代器加法的复合赋值语句，将`iter1`加n的结果赋给`iter1`      |
| `iter1 -= n`         | 迭代器减法的复合赋值语句，将`iter2`减n的加过赋给`iter1`      |
| `iter1 - iter2`      | 两个迭代器相减的结果是它们之间的距离，也就是说，将运算符右侧的迭代器向前移动差值个元素后得到左侧的迭代器。参与运算的两个迭代器必须指向的是同一个容器中的元素或者尾元素的下一位置。 |
| `>`、`>=`、`<`、`<=` | 迭代器的关系运算符，如果某迭代器                             |

- **difference_type**：保证足够大以存储任何两个迭代器对象间的距离，可正可负。

## 数组

- 相当于vector的低级版，**长度固定**。

### 定义和初始化内置数组

- 初始化：`char input_buffer[buffer_size];`，长度必须是const表达式，或者不写，让编译器自己推断。
- 数组不允许直接赋值给另一个数组。

### 访问数组元素

- 数组下标的类型：`size_t` 。
- 字符数组的特殊性：结尾处有一个空字符，如 `char a[] = "hello";` 。
- 用数组初始化 `vector`： `int a[] = {1,2,3,4,5}; vector<int> v(begin(a), end(a));` 。

### 数组和指针

- 使用数组时，编译器一般会把它转换成指针。
- 标准库类型限定使用的下标必须是无符号类型，而内置的下标可以处理负值。 
- **指针访问数组**：在表达式中使用数组名时，名字会自动转换成指向数组的第一个元素的指针。

## C风格字符串

- 从C继承来的字符串。
- 用空字符结束（`\0`）。
- 对大多数应用来说，使用标准库 `string`比使用C风格字符串更安全、更高效。
- 获取 `string` 中的 `cstring` ： `const char *str = s.c_str();` 。

C标准库String函数，定义在`<cstring>` 中：

| 函数             | 介绍                                                         |
| ---------------- | ------------------------------------------------------------ |
| `strlen(p)`      | 返回`p`的长度，空字符不计算在内                              |
| `strcmp(p1, p2)` | 比较`p1`和`p2`的相等性。如果`p1==p2`，返回0；如果`p1>p2`，返回一个正值；如果`p1<p2`，返回一个负值。 |
| `strcat(p1, p2)` | 将`p2`附加到`p1`之后，返回`p1`                               |
| `strcpy(p1, p2)` | 将`p2`拷贝给`p1`，返回`p1`                                   |

## 多维数组

- **多维数组的初始化**： `int ia[3][4] = {{0,1,2,3}, ...}`。
- 使用范围for语句时，除了最内层的循环外，其他所有循环的控制变量都应该是引用类型。

## 指针vs引用

- 引用总是指向某个对象，定义引用时没有初始化是错的。
- 给引用赋值，修改的是该引用所关联的对象的值，而不是让引用和另一个对象相关联。

## 指向指针的指针

- 定义： `int **ppi = &pi;`
- 解引用：`**ppi`

## 动态数组

- 使用 `new`和 `delete`表达和c中`malloc`和`free`类似的功能，即在堆（自由存储区）中分配存储空间。
- 定义： `int *pia = new int[10];` 10可以被一个变量替代。
- 释放： `delete [] pia;`，注意不要忘记`[]`。



# 第四章 表达式

## 表达式基础

- **重载运算符**：当运算符作用在类类型的运算对象时，用户可以自行定义其含义。
- **左值和右值**：
  - C中原意：左值**可以**在表达式左边，右值不能。
  - `C++`：当一个对象被用作**右值**的时候，用的是对象的**值**（内容）；
  - 被用做**左值**时，用的是对象的**身份**（在内存中的位置）。

## 算术运算符

- **溢出**：当计算的结果超出该类型所能表示的范围时就会产生溢出。

## 逻辑运算符

- **短路求值**：逻辑与运算符和逻辑或运算符都是先求左侧运算对象的值再求右侧运算对象的值，当且仅当左侧运算对象无法确定表达式的结果时才会计算右侧运算对象的值。

## 赋值运算符

- 如果赋值运算的左右侧运算对象类型不同，则右侧运算对象将转换成左侧运算对象的类型。
- 赋值运算符满足右结合律，这点和其他二元运算符不一样。 `ival = jval = 0;`等价于`ival = (jval = 0);`
- 赋值运算优先级比较低。

## 条件运算符

- 条件运算符（`?:`）允许我们把简单的`if-else`逻辑嵌入到单个表达式中去，按照如下形式：`cond? expr1: expr2`

## 位运算符

- 位运算符是作用于**整数类型**的运算对象。
- 二进制位向左移（`<<`）或者向右移（`>>`），移出边界外的位就被舍弃掉了。
- 位取反（`~`）、与（`&`）、或（`|`）、异或（`^`）

## sizeof运算符

- 返回一条表达式或一个类型名字所占的**字节数**。返回的类型是 `size_t`。
- 两种形式： `sizeof (type)`和 `sizeof expr`

## 类型转换

### 隐式类型转换

- 比 `int`类型小的整数值先提升为较大的整数类型。
- 条件中，非布尔转换成布尔。
- 初始化中，初始值转换成变量的类型。
- 算术运算或者关系运算的运算对象有多种类型，要转换成同一种类型。
- 函数调用时。

### 显式类型转换（尽量避免）

- **static_cast**：任何明确定义的类型转换，只要不包含底层const，都可以使用。 `double slope = static_cast<double>(j);`
- **dynamic_cast**：支持运行时类型识别。
- **const_cast**：只能改变运算对象的底层const，一般可用于去除const性质。 `const char *pc; char *p = const_cast<char*>(pc)`
- **reinterpret_cast**：通常为运算对象的位模式提供低层次上的重新解释。



# 第五章 语句

## 简单语句

- **表达式语句**：一个表达式末尾加上分号，就变成了表达式语句。
- **空语句**：只有一个单独的分号。
- **复合语句（块）**：用花括号 `{}`包裹起来的语句和声明的序列。一个块就是一个作用域。

## 条件语句

- **悬垂else**（dangling else）：用来描述在嵌套的`if else`语句中，如果`if`比`else`多时如何处理的问题。C++使用的方法是`else`匹配最近没有配对的`if`。

## 迭代语句

- **while**：当不确定到底要迭代多少次时，使用 `while`循环比较合适，比如读取输入的内容。
- **for**： `for`语句可以省略掉 `init-statement`， `condition`和 `expression`的任何一个；**甚至全部**。
- **范围for**： `for (declaration: expression) statement`

## 跳转语句

- **break**：`break`语句负责终止离它最近的`while`、`do while`、`for`或者`switch`语句，并从这些语句之后的第一条语句开始继续执行。
- **continue**：终止最近的循环中的当前迭代并立即开始下一次迭代。只能在`while`、`do while`、`for`循环的内部。

## try语句块和异常处理

- **throw表达式**：异常检测部分使用 `throw`表达式来表示它遇到了无法处理的问题。我们说 `throw`引发 `raise`了异常。
- **try语句块**：以 `try`关键词开始，以一个或多个 `catch`字句结束。 `try`语句块中的代码抛出的异常通常会被某个 `catch`捕获并处理。 `catch`子句也被称为**异常处理代码**。
- **异常类**：用于在 `throw`表达式和相关的 `catch`子句之间传递异常的具体信息。



# 第六章 函数

## 函数基础

- **函数定义**：包括返回类型、函数名字和0个或者多个**形参**（parameter）组成的列表和函数体。
- **调用运算符**：调用运算符的形式是一对圆括号 `()`，作用于一个表达式，该表达式是函数或者指向函数的指针。
- 圆括号内是用逗号隔开的**实参**（argument）列表。
- 函数调用过程：
  - 1.主调函数（calling function）的执行被中断。
  - 2.被调函数（called function）开始执行。
- **形参和实参**：形参和实参的**个数**和**类型**必须匹配上。
- **返回类型**： `void`表示函数不返回任何值。函数的返回类型不能是数组类型或者函数类型，但可以是指向数组或者函数的指针。
- **名字**：名字的作用于是程序文本的一部分，名字在其中可见。

### 局部对象

- **生命周期**：对象的生命周期是程序执行过程中该对象存在的一段时间。
- **局部变量**（local variable）：形参和函数体内部定义的变量统称为局部变量。它对函数而言是局部的，对函数外部而言是**隐藏**的。
- **自动对象**：只存在于块执行期间的对象。当块的执行结束后，它的值就变成**未定义**的了。
- **局部静态对象**： `static`类型的局部变量，生命周期贯穿函数调用前后。

### 函数声明

- **函数声明**：函数的声明和定义唯一的区别是声明无需函数体，用一个分号替代。函数声明主要用于描述函数的接口，也称**函数原型**。
- **在头文件中进行函数声明**：建议变量在头文件中声明；在源文件中定义。
- **分离编译**： `CC a.cc b.cc`直接编译生成可执行文件；`CC -c a.cc b.cc`编译生成对象代码`a.o b.o`； `CC a.o b.o`编译生成可执行文件。

## 参数传递

- 形参初始化的机理和变量初始化一样。
- **引用传递**（passed by reference）：又称传引用调用（called by reference），指**形参是引用类型**，引用形参是它对应的实参的别名。
- **值传递**（passed by value）：又称传值调用（called by value），指实参的值是通过**拷贝**传递给形参。

### 传值参数

- 当初始化一个非引用类型的变量时，初始值被拷贝给变量。
- 函数对形参做的所有操作都不会影响实参。
- **指针形参**：常用在C中，`C++`建议使用引用类型的形参代替指针。

### 传引用参数

- 通过使用引用形参，允许函数改变一个或多个实参的值。
- 引用形参直接关联到绑定的对象，而非对象的副本。
- 使用引用形参可以用于**返回额外的信息**。
- 经常用引用形参来避免不必要的复制。
- `void swap(int &v1, int &v2)`
- 如果无需改变引用形参的值，最好将其声明为常量引用。

### const形参和实参

- 形参的顶层`const`被忽略。`void func(const int i);`调用时既可以传入`const int`也可以传入`int`。
- 我们可以使用非常量初始化一个底层`const`对象，但是反过来不行。
- 在函数中，不能改变实参的**局部副本**。
- 尽量使用常量引用。

### 数组形参

- 当我们为函数传递一个数组时，实际上传递的是指向数组首元素的指针。
- 要注意数组的实际长度，不能越界。

### main处理命令行选项

- `int main(int argc, char *argv[]){...}`
- 第一个形参代表参数的个数；第二个形参是参数C风格字符串数组。

### 可变形参

`initializer_list`提供的操作（`C++11`）：

| 操作                                 | 解释                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| `initializer_list<T> lst;`           | 默认初始化；`T`类型元素的空列表                              |
| `initializer_list<T> lst{a,b,c...};` | `lst`的元素数量和初始值一样多；`lst`的元素是对应初始值的副本；列表中的元素是`const`。 |
| `lst2(lst)`                          | 拷贝或赋值一个`initializer_list`对象不会拷贝列表中的元素；拷贝后，原始列表和副本共享元素。 |
| `lst2 = lst`                         | 同上                                                         |
| `lst.size()`                         | 列表中的元素数量                                             |
| `lst.begin()`                        | 返回指向`lst`中首元素的指针                                  |
| `lst.end()`                          | 返回指向`lst`中微元素下一位置的指针                          |

`initializer_list`使用demo：

```cpp
void err_msg(ErrCode e, initializer_list<string> il){
    cout << e.msg << endl;
    for (auto bed = il.begin(); beg != il.end(); ++ beg)
        cout << *beg << " ";
    cout << endl;
}

err_msg(ErrCode(0), {"functionX", "okay});
```

- 所有实参类型相同，可以使用 `initializer_list`的标准库类型。
- 实参类型不同，可以使用`可变参数模板`。
- 省略形参符： `...`，便于`C++`访问某些C代码，这些C代码使用了 `varargs`的C标准功能。

## 返回类型和return语句

### 无返回值函数

没有返回值的 `return`语句只能用在返回类型是 `void`的函数中，返回 `void`的函数不要求非得有 `return`语句。

### 有返回值函数

- `return`语句的返回值的类型必须和函数的返回类型相同，或者能够**隐式地**转换成函数的返回类型。
- 值的返回：返回的值用于初始化调用点的一个**临时量**，该临时量就是函数调用的结果。
- **不要返回局部对象的引用或指针**。
- **引用返回左值**：函数的返回类型决定函数调用是否是左值。调用一个返回引用的函数得到左值；其他返回类型得到右值。
- **列表初始化返回值**：函数可以返回花括号包围的值的列表。（`C++11`）
- **主函数main的返回值**：如果结尾没有`return`，编译器将隐式地插入一条返回0的`return`语句。返回0代表执行成功。

### 返回数组指针

- `Type (*function (parameter_list))[dimension]`
- 使用类型别名： `typedef int arrT[10];` 或者 `using arrT = int[10;]`，然后 `arrT* func() {...}`
- 使用 `decltype`： `decltype(odd) *arrPtr(int i) {...}`
- **尾置返回类型**： 在形参列表后面以一个`->`开始：`auto func(int i) -> int(*)[10]`（`C++11`）

## 函数重载

- **重载**：如果同一作用域内几个函数名字相同但形参列表不同，我们称之为重载（overload）函数。
- `main`函数不能重载。
- **重载和const形参**：
  - 一个有顶层const的形参和没有它的函数无法区分。 `Record lookup(Phone* const)`和 `Record lookup(Phone*)`无法区分。
  - 相反，是否有某个底层const形参可以区分。 `Record lookup(Account*)`和 `Record lookup(const Account*)`可以区分。
- **重载和作用域**：若在内层作用域中声明名字，它将隐藏外层作用域中声明的同名实体，在不同的作用域中无法重载函数名。

## 特殊用途语言特性

### 默认实参

- `string screen(sz ht = 24, sz wid = 80, char backgrnd = ' ');`
- 一旦某个形参被赋予了默认值，那么它之后的形参都必须要有默认值。

### 内联（inline）函数

- 普通函数的缺点：调用函数比求解等价表达式要慢得多。
- `inline`函数可以避免函数调用的开销，可以让编译器在编译时**内联地展开**该函数。
- `inline`函数应该在头文件中定义。

### constexpr函数

- 指能用于常量表达式的函数。
- `constexpr int new_sz() {return 42;}`
- 函数的返回类型及所有形参类型都要是字面值类型。
- `constexpr`函数应该在头文件中定义。

### 调试帮助

- `assert`预处理宏（preprocessor macro）：`assert(expr);`

开关调试状态：

`CC -D NDEBUG main.c`可以定义这个变量`NDEBUG`。

```cpp
void print(){
    #ifndef NDEBUG
        cerr << __func__ << "..." << endl;
    #endif
}
```

## 函数匹配

- 重载函数匹配的**三个步骤**：1.候选函数；2.可行函数；3.寻找最佳匹配。
- **候选函数**：选定本次调用对应的重载函数集，集合中的函数称为候选函数（candidate function）。
- **可行函数**：考察本次调用提供的实参，选出可以被这组实参调用的函数，新选出的函数称为可行函数（viable function）。
- **寻找最佳匹配**：基本思想：实参类型和形参类型越接近，它们匹配地越好。

## 函数指针

- **函数指针**：是指向函数的指针。
- `bool (*pf)(const string &, const string &);` 注：两端的括号不可少。
- **函数指针形参**：
  - 形参中使用函数定义或者函数指针定义效果一样。
  - 使用类型别名或者`decltype`。
- **返回指向函数的指针**：1.类型别名；2.尾置返回类型。



# 第七章 类 （Class）

## 定义抽象数据类型

- **类背后的基本思想**：**数据抽象**（data abstraction）和**封装**（encapsulation）。
- 数据抽象是一种依赖于**接口**（interface）和**实现**（implementation）分离的编程技术。
- 封装实现了类的**接口**（interface）和**实现**（implementation）的分离。

### 类成员 （Member）

- 必须在类的内部声明，不能在其他地方增加成员。
- 成员可以是数据，函数，类型别名。

### 类的成员函数

- 成员函数的**声明**必须在类的内部。
- 成员函数的**定义**既可以在类的内部也可以在外部。
- 使用点运算符 `.` 调用成员函数。
- 必须对任何`const`或引用类型成员以及没有默认构造函数的类类型的任何成员使用初始化式。
- `ConstRef::ConstRef(int ii): i(ii), ci(i), ri(ii) { }`
- 默认实参： `Sales_item(const std::string &book): isbn(book), units_sold(0), revenue(0.0) { }`
- `*this`：
  - 每个成员函数都有一个额外的，隐含的形参`this`。
  - `this`总是指向当前对象，因此`this`是一个常量指针。
  - 形参表后面的`const`，改变了隐含的`this`形参的类型，如 `bool same_isbn(const Sales_item &rhs) const`，这种函数称为“常量成员函数”（`this`指向的当前对象是常量）。
  - `return *this;`可以让成员函数连续调用。
  - 普通的非`const`成员函数：`this`是指向类类型的`const`指针（可以改变`this`所指向的值，不能改变`this`保存的地址）。
  - `const`成员函数：`this`是指向const类类型的`const`指针（既不能改变`this`所指向的值，也不能改变`this`保存的地址）。

### 非成员函数

- 和类相关的非成员函数，定义和声明都应该在类的外部。

### 类的构造函数

- 类通过一个或者几个特殊的成员函数来控制其对象的初始化过程，这些函数叫做**构造函数**。
- 构造函数是特殊的成员函数。
- 构造函数放在类的`public`部分。
- 与类同名的成员函数。
- `Sales_item(): units_sold(0), revenue(0.0) { }`
- `=default`要求编译器合成默认的构造函数。(`C++11`)
- 初始化列表：冒号和花括号之间的代码： `Sales_item(): units_sold(0), revenue(0.0) { }`

## 访问控制与封装

- **访问说明符**（access specifiers）：
  - `public`：定义在 `public`后面的成员在整个程序内可以被访问； `public`成员定义类的接口。
  - `private`：定义在 `private`后面的成员可以被类的成员函数访问，但不能被使用该类的代码访问； `private`隐藏了类的实现细节。
- 使用 `class`或者 `struct`：都可以被用于定义一个类。唯一的却别在于访问权限。
  - 使用 `class`：在第一个访问说明符之前的成员是 `priavte`的。
  - 使用 `struct`：在第一个访问说明符之前的成员是 `public`的。

### 友元

- 允许特定的**非成员函数**访问一个类的**私有成员**.
- 友元的声明以关键字 `friend`开始。 `friend Sales_data add(const Sales_data&, const Sales_data&);`表示非成员函数`add`可以访问类的非公有成员。
- 通常将友元声明成组地放在**类定义的开始或者结尾**。
- 类之间的友元：
  - 如果一个类指定了友元类，则友元类的成员函数可以访问此类包括非公有成员在内的所有成员。

### 封装的益处

- 确保用户的代码不会无意间破坏封装对象的状态。
- 被封装的类的具体实现细节可以随时改变，而无需调整用户级别的代码。

## 类的其他特性

- 成员函数作为内联函数 `inline`：
  - 在类的内部，常有一些规模较小的函数适合于被声明成内联函数。
  - **定义**在类内部的函数是**自动内联**的。
  - 在类外部定义的成员函数，也可以在声明时显式地加上 `inline`。
- **可变数据成员** （mutable data member）：
  - `mutable size_t access_ctr;`
  - 永远不会是`const`，即使它是`const`对象的成员。
- **类类型**：
  - 每个类定义了唯一的类型。

## 类的作用域

- 每个类都会定义它自己的作用域。在类的作用域之外，普通的数据和函数成员只能由引用、对象、指针使用成员访问运算符来访问。
- 函数的**返回类型**通常在函数名前面，因此当成员函数定义在类的外部时，返回类型中使用的名字都位于类的作用域之外。
- 如果成员使用了外层作用域中的某个名字，而该名字代表一种**类型**，则类不能在之后重新定义该名字。
- 类中的**类型名定义**都要放在一开始。

## 构造函数再探

- 构造函数初始值列表：
  - 类似`python`使用赋值的方式有时候不行，比如`const`或者引用类型的数据，只能初始化，不能赋值。（注意初始化和赋值的区别）
  - 最好让构造函数初始值的顺序和成员声明的顺序保持一致。
  - 如果一个构造函数为所有参数都提供了默认参数，那么它实际上也定义了默认的构造函数。

### 委托构造函数 （delegating constructor, `C++11`）

- 委托构造函数将自己的职责委托给了其他构造函数。
- `Sale_data(): Sale_data("", 0, 0) {}`

### 隐式的类型转换

- 如果构造函数**只接受一个实参**，则它实际上定义了转换为此类类型的**隐式转换机制**。这种构造函数又叫**转换构造函数**（converting constructor）。
- 编译器只会自动地执行`仅一步`类型转换。
- 抑制构造函数定义的隐式转换：
  - 将构造函数声明为`explicit`加以阻止。
  - `explicit`构造函数只能用于直接初始化，不能用于拷贝形式的初始化。

### 聚合类 （aggregate class）

- 满足以下所有条件：
  - 所有成员都是`public`的。
  - 没有定义任何构造函数。
  - 没有类内初始值。
  - 没有基类，也没有`virtual`函数。
- 可以使用一个花括号括起来的成员初始值列表，初始值的顺序必须和声明的顺序一致。

### 字面值常量类

- `constexpr`函数的参数和返回值必须是字面值。
- **字面值类型**：除了算术类型、引用和指针外，某些类也是字面值类型。
- 数据成员都是字面值类型的聚合类是字面值常量类。
- 如果不是聚合类，则必须满足下面所有条件：
  - 数据成员都必须是字面值类型。
  - 类必须至少含有一个`constexpr`构造函数。
  - 如果一个数据成员含有类内部初始值，则内置类型成员的初始值必须是一条常量表达式；或者如果成员属于某种类类型，则初始值必须使用成员自己的`constexpr`构造函数。
  - 类必须使用析构函数的默认定义，该成员负责销毁类的对象。

## 类的静态成员

- 非`static`数据成员存在于类类型的每个对象中。
- `static`数据成员独立于该类的任意对象而存在。
- 每个`static`数据成员是与类关联的对象，并不与该类的对象相关联。
- 声明：
  - 声明之前加上关键词`static`。
- 使用：
  - 使用**作用域运算符**`::`直接访问静态成员:`r = Account::rate();`
  - 也可以使用对象访问：`r = ac.rate();`
- 定义：
  - 在类外部定义时不用加`static`。
- 初始化：
  - 通常不在类的内部初始化，而是在定义时进行初始化，如 `double Account::interestRate = initRate();`
  - 如果一定要在类内部定义，则要求必须是字面值常量类型的`constexpr`。

