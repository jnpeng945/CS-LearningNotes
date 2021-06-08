# 第十七章 标准库特殊设施

## tuple类型

- `tuple`是类似`pair`的模板，每个成员类型都可以不同，但`tuple`可以有任意数量的成员。
- 但每个确定的`tuple`类型的成员数目是固定的。
- 我们可以将`tuple`看做一个“快速而随意”的数据结构。

**tuple支持的操作**：

| 操作                                         | 解释                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| `tuple<T1, T2, ..., Tn> t;`                  | `t`是一个`tuple`，成员数为`n`，第`i`个成员的类型是`Ti`所有成员都进行值初始化。 |
| `tuple<T1, T2, ..., Tn> t(v1, v2, ..., vn);` | 每个成员用对应的初始值`vi`进行初始化。此构造函数是`explicit`的。 |
| `make_tuple(v1, v2, ..., vn)`                | 返回一个用给定初始值初始化的`tuple`。`tuple`的类型从初始值的类型**推断**。 |
| `t1 == t2`                                   | 当两个`tuple`具有相同数量的成员且成员对应相等时，两个`tuple`相等。 |
| `t1 relop t2`                                | `tuple`的关系运算使用**字典序**。两个`tuple`必须具有相同数量的成员。 |
| `get<i>(t)`                                  | 返回`t`的第`i`个数据成员的引用：如果`t`是一个左值，结果是一个左值引用；否则，结果是一个右值引用。`tuple`的所有成员都是`public`的。 |
| `tuple_size<tupleType>::value`               | 一个类模板，可以通过一个`tuple`类型来初始化。它有一个名为`value`的`public constexpr static`数据成员，类型为`size_t`，表示给定`tuple`类型中成员的数量。 |
| `tuple_element<i, tupleType>::type`          | 一个类模板，可以通过一个整型常量和一个`tuple`类型来初始化。它有一个名为`type`的`public`成员，表示给定`tuple`类型中指定成员的类型。 |

### 定义和初始化tuple

定义和初始化示例：

- `tuple<size_t, size_t, size_t> threeD;`
- `tuple<size_t, size_t, size_t> threeD{1,2,3};`
- `auto item = make_tuple("0-999-78345-X", 3, 2.00)；`

访问tuple成员：

- `auto book = get<0>(item);`
- `get<2>(item) *= 0.8;`

### 使用tuple返回多个值

- `tuple`最常见的用途是从一个函数返回多个值。

## bitset类型

- 处理二进制位的有序集；
- `bitset`也是类模板，但尖括号中输入的是`bitset`的长度而不是元素类型，因为元素类型是固定的，都是一个二进制位。

初始化`bitset`的方法：

| 操作                                  | 解释                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| `bitset<n> b;`                        | `b`有`n`位；每一位均是0.此构造函数是一个`constexpr`。        |
| `bitset<n> b(u);`                     | `b`是`unsigned long long`值`u`的低`n`位的拷贝。如果`n`大于`unsigned long long`的大小，则`b`中超出`unsigned long long`的高位被置为0。此构造函数是一个`constexpr`。 |
| `bitset<n> b(s, pos, m, zero, one);`  | `b`是`string s`从位置`pos`开始`m`个字符的拷贝。`s`只能包含字符`zero`或`one`：如果`s`包含任何其他字符，构造函数会抛出`invalid_argument`异常。字符在`b`中分别保存为`zero`和`one`。`pos`默认为0，`m`默认为`string::npos`，`zero`默认为'0'，`one`默认为'1'。 |
| `bitset<n> b(cp, pos, m, zero, one);` | 和上一个构造函数相同，但从`cp`指向的字符数组中拷贝字符。如果未提供`m`，则`cp`必须指向一个`C`风格字符串。如果提供了`m`，则从`cp`开始必须至少有`m`个`zero`或`one`字符。 |

初始化案例；

- `bitset<13> bitvec1(0xbeef);`
- `bitset<32> bitvec4("1100");`

`bitset`操作：

| 操作                     | 解释                                                         |
| ------------------------ | ------------------------------------------------------------ |
| `b.any()`                | `b`中是否存在1。                                             |
| `b.all()`                | `b`中都是1。                                                 |
| `b.none()`               | `b`中是否没有1。                                             |
| `b.count()`              | `b`中1的个数。                                               |
| `b.size()`               |                                                              |
| `b.test(pos)`            | `pos`下标是否是1                                             |
| `b.set(pos)`             | `pos`置1                                                     |
| `b.set()`                | 所有都置1                                                    |
| `b.reset(pos)`           | 将位置`pos`处的位复位                                        |
| `b.reset()`              | 将`b`中所有位复位                                            |
| `b.flip(pos)`            | 将位置`pos`处的位取反                                        |
| `b.flip()`               | 将`b`中所有位取反                                            |
| `b[pos]`                 | 访问`b`中位置`pos`处的位；如果`b`是`const`的，则当该位置位时，返回`true`；否则返回`false`。 |
| `b.to_ulong()`           | 返回一个`unsigned long`值，其位模式和`b`相同。如果`b`中位模式不能放入指定的结果类型，则抛出一个`overflow_error`异常。 |
| `b.to_ullong()`          | 类似上面，返回一个`unsigned long long`值。                   |
| `b.to_string(zero, one)` | 返回一个`string`，表示`b`中位模式。`zero`和`one`默认为0和1。 |
| `os << b`                | 将`b`中二进制位打印为字符`1`或`0`，打印到流`os`。            |
| `is >> b`                | 从`is`读取字符存入`b`。当下一个字符不是1或0时，或是已经读入`b.size()`个位时，读取过程停止。 |

