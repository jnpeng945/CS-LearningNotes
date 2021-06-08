# 第八章 IO库

## 练习8.1

> 编写函数，接受一个`istream&`参数，返回值类型也是`istream&`。此函数须从给定流中读取数据，直至遇到文件结束标识时停止。它将读取的数据打印在标准输出上。完成这些操作后，在返回流之前，对流进行复位，使其处于有效状态。

解：

```cpp
std::istream& func(std::istream &is)
{
    std::string buf;
    while (is >> buf)
        std::cout << buf << std::endl;
    is.clear();
    return is;
}
```

## 练习8.2

> 测试函数，调用参数为`cin`。

解：

```cpp
#include <iostream>
using std::istream;

istream& func(istream &is)
{
    std::string buf;
    while (is >> buf)
        std::cout << buf << std::endl;
    is.clear();
    return is;
}

int main()
{
    istream& is = func(std::cin);
    std::cout << is.rdstate() << std::endl;
    return 0;
}
```

## 练习8.3

> 什么情况下，下面的`while`循环会终止？

```cpp
while (cin >> i) /*  ...    */
```

解：

如`badbit`、`failbit`、`eofbit` 的任一个被置位，那么检测流状态的条件会失败。

## 练习8.4

> 编写函数，以读模式打开一个文件，将其内容读入到一个`string`的`vector`中，将每一行作为一个独立的元素存于`vector`中。

解：

```cpp
void ReadFileToVec(const string& fileName, vector<string>& vec)
{
    ifstream ifs(fileName);
    if (ifs)
    {
        string buf;
        while (getline(ifs, buf))
            vec.push_back(buf);
    }
}
```

## 练习8.5

> 重写上面的程序，将每个单词作为一个独立的元素进行存储。
> 解：

```cpp
void ReadFileToVec(const string& fileName, vector<string>& vec)
{
    ifstream ifs(fileName);
    if (ifs)
    {
        string buf;
        while (ifs >> buf)
            vec.push_back(buf);
    }
}
```

## 练习8.6

> 重写7.1.1节的书店程序，从一个文件中读取交易记录。将文件名作为一个参数传递给`main`。

解：

```cpp
#include <fstream>
#include <iostream>

#include "../ch07/ex7_26.h"
using std::ifstream; using std::cout; using std::endl; using std::cerr;

int main(int argc, char **argv)
{
    ifstream input(argv[1]);
    
    Sales_data total;
    if (read(input, total))
    {
        Sales_data trans;
        while (read(input, trans))
        {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else
            {
                print(cout, total) << endl;
                total = trans;
            }
        }
        print(cout, total) << endl;
    }
    else
    {
        cerr << "No data?!" << endl;
    }
    
    return 0;
}
```

## 练习8.7

> 修改上一节的书店程序，将结果保存到一个文件中。将输出文件名作为第二个参数传递给`main`函数。

解：

```cpp
#include <fstream>
#include <iostream>

#include "../ch07/ex7_26.h"
using std::ifstream; using std::ofstream; using std::endl; using std::cerr;

int main(int argc, char **argv)
{
    ifstream input(argv[1]);
    ofstream output(argv[2]);
    
    Sales_data total;
    if (read(input, total))
    {
        Sales_data trans;
        while (read(input, trans))
        {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else
            {
                print(output, total) << endl;
                total = trans;
            }
        }
        print(output, total) << endl;
    }
    else
    {
        cerr << "No data?!" << endl;
    }
    
    return 0;
}
```

## 练习8.8

> 修改上一题的程序，将结果追加到给定的文件末尾。对同一个输出文件，运行程序至少两次，检验数据是否得以保留。

解：

```cpp
#include <fstream>
#include <iostream>

#include "../ch07/ex7_26.h"
using std::ifstream; using std::ofstream; using std::endl; using std::cerr;

int main(int argc, char **argv)
{
    ifstream input(argv[1]);
    ofstream output(argv[2], ofstream::app);
    
    Sales_data total;
    if (read(input, total))
    {
        Sales_data trans;
        while (read(input, trans))
        {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else
            {
                print(output, total) << endl;
                total = trans;
            }
        }
        print(output, total) << endl;
    }
    else
    {
        cerr << "No data?!" << endl;
    }
    
    return 0;
}
```


## 练习8.9

> 使用你为8.1.2节第一个练习所编写的函数打印一个`istringstream`对象的内容。

解：

```cpp
#include <iostream>#include <sstream>using std::istream;istream& func(istream &is){    std::string buf;    while (is >> buf)        std::cout << buf << std::endl;    is.clear();    return is;}int main(){    std::istringstream iss("hello");    func(iss);    return 0;}
```

## 练习8.10

> 编写程序，将来自一个文件中的行保存在一个`vector`中。然后使用一个`istringstream`从`vector`读取数据元素，每次读取一个单词。

解：

```cpp
#include <iostream>#include <fstream>#include <sstream>#include <vector>#include <string>using std::vector; using std::string; using std::ifstream; using std::istringstream; using std::cout; using std::endl; using std::cerr;int main(){    ifstream ifs("../data/book.txt");    if (!ifs)    {        cerr << "No data?" << endl;        return -1;    }        vector<string> vecLine;    string line;    while (getline(ifs, line))        vecLine.push_back(line);        for (auto &s : vecLine)    {        istringstream iss(s);        string word;        while (iss >> word)            cout << word << endl;    }        return 0;}
```

## 练习8.11

> 本节的程序在外层`while`循环中定义了`istringstream`对象。如果`record`对象定义在循环之外，你需要对程序进行怎样的修改？重写程序，将`record`的定义移到`while`循环之外，验证你设想的修改方法是否正确。

解：

```cpp
#include <iostream>#include <sstream>#include <string>#include <vector>using std::vector; using std::string; using std::cin; using std::istringstream;struct PersonInfo {    string name;    vector<string> phones;};int main(){    string line, word;    vector<PersonInfo> people;    istringstream record;    while (getline(cin, line))    {        PersonInfo info;        record.clear();        record.str(line);        record >> info.name;        while (record >> word)            info.phones.push_back(word);        people.push_back(info);    }        for (auto &p : people)    {        std::cout << p.name << " ";        for (auto &s : p.phones)            std::cout << s << " ";        std::cout << std::endl;    }        return 0;}
```

## 练习8.12

> 我们为什么没有在`PersonInfo`中使用类内初始化？

解：

因为这里只需要聚合类就够了，所以没有必要在`PersionInfo`中使用类内初始化。

## 练习8.13

> 重写本节的电话号码程序，从一个命名文件而非`cin`读取数据。

解：

```cpp
#include <iostream>#include <sstream>#include <fstream>#include <string>#include <vector>using std::vector; using std::string; using std::cin; using std::istringstream;using std::ostringstream; using std::ifstream; using std::cerr; using std::cout; using std::endl;using std::isdigit;struct PersonInfo {    string name;    vector<string> phones;};bool valid(const string& str){    return isdigit(str[0]);}string format(const string& str){    return str.substr(0,3) + "-" + str.substr(3,3) + "-" + str.substr(6);}int main(){    ifstream ifs("../data/phonenumbers.txt");    if (!ifs)    {        cerr << "no phone numbers?" << endl;        return -1;    }    string line, word;    vector<PersonInfo> people;    istringstream record;    while (getline(ifs, line))    {        PersonInfo info;        record.clear();        record.str(line);        record >> info.name;        while (record >> word)            info.phones.push_back(word);        people.push_back(info);    }    for (const auto &entry : people)    {        ostringstream formatted, badNums;        for (const auto &nums : entry.phones)            if (!valid(nums)) badNums << " " << nums;            else formatted << " " << format(nums);        if (badNums.str().empty())            cout << entry.name << " " << formatted.str() << endl;        else            cerr << "input error: " << entry.name                 << " invalid number(s) " << badNums.str() << endl;    }    return 0;}
```

## 练习8.14

> 我们为什么将`entry`和`nums`定义为`const auto&`？

解：

它们都是类类型，因此使用引用避免拷贝。
在循环当中不会改变它们的值，因此用`const`。



## 练习9.1

> 对于下面的程序任务，`vector`、`deque`和`list`哪种容器最为适合？解释你的选择的理由。如果没有哪一种容器优于其他容器，也请解释理由。

* (a) 读取固定数量的单词，将它们按字典序插入到容器中。我们将在下一章中看到，关联容器更适合这个问题。
* (b) 读取未知数量的单词，总是将单词插入到末尾。删除操作在头部进行。
* (c) 从一个文件读取未知数量的整数。将这些数排序，然后将它们打印到标准输出。

解：

* (a) `list` ，因为需要频繁的插入操作。
* (b) `deque` ，总是在头尾进行插入、删除操作。
* (c) `vector` ，不需要进行插入删除操作。

## 练习9.2

> 定义一个`list`对象，其元素类型是`int`的`deque`。

解：


```cpp
std::list<std::deque<int>> l;
```

## 练习9.3

> 构成迭代器范围的迭代器有何限制？

解：

两个迭代器 `begin` 和 `end`需满足以下条件：

* 它们指向同一个容器中的元素，或者是容器最后一个元素之后的位置。
* 我们可以通过反复递增`begin`来到达`end`。换句话说，`end` 不在`begin`之前。

## 练习9.4

> 编写函数，接受一对指向`vector<int>`的迭代器和一个`int`值。在两个迭代器指定的范围中查找给定的值，返回一个布尔值来指出是否找到。

解：

```cpp
bool find(vector<int>::const_iterator begin, vector<int>::const_iterator end, int i)
{
	while (begin++ != end)
	{
		if (*begin == i) 
			return true;
    }	
    return false;
}
```

## 练习9.5

> 重写上一题的函数，返回一个迭代器指向找到的元素。注意，程序必须处理未找到给定值的情况。

解：

```cpp
vector<int>::const_iterator find(vector<int>::const_iterator begin, vector<int>::const_iterator end, int i)
{
	while (begin != end)
	{
		if (*begin == i) 
			return begin;
		++begin;
    }	
    return end;
}
```

## 练习9.6

> 下面的程序有何错误？你应该如何修改它？

```cpp
list<int> lst1;
list<int>::iterator iter1 = lst1.begin(),
					iter2 = lst1.end();
while (iter1 < iter2) /* ... */
```

解:

修改成如下：

```cpp
while (iter1 != iter2)
```

## 练习9.7

> 为了索引`int`的`vector`中的元素，应该使用什么类型？

解:

```cpp
vector<int>::size_type
```

## 练习9.8

> 为了读取`string`的`list`中的元素，应该使用什么类型？如果写入`list`，又应该使用什么类型？

解:

```cpp
list<string>::const_iterator // 读
list<string>::iterator // 写
```

## 练习9.9

> `begin`和`cbegin`两个函数有什么不同？

解:

`begin` 返回的是普通迭代器，`cbegin` 返回的是常量迭代器。

## 练习9.10

> 下面4个对象分别是什么类型？

```cpp
vector<int> v1;
const vector<int> v2;
auto it1 = v1.begin(), it2 = v2.begin();
auto it3 = v1.cbegin(), it4 = v2.cbegin();
```

解:

`it1` 是 `vector<int>::iterator`

`it2`，`it3` 和 `it4` 是 `vector<int>::const_iterator`


## 练习9.11

> 对6种创建和初始化`vector`对象的方法，每一种都给出一个实例。解释每个`vector`包含什么值。

解：

```cpp
vector<int> vec;    // 0vector<int> vec(10);    // 10个0vector<int> vec(10, 1);  // 10个1vector<int> vec{ 1, 2, 3, 4, 5 }; // 1, 2, 3, 4, 5vector<int> vec(other_vec); // 拷贝 other_vec 的元素vector<int> vec(other_vec.begin(), other_vec.end()); // 拷贝 other_vec 的元素
```

## 练习9.12

> 对于接受一个容器创建其拷贝的构造函数，和接受两个迭代器创建拷贝的构造函数，解释它们的不同。

解：

* 接受一个容器创建其拷贝的构造函数，必须容器类型和元素类型都相同。
* 接受两个迭代器创建拷贝的构造函数，只需要元素的类型能够相互转换，容器类型和元素类型可以不同。

## 练习9.13

> 如何从一个`list<int>`初始化一个`vector<double>`？从一个`vector<int>`又该如何创建？编写代码验证你的答案。

解：

```cpp
list<int> ilst(5, 4);vector<int> ivc(5, 5);vector<double> dvc(ilst.begin(), ilst.end());vector<double> dvc2(ivc.begin(), ivc.end());
```

## 练习9.14

> 编写程序，将一个`list`中的`char *`指针元素赋值给一个`vector`中的`string`。

解：

```cpp
    std::list<const char*> l{ "hello", "world" };    std::vector<std::string> v;    v.assign(l.cbegin(), l.cend());
```