## 正则表达式

- 正则表达式（reqular expression）是一种描述字符序列的方法，是一种很强大的工具。

正则表达式库组件：

| 组件              | 解释                                                         |
| ----------------- | ------------------------------------------------------------ |
| `regex`           | 表示一个正则表达式的类                                       |
| `regex_match`     | 将一个字符序列与一个正则表达式匹配                           |
| `regex_search`    | 寻找第一个与正则表达式匹配的子序列                           |
| `regex_replace`   | 使用给定格式替换一个正则表达式                               |
| `sregex_iterator` | 迭代器适配器，调用`regex_searcg`来遍历一个`string`中所有匹配的子串 |
| `smatch`          | 容器类，保存在`string`中搜索的结果                           |
| `ssub_match`      | `string`中匹配的子表达式的结果                               |


`regex_match`和`regex_search`的参数：

| 操作               | 解释                                                         |
| ------------------ | ------------------------------------------------------------ |
| `(seq, m, r, mft)` | 在字符序列`seq`中查找`regex`对象`r`中的正则表达式。`seq`可以是一个`string`、标识范围的一对迭代器、一个指向空字符结尾的字符数组的指针。 |
| `(seq, r, mft)`    | `m`是一个`match`对象，用来保存匹配结果的相关细节。`m`和`seq`必须具有兼容的类型。`mft`是一个可选的`regex_constants::match_flag_type`值。 |

- 这些操作会返回`bool`值，指出是否找到匹配。

### 使用正则表达式库

- `regex`使用的正则表达式语言是`ECMAScript`，模式`[[::alpha::]]`匹配任意字母。
- 由于反斜线是C++中的特殊字符，在模式中每次出现`\`的地方，必须用一个额外的反斜线`\\`告知C++我们需要一个反斜线字符。
- 简单案例：
  - `string pattern("[^c]ei"); pattern = "[[:alpha:]]*" + pattern + "[[:alpha:]]*"` 查找不在字符c之后的字符串ei
  - `regex r(pattern);` 构造一个用于查找模式的regex
  - `smatch results;` 定义一个对象保存搜索结果
  - `string test_str = "receipt freind theif receive";`
  - `if (regex_search(test_str, results, r)) cout << results.str() << endl;` 如有匹配子串，打印匹配的单词。

`regex`（和`wregex`）选项：

| 操作                           | 解释                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| `regex r(re)` `regex r(re, f)` | `re`表示一个正则表达式，它可以是一个`string`、一对表示字符范围的迭代器、一个指向空字符结尾的字符数组的指针、一个字符指针和一个计数器、一个花括号包围的字符列表。`f`是指出对象如何处理的标志。`f`通过下面列出来的值来设置。如果未指定`f`，其默认值为`ECMAScript`。 |
| `r1 = re`                      | 将`r1`中的正则表达式替换Wie`re`。`re`表示一个正则表达式，它可以是另一个`regex`对象、一个`string`、一个指向空字符结尾的字符数组的指针或是一个花括号包围的字符列表。 |
| `r1.assign(re, f)`             | 和使用赋值运算符（=）的效果相同：可选的标志`f`也和`regex`的构造函数中对应的参数含义相同。 |
| `r.mark_count()`               | `r`中子表达式的数目                                          |
| `r.flags()`                    | 返回`r`的标志集                                              |

定义`regex`时指定的标志：

| 操作         | 解释                             |
| ------------ | -------------------------------- |
| `icase`      | 在匹配过程中忽略大小写           |
| `nosubs`     | 不保存匹配的子表达式             |
| `optimize`   | 执行速度优先于构造速度           |
| `ECMAScript` | 使用`ECMA-262`指定的语法         |
| `basic`      | 使用`POSIX`基本的正则表达式语法  |
| `extended`   | 使用`POSIX`扩展的正则表达式语法  |
| `awk`        | 使用`POSIX`版本的`awk`语言的语法 |
| `grep`       | 使用`POSIX`版本的`grep`的语法    |
| `egrep`      | 使用`POSIX`版本的`egrep`的语法   |

- 可以将正则表达式本身看做是一种简单程序语言设计的程序。在运行时，当一个`regex`对象被初始化或被赋予新模式时，才被“编译”。
- 如果编写的正则表达式存在错误，会在运行时抛出一个`regex_error`的异常。
- 避免创建不必要的正则表达式。构建一个`regex`对象可能比较耗时。

### 匹配与regex迭代器类型

`sregex_iterator`操作（用来获得所有匹配）：

| 操作                           | 解释                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| `sregex_iterator it(b, e, r);` | 一个`sregex_iterator`，遍历迭代器`b`和`e`表示的`string`。它调用`sregex_search(b, e, r)`将`it`定位到输入中第一个匹配的位置。 |
| `sregex_iterator end;`         | `sregex_iterator`的尾后迭代器                                |
| `*it`， `it->`                 | 根据最后一个调用`regex_search`的结果，返回一个`smatch`对象的引用或一个指向`smatch`对象的指针。 |
| `++it` ， `it++`               | 从输入序列当前匹配位置开始调用`regex_search`。前置版本返回递增后迭代器；后置版本返回旧值。 |
| `it1 == it2`                   | 如果两个`sregex_iterator`都是尾后迭代器，则它们相等。两个非尾后迭代器是从相同的输入序列和`regex`对象构造，则它们相等。 |

示例：

```cpp
// 将字符串file中所有匹配模式r的子串输出
for (sregex_iterator it(file.begin(), file.end(), r), end_it; it != end_it; ++it){
    cout << it ->str() << endl;
}
```

`smatch`操作：

| 操作                   | 解释                                                         |
| ---------------------- | ------------------------------------------------------------ |
| `m.ready()`            | 如果已经通过调用`regex_search`或`regex_match`设置了`m`，则返回`true`；否则返回`false`。如果`ready`返回`false`，则对`m`进行操作是未定义的。 |
| `m.size()`             | 如果匹配失败，则返回0，；否则返回最近一次匹配的正则表达式中子表达式的数目。 |
| `m.empty()`            | 等价于`m.size() == 0`                                        |
| `m.prefix()`           | 一个`ssub_match`对象，标识当前匹配之前的序列                 |
| `m.suffix()`           | 一个`ssub_match`对象，标识当前匹配之后的部分                 |
| `m.format(...)`        |                                                              |
| `m.length(n)`          | 第`n`个匹配的子表达式的大小                                  |
| `m.position(n)`        | 第`n`个子表达式距离序列开始的长度                            |
| `m.str(n)`             | 第`n`个子表达式匹配的`string`                                |
| `m[n]`                 | 对应第`n`个子表达式的`ssub_match`对象                        |
| `m.begin(), m.end()`   | 表示`m`中`ssub_match`元素范围的迭代器。                      |
| `m.cbegin(), m.cend()` | 常量迭代器                                                   |

### 使用子表达式

- 正则表达式语法通常用括号表示子表达式。
- 子表达式的索引从1开始。
- 在`fmt`中用`$`后跟子表达式的索引号来标识一个特定的子表达式。

示例：

```cpp
if (regex_search(filename, results, r))
    cout << results.str(1) << endl;  // .str(1)获取第一个子表达式匹配结果