## 练习9.15

> 编写程序，判定两个`vector<int>`是否相等。

解：

```cpp
    std::vector<int> vec1{ 1, 2, 3, 4, 5 };    std::vector<int> vec2{ 1, 2, 3, 4, 5 };    std::vector<int> vec3{ 1, 2, 3, 4 };    std::cout << (vec1 == vec2 ? "true" : "false") << std::endl;    std::cout << (vec1 == vec3 ? "true" : "false") << std::endl;
```

## 练习9.16

> 重写上一题的程序，比较一个list<int>中的元素和一个vector<int>中的元素。

解：

```cpp
    std::list<int>      li{ 1, 2, 3, 4, 5 };    std::vector<int>    vec2{ 1, 2, 3, 4, 5 };    std::vector<int>    vec3{ 1, 2, 3, 4 };    std::cout << (std::vector<int>(li.begin(), li.end()) == vec2 ? "true" : "false") << std::endl;    std::cout << (std::vector<int>(li.begin(), li.end()) == vec3 ? "true" : "false") << std::endl;
```

## 练习9.17

> 假定`c1`和`c2`是两个容器，下面的比较操作有何限制？

解：

```cpp
	if (c1 < c2)
```

* `c1`和`c2`必须是相同类型的容器并且保存相同类型的元素
* 元素类型要支持关系运算符

## 练习9.18

> 编写程序，从标准输入读取`string`序列，存入一个`deque`中。编写一个循环，用迭代器打印`deque`中的元素。

解：

```cpp
#include <iostream>#include <string>#include <deque>using std::string; using std::deque; using std::cout; using std::cin; using std::endl;int main(){    deque<string> input;    for (string str; cin >> str; input.push_back(str));    for (auto iter = input.cbegin(); iter != input.cend(); ++iter)        cout << *iter << endl;    return 0;}
```

## 练习9.19

> 重写上一题的程序，用`list`替代`deque`。列出程序要做出哪些改变。

解：

只需要在声明上做出改变即可，其他都不变。

```cpp
deque<string> input; //改为list<string> input;
```

## 练习9.20

> 编写程序，从一个`list<int>`拷贝元素到两个`deque`中。值为偶数的所有元素都拷贝到一个`deque`中，而奇数值元素都拷贝到另一个`deque`中。

解：

```cpp
#include <iostream>#include <deque>#include <list>using std::deque; using std::list; using std::cout; using std::cin; using std::endl;int main(){    list<int> l{ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };    deque<int> odd, even;    for (auto i : l)        (i & 0x1 ? odd : even).push_back(i);    for (auto i : odd) cout << i << " ";    cout << endl;    for (auto i : even)cout << i << " ";    cout << endl;    return 0;}
```

## 练习9.21

> 如果我们将第308页中使用`insert`返回值将元素添加到`list`中的循环程序改写为将元素插入到`vector`中，分析循环将如何工作。

解：

一样的。如书上所说：

> 第一次调用 `insert` 会将我们刚刚读入的 `string` 插入到 `iter` 所指向的元素之前的位置。`insert` 返回的迭代器恰好指向这个新元素。我们将此迭代器赋予 `iter` 并重复循环，读取下一个单词。只要继续有单词读入，每步 while 循环就会将一个新元素插入到 `iter` 之前，并将 `iter` 改变为新加入元素的尾置。此元素为（新的）首元素。因此，每步循环将一个元素插入到 `list` 首元素之前的位置。

## 练习9.22

> 假定`iv`是一个`int`的`vector`，下面的程序存在什么错误？你将如何修改？

解：

```cpp
vector<int>::iterator iter = iv.begin(),					  mid = iv.begin() + iv.size() / 2;while (iter != mid)	if (*iter == some_val)		iv.insert(iter, 2 * some_val);
```

解：

* 循环不会结束
* 迭代器可能会失效

要改为下面这样：

```cpp
while (iter != mid){	if (*iter == some_val)	{		iter = v.insert(iter, 2 * some_val);		++iter;    }	++iter;}
```

## 练习9.23

> 在本节第一个程序中，若`c.size()` 为1，则`val`、`val2`、`val3`和`val4`的值会是什么？

解：

都会是同一个值（容器中仅有的那个）。

## 练习9.24

> 编写程序，分别使用`at`、下标运算符、`front` 和 `begin` 提取一个`vector`中的第一个元素。在一个空`vector`上测试你的程序。

解：

```cpp
#include <iostream>#include <vector>int main(){    std::vector<int> v;    std::cout << v.at(0);       // terminating with uncaught exception of type std::out_of_range    std::cout << v[0];          // Segmentation fault: 11    std::cout << v.front();     // Segmentation fault: 11    std::cout << *v.begin();    // Segmentation fault: 11    return 0;}
```

## 练习9.25

> 对于第312页中删除一个范围内的元素的程序，如果 `elem1` 与 `elem2` 相等会发生什么？如果 `elem2` 是尾后迭代器，或者 `elem1` 和 `elem2` 皆为尾后迭代器，又会发生什么？

解：

* 如果 `elem1` 和 `elem2` 相等，那么不会发生任何操作。
* `如果elem2` 是尾后迭代器，那么删除从 `elem1` 到最后的元素。
* 如果两者皆为尾后迭代器，也什么都不会发生。

## 练习9.26

> 使用下面代码定义的`ia`，将`ia`拷贝到一个`vector`和一个`list`中。是用单迭代器版本的`erase`从`list`中删除奇数元素，从`vector`中删除偶数元素。

```cpp
int ia[] = { 0, 1, 1, 2, 3, 5, 8, 13, 21, 55, 89 };
```

解：

```cpp
vector<int> vec(ia, end(ia));list<int> lst(vec.begin(), vec.end());for (auto it = lst.begin(); it != lst.end(); )	if (*it & 0x1)		it = lst.erase(it);	else 		++it;for (auto it = vec.begin(); it != vec.end(); )	if (!(*it & 0x1))		it = vec.erase(it);	else		++it;			
```

## 练习9.27

> 编写程序，查找并删除`forward_list<int>`中的奇数元素。

解：

```cpp
#include <iostream>#include <forward_list>using std::forward_list;using std::cout;auto remove_odds(forward_list<int>& flist){    auto is_odd = [] (int i) { return i & 0x1; };    flist.remove_if(is_odd);}int main(){    forward_list<int> data = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };    remove_odds(data);    for (auto i : data)         cout << i << " ";    return 0;}
```

## 练习9.28

> 编写函数，接受一个`forward_list<string>`和两个`string`共三个参数。函数应在链表中查找第一个`string`，并将第二个`string`插入到紧接着第一个`string`之后的位置。若第一个`string`未在链表中，则将第二个`string`插入到链表末尾。

```cpp
void find_and_insert(forward_list<string>& flst, const string& s1, const string& s2){	auto prev = flst.before_begin();	auto curr = flst.begin();	while (curr != flst.end())	{		if (*curr == s1)		{			flst.insert_after(curr, s2);			return;	    }	    prev = curr;	    ++curr;    }    flst.insert_after(prev, s2);}
```

## 练习9.29

> 假定`vec`包含25个元素，那么`vec.resize(100)`会做什么？如果接下来调用`vec.resize(10)`会做什么？

解：

* 将75个值为0的元素添加到`vec`的末尾
* 从`vec`的末尾删除90个元素

## 练习9.30

> 接受单个参数的`resize`版本对元素类型有什么限制（如果有的话）？

解：

元素类型必须提供一个默认构造函数。

## 练习9.31

> 第316页中删除偶数值元素并复制奇数值元素的程序不能用于`list`或`forward_list`。为什么？修改程序，使之也能用于这些类型。

解：

```cpp
iter += 2;
```

因为复合赋值语句只能用于`string`、`vector`、`deque`、`array`，所以要改为：

```cpp
++iter;++iter;
```

如果是`forward_list`的话，要增加一个首先迭代器`prev`：

```cpp
auto prev = flst.before_begin();//...curr == flst.insert_after(prev, *curr);++curr; ++curr;++prev; ++prev;
```

## 练习9.32

> 在第316页的程序中，向下面语句这样调用`insert`是否合法？如果不合法，为什么？

```cpp
iter = vi.insert(iter, *iter++);
```

解：

不合法。因为参数的求值顺序是未指定的。

## 练习9.33

> 在本节最后一个例子中，如果不将`insert`的结果赋予`begin`，将会发生什么？编写程序，去掉此赋值语句，验证你的答案。

解：

`begin`将会失效。

```cpp
#include <iostream>#include <vector>using std::cout;using std::endl;using std::vector;int main(){    vector<int> data { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };    for(auto cur = data.begin(); cur != data.end(); ++cur)        if(*cur & 0x1)            cur = data.insert(cur, *cur), ++cur;        for (auto i : data)        cout << i << " ";    return 0;}
```

## 练习9.34

> 假定`vi`是一个保存`int`的容器，其中有偶数值也有奇数值，分析下面循环的行为，然后编写程序验证你的分析是否正确。

```cpp
iter = vi.begin();while (iter != vi.end())	if (*iter % 2)		iter = vi.insert(iter, *iter);	++iter;
```

解：

循环永远不会结束。

## 练习9.35

> 解释一个`vector`的`capacity`和`size`有何区别。

解：

* `capacity`的值表明，在不重新分配内存空间的情况下，容器可以保存多少元素
* 而`size`的值是指容器已经保存的元素的数量

## 练习9.36

> 一个容器的`capacity`可能小于它的`size`吗？

解：

不可能。

## 练习9.37

> 为什么`list`或`array`没有`capacity`成员函数？

解：

因为`list`是链表，而`array`不允许改变容器大小。

## 练习9.38

> 编写程序，探究在你的标准实现中，`vector`是如何增长的。

解：

```cpp
#include <iostream>#include <string>#include <vector>using namespace std;int main(){	vector<int> v;	for (int i = 0; i < 100; i++)	{		cout << "capacity: " << v.capacity() << "  size: " << v.size() << endl;		v.push_back(i);	}	return 0;}
```


输出：

```
capacity: 0  size: 0capacity: 1  size: 1capacity: 2  size: 2capacity: 3  size: 3capacity: 4  size: 4capacity: 6  size: 5capacity: 6  size: 6capacity: 9  size: 7capacity: 9  size: 8capacity: 9  size: 9capacity: 13  size: 10capacity: 13  size: 11capacity: 13  size: 12capacity: 13  size: 13capacity: 19  size: 14capacity: 19  size: 15capacity: 19  size: 16capacity: 19  size: 17capacity: 19  size: 18capacity: 19  size: 19capacity: 28  size: 20capacity: 28  size: 21capacity: 28  size: 22capacity: 28  size: 23capacity: 28  size: 24capacity: 28  size: 25capacity: 28  size: 26capacity: 28  size: 27capacity: 28  size: 28capacity: 42  size: 29capacity: 42  size: 30capacity: 42  size: 31capacity: 42  size: 32capacity: 42  size: 33capacity: 42  size: 34capacity: 42  size: 35capacity: 42  size: 36capacity: 42  size: 37capacity: 42  size: 38capacity: 42  size: 39capacity: 42  size: 40capacity: 42  size: 41capacity: 42  size: 42capacity: 63  size: 43capacity: 63  size: 44capacity: 63  size: 45capacity: 63  size: 46capacity: 63  size: 47capacity: 63  size: 48capacity: 63  size: 49capacity: 63  size: 50capacity: 63  size: 51capacity: 63  size: 52capacity: 63  size: 53capacity: 63  size: 54capacity: 63  size: 55capacity: 63  size: 56capacity: 63  size: 57capacity: 63  size: 58capacity: 63  size: 59capacity: 63  size: 60capacity: 63  size: 61capacity: 63  size: 62capacity: 63  size: 63capacity: 94  size: 64capacity: 94  size: 65capacity: 94  size: 66capacity: 94  size: 67capacity: 94  size: 68capacity: 94  size: 69capacity: 94  size: 70capacity: 94  size: 71capacity: 94  size: 72capacity: 94  size: 73capacity: 94  size: 74capacity: 94  size: 75capacity: 94  size: 76capacity: 94  size: 77capacity: 94  size: 78capacity: 94  size: 79capacity: 94  size: 80capacity: 94  size: 81capacity: 94  size: 82capacity: 94  size: 83capacity: 94  size: 84capacity: 94  size: 85capacity: 94  size: 86capacity: 94  size: 87capacity: 94  size: 88capacity: 94  size: 89capacity: 94  size: 90capacity: 94  size: 91capacity: 94  size: 92capacity: 94  size: 93capacity: 94  size: 94capacity: 141  size: 95capacity: 141  size: 96capacity: 141  size: 97capacity: 141  size: 98capacity: 141  size: 99
```

## 练习9.39