```

`ssub_match`子匹配操作：

| 操作               | 解释                                                         |
| ------------------ | ------------------------------------------------------------ |
| `matched`          | 一个`public bool`数据成员，指出`ssub_match`是否匹配了        |
| `first`， `second` | `public`数据成员，指向匹配序列首元素和尾后位置的迭代器。如果未匹配，则`first`和`second`是相等的。 |
| `length()`         | 匹配的大小，如果`matched`为`false`，则返回0。                |
| `str()`            | 返回一个包含输入中匹配部分的`string`。如果`matched`为`false`，则返回空`string`。 |
| `s = ssub`         | 将`ssub_match`对象`ssub`转化为`string`对象`s`。等价于`s=ssub.str()`，转换运算符不是`explicit`的。 |

### 使用regex_replace

正则表达式替换操作：

| 操作                                                         | 解释                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `m.format(dest, fmt, mft)`, `m.format(fmt, mft)`             | 使用格式字符串`fmt`生成格式化输出，匹配在`m`中，可选的`match_flag_type`标志在`mft`中。第一个版本写入迭代器`dest`指向的目的为止，并接受`fmt`参数，可以是一个`string`，也可以是一个指向空字符结尾的字符数组的指针。`mft`的默认值是`format_default`。 |
| `rege_replace(dest, seq, r, fmt, mft)`，  `regex_replace(seq, r, fmt, mft)` | 遍历`seq`，用`regex_search`查找与`regex`对象`r`相匹配的子串，使用格式字符串`fmt`和可选的`match_flag_type`标志来生成输出。`mft`的默认值是`match_default` |

示例：

```cpp
string phone = "(\\()?(\\d{3})(\\))?([-. ])?(\\d{3})([-. ]?)(\\d{4})"
string fmt = "$2.$5.$7";  // 将号码格式改为ddd.ddd.dddd
regex r(phone);  // 用来寻找模式的regex对象
string number = "(908) 555-1800";
cout << regex_replace(number, r, fmt) << endl;
```

匹配标志：

| 操作                | 解释                                         |
| ------------------- | -------------------------------------------- |
| `match_default`     | 等价于`format_default`                       |
| `match_not_bol`     | 不将首字符作为行首处理                       |
| `match_not_eol`     | 不将尾字符作为行尾处理                       |
| `match_not_bow`     | 不将首字符作为单词首处理                     |
| `match_not_eow`     | 不将尾字符作为单词尾处理                     |
| `match_any`         | 如果存在多于一个匹配，则可以返回任意一个匹配 |
| `match_not_null`    | 不匹配任何空序列                             |
| `match_continuous`  | 匹配必须从输入的首字符开始                   |
| `match_prev_avail`  | 输入序列包含第一个匹配之前的内容             |
| `format_default`    | 用`ECMAScript`规则替换字符串                 |
| `format_sed`        | 用`POSIX sed`规则替换字符串                  |
| `format_no_copy`    | 不输出输入序列中未匹配的部分                 |
| `format_first_only` | 只替换子表达式的第一次出现                   |

## 随机数

- 新标准之前，C和C++都依赖一个简单的C库函数`rand`来生成随机数，且只符合均匀分布。
- 新标准：**随机数引擎** + **随机数分布类**， 定义在   `random`头文件中。
- C++程序应该使用`default_random_engine`类和恰当的分布类对象。

### 随机数引擎和分布

随机数引擎操作

| 操作                  | 解释                                             |
| --------------------- | ------------------------------------------------ |
| `Engine e;`           | 默认构造函数；使用该引擎类型默认的种子           |
| `Engine e(s);`        | 使用整型值`s`作为种子                            |
| `e.seed(s)`           | 使用种子`s`重置引擎的状态                        |
| `e.min()`，`e.max()`  | 此引擎可生成的最小值和最大值                     |
| `Engine::result_type` | 此引擎生成的`unsigned`整型类型                   |
| `e.discard(u)`        | 将引擎推进`u`步；`u`的类型为`unsigned long long` |

示例：

```cpp
// 初始化分布类型
uniform_int_distribution<unsigned> u(0, 9);
// 初始化引擎
default_random_engine e;
// 随机生成0-9的无符号整数
cout << u(e) << endl;
```

**设置随机数发生器种子**：

- 种子就是一个数值，引擎可以利用它从序列中一个新位置重新开始生成随机数。
- 种子可以使用系统函数`time(0)`。

### 其他随机数分布

分布类型的操作：

| 操作                | 解释                                                         |
| ------------------- | ------------------------------------------------------------ |
| `Dist d;`           | 默认够赞函数；使`d`准备好被使用。其他构造函数依赖于`Dist`的类型；分布类型的构造函数是`explicit`的。 |
| `d(e)`              | 用相同的`e`连续调用`d`的话，会根据`d`的分布式类型生成一个随机数序列；`e`是一个随机数引擎对象。 |
| `d.min()`,`d.max()` | 返回`d(e)`能生成的最小值和最大值。                           |
| `d.reset()`         | 重建`d`的状态，是的随后对`d`的使用不依赖于`d`已经生成的值。  |

## IO库再探

### 格式化输入与输出

- 使用操纵符改变格式状态。
- 控制布尔值的格式： `cout << boolalpha << true << endl;`
- 指定整型的进制：`cout << dec << 20 << endl;`

定义在`iostream`中的操纵符：

| 操纵符          | 解释                                        |
| --------------- | ------------------------------------------- |
| `boolalpha`     | 将`true`和`false`输出为字符串               |
| `* noboolalpha` | 将`true`和`false`输出为1,0                  |
| `showbase`      | 对整型值输出表示进制的前缀                  |
| `* noshowbase`  | 不生成表示进制的前缀                        |
| `showpoint`     | 对浮点值总是显示小数点                      |
| `* noshowpoint` | 只有当浮点值包含小数部分时才显示小数点      |
| `showpos`       | 对非负数显示`+`                             |
| `* noshowpos`   | 对非负数不显示`+`                           |
| `uppercase`     | 在十六进制中打印`0X`，在科学计数法中打印`E` |
| `* nouppercase` | 在十六进制中打印`0x`，在科学计数法中打印`e` |
| `* dec`         | 整型值显示为十进制                          |
| `hex`           | 整型值显示为十六进制                        |
| `oct`           | 整型值显示为八进制                          |
| `left`          | 在值的右侧添加填充字符                      |
| `right`         | 在值的左侧添加填充字符                      |
| `internal`      | 在符号和值之间添加填充字符                  |
| `fixed`         | 浮点值显示为定点十进制                      |
| `scientific`    | 浮点值显示为科学计数法                      |
| `hexfloat`      | 浮点值显示为十六进制（C++11）               |
| `defaultfloat`  | 充值浮点数格式为十进制（C++11）             |
| `unitbuf`       | 每次输出操作后都刷新缓冲区                  |

1| `* nounitbuf` | 恢复正常的缓冲区刷新模式 |
| `* skipws` | 输入运算符跳过空白符 |
| `noskipws` | 输入运算符不跳过空白符 |
| `flush` | 刷新`ostream`缓冲区 |
| `ends` | 插入空字符，然后刷新`ostream`缓冲区 |
| `endl` | 插入换行，然后刷新`ostream`缓冲区 |

其中`*`表示默认的流状态。

### 未格式化的输入/输出操作

单字节低层IO操作：

| 操作             | 解释                                                   |
| ---------------- | ------------------------------------------------------ |
| `is.get(ch)`     | 从`istream is`读取下一个字节存入字符`cn`中。返回`is`。 |
| `os.put(ch)`     | 将字符`ch`输出到`ostream os`。返回`os`。               |
| `is.get()`       | 将`is`的下一个字节作为`int`返回                        |
| `is.putback(ch)` | 将字符`ch`放回`is`。返回`is`。                         |
| `is.unget()`     | 将`is`向后移动一个字节。返回`is`。                     |
| `is.peek()`      | 将下一个字节作为`int`返回，但不从流中删除它。          |

多字节低层IO操作：

| 操作                            | 解释                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| `is.get(sink, size, delim)`     | 从`is`中读取最多`size`个字节，并保存在字符数组中，字符数组的起始地址由`sink`给出。读取过程直到遇到字符`delim`或读取了`size`个字节或遇到文件尾时停止。如果遇到了`delim`，则将其留在输入流中，不读取出来存入`sink`。 |
| `is.getline(sink, size, delim)` | 与接收三个参数的`get`版本类似，但会读取并丢弃`delim`。       |
| `is.read(sink, size)`           | 读取最多`size`个字节，存入字符数组`sink`中。返回`is`。       |
| `is.gcount()`                   | 返回上一个未格式化读取从`is`读取的字节数                     |
| `os.write(source, size)`        | 将字符数组`source`中的`size`个字节写入`os`。返回`os`。       |
| `is.ignore(size, delim)`        | 读取并忽略最多`size`个字符，包括`delim`。与其他未格式化函数不同，`ignore`有默认参数：`size`默认值是1，`delim`的默认值为文件尾。 |

- 注意：一般情况下，主张使用标准库提供的高层抽象，低层函数容易出错。

### 流随机访问

- 只适用于`fstream`和`sstream`。
- 通过将标记`seek`到一个给定位置来重定位它。
- `tell`告诉我们标记的当前位置。

| 操作                                   | 解释                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| `tellg()`，`tellp`                     | 返回一个输入流中（`tellg`）或输出流中（`tellp`）标记的当前位置。 |
| `seekg(pos)`，`seekp(pos)`             | 在一个输入流或输出流中将标记重定位到给定的绝对地址。`pos`通常是一个当前`teelg`或`tellp`返回的值。 |
| `seekp(off, from)`，`seekg(off, from)` | 在一个输入流或输出流中将标记定位到`from`之前或之后`off`个字符，`from`可以是下列值之一：`beg`，偏移量相对于流开始位置；`cur`，偏移量相对于流当前位置；`end`，偏移量相对于流结尾位置。 |



# 第十八章 用于大型程序的工具

大规模应用程序的特殊要求包括：

- 在独立开发的子系统之间协同处理错误的能力。
- 使用各种库进行协同开发的能力。
- 对比较复杂的应用概念建模的能力。

## 异常处理

**异常处理**（exception handling）机制允许程序中独立开发的部分能够在运行时就出现的问题进行通信并作出相应的处理。

### 抛出异常

在C++语言中，我们通过**抛出**（throwing）一条表达式来**引发**（raised）一个异常。异常类型和当前的调用链决定了哪段**处理代码**（handler）将用来处理该异常。

程序的控制权从`throw`转移到`catch`模块。

**栈展开**：当`throw`出现在一个`try语句块`时，检查该`try语句块`相关的`catch`字句，若有匹配则处理；若无匹配，则继续检查外层的`try`匹配的`catch`。

若一个异常没有被捕获，则它将终止当前的程序。

对象销毁：

- 块退出后，它的局部对象将被销毁。
- 若异常发生在构造函数中，即使某个对象只构造了一部分，也要确保已构造的成员正确地被销毁。
- 将资源释放放在类的析构函数中，以保证资源能被正确释放。析构函数本身不会引发异常。

### 捕获异常

若无需访问抛出的异常对象，则可以忽略捕获形参的名字。

通常，若`catch`接受的异常与某个继承体系有关，则最好将该`catch`的参数定义成引用类型。

搜索`catch`未必是最佳匹配，而是第一个匹配，因此，越细化的`catch`越应该放在`catch`列表前段。

重新抛出：`catch`代码执行一条`throw;`将异常传递给另一个`catch`语句。

捕获所有异常：`catch(...)`

### 构造函数

处理构造函数初始值异常的唯一方法是将构造函数协程函数`try`语句块。

示例：

```cpp
template <typename T>
Blob<T>::Blob(std::initializer_list<T> il) try: 
    data(std::make_shared<std::vector<T> >(il){
        /*函数体*/
    } catch(const std::bad_alloc &e){ handle_out_of_memory(e); }
```

### noexcept异常说明

使用`noexcept`说明指定某个函数不会抛出异常。

示例：

```cpp
void recoup(int) noexcept; //C++11
coid recoup(int) throw(); //老版本
```

### 异常类层次

标准exception层次：

- exception
  - bad_cast
  - bad_alloc
  - runtime_error
    - overflow_error
    - underflow_error
    - range_error
  - logic_error
    - domain_error
    - invalid_argument
    - out_of_range
    - length_error

自定义异常类：

示例：

```cpp
class out_of_stock: public std::runtime_error {
    explicit out_of_stock(const std::string &s):
    std::runtime_error(s){ }
};
```

## 命名空间

多个库将名字放置在全局命名空间中将引发**命名空间污染**（namespace pollution）。**命名空间**（namespace）分割了全局命名空间，其中每个命名空间是一个作用域。

### 命名空间定义

命名空间的定义包含两部分：1.关键字`namespace`；2.命名空间名称。后面是一系列由花括号括起来的声明和定义。命名空间作用域后面无需分号。

示例：

```cpp
namespace cplusplus_primer{
    
}
```

每个命名空间都是一个**作用域**。定义在某个命名空间内的名字可以被该命名空间内的其他成员直接访问，也可以被这些成员内嵌套作用域中的任何单位访问。位于该命名空间之外的代码必须明确指出所用的名字是属于哪个命名空间的。

命名空间可以是**不连续**的。这点不同于其他作用域，意味着同一命名空间可以在多处出现。

**内联命名空间**（C++11）：

无需使用该命名空间的前缀，通过外层命名空间就可以直接访问。

示例：

```cpp
namespace cplusplus_primer{
    inline namespace FifthEd{
        // 表示本书第5版代码
        class Query_base {};
    }
}

cplusplus_primer::Query_base qb;
```

**未命名的命名空间**：

指关键字`namespace`后面紧跟花括号的用法。未命名的命名空间中定义的变量拥有静态的声明周期：在第一次使用前创建，直到程序结束才销毁。不能跨越多个文件。

### 使用命名空间成员

像`namespace_name::member_name`这样使用命名空间的成员非常繁琐。

**命名空间的别名**：

```cpp
namespace primer = cplusplus_primer;
```

**using声明**（using declaration）：

一条`using`声明语句一次只引入命名空间的一个成员。

```cpp
using std::string;

string s = "hello";
```

**using指示**（using directive）：

使得某个特定的命名空间中所有的名字都可见。

```cpp
using namespace std;

string s = "hello";
```

### 类、命名空间与作用域

```cpp
namespace A{    class C1{        public:            int f3();    }}A::C1::f3
```

### 重载与命名空间

`using`声明语句声明的是一个名字，而非特定的函数，也就是包括该函数的所有版本，都被引入到当前作用域中。

## 多重继承与虚继承

### 多重继承

### 类型转换与多个基类

### 多重继承下的类作用域

* 当一个类拥有多个基类时，有可能出现派生类从两个或更多基类中继承了同名成员的情况。此时，不加前缀限定符直接使用该名字将引发二义性。

### 虚继承

* 虚继承的目的是令某个类做出声明，承诺愿意共享它的基类。其中，共享的基类子对象成为**虚基类**。在这种机制下，不论虚基类在继承体系中出现了多少次，在派生类中都只包含唯一一个共享的虚基类子对象。
* 虚派生只影响从指定了虚基类的派生类中进一步派生出的类，它不会影响派生类本身。

### 构造函数与虚继承

* h含有虚基类的对象的构造顺序与一般的顺序稍有**区别**：首先使用提供给最底层派生类构造函数的初始值初始化该对象的虚基类子部分，接下来按照直接基类在派生列表中出现的次序对其进行初始化。
* 虚基类总是先于非虚基类构造，与它们在继承体系中的次序和位置无关。



# 第十九章 特殊工具与技术

## 控制内存分配

### 重载new和delete

* **`new`表达式的工作机理**：

```c++
string *sp = new string("a value"); //分配并初始化一个string对象
string *arr = new string[10];   // 分配10个默认初始化的string对象
```

* 上述代码实际执行了**三步操作**：
  * `new`表达式调用一个名为`operator new`(或`operator new []`)的标准库函数，它分配一块**足够大的**、**原始的**、**未命名的**内存空间以便存储特定类型的对象(或对象的数组)。
  * 编译器运行相应的构造函数以构造这些对象，并为其传入初始值。
  * 对象被分配了空间并构造完成，返回一个指向该对象的指针。

* **`delete`表达式的工作机理**：

```c++
delete sp;  // 销毁*sp，然后释放sp指向的内存空间
delete [] arr;  // 销毁数组中的元素，然后释放对应的内存空间
```

* 上述代码实际执行了**两步操作**：
  * 对`sp`所指向的对象或者`arr`所指的数组中的元素执行对应的析构函数。
  * 编译器调用名为`operator delete`(或`operator delete[]`)的标准库函数释放内存空间。
* 当自定义了全局的`operator new`函数和`operator delete`函数后，我们就担负起了控制动态内存分配的职责。这两个函数**必须是正确的**。因为它们是程序整个处理过程中至关重要的一部分。
* 标准库定义了`operator new`函数和`operator delete`函数的8个重载版本：

```c++
// 这些版本可能抛出异常
void *operator new(size_t); // 分配一个对象
void *operator new[](size_t);   // 分配一个数组
void *operator delete(void*) noexcept;  // 释放一个对象
void *operator delete[](void*) noexcept;    // 释放一个数组

// 这些版本承诺不会抛出异常
void *operator new(size_t, nothrow_t&) noexcept;
void *operator new[](size_t, nothrow_t&) noexcept;
void *operator delete(void*, nothrow_t&) noexcept;
void *operator delete[](void*, nothrow_t&) noexcept;
```

* 应用程序可以自定义上面函数版本中的任意一个，前提是自定义的版本必须位于**全局作用域**或者**类作用域**中。
* **注意：** 提供新的`operator new`函数和`operator delete`函数的目的在于改变内存分配的方式，但是不管怎样，都不能改变`new`运算符和`delete`运算符的基本含义。
* 使用从C语言继承的函数`malloc`和`free`函数能实现以某种方式执行分配内存和释放内存的操作：

```c++
#include <cstdlib>

void *operator new(size_t size) {
    if(void *mem = malloc(size))
        return mme;
    else
        throw bad_alloc();
}

void operator delete(void *mem) noexcept {
    free(mem);
}
```

### 定位new表达式

* 应该使用new的定位`new(placement new)`形式传递一个地址，定位`new`的形式如下：

```c++
new (place_address) type
new (place_address) type (initializers)
new (place_address) type [size]
new (place_address) type [size] {braced initializer list}
// place_address必须是一个指针，同时在initializers中提供一个(可能为空的)以逗号分隔的初始值列表，该初始值列表将用于构造新分配的对象。
```

* 当只传入一个指针类型的实参时，定位`new`表达式构造对象但是不分配内存。
* 调用析构函数会销毁对象，但是不会释放内存。

```c++
string *sp = new string("a value"); // 分配并初始化一个string对象
sp->~string();
```

## 运行时类型识别

* 运行时类型识别`(run-time type identification, RTTI)`的功能由两个运算符实现：
  * `typeid`运算符， 用于返回表达式的类型。
  * `dynamic_cast`运算符，用于将基类的指针或引用安全地转换曾派生类的指针或引用。
* 使用`RTTI`必须要加倍小心。在可能的情况下，最好定义虚函数而非直接接管类型管理的重任。

### dynamic_cast运算符

* dynamic_cast运算符的使用形式如下：

```c++
dynamic_cast<type*>(e)  // e必须是一个有效的指针
dynamic_cast<type&>(e)  // e必须是一个左值
dynamic_cast<type&&>(e) // e不能是左值
// 以上，type类型必须时一个类类型，并且通常情况下该类型应该含有虚函数。
// e的类型必须符合三个条件中的任意一个，它们是：
// 1. e的类型是目标type的公有派生类；
// 2. e的类型是目标type的共有基类；
// 3. e的类型就是目标type的类型；

// 指针类型的dynamic_cast
// 假设Base类至少含有一个虚函数，Derived是Base的共有派生类。
if (Derived *dp = dynamic_cast<Derived*>(bp)) {
    // 使用dp指向的Derived对象
} else {    // bp指向一个Base对象
    // 使用dp指向的Base对象
}

// 引用类型的dynamic_cast
void f(const Base &b) {
    try {
        const Derived &d = dynamic_cast<const Derived&>(b);
        // 使用b引用的Derived对象
    } catch (bad_cast) {
        // 处理类型转换失败的情况
    }
}
```

* 可以对一个空指针执行`dynamic_cast`，结果是所需类型的空指针。

### typeid运算符

* `typeid运算符(typeid operator)`，它允许程序向表达式提问：**你的对象是什么类型？**
* `typeid`表达式的形式是`typeid(e)`，其中`e`可以是任意表达式或类型的名字，它操作的结果是一个常量对象的引用。它可以作用于任意类型的表达式。
* 通常情况下，使用typeid比较两条表达式的类型是否相同，或者比较一条表达式的类型是否与指定类型相同：

```c++
Derived *dp = new Derived;
Base *bp = dp;

if (typeid(*bp) == typeid(*dp)) {
    // bp和dp指向同一类型的对象
}

if (typeid(*bp) == typeid(Derived)) {
    // bp实际指向Derived对象
}
```

* 当typeid作用于指针时(而非指针所指向的对象)，返回的结果是该指针的静态编译时类型。

```c++
// 下面的检查永远是失败的：bp的类型是指向Base的指针if (typeid(bp) == typeid(Derived)) {    // 永远不会执行}
```

### 使用RTTI

* 用途：为具有继承关系的类实现相等运算符时。对于两个对象来说，如果它们的类型相同并且对应的数据成员取值相同，则说这两个对象是相等的。

```c++
// 类的层次关系class Base {    friend bool operator==(const Base&, const Base&);public:    // Base的接口成员protected:    virtual bool equal(const Base&) const;    // Base的数据成员和其他用于实现的成员};class Derived: public Base {public:    // Derived的其他接口成员protected:    bool equal(const Base&) const;    // Derived的数据成员和其他用于实现的成员};// 类型敏感的相等运算符bool operator==(const Base &lhs, const Base &rhs) {    // 如果typeid不相同，返回false；否则虚调用equal    return typeid(lhs) == typeid(rhs) && lhs.equal(rhs);}// 虚equal函数bool Derived::equal(const Base &rhs) const {    auto r = dynamic_cast<const Derived&>(rhs);    // 执行比较两个Derived对象的操作并返回结果}// 基类equal函数bool Base::equal(const Base &rhs) const {    // 执行比较Base对象的操作}
```

### type_info类

## 枚举类型

* 枚举类型`(enumeration)`使我们可以将一组整型常量组织在一起。枚举属于字面值常量类型。
* **限定作用域的枚举类型(scoped enumeration)**：首先是关键字`enum class(或enum struct)`，随后是枚举类型名字以及用花括号括起来的以逗号分隔的枚举成员列表，最后是一个分号。

```c++
enum class open_modes {input, output, append};
```

* 不限定作用域的枚举类型`(unscoped enumeration)`：省略关键字`class(或struct)`，枚举类型的名字是可选的。

```c++
enum color {red, yellow, green};enum {floatPrec = 6, doublePrec = 10, double_doublePrec = 10};
```

## 类成员指针

**成员指针**：指可以指向类的非静态成员的指针。

### 数据成员指针

* 和其他指针一样，在声明成员指针时也使用*来表示当前声明的名字是一个指针。与普通指针不同的时，成员指针还必须包含成员所属的类。

```c++
// pdata可以指向一个常量(非常量)Screen对象的string成员const string Screen::*pdata;// C++11auto pdata = &Screen::contents;
```

* 当我们初始化一个成员指针或为成员指针赋值时，该指针没有指向任何数据。成员指针指定了成员而非该成员所属的对象，只有当解引用成员指针时才提供对象的信息。

```c++
Screen myScreen, *pScreen = &myScreen;auto s = myScreen.*pdata;s = pScreen->*pdata;
```

### 成员函数指针

* 因为函数调用运算符的优先级较高，所以在声明指向成员函数的指针并使用这些的指针进行函数调用时，括号必不可少：`(C::*p)(parms)`和`(obj.*p)(args)`。

### 将成员函数用作可调用对象

## 嵌套类

* 一个类可以定义在另一个类的内部，前者称为嵌套类(nested class)或嵌套类型(nested type)。**嵌套类常用于定义作为实现部分的类**。
* 嵌套类是一个独立的类，与外层类基本没有什么关系。特别是，外层类的对象和嵌套类的对象是相互独立的。
* 嵌套类的名字在外层类作用域中是可见的，在外层类作用域之外不可见。

## union：一种节省空间的类

* `联合(union)`是一种特殊的类。一个`union`可以有多个数据成员，但是在任意时刻只有一个数据成员可以有值。**它不能含有引用类型的成员和虚函数**。

```c++
// Token类型的对象只有一个成员，该成员的类型可能是下列类型中的任意一种union Token {    // 默认情况下成员是共有的    char cval;    int ival;    double dval;}；
```

* `匿名union(anonymous union)`是一个未命名的`union`，并且在右花括号和分号之间没有任何声明。

```c++
union {    char cval;    int ival;    double dval;};// 可以直接访问它的成员cal = 'c';ival = 42;
```

* **注意：** `匿名union`不能包含受保护的成员或私有成员，也不能定义成员函数。

## 局部类

* `局部类(local class)`：可以定义在某个函数的内部的类。它的类型只在定义它的作用域内可见。和嵌套类不同，局部类的成员受到严格限制。
* 局部类的所有成员(包括函数在内)都必须完整定义在类的内部。因此，局部类的作用与嵌套类相比相差很远。
* **局部类不能使用函数作用域中的变量。**

```c++
int a, val;
void foo(int val) {
    static inti si;
    enum loc { a = 1024, b};

    // Bar是foo的局部类
    struct Bar {
        Loc locVal; // 正确：使用一个局部类型名
        int barVal;

        void fooBar(Loc l = a) {    // 正确：默认实参是Loc::a
            barVal = val;   // 错误：val是foo的局部变量
            barVal == ::val;    // 正确：使用一个全局对象
            barVal = si;    // 正确：使用一个静态局部对象
            locVal = b; // 正确：使用一个枚举成员
        }
    }；
}

```

## 固有的不可移植的特性

所谓不可移植的特性是指**因机器而异的特性**，当将含有不可移植特性的程序从一台机器转移到另一台机器上时，通常需要重新编写该程序。

### 位域

* 类可以将其(非静态)数据成员定义成**位域(bit-field)**，在一个位域中含有一定数量的二进制位。当一个程序需要向其他程序或硬件设备传递二进制数据时，通常会用到位域。
* 位域在内存中的布局是与机器相关的。
* 位域的类型必须是整型或枚举类型。因为带符号位域的行为是由具体实现确定的，通常情况下我们使用无符号类型保存一个位域。

```c++
typedef unsigned int Bit;
class File {
    Bit mode: 2;
    Bit modified: 1;
    Bit prot_owner: 3;
    Bit prot_group: 3;
    Bit prot_world: 3;
public:
    enum modes {READ = 01, WRITE = 02, EXECUTE = 03};
    File &open(modes);
    void close();
    void write();
    bool isRead() const;
    void setWrite();
}

// 使用位域
void File::write() {
    modified = 1;
    // ...
}

void File::close() {
    if( modified)
        // ...保存内容
}

File &File::open(File::modes m) {
    mode |= READ;   // 按默认方式设置READ
    // 其他处理
    if(m & WRITE)   // 如果打开了READ和WRITE
        // 按照读/写方式打开文件
    return *this;
}
```

### volatile限定符

* 当对象的值可能在程序的控制或检测之外被改变时，应该将该对象声明为`volatile`。关键字`volatile`告诉编译器不应对这样的对象进行优化。
* `const`和`volatile`的一个重要区别是不能使用合成的拷贝/移动构造函数及赋值运算符初始化`volatile`对象或者从`volatile`对象赋值。

### 链接指示：extern "C"

* `C++`使用`链接指示(linkage directive)`指出任意非`C++`函数所用的语言。
* 要想把`C++`代码和其他语言(包括`C`语言)编写的代码放在一起使用，要求我们必须有权访问该语言的编译器，并且这个编译器与当前的`C++`编译器是兼容的。
* `C++`从C语言继承的标准库函数可以定义为`C`函数，但并非必须：决定使用`C`还是`C++`实现的`C`标准库，是每个`C++`实现的事情。
* 有时需要在C和C++中编译同一个源文件，为了实现这一目的，在编译C++版本的程序时预处理器定义`__cplusplus`。

```c++
#ifdef __cplusplus
extern "C"
#endif
int strcmp(const char*, const char*);
```