> 解释下面程序片段做了什么：

```cpp
vector<string> svec;svec.reserve(1024);string word;while (cin >> word)	svec.push_back(word);svec.resize(svec.size() + svec.size() / 2);
```

解：

定义一个`vector`，为它分配1024个元素的空间。然后通过一个循环从标准输入中读取字符串并添加到`vector`当中。循环结束后，改变`vector`的容器大小（元素数量）为原来的1.5倍，使用元素的默认初始化值填充。如果容器的大小超过1024，`vector`也会重新分配空间以容纳新增的元素。

## 练习9.40

> 如果上一题的程序读入了256个词，在`resize`之后容器的`capacity`可能是多少？如果读入了512个、1000个、或1048个呢？

解：

* 如果读入了256个词，`capacity` 仍然是 1024
* 如果读入了512个词，`capacity` 仍然是 1024
* 如果读入了1000或1048个词，`capacity` 取决于具体实现。

## 练习9.41

> 编写程序，从一个`vector<char>`初始化一个`string`。

解：

```cpp
    vector<char> v{ 'h', 'e', 'l', 'l', 'o' };    string str(v.cbegin(), v.cend());
```

## 练习9.42

> 假定你希望每次读取一个字符存入一个`string`中，而且知道最少需要读取100个字符，应该如何提高程序的性能？

解：

使用 `reserve(100)` 函数预先分配100个元素的空间。

## 练习9.43

> 编写一个函数，接受三个`string`参数是`s`、`oldVal` 和`newVal`。使用迭代器及`insert`和`erase`函数将`s`中所有`oldVal`替换为`newVal`。测试你的程序，用它替换通用的简写形式，如，将"tho"替换为"though",将"thru"替换为"through"。

解：

```cpp
#include <iterator>#include <iostream>#include <string>#include <cstddef>using std::cout; using std::endl; using std::string;auto replace_with(string &s, string const& oldVal, string const& newVal){    for (auto cur = s.begin(); cur <= s.end() - oldVal.size(); )        if (oldVal == string{ cur, cur + oldVal.size() })            cur = s.erase(cur, cur + oldVal.size()),            cur = s.insert(cur, newVal.begin(), newVal.end()),            cur += newVal.size();        else              ++cur;}int main(){    string s{ "To drive straight thru is a foolish, tho courageous act." };    replace_with(s, "tho", "though");    replace_with(s, "thru", "through");    cout << s << endl;    return 0;}
```

## 练习9.44

> 重写上一题的函数，这次使用一个下标和`replace`。

解：

```cpp
#include <iostream>#include <string>using std::cout; using std::endl;using std::string;auto replace_with(string &s, string const& oldVal, string const& newVal){    for (size_t pos = 0; pos <= s.size() - oldVal.size();)        if (s[pos] == oldVal[0] && s.substr(pos, oldVal.size()) == oldVal)            s.replace(pos, oldVal.size(), newVal),            pos += newVal.size();        else            ++pos;}int main(){    string str{ "To drive straight thru is a foolish, tho courageous act." };    replace_with(str, "tho", "though");    replace_with(str, "thru", "through");    cout << str << endl;    return 0;}
```

## 练习9.45

> 编写一个函数，接受一个表示名字的`string`参数和两个分别表示前缀（如"Mr."或"Mrs."）和后缀（如"Jr."或"III"）的字符串。使用迭代器及`insert`和`append`函数将前缀和后缀添加到给定的名字中，将生成的新`string`返回。

解：

```cpp
#include <iostream>#include <string>using std::string;using std::cin;using std::cout;using std::endl;// Exercise 9.45auto add_pre_and_suffix(string name, string const& pre, string const& su){    name.insert(name.begin(), pre.cbegin(), pre.cend());    return name.append(su);}int main(){    string name("Wang");    cout << add_pre_and_suffix(name, "Mr.", ", Jr.") << endl;    return 0;}
```

## 练习9.46

> 重写上一题的函数，这次使用位置和长度来管理`string`，并只使用`insert`。

解：

```cpp
#include <iostream>#include <string>auto add_pre_and_suffix(std::string name, std::string const& pre, std::string const& su){    name.insert(0, pre);    name.insert(name.size(), su);    return name;}int main(){    std::string name("alan");    std::cout << add_pre_and_suffix(name, "Mr.", ",Jr.");    return 0;}
```

## 练习9.47

> 编写程序，首先查找`string`"ab2c3d7R4E6"中每个数字字符，然后查找其中每个字母字符。编写两个版本的程序，第一个要使用`find_first_of`，第二个要使用`find_first_not_of`。

解：

```cpp
#include <iostream>#include <string>using namespace std;int main(){	string numbers("0123456789");	string s("ab2c3d7R4E6");	cout << "numeric characters: ";	for (int pos = 0; (pos = s.find_first_of(numbers, pos)) != string::npos; ++pos)	{		cout << s[pos] << " ";	}	cout << "\nalphabetic characters: ";	for (int pos = 0; (pos = s.find_first_not_of(numbers, pos)) != string::npos; ++pos)	{		cout << s[pos] << " ";	}	return 0;}
```

## 练习9.48

> 假定`name`和`numbers`的定义如325页所示，`numbers.find(name)`返回什么？

解：

返回 `string::npos`

## 练习9.49

> 如果一个字母延伸到中线之上，如d或f，则称其有上出头部分（`ascender`）。如果一个字母延伸到中线之下，如p或g，则称其有下出头部分（`descender`）。编写程序，读入一个单词文件，输出最长的既不包含上出头部分，也不包含下出头部分的单词。

解：

```cpp
#include <string>#include <fstream>#include <iostream>using std::string; using std::cout; using std::endl; using std::ifstream;int main(){    ifstream ifs("../data/letter.txt");    if (!ifs) return -1;    string longest;    auto update_with = [&longest](string const& curr)    {        if (string::npos == curr.find_first_not_of("aceimnorsuvwxz"))            longest = longest.size() < curr.size() ? curr : longest;    };    for (string curr; ifs >> curr; update_with(curr));    cout << longest << endl;    return 0;}
```

## 练习9.50

> 编写程序处理一个`vector<string>`，其元素都表示整型值。计算`vector`中所有元素之和。修改程序，使之计算表示浮点值的`string`之和。

解：

```cpp
#include <iostream>#include <string>#include <vector>auto sum_for_int(std::vector<std::string> const& v){    int sum = 0;    for (auto const& s : v)        sum += std::stoi(s);    return sum;}auto sum_for_float(std::vector<std::string> const& v){    float sum = 0.0;    for (auto const& s : v)        sum += std::stof(s);    return sum;}int main(){    std::vector<std::string> v = { "1", "2", "3", "4.5" };    std::cout << sum_for_int(v) << std::endl;    std::cout << sum_for_float(v) << std::endl;    return 0;}
```

## 练习9.51

> 设计一个类，它有三个`unsigned`成员，分别表示年、月和日。为其编写构造函数，接受一个表示日期的`string`参数。你的构造函数应该能处理不同的数据格式，如January 1,1900、1/1/1990、Jan 1 1900 等。

解：

```cpp
#include <iostream>#include <string>#include <vector>using namespace std;class My_date{private:    unsigned year, month, day;public:    My_date(const string &s){        unsigned tag;        unsigned format;        format = tag = 0;        // 1/1/1900        if(s.find_first_of("/")!= string :: npos)        {            format = 0x01;        }        // January 1, 1900 or Jan 1, 1900        if((s.find_first_of(',') >= 4) && s.find_first_of(',')!= string :: npos){            format = 0x10;        }        else{ // Jan 1 1900            if(s.find_first_of(' ') >= 3                && s.find_first_of(' ')!= string :: npos){                format = 0x10;                tag = 1;            }        }        switch(format){        case 0x01:            day = stoi(s.substr(0, s.find_first_of("/")));            month = stoi(s.substr(s.find_first_of("/") + 1, s.find_last_of("/")- s.find_first_of("/")));            year = stoi(s.substr(s.find_last_of("/") + 1, 4));        break;        case 0x10:            if( s.find("Jan") < s.size() )  month = 1;            if( s.find("Feb") < s.size() )  month = 2;            if( s.find("Mar") < s.size() )  month = 3;            if( s.find("Apr") < s.size() )  month = 4;            if( s.find("May") < s.size() )  month = 5;            if( s.find("Jun") < s.size() )  month = 6;            if( s.find("Jul") < s.size() )  month = 7;            if( s.find("Aug") < s.size() )  month = 8;            if( s.find("Sep") < s.size() )  month = 9;            if( s.find("Oct") < s.size() )  month =10;            if( s.find("Nov") < s.size() )  month =11;            if( s.find("Dec") < s.size() )  month =12;            char chr = ',';            if(tag == 1){                chr = ' ';            }            day = stoi(s.substr(s.find_first_of("123456789"), s.find_first_of(chr) - s.find_first_of("123456789")));            year = stoi(s.substr(s.find_last_of(' ') + 1, 4));            break;        }    }    void print(){        cout << "day:" << day << " " << "month: " << month << " " << "year: " << year;    }};int main(){    My_date d("Jan 1 1900");    d.print();    return 0;}
```

## 练习9.52

> 使用`stack`处理括号化的表达式。当你看到一个左括号，将其记录下来。当你在一个左括号之后看到一个右括号，从`stack`中`pop`对象，直至遇到左括号，将左括号也一起弹出栈。然后将一个值（括号内的运算结果）`push`到栈中，表示一个括号化的（子）表达式已经处理完毕，被其运算结果所替代。

解：

这道题可以延伸为逆波兰求值，以及中缀转后缀表达式。

```cpp
#include <stack>
#include <string>
#include <iostream>

using std::string; using std::cout; using std::endl; using std::stack;

int main()
{
    string expression{ "This is (pezy)." };
    bool bSeen = false;
    stack<char> stk;
    for (const auto &s : expression)
    {
        if (s == '(') { bSeen = true; continue; }
        else if (s == ')') bSeen = false;
        
        if (bSeen) stk.push(s);
    }
    
    string repstr;
    while (!stk.empty())
    {
        repstr += stk.top();
        stk.pop();
    }
    
    expression.replace(expression.find("(")+1, repstr.size(), repstr);
    
    cout << expression << endl;
    
    return 0;
}
```



# 第十章 泛型算法

## 练习10.1

> 头文件`algorithm`中定义了一个名为`count`的函数，它类似`find`， 接受一对迭代器和一个值作为参数。`count`返回给定值在序列中出现的次数。编写程序，读取`int`序列存入`vector`中，打印有多少个元素的值等于给定值。

解：

见下题

## 练习10.2

> 重做上一题，但读取 `string` 序列存入 `list` 中。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <list>


int main()
{
    // 10.1
    std::vector<int> v = { 1, 2, 3, 4, 5, 6, 6, 6, 2 };
    std::cout << "ex 10.01: " << std::count(v.cbegin(), v.cend(), 6) << std::endl;

    // 10.2
    std::list<std::string> l = { "aa", "aaa", "aa", "cc" };
    std::cout << "ex 10.02: " << std::count(l.cbegin(), l.cend(), "aa") << std::endl;

    return 0;
}
```

## 练习10.3

> 用 `accumulate`求一个 `vector<int>` 中元素之和。

解：

见下题。

## 练习10.4

> 假定 `v` 是一个`vector<double>`，那么调用 `accumulate(v.cbegin(),v.cend(),0)` 有何错误（如果存在的话）？

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <numeric>

int main()
{
    // Exercise 10.3
    std::vector<int> v = { 1, 2, 3, 4 };
    std::cout << "ex 10.03: " << std::accumulate(v.cbegin(), v.cend(), 0) << std::endl;

    // Exercise 10.4
    std::vector<double> vd = { 1.1, 0.5, 3.3 };
    std::cout   << "ex 10.04: "
                << std::accumulate(vd.cbegin(), vd.cend(), 0)
                << std::endl;                        //   ^<-- note here.
    // @attention
    //
    // The ouput is 4 rather than 4.9 as expected.
    // The reason is std::accumulate is a template function. The third parameter is _Tp __init
    // When "0" , an integer, had been specified here, the compiler deduced _Tp as
    // interger.As a result , when the following statments were being excuted :
    //  for (; __first != __last; ++__first)
    //	__init = __init + *__first;
    //  return __init;
    // all calculation would be converted to integer.

    return 0;
}
```

结果会是 `int` 类型。

## 练习10.5

> 在本节对名册（`roster`）调用`equal`的例子中，如果两个名册中保存的都是C风格字符串而不是`string`，会发生什么？

C风格字符串是用指向字符的指针表示的，因此会比较两个指针的值（地址），而不会比较这两个字符串的内容。

## 练习10.6

> 编写程序，使用 `fill_n` 将一个序列中的 `int` 值都设置为0。

解：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using std::vector; using std::cout; using std::endl; using std::fill_n;

int main()
{
    vector<int> vec{ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
    fill_n(vec.begin(), vec.size(), 0);

    for (auto i : vec)
        cout << i << " ";
    cout << endl;
}
```

## 练习10.7

> 下面程序是否有错误？如果有，请改正：

```cpp
(a) vector<int> vec; list<int> lst; int i;
	while (cin >> i)
		lst.push_back(i);
	copy(lst.cbegin(), lst.cend(), vec.begin());
(b) vector<int> vec;
	vec.reserve(10);
	fill_n(vec.begin(), 10, 0);
```

解：

* (a) 应该加一条语句 `vec.resize(lst.size())` 。`copy`时必须保证目标目的序列至少要包含与输入序列一样多的元素。
* (b) 从语句上来说没错误，这段代码没有任何结果。但是从逻辑上来说，应该将 `vec.reserve(10)` 改为 `vec.resize(10)` 。

## 练习10.8

> 本节提到过，标准库算法不会改变它们所操作的容器的大小。为什么使用 `back_inserter`不会使这一断言失效？

`back_inserter` 是插入迭代器，在 `iterator.h` 头文件中，不是标准库的算法。

## 练习10.9

> 实现你自己的`elimDups`。分别在读取输入后、调用`unique`后以及调用`erase`后打印`vector`的内容。

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

// print containers like vector, deque, list, etc.
template<typename Sequence>
auto println(Sequence const& seq) -> std::ostream&
{
    for (auto const& elem : seq) 
        std::cout << elem << " ";
    return std::cout << std::endl;
}

auto eliminate_duplicates(std::vector<std::string> &vs) -> std::vector<std::string>&
{
    std::sort(vs.begin(), vs.end());
    println(vs);

    auto new_end = std::unique(vs.begin(), vs.end());
    println(vs);

    vs.erase(new_end, vs.end());
    return vs;
}

int main()
{
    std::vector<std::string> vs{ "a", "v", "a", "s", "v", "a", "a" };
    println(vs);
    println(eliminate_duplicates(vs));

    return 0;
}
```

## 练习10.10

> 你认为算法不改变容器大小的原因是什么？

解：

* 将算法和容器的成员函数区分开。
* 算法的参数是迭代器，不是容器本身。

## 练习10.11

> 编写程序，使用 `stable_sort` 和 `isShorter` 将传递给你的 `elimDups` 版本的 `vector` 排序。打印 `vector`的内容，验证你的程序的正确性。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <numeric>
#include <list>

// print a container like vector, deque, list, etc.
template<typename Sequence>
inline std::ostream& println(Sequence const& seq)
{
    for(auto const& elem : seq) std::cout << elem << " ";
    std::cout << std::endl;

    return std::cout;
}


inline bool
is_shorter(std::string const& lhs, std::string const& rhs)
{
    return  lhs.size() < rhs.size();
}


void elimdups(std::vector<std::string> &vs)
{
    std::sort(vs.begin(), vs.end());
    auto new_end = std::unique(vs.begin(), vs.end());
    vs.erase(new_end, vs.end());
}


int main()
{
    std::vector<std::string> v{
        "1234", "1234", "1234", "Hi", "alan", "wang"
    };
    elimdups(v);
    std::stable_sort(v.begin(), v.end(), is_shorter);
    std::cout << "ex10.11 :\n";
    println(v);

    return 0;
}
```

## 练习10.12

> 编写名为 `compareIsbn` 的函数，比较两个 `Sales_data` 对象的`isbn()` 成员。使用这个函数排序一个保存 `Sales_data` 对象的 `vector`。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include "../ch07/ex7_26.h"     // Sales_data class.

inline bool compareIsbn(const Sales_data &sd1, const Sales_data &sd2)
{
    return sd1.isbn().size() < sd2.isbn().size();
}

int main()
{
    Sales_data d1("aa"), d2("aaaa"), d3("aaa"), d4("z"), d5("aaaaz");
    std::vector<Sales_data> v{ d1, d2, d3, d4, d5 };

    // @note   the elements the iterators pointing to
    //         must match the parameters of the predicate.
    std::sort(v.begin(), v.end(), compareIsbn);

    for(const auto &element : v)
        std::cout << element.isbn() << " ";
    std::cout << std::endl;

    return 0;
}
```

## 练习10.13

> 标准库定义了名为 `partition` 的算法，它接受一个谓词，对容器内容进行划分，使得谓词为`true` 的值会排在容器的前半部分，而使得谓词为 `false` 的值会排在后半部分。算法返回一个迭代器，指向最后一个使谓词为 `true` 的元素之后的位置。编写函数，接受一个 `string`，返回一个 `bool` 值，指出 `string` 是否有5个或更多字符。使用此函数划分 `words`。打印出长度大于等于5的元素。

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

bool predicate(const std::string &s) 
{ 
    return s.size() >= 5; 
}

int main()
{
    auto v = std::vector<std::string>{ "a", "as", "aasss", "aaaaassaa", "aaaaaabba", "aaa" };
    auto pivot = std::partition(v.begin(), v.end(), predicate);
    
    for (auto it = v.cbegin(); it != pivot; ++it)
        std::cout << *it << " ";
    std::cout << std::endl;

    return 0;
}
```

## 练习10.14

> 编写一个`lambda`，接受两个`int`，返回它们的和。

```cpp
auto f = [](int i, int j) { return i + j; };
```

## 练习10.15

> 编写一个 `lambda` ，捕获它所在函数的 `int`，并接受一个 `int`参数。`lambda` 应该返回捕获的 `int` 和 `int` 参数的和。

解：

```cpp
int x = 10;auto f = [x](int i) { i + x; };
```

## 练习10.16

> 使用 `lambda` 编写你自己版本的 `biggies`。

解：

```cpp
#include <iostream>#include <string>#include <vector>#include <algorithm>// from ex 10.9void elimdups(std::vector<std::string> &vs){    std::sort(vs.begin(), vs.end());    auto new_end = std::unique(vs.begin(), vs.end());    vs.erase(new_end, vs.end());}void biggies(std::vector<std::string> &vs, std::size_t sz){    using std::string;    elimdups(vs);    // sort by size, but maintain alphabetical order for same size.    std::stable_sort(vs.begin(), vs.end(), [](string const& lhs, string const& rhs){        return lhs.size() < rhs.size();    });    // get an iterator to the first one whose size() is >= sz    auto wc = std::find_if(vs.begin(), vs.end(), [sz](string const& s){            return s.size() >= sz;    });            // print the biggies    std::for_each(wc, vs.end(), [](const string &s){        std::cout << s << " ";    }); }int main(){    // ex10.16    std::vector<std::string> v    {        "1234","1234","1234","hi~", "alan", "alan", "cp"    };    std::cout << "ex10.16: ";    biggies(v, 3);    std::cout << std::endl;    return 0;}
```

## 练习10.17

> 重写10.3.1节练习10.12的程序，在对`sort`的调用中使用 `lambda` 来代替函数 `compareIsbn`。

解：

```cpp
#include <iostream>#include <string>#include <vector>#include <algorithm>#include "../ch07/ex7_26.h"     // Sales_data class.int main(){    Sales_data d1("aa"), d2("aaaa"), d3("aaa"), d4("z"), d5("aaaaz");    std::vector<Sales_data> v{ d1, d2, d3, d4, d5 };    std::sort(v.begin(), v.end(), [](const Sales_data &sd1, const Sales_data &sd2){        return sd1.isbn().size() < sd2.isbn().size();    });    for(const auto &element : v)        std::cout << element.isbn() << " ";    std::cout << std::endl;    return 0;}
```

## 练习10.18

> 重写 `biggies`，用 `partition` 代替 `find_if`。我们在10.3.1节练习10.13中介绍了 `partition` 算法。

解：

见下题

## 练习10.19

> 用 `stable_partition` 重写前一题的程序，与 `stable_sort` 类似，在划分后的序列中维持原有元素的顺序。

解：

```cpp
#include <iostream>#include <string>#include <vector>#include <algorithm>// from ex 10.9void elimdups(std::vector<std::string> &vs){    std::sort(vs.begin(), vs.end());    auto new_end = std::unique(vs.begin(), vs.end());    vs.erase(new_end, vs.end());}// ex10.18void biggies_partition(std::vector<std::string> &vs, std::size_t sz){    elimdups(vs);        auto pivot = partition(vs.begin(), vs.end(), [sz](const std::string &s){        return s.size() >= sz;}    );    for(auto it = vs.cbegin(); it != pivot; ++it)        std::cout << *it << " ";}// ex10.19void biggies_stable_partition(std::vector<std::string> &vs, std::size_t sz){    elimdups(vs);        auto pivot = stable_partition(vs.begin(), vs.end(), [sz](const std::string& s){        return s.size() >= sz;    });    for(auto it = vs.cbegin(); it != pivot; ++it)        std::cout << *it << " ";}int main(){    // ex10.18    std::vector<std::string> v{        "the", "quick", "red", "fox", "jumps", "over", "the", "slow", "red", "turtle"    };        std::cout << "ex10.18: ";    std::vector<std::string> v1(v);    biggies_partition(v1, 4);    std::cout << std::endl;    // ex10.19    std::cout << "ex10.19: ";    std::vector<std::string> v2(v);    biggies_stable_partition(v2, 4);    std::cout << std::endl;    return 0;}
```

## 练习10.20

> 标准库定义了一个名为 `count_if` 的算法。类似 `find_if`，此函数接受一对迭代器，表示一个输入范围，还接受一个谓词，会对输入范围中每个元素执行。`count_if`返回一个计数值，表示谓词有多少次为真。使用`count_if`重写我们程序中统计有多少单词长度超过6的部分。

解：

```cpp
#include <iostream>#include <string>#include <vector>#include <algorithm>using std::vector;using std::count_if;using std::string;// Exercise 10.20std::size_t bigerThan6(vector<string> const& v){    return count_if(v.cbegin(), v.cend(), [](string const& s){        return s.size() > 6;    });}int main(){    // ex10.20    vector<string> v{        "alan","moophy","1234567","1234567","1234567","1234567"    };    std::cout << "ex10.20: " << bigerThan6(v) << std::endl;    // ex10.21    int i = 7;    auto check_and_decrement = [&i]() { return i > 0 ? !--i : !i; };    std::cout << "ex10.21: ";    while(!check_and_decrement())        std::cout << i << " ";    std::cout << i << std::endl;    return 0;}
```

## 练习10.21

> 编写一个 `lambda`，捕获一个局部 `int` 变量，并递减变量值，直至它变为0。一旦变量变为0，再调用`lambda`应该不再递减变量。`lambda`应该返回一个`bool`值，指出捕获的变量是否为0。

解：

```cpp
	int i = 10;	auto f = [&i]() -> bool { return (i == 0 ? true : !(i--)); };	while (!f()) cout << i << endl;
```

## 练习10.22

> 重写统计长度小于等于6的单词数量的程序，使用函数代替`lambda`。

```cpp
#include <iostream>#include <vector>#include <string>#include <algorithm>#include <functional>using std::string;using namespace std::placeholders;bool isLesserThanOrEqualTo6(const string &s, string::size_type sz){    return s.size() <= sz;}int main(){    std::vector<string> authors{ "Mooophy", "pezy", "Queequeg90", "shbling", "evan617" };    std::cout << count_if(authors.cbegin(), authors.cend(), bind(isLesserThanOrEqualTo6, _1, 6));}
```

## 练习10.23

> `bind` 接受几个参数？

假设被绑定的函数接受 `n` 个参数，那么`bind` 接受`n + 1`个参数。

## 练习10.24

> 给定一个`string`，使用 `bind` 和 `check_size` 在一个 `int` 的`vector` 中查找第一个大于`string`长度的值。

解：

```cpp
#include <iostream>#include <vector>#include <algorithm>#include <functional>using std::cout;using std::endl;using std::string;using std::vector;using std::find_if;using std::bind;using std::size_t;auto check_size(string const& str, size_t sz){    return str.size() < sz;}int main(){    vector<int> vec{ 0, 1, 2, 3, 4, 5, 6, 7 };    string str("123456");    auto result = find_if(vec.begin(), vec.end(), bind(check_size, str, std::placeholders::_1));    if (result != vec.end())        cout << *result << endl;    else        cout << "Not found" << endl;    return 0;}
```

## 练习10.25

> 在10.3.2节的练习中，编写了一个使用`partition` 的`biggies`版本。使用 `check_size` 和 `bind` 重写此函数。

解：

```cpp
#include <iostream>#include <vector>#include <string>#include <algorithm>#include <functional>using std::string; using std::vector;using namespace std::placeholders;void elimdups(vector<string> &vs){    std::sort(vs.begin(), vs.end());    vs.erase(unique(vs.begin(), vs.end()), vs.end());}bool check_size(const string &s, string::size_type sz){    return s.size() >= sz;}void biggies(vector<string> &words, vector<string>::size_type sz){    elimdups(words);    auto iter = std::stable_partition(words.begin(), words.end(), bind(check_size, _1, sz));    for_each(words.begin(), iter, [](const string &s){ std::cout << s << " "; });}int main(){    std::vector<std::string> v{        "the", "quick", "red", "fox", "jumps", "over", "the", "slow", "red", "turtle"    };    biggies(v, 4);}
```

## 练习10.26

> 解释三种插入迭代器的不同之处。

解：

* `back_inserter` 使用 `push_back` 。
* `front_inserter` 使用 `push_front` 。
* `inserter` 使用 `insert`，此函数接受第二个参数，这个参数必须是一个指向给定容器的迭代器。元素将被插入到给定迭代器所表示的元素之前。

## 练习10.27

> 除了 `unique` 之外，标准库还定义了名为 `unique_copy` 的函数，它接受第三个迭代器，表示拷贝不重复元素的目的位置。编写一个程序，使用 `unique_copy`将一个`vector`中不重复的元素拷贝到一个初始化为空的`list`中。

解：

```cpp
#include <iostream>#include <algorithm>#include <vector>#include <list>#include <iterator>int main(){    std::vector<int> vec{ 1, 1, 3, 3, 5, 5, 7, 7, 9 };    std::list<int> lst;        std::unique_copy(vec.begin(), vec.end(), back_inserter(lst));    for (auto i : lst)        std::cout << i << " ";    std::cout << std::endl;}
```

## 练习10.28

> 一个`vector` 中保存 1 到 9，将其拷贝到三个其他容器中。分别使用`inserter`、`back_inserter` 和 `front_inserter` 将元素添加到三个容器中。对每种 `inserter`，估计输出序列是怎样的，运行程序验证你的估计是否正确。

解：

```cpp
#include <iostream>#include <algorithm>#include <vector>#include <list>#include <iterator>using std::list; using std::copy; using std::cout; using std::endl;template<typename Sequence>void print(Sequence const& seq){    for (const auto& i : seq)        std::cout << i << " ";    std::cout << std::endl;}int main(){    std::vector<int> vec{ 1, 2, 3, 4, 5, 6, 7, 8, 9 };        // uses inserter    list<int> lst1;    copy(vec.cbegin(), vec.cend(), inserter(lst1, lst1.begin()));    print(lst1);        // uses back_inserter    list<int> lit2;    copy(vec.cbegin(), vec.cend(), back_inserter(lit2));    print(lit2);        // uses front_inserter    list<int> lst3;    copy(vec.cbegin(), vec.cend(), front_inserter(lst3));    print(lst3);}
```

前两种为正序，第三种为逆序，输出和预计一样。

## 练习10.29

> 编写程序，使用流迭代器读取一个文本文件，存入一个`vector`中的`string`里。

解：

```cpp
#include <iostream>#include <fstream>#include <vector>#include <string>#include <iterator>using std::string;int main(){    std::ifstream ifs("../data/book.txt");    std::istream_iterator<string> in(ifs), eof;    std::vector<string> vec;    std::copy(in, eof, back_inserter(vec));        // output    std::copy(vec.cbegin(), vec.cend(), std::ostream_iterator<string>(std::cout, "\n"));}
```

## 练习10.30

> 使用流迭代器、`sort` 和 `copy` 从标准输入读取一个整数序列，将其排序，并将结果写到标准输出。

解：

```cpp
#include <iostream>#include <vector>#include <algorithm>#include <iterator>using namespace std;int main(){	vector<int> v;	istream_iterator<int> int_it(cin), int_eof;	copy(int_it, int_eof, back_inserter(v));	sort(v.begin(), v.end());	copy(v.begin(), v.end(), ostream_iterator<int>(cout," "));	cout << endl;	return 0;}
```

## 练习10.31

> 修改前一题的程序，使其只打印不重复的元素。你的程序应该使用 `unique_copy`。

解：

```cpp
#include <iostream>#include <vector>#include <algorithm>#include <iterator>using namespace std;int main(){	vector<int> v;	istream_iterator<int> int_it(cin), int_eof;		copy(int_it, int_eof, back_inserter(v));	sort(v.begin(), v.end());	unique(v.begin(), v.end());	copy(v.begin(), v.end(), ostream_iterator<int>(cout, " "));	cout << endl;	return 0;}
```

## 练习10.32

> 重写1.6节中的书店程序，使用一个`vector`保存交易记录，使用不同算法完成处理。使用 `sort` 和10.3.1节中的 `compareIsbn` 函数来排序交易记录，然后使用 `find` 和 `accumulate` 求和。

解：

```cpp
#include <iostream>#include <vector>#include <algorithm>#include <iterator>#include <numeric>#include "../include/Sales_item.h"int main(){    std::istream_iterator<Sales_item> in_iter(std::cin), in_eof;    std::vector<Sales_item> vec;        while (in_iter != in_eof)        vec.push_back(*in_iter++);    sort(vec.begin(), vec.end(), compareIsbn);    for (auto beg = vec.cbegin(), end = beg; beg != vec.cend(); beg = end) {        end = find_if(beg, vec.cend(), [beg](const Sales_item &item){ return item.isbn() != beg->isbn(); });        std::cout << std::accumulate(beg, end, Sales_item(beg->isbn())) << std::endl;    }}
```

## 练习10.33

> 编写程序，接受三个参数：一个输入文件和两个输出文件的文件名。输入文件保存的应该是整数。使用 `istream_iterator` 读取输入文件。使用 `ostream_iterator` 将奇数写入第一个输入文件，每个值后面都跟一个空格。将偶数写入第二个输出文件，每个值都独占一行。

解：

```cpp
#include <fstream>#include <iterator>#include <algorithm>int main(int argc, char **argv){	if (argc != 4) return -1;	std::ifstream ifs(argv[1]);	std::ofstream ofs_odd(argv[2]), ofs_even(argv[3]);	std::istream_iterator<int> in(ifs), in_eof;	std::ostream_iterator<int> out_odd(ofs_odd, " "), out_even(ofs_even, "\n");	std::for_each(in, in_eof, [&out_odd, &out_even](const int i)	{		*(i & 0x1 ? out_odd : out_even)++ = i;	});	return 0;}
```

## 练习10.34

> 使用 `reverse_iterator` 逆序打印一个`vector`。

解：

```cpp
#include <iostream>#include <vector>using namespace std;int main(){	vector<int> v = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };	for (auto it = v.crbegin(); it != v.crend(); ++it)	{		cout << *it << endl;	}	return 0;}
```

## 练习10.35

> 使用普通迭代器逆序打印一个`vector`。

解：

```cpp
#include <iostream>#include <vector>using namespace std;int main(){	vector<int> v = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };	for (auto it = std::prev(v.cend()); true; --it)	{		std::cout << *it << " ";		if (it == v.cbegin()) break;	}	std::cout << std::endl;	return 0;}
```

## 练习10.36

> 使用 `find` 在一个 `int` 的`list` 中查找最后一个值为0的元素。

解：

```cpp
#include <iostream>#include <list>#include <algorithm>using namespace std;int main(){	list<int> l = { 1, 2, 0, 4, 5, 6, 7, 0, 9 };	auto it = find(l.crbegin(), l.crend(), 0);	cout << distance(it, l.crend()) << endl;	return 0;}
```

## 练习10.37

> 给定一个包含10 个元素的`vector`，将位置3到7之间的元素按逆序拷贝到一个`list`中。

解：

```cpp
#include <iostream>#include <list>#include <algorithm>#include <vector>#include <iterator>using namespace std;int main(){	vector<int> v = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };	list<int> l;	copy(v.crbegin() + 3, v.crbegin() + 8, back_inserter(l));	for (auto i : l) std::cout << i << " ";	cout << endl;	return 0;}
```

## 练习10.38

> 列出5个迭代器类别，以及每类迭代器所支持的操作。

* 输入迭代器 : `==`,`!=`,`++`,`*`,`->`
* 输出迭代器 : `++`,`*`
* 前向迭代器 : `==`,`!=`,`++`,`*`,`->`
* 双向迭代器 : `==`,`!=`,`++`,`--`,`*`,`->`
* 随机访问迭代器 : `==`,`!=`,`<`,`<=`,`>`,`>=`,`++`,`--`,`+`,`+=`,`-`,`-=`,`*`,`->`,`iter[n]`==`*(iter[n])`

## 练习10.39

> `list` 上的迭代器属于哪类？`vector`呢？

* `list` 上的迭代器是 **双向迭代器**
* `vector` 上的迭代器是 **随机访问迭代器**

## 练习10.40

> 你认为 `copy` 要求哪类迭代器？`reverse` 和 `unique` 呢？

* `copy` 需要两个**输入迭代器**，一个**输出迭代器**
* `reverse` 需要**双向迭代器**
* `unique`需要**随机访问迭代器**

## 练习10.41

> 仅根据算法和参数的名字，描述下面每个标准库算法执行什么操作：

```cpp
replace(beg, end, old_val, new_val);replace_if(beg, end, pred, new_val);replace_copy(beg, end, dest, old_val, new_val);replace_copy_if(beg, end, dest, pred, new_val);
```

解：

* `replace` 在迭代器范围内用新值替换所有原来的旧值。
* `replace_if` 在迭代器范围内，满足谓词条件的元素用新值替换。
* `replace_copy` 复制迭代器范围内的元素到目标迭代器位置，如果元素等于某个旧值，则用新值替换
* `replace_copy_if` 复制迭代器范围内的元素到目标迭代器位置，满足谓词条件的元素用新值替换

## 练习10.42

> 使用 `list` 代替 `vector` 重新实现10.2.3节中的去除重复单词的程序。

解：

```cpp
#include <iostream>
#include <string>
#include <list>

using std::string; using std::list;

void elimDups(list<string> &words)
{
	words.sort();
	words.unique();
}

int main()
{
	list<string> l = { "aa", "aa", "aa", "aa", "aasss", "aa" };
	elimDups(l);
	for (const auto& e : l)
		std::cout << e << " ";
	std::cout << std::endl;
}
```



# 第十一章 关联容器

## 练习11.1

> 描述`map`和`vector`的不同。

解：

`map` 是关联容器， `vector` 是顺序容器。

## 练习11.2

> 分别给出最适合使用`list`、`vector`、`deque`、`map`以及`set`的例子。

解：

* `list`：双向链表，适合频繁插入删除元素的场景。
* `vector`：适合频繁访问元素的场景。
* `deque`：双端队列，适合频繁在头尾插入删除元素的场景。
* `map`：字典。
* `set`：适合有序不重复的元素的场景。

## 练习11.3

> 编写你自己的单词计数程序。

解：

```cpp
#include <string>
#include <map>
#include <iostream>

using namespace std;

int main(){
    map<string, int> word_count;
    string tmp;
    while (cin >> tmp){
        word_count[tmp] += 1;
    }
    for (const auto& elem : word_count)
		std::cout << elem.first << " : " << elem.second << endl;
	return 0;
}
```

## 练习11.4

> 扩展你的程序，忽略大小写和标点。例如，"example."、"example,"和"Example"应该递增相同的计数器。

解：

```cpp
#include <iostream>
#include <map>
#include <string>
#include <algorithm>
#include <cctype>

void word_count_pro(std::map<std::string, int>& m)
{
	std::string word;
	while (std::cin >> word)
	{
		for (auto& ch : word) 
			ch = tolower(ch);
		
		word.erase(std::remove_if(word.begin(), word.end(), ispunct),
			word.end());
		++m[word];
	}
	for (const auto& e : m) std::cout << e.first << " : " << e.second << "\n";
}

int main()
{
	std::map<std::string, int> m;
	word_count_pro(m);

	return 0;
}
```

## 练习11.5

> 解释`map`和`set`的区别。你如何选择使用哪个？

解：

`map` 是键值对，而 `set` 只有键没有值。当我需要存储键值对的时候使用 `map`，而只需要键的时候使用 `set`。

## 练习11.6

> 解释`set`和`list`的区别。你如何选择使用哪个？

`set` 是有序不重复集合，底层实现是红黑树，而 `list` 是无序可重复集合，底层实现是链表。

## 练习11.7

> 定义一个`map`，关键字是家庭的姓，值是一个`vector`，保存家中孩子（们）的名。编写代码，实现添加新的家庭以及向已有家庭中添加新的孩子。

解：

```cpp
map<string, vector<string>> m;
for (string ln; cout << "Last name:\n", cin >> ln && ln != "@q";)
	for (string cn; cout << "|-Children's names:\n", cin >> cn && cn != "@q";)
		m[ln].push_back(cn);
```

## 练习11.8

> 编写一个程序，在一个`vector`而不是一个`set`中保存不重复的单词。使用`set`的优点是什么？

解：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

int main()
{
	std::vector<std::string> exclude = { "aa", "bb", "cc", "dd", "ee", "ff" };
	for (std::string word; std::cout << "Enter plz:\n", std::cin >> word;)
	{
		auto is_excluded = std::binary_search(exclude.cbegin(), exclude.cend(), word);
		auto reply = is_excluded ? "excluded" : "not excluded";
		std::cout << reply << std::endl;
	}
	return 0;
}
```

`set`的优点是集合本身的元素就是不重复。

## 练习11.9

> 定义一个`map`，将单词与一个行号的`list`关联，`list`中保存的是单词所出现的行号。

解：

```cpp
std::map<std::string, std::list<std::size_t>> m;
```

## 练习11.10

> 可以定义一个`vector<int>::iterator` 到 `int` 的`map`吗？`list<int>::iterator` 到 `int` 的`map`呢？对于两种情况，如果不能，解释为什么。

解：

可以定义 `vector<int>::iterator` 到 `int` 的`map`，但是不能定义 `list<int>::iterator` 到 `int` 的`map`。因为`map`的键必须实现 `<` 操作，`list` 的迭代器不支持比较运算。

## 练习11.11

> 不使用`decltype` 重新定义 `bookstore`。

解：

```cpp
using Less = bool (*)(Sales_data const&, Sales_data const&);
std::multiset<Sales_data, Less> bookstore(less);
```

## 练习11.12

> 编写程序，读入`string`和`int`的序列，将每个`string`和`int`存入一个`pair` 中，`pair`保存在一个`vector`中。

解：

```cpp
#include <vector>
#include <utility>
#include <string>
#include <iostream>

int main()
{
	std::vector<std::pair<std::string, int>> vec;
	std::string str;
	int i;
	while (std::cin >> str >> i)
		vec.push_back(std::pair<std::string, int>(str, i));

	for (const auto &p : vec)
		std::cout << p.first << ":" << p.second << std::endl;
}
```

## 练习11.13

> 在上一题的程序中，至少有三种创建`pair`的方法。编写此程序的三个版本，分别采用不同的方法创建`pair`。解释你认为哪种形式最易于编写和理解，为什么？

解：

```cpp
vec.push_back(std::make_pair(str, i));
vec.push_back({ str, i });
vec.push_back(std::pair<string, int>(str, i)); 
```

使用花括号的初始化器最易于理解和编写。

## 练习11.14

> 扩展你在11.2.1节练习中编写的孩子姓达到名的`map`，添加一个`pair`的`vector`，保存孩子的名和生日。

解：

```cpp
#include <iostream>#include <map>#include <string>#include <vector>using std::ostream;using std::cout;using std::cin;using std::endl;using std::string;using std::make_pair;using std::pair;using std::vector;using std::map;class Families{public:	using Child = pair<string, string>;	using Children = vector<Child>;	using Data = map<string, Children>;	void add(string const& last_name, string const& first_name, string birthday)	{		auto child = make_pair(first_name, birthday);		_data[last_name].push_back(child);	}	void print() const	{		for (auto const& pair : _data)		{			cout << pair.first << ":\n";			for (auto const& child : pair.second)				cout << child.first << " " << child.second << endl;			cout << endl;		}	}private:	Data _data;};int main(){	Families families;	auto msg = "Please enter last name, first name and birthday:\n";	for (string l, f, b; cout << msg, cin >> l >> f >> b; families.add(l, f, b));	families.print();	return 0;}
```

## 练习11.15

> 对一个`int`到`vector<int>的map`，其`mapped_type`、`key_type`和 `value_type`分别是什么？

解：

* `mapped_type` : `vector<int>`
* `key_type` : `int`
* `value_type` : `std::pair<const int,vector >`

## 练习11.16

> 使用一个`map`迭代器编写一个表达式，将一个值赋予一个元素。

解：

```cpp
std::map<int, string>::iterator it = m.begin();it->second = "hello";
```

## 练习11.17

> 假定`c`是一个`string`的`multiset`，`v` 是一个`string` 的`vector`，解释下面的调用。指出每个调用是否合法：

```cpp
copy(v.begin(), v.end(), inserter(c, c.end()));copy(v.begin(), v.end(), back_inserter(c));copy(c.begin(), c.end(), inserter(v, v.end()));copy(c.begin(), c.end(), back_inserter(v));
```

解：

第二个调用不合法，因为 `multiset` 没有 `push_back` 方法。其他调用都合法。

## 练习11.18

> 写出第382页循环中`map_it` 的类型，不要使用`auto` 或 `decltype`。

解：

```cpp
map<string, size_t>::const_iterator map_it = word_count.cbegin();
```

## 练习11.19

> 定义一个变量，通过对11.2.2节中的名为 `bookstore` 的`multiset` 调用`begin()`来初始化这个变量。写出变量的类型，不要使用`auto` 或 `decltype`。

解：

```cpp
using compareType = bool (*)(const Sales_data &lhs, const Sales_data &rhs);std::multiset<Sales_data, compareType> bookstore(compareIsbn);std::multiset<Sales_data, compareType>::iterator c_it = bookstore.begin();
```

## 练习11.20

> 重写11.1节练习的单词计数程序，使用`insert`代替下标操作。你认为哪个程序更容易编写和阅读？解释原因。

解：

```cpp
#include <iostream>#include <map>#include <string>using std::string;using std::map;using std::cin;using std::cout;int main(){	map<string, size_t> counts;	for (string word; cin >> word;)	{		auto result = counts.insert({ word, 1 });		if (!result.second)			++result.first->second;	}	for (auto const& count : counts)		cout << count.first << " " << count.second << ((count.second > 1) ? " times\n" : " time\n");	return 0;}
```

使用`insert`更容易阅读和编写。`insert`有返回值，可以明确的体现出插入操作的结果。

## 练习11.21

> 假定`word_count`是一个`string`到`size_t`的`map`，`word`是一个`string`，解释下面循环的作用：

```cpp
while (cin >> word)	++word_count.insert({word, 0}).first->second;
```

解：

这条语句等价于：

```cpp
while (cin >> word){	auto result = word_count.insert({word, 0});	++(result.first->second);}
```

若`insert`成功：先添加一个元素，然后返回一个 `pair`，`pair` 的 `first`元素是一个迭代器。这个迭代器指向刚刚添加的元素，这个元素是`pair`，然后递增`pair`的`second`成员。
若`insert`失败：递增已有指定关键字的元素的 `second`成员。

## 练习11.22

> 给定一个`map<string, vector<int>>`，对此容器的插入一个元素的`insert`版本，写出其参数类型和返回类型。

```cpp
std::pair<std::string, std::vector<int>>    // 参数类型std::pair<std::map<std::string, std::vector<int>>::iterator, bool> // 返回类型
```

## 练习11.23

> 11.2.1节练习中的`map` 以孩子的姓为关键字，保存他们的名的`vector`，用`multimap` 重写此`map`。

解：

```cpp
#include <map>#include <string>#include <iostream>using std::string;using std::multimap;using std::cin;using std::endl;int main(){    multimap<string, string> families;    for (string lname, cname; cin >> cname >> lname; families.emplace(lname, cname));    for (auto const& family : families)        std::cout << family.second << " " << family.first << endl;}
```

## 练习11.24

> 下面的程序完成什么功能？

```cpp
map<int, int> m;m[0] = 1;
```

解：

添加一个元素到`map`中，如果该键存在，则重新赋值。

## 练习11.25

> 对比下面的程序与上一题程序

```cpp
vector<int> v;v[0] = 1;
```

解：

未定义行为，`vector` 的下标越界访问。

## 练习11.26

> 可以用什么类型来对一个`map`进行下标操作？下标运算符返回的类型是什么？请给出一个具体例子——即，定义一个`map`，然后写出一个可以用来对`map`进行下标操作的类型以及下标运算符将会返会的类型。

解：

```cpp
std::map<int, std::string> m = { { 1,"ss" },{ 2,"sz" } };using KeyType = std::map<int, std::string>::key_type;	using ReturnType = std::map<int, std::string>::mapped_type;
```

## 练习11.27

> 对于什么问题你会使用`count`来解决？什么时候你又会选择`find`呢？

解：

对于允许重复关键字的容器，应该用 `count` ; 对于不允许重复关键字的容器，应该用 `find` 。

## 练习11.28

> 对一个`string`到`int`的`vector`的`map`，定义并初始化一个变量来保存在其上调用`find`所返回的结果。

解：

```cpp
map<string, vector<int>> m;map<string, vector<int>>::iterator it = m.find("key");
```

## 练习11.29

> 如果给定的关键字不在容器中，`upper_bound`、`lower_bound` 和 `equal_range` 分别会返回什么？

解：

如果给定的关键字不在容器中，则 `lower_bound`和 `upper_bound` 会返回相等的迭代器，指向一个不影响排序的关键字插入位置。而`equal_range` 会返回一个 `pair`，`pair` 中的两个迭代器都指向关键字可以插入的位置。

## 练习11.30

> 对于本节最后一个程序中的输出表达式，解释运算对象`pos.first->second`的含义。

解：

`pos` 是一个`pair`，`pos.first` 是一个迭代器，指向匹配关键字的元素，该元素是一个 `pair`，访问该元素的第二个成员。

## 练习11.31

> 编写程序，定义一个作者及其作品的`multimap`。使用`find`在`multimap`中查找一个元素并用`erase`删除它。确保你的程序在元素不在`map` 中时也能正常运行。

解：

```cpp
#include <map>#include <string>#include <iostream>using std::string;int main(){	std::multimap<string, string> authors{		{ "alan", "DMA" },		{ "pezy", "LeetCode" },		{ "alan", "CLRS" },		{ "wang", "FTP" },		{ "pezy", "CP5" },		{ "wang", "CPP-Concurrency" } };	string author = "pezy";	string work = "CP5";	auto found = authors.find(author);	auto count = authors.count(author);	while (count)	{		if (found->second == work)		{			authors.erase(found);			break;		}		++found;		--count;	}	for (const auto &author : authors)		std::cout << author.first << " " << author.second << std::endl;	return 0;}
```

## 练习11.32

> 使用上一题定义的`multimap`编写一个程序，按字典序打印作者列表和他们的作品。

解：

```cpp
#include <map>#include <set>#include <string>#include <iostream>using std::string;int main(){	std::multimap<string, string> authors{		{ "alan", "DMA" },		{ "pezy", "LeetCode" },		{ "alan", "CLRS" },		{ "wang", "FTP" },		{ "pezy", "CP5" },		{ "wang", "CPP-Concurrency" } };	std::map<string, std::multiset<string>> order_authors;	for (const auto &author : authors)		order_authors[author.first].insert(author.second);	for (const auto &author : order_authors)	{		std::cout << author.first << ": ";		for (const auto &work : author.second)			std::cout << work << " ";		std::cout << std::endl;	}	return 0;}
```

## 练习11.33

> 实现你自己版本的单词转换程序。

解：

```cpp
#include <map>#include <string>#include <fstream> #include <iostream>#include <sstream>using std::string; using std::ifstream;std::map<string, string> buildMap(ifstream &map_file){    std::map<string, string> trans_map;    for (string key, value; map_file >> key && getline(map_file, value); )        if (value.size() > 1) trans_map[key] = value.substr(1).substr(0, value.find_last_not_of(' '));    return trans_map;}const string & transform(const string &s, const std::map<string, string> &m){    auto map_it = m.find(s);    return map_it == m.cend() ? s : map_it->second;}void word_transform(ifstream &map, ifstream &input){    auto trans_map = buildMap(map);    for (string text; getline(input, text); ) {        std::istringstream iss(text);        for (string word; iss >> word; )            std::cout << transform(word, trans_map) << " ";        std::cout << std::endl;    }}int main(){    ifstream ifs_map("../data/word_transformation_bad.txt"), ifs_content("../data/given_to_transform.txt");    if (ifs_map && ifs_content) word_transform(ifs_map, ifs_content);    else std::cerr << "can't find the documents." << std::endl;}
```

## 练习11.34

> 如果你将`transform` 函数中的`find`替换为下标运算符，会发生什么情况？

解：

如果使用下标运算符，当关键字未在容器中时，会往容器中添加一个新元素。

## 练习11.35

> 在`buildMap`中，如果进行如下改写，会有什么效果？

```cpp
trans_map[key] = value.substr(1);//改为trans_map.insert({key, value.substr(1)});
```

解：

当一个转换规则的关键字多次出现的时候，使用下标运算符会保留最后一次添加的规则，而用insert则保留第一次添加的规则。

## 练习11.36

> 我们的程序并没检查输入文件的合法性。特别是，它假定转换规则文件中的规则都是有意义的。如果文件中的某一行包含一个关键字、一个空格，然后就结束了，会发生什么？预测程序的行为并进行验证，再与你的程序进行比较。

解：

如果关键字没有对应的规则，那么程序会抛出一个 `runtime_error`。

## 练习11.37

> 一个无序容器与其有序版本相比有何优势？有序版本有何优势？

无序容器拥有更好的性能，有序容器使得元素始终有序。

## 练习11.38

> 用 `unordered_map` 重写单词计数程序和单词转换程序。

解：

```cpp
#include <unordered_map>
#include <set>
#include <string>
#include <iostream>
#include <fstream>
#include <sstream>

using std::string;

void wordCounting()
{
    std::unordered_map<string, size_t> word_count;
    for (string word; std::cin >> word; ++word_count[word]);
    for (const auto &w : word_count)
        std::cout << w.first << " occurs " << w.second << (w.second > 1 ? "times" : "time") << std::endl;
}

void wordTransformation()
{
    std::ifstream ifs_map("../data/word_transformation.txt"), ifs_content("../data/given_to_transform.txt");
    if (!ifs_map || !ifs_content) {
        std::cerr << "can't find the documents." << std::endl;
        return;
    }
    
    std::unordered_map<string, string> trans_map;
    for (string key, value; ifs_map >> key && getline(ifs_map, value); )
        if (value.size() > 1) trans_map[key] = value.substr(1).substr(0, value.find_last_not_of(' '));
    
    for (string text, word; getline(ifs_content, text); std::cout << std::endl)
        for (std::istringstream iss(text); iss >> word; ) {
            auto map_it = trans_map.find(word);
            std::cout << (map_it == trans_map.cend() ? word : map_it->second) << " ";
        }
}

int main()
{
    //wordCounting();
    wordTransformation();
}
```



# 第十二章 动态内存

## 练习12.1

> 在此代码的结尾，`b1` 和 `b2` 各包含多少个元素？

```cpp
StrBlob b1;
{
	StrBlob b2 = {"a", "an", "the"};
	b1 = b2;
	b2.push_back("about");
}
```

解：

它们实际操作的是同一个`vector`，都包含4个元素。在代码的结尾，`b2` 被析构了，不影响 `b1` 的元素。

## 练习12.2

> 编写你自己的`StrBlob` 类，包含`const` 版本的 `front` 和 `back`。

解：

头文件：

```cpp
#include <vector>
#include <string>
#include <initializer_list>
#include <memory>
#include <exception>

using std::vector; using std::string;

class StrBlob {
public:
    using size_type = vector<string>::size_type;

    StrBlob():data(std::make_shared<vector<string>>()) { }
    StrBlob(std::initializer_list<string> il):data(std::make_shared<vector<string>>(il)) { }

    size_type size() const { return data->size(); }
    bool empty() const { return data->empty(); }

    void push_back(const string &t) { data->push_back(t); }
    void pop_back() {
        check(0, "pop_back on empty StrBlob");
        data->pop_back();
    }

    std::string& front() {
        check(0, "front on empty StrBlob");
        return data->front();
    }

    std::string& back() {
        check(0, "back on empty StrBlob");
        return data->back();
    }

    const std::string& front() const {
        check(0, "front on empty StrBlob");
        return data->front();
    }
    const std::string& back() const {
        check(0, "back on empty StrBlob");
        return data->back();
    }

private:
    void check(size_type i, const string &msg) const {
        if (i >= data->size()) throw std::out_of_range(msg);
    }

private:
    std::shared_ptr<vector<string>> data;
};
```

主函数：

```cpp
#include "ex12_02.h"
#include <iostream>

int main()
{
    const StrBlob csb{ "hello", "world", "pezy" };
    StrBlob sb{ "hello", "world", "Mooophy" };

    std::cout << csb.front() << " " << csb.back() << std::endl;
    sb.back() = "pezy";
    std::cout << sb.front() << " " << sb.back() << std::endl;
}
```

## 练习12.3

> `StrBlob` 需要`const` 版本的`push_back` 和 `pop_back`吗？如需要，添加进去。否则，解释为什么不需要。

解：

不需要。`push_back` 和 `pop_back` 会改变对象的内容。而 `const` 对象是只读的，因此不需要。

## 练习12.4

> 在我们的 `check` 函数中，没有检查 `i` 是否大于0。为什么可以忽略这个检查？

解：

因为 `size_type` 是一个无符号整型，当传递给 `check` 的参数小于 0 的时候，参数值会转换成一个正整数。

## 练习12.5

> 我们未编写接受一个 `initializer_list explicit` 参数的构造函数。讨论这个设计策略的优点和缺点。

解：

构造函数不是 `explicit` 的，意味着可以从 `initializer_list` 隐式转换为 `StrBlob`。在 `StrBlob` 对象中，只有一个数据成员 `data`，而 `StrBlob` 对象本身的含义，也是一个**管理字符串的序列**。因此，从 `initializer_list` 到 `StrBlob` 的转换，在逻辑上是可行的。而这个设计策略的缺点，可能在某些地方我们确实需要 `initializer_list`，而编译器仍会将之转换为 `StrBlob`。

## 练习12.6

> 编写函数，返回一个动态分配的 `int` 的`vector`。将此`vector` 传递给另一个函数，这个函数读取标准输入，将读入的值保存在 `vector` 元素中。再将`vector`传递给另一个函数，打印读入的值。记得在恰当的时刻`delete vector`。

解：

```cpp
#include <iostream>
#include <vector>

using std::vector;

vector<int>* alloc_vector()
{
	return new vector<int>();
}

void assign_vector(vector<int>* p)
{
	int i;
	while (std::cin >> i)
	{
		p->push_back(i);
	}
}

void print_vector(vector<int>* p)
{
	for (auto i : *p)
	{
		std::cout << i << std::endl;
	}
}

int main()
{
	auto p = alloc_vector();
	assign_vector(p);
	print_vector(p);
	delete p;
	return 0;
}
```

## 练习12.7

> 重做上一题，这次使用 `shared_ptr` 而不是内置指针。

解：

```cpp
#include <iostream>
#include <vector>
#include <memory>

using std::vector;

std::shared_ptr<vector<int>> alloc_vector()
{
	return std::make_shared<vector<int>>();
}

void assign_vector(std::shared_ptr<vector<int>> p)
{
	int i;
	while (std::cin >> i)
	{
		p->push_back(i);
	}
}

void print_vector(std::shared_ptr<vector<int>> p)
{
	for (auto i : *p)
	{
		std::cout << i << std::endl;
	}
}

int main()
{
	auto p = alloc_vector();
	assign_vector(p);
	print_vector(p);
	return 0;
}
```

## 练习12.8

> 下面的函数是否有错误？如果有，解释错误原因。

```cpp
bool b() {
	int* p = new int;
	// ...
	return p;
}
```

解：

有错误。`p`会被强制转换成`bool`，继而没有释放指针 `p` 指向的对象。

## 练习12.9

> 解释下面代码执行的结果。

```cpp
int *q = new int(42), *r = new int(100);
r = q;
auto q2 = make_shared<int>(42), r2 = make_shared<int>(100);
r2 = q2;
```

解：

`r` 和 `q` 指向 42，而之前 `r` 指向的 100 的内存空间并没有被释放，因此会发生内存泄漏。`r2` 和 `q2` 都是智能指针，当对象空间不被引用的时候会自动释放。

## 练习12.10

> 下面的代码调用了第413页中定义的`process` 函数，解释此调用是否正确。如果不正确，应如何修改？

```cpp
shared_ptr<int> p(new int(42));
process(shared_ptr<int>(p));
```

解：

正确。`shared_ptr<int>(p)` 会创建一个临时的智能指针，这个智能指针与 `p` 引用同一个对象，此时引用计数为 2。当表达式结束时，临时的智能指针被销毁，此时引用计数为 1。

## 练习12.11

> 如果我们像下面这样调用 `process`，会发生什么？

```cpp
process(shared_ptr<int>(p.get()));
```

解：

这样会创建一个新的智能指针，它的引用计数为 1，这个智能指针所指向的空间与 `p` 相同。在表达式结束后，这个临时智能指针会被销毁，引用计数为 0，所指向的内存空间也会被释放。而导致 `p` 所指向的空间被释放，使得 p` 成为一个空悬指针。

## 练习12.12

> `p` 和 `sp` 的定义如下，对于接下来的对 `process` 的每个调用，如果合法，解释它做了什么，如果不合法，解释错误原因：

```cpp
auto p = new int();auto sp = make_shared<int>();(a) process(sp);(b) process(new int());(c) process(p);(d) process(shared_ptr<int>(p));
```

解：

* (a) 合法。将`sp` 拷贝给 `process`函数的形参，在函数里面引用计数为 2，函数结束后引用计数为 1。
* (b) 不合法。不能从内置指针隐式转换为智能指针。
* (c) 不合法。不能从内置指针隐式转换为智能指针。
* (d) 合法。但是智能指针和内置指针一起使用可能会出现问题，在表达式结束后智能指针会被销毁，它所指向的对象也被释放。而此时内置指针 `p` 依旧指向该内存空间。之后对内置指针 `p` 的操作可能会引发错误。

## 练习12.13

> 如果执行下面的代码，会发生什么？

```cpp
auto sp = make_shared<int>();auto p = sp.get();delete p;
```

解：

智能指针 `sp` 所指向空间已经被释放，再对 `sp` 进行操作会出现错误。

## 练习12.14

> 编写你自己版本的用 `shared_ptr` 管理 `connection` 的函数。

解：

```cpp
#include <iostream>#include <memory>#include <string>struct connection{	std::string ip;	int port;	connection(std::string i, int p) : ip(i), port(p) {}};struct destination{	std::string ip;	int port;	destination(std::string i, int p) : ip(i), port(p) {}};connection connect(destination* pDest){	std::shared_ptr<connection> pConn(new connection(pDest->ip, pDest->port));	std::cout << "creating connection(" << pConn.use_count() << ")" << std::endl;	return *pConn;}void disconnect(connection pConn){	std::cout << "connection close(" << pConn.ip << ":" << pConn.port << ")" << std::endl;	}void end_connection(connection* pConn){	disconnect(*pConn);}void f(destination &d){	connection conn = connect(&d);	std::shared_ptr<connection> p(&conn, end_connection);	std::cout << "connecting now(" << p.use_count() << ")" << std::endl;}int main(){	destination dest("220.181.111.111", 10086);	f(dest);	return 0;}
```

## 练习12.15

> 重写上一题的程序，用 `lambda` 代替`end_connection` 函数。

解：

```cpp
#include <iostream>#include <memory>#include <string>struct connection{	std::string ip;	int port;	connection(std::string i, int p) : ip(i), port(p) {}};struct destination{	std::string ip;	int port;	destination(std::string i, int p) : ip(i), port(p) {}};connection connect(destination* pDest){	std::shared_ptr<connection> pConn(new connection(pDest->ip, pDest->port));	std::cout << "creating connection(" << pConn.use_count() << ")" << std::endl;	return *pConn;}void disconnect(connection pConn){	std::cout << "connection close(" << pConn.ip << ":" << pConn.port << ")" << std::endl;}void f(destination &d){	connection conn = connect(&d);	std::shared_ptr<connection> p(&conn, [] (connection* p){ disconnect(*p); });	std::cout << "connecting now(" << p.use_count() << ")" << std::endl;}int main(){	destination dest("220.181.111.111", 10086);	f(dest);	return 0;}
```

## 练习12.16

> 如果你试图拷贝或赋值 `unique_ptr`，编译器并不总是能给出易于理解的错误信息。编写包含这种错误的程序，观察编译器如何诊断这种错误。

解：

```cpp
#include <iostream>#include <string>#include <memory>using std::string; using std::unique_ptr;int main(){    unique_ptr<string> p1(new string("pezy"));    // unique_ptr<string> p2(p1); // copy    //                      ^    // Error: Call to implicitly-deleted copy constructor of 'unique_ptr<string>'    //    // unique_ptr<string> p3 = p1; // assign    //                      ^    // Error: Call to implicitly-deleted copy constructor of 'unique_ptr<string>'    std::cout << *p1 << std::endl;    p1.reset(nullptr);}
```

## 练习12.17

> 下面的 `unique_ptr` 声明中，哪些是合法的，哪些可能导致后续的程序错误？解释每个错误的问题在哪里。

```cpp
int ix = 1024, *pi = &ix, *pi2 = new int(2048);typedef unique_ptr<int> IntP;(a) IntP p0(ix);(b) IntP p1(pi);(c) IntP p2(pi2);(d) IntP p3(&ix);(e) IntP p4(new int(2048));(f) IntP p5(p2.get());
```

解：

* (a) 不合法。在定义一个 `unique_ptr` 时，需要将其绑定到一个`new` 返回的指针上。
* (b) 不合法。理由同上。
* (c) 合法。但是也可能会使得 `pi2` 成为空悬指针。
* (d) 不合法。当 `p3` 被销毁时，它试图释放一个栈空间的对象。
* (e) 合法。
* (f) 不合法。`p5` 和 `p2` 指向同一个对象，当 `p5` 和 `p2` 被销毁时，会使得同一个指针被释放两次。

## 练习12.18

> `shared_ptr` 为什么没有 `release` 成员？

`release` 成员的作用是放弃控制权并返回指针，因为在某一时刻只能有一个 `unique_ptr` 指向某个对象，`unique_ptr` 不能被赋值，所以要使用 `release` 成员将一个 `unique_ptr` 的指针的所有权传递给另一个 `unique_ptr`。而 `shared_ptr` 允许有多个 `shared_ptr` 指向同一个对象，因此不需要 `release` 成员。

## 练习12.19

> 定义你自己版本的 `StrBlobPtr`，更新 `StrBlob` 类，加入恰当的 `friend` 声明以及 `begin` 和 `end` 成员。

解：

```cpp
#include <string>#include <vector>#include <initializer_list>#include <memory>#include <stdexcept>using std::vector; using std::string;class StrBlobPtr;class StrBlob{public:	using size_type = vector<string>::size_type;	friend class StrBlobPtr;	StrBlobPtr begin();	StrBlobPtr end();	StrBlob() : data(std::make_shared<vector<string>>()) {}	StrBlob(std::initializer_list<string> il) : data(std::make_shared<vector<string>>(il)) {}	size_type size() const { return data->size(); }	bool empty() const { return data->empty(); }	void push_back(const string& s) { data->push_back(s); }	void pop_back()	{		check(0, "pop_back on empty StrBlob");		data->pop_back();	}	std::string& front()	{		check(0, "front on empty StrBlob");		return data->front();	}	std::string& back()	{		check(0, "back on empty StrBlob");		return data->back();	}	const std::string& front() const	{		check(0, "front on empty StrBlob");		return data->front();	}	const std::string& back() const	{		check(0, "back on empty StrBlob");		return data->back();	}private:	void check(size_type i, const string& msg) const	{		if (i >= data->size())			throw std::out_of_range(msg);	}private:	std::shared_ptr<vector<string>> data;};class StrBlobPtr{public:	StrBlobPtr() :curr(0) {}	StrBlobPtr(StrBlob &a, size_t sz = 0) :wptr(a.data), curr(sz) {}	bool operator!=(const StrBlobPtr& p) { return p.curr != curr; }	string& deref() const	{		auto p = check(curr, "dereference past end");		return (*p)[curr];	}	StrBlobPtr& incr()	{		check(curr, "increment past end of StrBlobPtr");		++curr;		return *this;	}private:	std::shared_ptr<vector<string>> check(size_t i, const string &msg) const	{		auto ret = wptr.lock();		if (!ret) throw std::runtime_error("unbound StrBlobPtr");		if (i >= ret->size()) throw std::out_of_range(msg);		return ret;	}	std::weak_ptr<vector<string>> wptr;	size_t curr;};StrBlobPtr StrBlob::begin(){	return StrBlobPtr(*this);}StrBlobPtr StrBlob::end(){	return StrBlobPtr(*this, data->size());}
```

## 练习12.20

> 编写程序，逐行读入一个输入文件，将内容存入一个 `StrBlob` 中，用一个 `StrBlobPtr` 打印出 `StrBlob` 中的每个元素。

解：

```cpp
#include <iostream>#include <fstream>#include "exercise12_19.h"using namespace std;int main(){	ifstream ifs("books.txt");	StrBlob sb;	string s;	while (getline(ifs, s))	{		sb.push_back(s);	}	for (StrBlobPtr sbp = sb.begin(); sbp != sb.end(); sbp.incr())	{		cout << sbp.deref() << endl;	}	return 0;}
```

## 练习12.21

> 也可以这样编写 `StrBlobPtr` 的 `deref` 成员：

```cpp
std::string& deref() const {	return (*check(curr, "dereference past end"))[curr];}
```

你认为哪个版本更好？为什么？

解：

原来的版本更好，可读性更高。

## 练习12.22

> 为了能让 `StrBlobPtr` 使用 `const StrBlob`，你觉得应该如何修改？定义一个名为`ConstStrBlobPtr` 的类，使其能够指向 `const StrBlob`。

解：

构造函数改为接受 `const Strblob &` , 然后给 `Strblob` 类添加两个 `const` 成员函数 `cbegin` 和 `cend`，返回 `ConstStrBlobPtr`。

## 练习12.23

> 编写一个程序，连接两个字符串字面常量，将结果保存在一个动态分配的`char`数组中。重写这个程序，连接两个标准库`string`对象。

解:

```cpp
#include <iostream>#include <string>#include <cstring>#include <memory>int main() {	const char *c1 = "Hello ";	const char *c2 = "World";	unsigned len = strlen(c1) + strlen(c2) + 1;	char *r = new char[len]();	strcat_s(r, len, c1);	strcat_s(r, len, c2);	std::cout << r << std::endl;	std::string s1 = "Hello ";	std::string s2 = "World";	strcpy_s(r, len, (s1 + s2).c_str());	std::cout << r << std::endl;	delete[] r;	return 0;}
```

## 练习12.24

> 编写一个程序，从标准输入读取一个字符串，存入一个动态分配的字符数组中。描述你的程序如何处理变长输入。测试你的程序，输入一个超出你分配的数组长度的字符串。

解：

```cpp
#include <iostream>int main(){	std::cout << "How long do you want the string? ";	int size{ 0 };	std::cin >> size;	char *input = new char[size + 1]();	std::cin.ignore();	std::cout << "input the string: ";	std::cin.get(input, size + 1);	std::cout << input;	delete[] input;	return 0;}
```

## 练习12.25

> 给定下面的`new`表达式，你应该如何释放`pa`？

```cpp
int *pa = new int[10];
```

解：

```cpp
delete [] pa;
```

## 练习12.26

> 用 `allocator` 重写第427页中的程序。

```cpp
#include <iostream>#include <string>#include <memory>using namespace std;int main(){	int n = 5;	allocator<string> alloc;	auto p = alloc.allocate(n);	string s;	auto q = p;	while (cin >> s && q != p + n)	{		alloc.construct(q++, s);	}	while (q != p)	{		std::cout << *--q << " ";		alloc.destroy(q);	}	alloc.deallocate(p, n);	return 0;}
```

## 练习12.27

> `TextQuery` 和 `QueryResult` 类只使用了我们已经介绍过的语言和标准库特性。不要提前看后续章节内容，只用已经学到的知识对这两个类编写你自己的版本。

解：

头文件：

```cpp
#ifndef EX12_27_H#define EX12_27_H#include <fstream>#include <memory>#include <vector>#include <string>#include <map>#include <set>class QueryResult;class TextQuery{public:	using line_no = std::vector<std::string>::size_type;	TextQuery(std::ifstream&);	QueryResult query(const std::string& s) const;private:	std::shared_ptr<std::vector<std::string>> file;	std::map<std::string, std::shared_ptr<std::set<line_no>>> wm;};class QueryResult{public:	friend std::ostream& print(std::ostream&, const QueryResult&);	QueryResult(std::string s,				std::shared_ptr<std::set<TextQuery::line_no>> p,				std::shared_ptr<std::vector<std::string>> f) :		sought(s), lines(p), file(f) 	{}private:	std::string sought;	std::shared_ptr<std::set<TextQuery::line_no>> lines;	std::shared_ptr<std::vector<std::string>> file;};std::ostream& print(std::ostream&, const QueryResult&);#endif
```

实现：

```cpp
#include "ex_12_27.h"#include <sstream>#include <fstream>#include <vector>#include <string>using namespace std;TextQuery::TextQuery(ifstream& ifs) : file(new vector<string>){	string text;	while (getline(ifs, text))	{		file->push_back(text);		int n = file->size() - 1;		istringstream line(text);		string word;		while (line >> word)		{			auto &lines = wm[word];			if (!lines)				lines.reset(new set<line_no>);			lines->insert(n);		}	}}QueryResult TextQuery::query(const string& s) const{	static shared_ptr<set<line_no>> nodata(new set<line_no>);	auto loc = wm.find(s);	if (loc == wm.end())		return QueryResult(s, nodata, file);	else		return QueryResult(s, loc->second, file);}std::ostream& print(std::ostream& os, const QueryResult& qr){	os << qr.sought << " occurs " << qr.lines->size() << " "		<< "time" << (qr.lines->size() > 1 ? "s" : "") << endl;	for (auto num : *qr.lines)		os << "\t(line " << num + 1 << ") " << *(qr.file->begin() + num) << endl;	return os;}
```

主函数：

```cpp
#include <iostream>#include <string>#include <fstream>#include "ex_12_27.h"using namespace std;void runQueries(ifstream& infile){	TextQuery tq(infile);	while (true)	{		cout << "enter word to look for, or q to quit: ";		string s;		if (!(cin >> s) || s == "q") break;		print(cout, tq.query(s)) << endl;	}}int main(){	ifstream ifs("storyDataFile.txt");	runQueries(ifs);	return 0;}
```

## 练习12.28

> 编写程序实现文本查询，不要定义类来管理数据。你的程序应该接受一个文件，并与用户交互来查询单词。使用`vector`、`map` 和 `set` 容器来保存来自文件的数据并生成查询结果。

解：

```cpp
#include <string>using std::string;#include <vector>using std::vector;#include <memory>using std::shared_ptr;#include <iostream>#include <fstream>#include <sstream>#include <map>#include <set>#include <algorithm>int main(){	std::ifstream file("H:/code/C++/Cpp_Primer_Answers/data/storyDataFile.txt");	vector<string> input;	std::map<string, std::set<decltype(input.size())>> dictionary;	decltype(input.size()) lineNo{ 0 };	for (string line; std::getline(file, line); ++lineNo)	{		input.push_back(line);		std::istringstream line_stream(line);		for (string text, word; line_stream >> text; word.clear())		{			std::remove_copy_if(text.begin(), text.end(), std::back_inserter(word), ispunct);			dictionary[word].insert(lineNo);		}	}	while (true)	{		std::cout << "enter word to look for, or q to quit: ";		string s;		if (!(std::cin >> s) || s == "q") break;		auto found = dictionary.find(s);		if (found != dictionary.end())		{			std::cout << s << " occurs " << found->second.size() << (found->second.size() > 1 ? " times" : " time") << std::endl;			for (auto i : found->second)				std::cout << "\t(line " << i + 1 << ") " << input.at(i) << std::endl;		}		else std::cout << s << " occurs 0 time" << std::endl;	}}
```

## 练习12.29

> 我们曾经用`do while` 循环来编写管理用户交互的循环。用`do while` 重写本节程序，解释你倾向于哪个版本，为什么？

解：

```cpp
do {    std::cout << "enter word to look for, or q to quit: ";    string s;    if (!(std::cin >> s) || s == "q") break;    print(std::cout, tq.query(s)) << std::endl;} while ( true );
```

我更喜欢 `while`，这可能是习惯的问题。

## 练习12.30

> 定义你自己版本的 `TextQuery` 和 `QueryResult` 类，并执行12.3.1节中的`runQueries` 函数。

解：

同12.27。

## 练习12.31

> 如果用`vector` 代替 `set` 保存行号，会有什么差别？哪个方法更好？为什么？

如果用 `vector` 则会有单词重复的情况出现。而这里保存的是行号，不需要重复元素，所以 `set` 更好。

## 练习12.32

> 重写 `TextQuery` 和 `QueryResult`类，用`StrBlob` 代替 `vector<string>` 保存输入文件。

解：

`TextQuery` 和 `QueryResult` 类中的 `file` 成员，改为 指向 `StrBlob` 的智能指针。在访问 `StrBlob` 时，要使用 `StrBlobPtr`。

## 练习12.33

> 在第15章中我们将扩展查询系统，在 `QueryResult` 类中将会需要一些额外的成员。添加名为 `begin` 和 `end` 的成员，返回一个迭代器，指向一个给定查询返回的行号的 `set` 中的位置。再添加一个名为 `get_file` 的成员，返回一个 `shared_ptr`，指向 `QueryResult` 对象中的文件。

解：

```cpp
class QueryResult{
public:
	using Iter = std::set<line_no>::iterator;	
	// ...
	Iter begin() const { return lines->begin(); }
	Iter end() const { return lines->end(); }
	shared_ptr<std::vector<std::string>> get_file() const 
	{ 
		return std::make_shared<std::vector<std::string>>(file); 
	}
private:
	// ...
};
```



