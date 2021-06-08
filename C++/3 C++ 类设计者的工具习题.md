# 第十三章 拷贝控制

## 练习13.1

> 拷贝构造函数是什么？什么时候使用它？

解：

如果一个构造函数的第一个参数是自身类类型的引用，且任何额外参数都有默认值，则此构造函数是拷贝构造函数。当使用**拷贝初始化**时，我们会用到拷贝构造函数。

## 练习13.2

> 解释为什么下面的声明是非法的：

```cpp
Sales_data::Sales_data(Sales_data rhs);
```

解：

参数类型应该是引用类型。

## 练习13.3

> 当我们拷贝一个`StrBlob`时，会发生什么？拷贝一个`StrBlobPtr`呢？

解：

当我们拷贝`StrBlob`时，会使 `shared_ptr` 的引用计数加1。当我们拷贝 `StrBlobPtr` 时，引用计数不会变化。

## 练习13.4

> 假定 `Point` 是一个类类型，它有一个`public`的拷贝构造函数，指出下面程序片段中哪些地方使用了拷贝构造函数：


```cpp
Point global;
Point foo_bar(Point arg) // 1
{
	Point local = arg, *heap = new Point(global); // 2: Point local = arg,  3: Point *heap = new Point(global) 
	*heap = local; 
	Point pa[4] = { local, *heap }; // 4, 5
	return *heap;  // 6
}
```

解：

上面有6处地方使用了拷贝构造函数。


## 练习13.5

> 给定下面的类框架，编写一个拷贝构造函数，拷贝所有成员。你的构造函数应该动态分配一个新的`string`，并将对象拷贝到`ps`所指向的位置，而不是拷贝ps本身：

```cpp
class HasPtr {
public:
	HasPtr(const std::string& s = std::string()):
		ps(new std::string(s)), i(0) { }
private:
	std::string *ps;
	int i;
}
```

解：

```cpp
#include <string>

class HasPtr {
public:
    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0) { }
    HasPtr(const HasPtr& hp) : ps(new std::string(*hp.ps)), i(hp.i) { }
private:
    std::string *ps;
    int i;
};
```

## 练习13.6

> 拷贝赋值运算符是什么？什么时候使用它？合成拷贝赋值运算符完成什么工作？什么时候会生成合成拷贝赋值运算符？

解：

拷贝赋值运算符是一个名为 `operator=` 的函数。当赋值运算发生时就会用到它。合成拷贝赋值运算符可以用来禁止该类型对象的赋值。如果一个类未定义自己的拷贝赋值运算符，编译器会为它生成一个合成拷贝赋值运算符。

## 练习13.7

> 当我们将一个 `StrBlob` 赋值给另一个 `StrBlob` 时，会发生什么？赋值 `StrBlobPtr` 呢？

解：

会发生浅层复制。

## 练习13.8

> 为13.1.1节练习13.5中的 `HasPtr` 类编写赋值运算符。类似拷贝构造函数，你的赋值运算符应该将对象拷贝到`ps`指向的位置。

解：

```cpp
#include <string>

class HasPtr {
public:
    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0) { }
    HasPtr(const HasPtr &hp) : ps(new std::string(*hp.ps)), i(hp.i) { }
    HasPtr& operator=(const HasPtr &rhs_hp) {
        if(this != &rhs_hp){
            std::string *temp_ps = new std::string(*rhs_hp.ps);
            delete ps;
            ps = temp_ps;
            i = rhs_hp.i;
        }
        return *this;
    }
private:
    std::string *ps;
    int i;
};
```

## 练习13.9

> 析构函数是什么？合成析构函数完成什么工作？什么时候会生成合成析构函数？

解：

析构函数是类的一个成员函数，名字由波浪号接类名构成。它没有返回值，也不接受参数。合成析构函数可被用来阻止该类型的对象被销毁。当一个类未定义自己的析构函数时，编译器会为它生成一个合成析构函数。

## 练习13.10

> 当一个 `StrBlob` 对象销毁时会发生什么？一个 `StrBlobPtr` 对象销毁时呢？

解：

当一个 `StrBlob` 对象被销毁时，`shared_ptr` 的引用计数会减少。当 `StrBlobPtr` 对象被销毁时，不影响引用计数。

## 练习13.11

> 为前面练习中的 `HasPtr` 类添加一个析构函数。

解：

```cpp
#include <string>

class HasPtr {
public:
    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0) { }
    HasPtr(const HasPtr &hp) : ps(new std::string(*hp.ps)), i(hp.i) { }
    HasPtr& operator=(const HasPtr &hp) {
        std::string *new_ps = new std::string(*hp.ps);
        delete ps;
        ps = new_ps;
        i = hp.i;
        return *this;
    }
    ~HasPtr() {
        delete ps;
    }
private:
    std::string *ps;
    int i;
};
```

## 练习13.12

> 在下面的代码片段中会发生几次析构函数调用？

```cpp
bool fcn(const Sales_data *trans, Sales_data accum)
{
	Sales_data item1(*trans), item2(accum);
	return item1.isbn() != item2.isbn();
}
```

解：

三次，分别是 `accum`、`item1`和`item2`。

## 练习13.13

> 理解拷贝控制成员和构造函数的一个好方法的定义一个简单的类，为该类定义这些成员，每个成员都打印出自己的名字：

```cpp
struct X {
	X() {std::cout << "X()" << std::endl;}
	X(const X&) {std::cout << "X(const X&)" << std::endl;}
}
```

给 `X` 添加拷贝赋值运算符和析构函数，并编写一个程序以不同的方式使用 `X` 的对象：将它们作为非引用参数传递；动态分配它们；将它们存放于容器中；诸如此类。观察程序的输出，直到你确认理解了什么时候会使用拷贝控制成员，以及为什么会使用它们。当你观察程序输出时，记住编译器可以略过对拷贝构造函数的调用。

解：

```cpp
#include <iostream>#include <vector>#include <initializer_list>struct X {    X() { std::cout << "X()" << std::endl; }    X(const X&) { std::cout << "X(const X&)" << std::endl; }    X& operator=(const X&) { std::cout << "X& operator=(const X&)" << std::endl; return *this; }    ~X() { std::cout << "~X()" << std::endl; }};void f(const X &rx, X x){    std::vector<X> vec;    vec.reserve(2);    vec.push_back(rx);    vec.push_back(x);}int main(){    X *px = new X;    f(*px, *px);    delete px;    return 0;}
```

## 练习13.14

> 假定 `numbered` 是一个类，它有一个默认构造函数，能为每个对象生成一个唯一的序号，保存在名为 `mysn` 的数据成员中。假定 `numbered` 使用合成的拷贝控制成员，并给定如下函数：

```cpp
void f (numbered s) { cout << s.mysn < endl; }
```

则下面代码输出什么内容？

```cpp
numbered a, b = a, c = b;f(a); f(b); f(c);
```

解：

输出3个完全一样的数。

## 练习13.15

> 假定`numbered` 定义了一个拷贝构造函数，能生成一个新的序列号。这会改变上一题中调用的输出结果吗？如果会改变，为什么？新的输出结果是什么？

解：

会输出3个不同的数。并且这3个数并不是a、b、c当中的数。

## 练习13.16

> 如果 `f` 中的参数是 `const numbered&`，将会怎样？这会改变输出结果吗？如果会改变，为什么？新的输出结果是什么？

解：

会输出 a、b、c的数。

## 练习13.17

> 分别编写前三题中所描述的 `numbered` 和 `f`，验证你是否正确预测了输出结果。

解：

13.14：

```cpp
#include <iostream>class numbered{public:	numbered()	{		mysn = unique++;	}	int mysn;	static int unique;};int numbered::unique = 10;void f(numbered s){	std::cout << s.mysn << std::endl;}int main(){	numbered a, b = a, c = b;	f(a);	f(b);	f(c);}
```

13.15：

```cpp
#include <iostream>class numbered {public:    numbered() {        mysn = unique++;    }	numbered(const numbered& n)	{		mysn = unique++;	}    int mysn;    static int unique;};int numbered::unique = 10;void f(numbered s) {    std::cout << s.mysn << std::endl;}int main(){    numbered a, b = a, c = b;    f(a);    f(b);    f(c);}
```

13.16：

```cpp
#include <iostream>class numbered{public:	numbered()	{		mysn = unique++;	}	numbered(const numbered& n)	{		mysn = unique++;	}	int mysn;	static int unique;};int numbered::unique = 10;void f(const numbered& s){	std::cout << s.mysn << std::endl;}int main(){	numbered a, b = a, c = b;	f(a);	f(b);	f(c);}
```

## 练习13.18

> 定义一个 `Employee` 类，它包含雇员的姓名和唯一的雇员证号。为这个类定义默认构造函数，以及接受一个表示雇员姓名的 `string` 的构造函数。每个构造函数应该通过递增一个 `static` 数据成员来生成一个唯一的证号。

解：

```cpp
#include <string>using std::string;class Employee{public:	Employee();	Employee(const string& name);	const int id() const { return id_; }private:	string name_;	int id_;	static int s_increment;};int Employee::s_increment = 0;Employee::Employee(){	id_ = s_increment++;}Employee::Employee(const string& name){	id_ = s_increment++;	name_ = name;}
```

## 练习13.19

> 你的 `Employee` 类需要定义它自己的拷贝控制成员吗？如果需要，为什么？如果不需要，为什么？实现你认为 `Employee` 需要的拷贝控制成员。

解：

可以显式地阻止拷贝。

```cpp
#include <string>using std::string;class Employee {public:    Employee();    Employee(const string &name);    Employee(const Employee&) = delete;    Employee& operator=(const Employee&) = delete;    const int id() const { return id_; }private:    string name_;    int id_;    static int s_increment;};
```

## 练习13.20

> 解释当我们拷贝、赋值或销毁 `TextQuery` 和 `QueryResult` 类对象时会发生什么？

解：

成员会被复制。

## 练习13.21

> 你认为 `TextQuery` 和 `QueryResult` 类需要定义它们自己版本的拷贝控制成员吗？如果需要，为什么？实现你认为这两个类需要的拷贝控制操作。

解：

合成的版本满足所有的需求。因此不需要自定义拷贝控制成员。

## 练习13.22

> 假定我们希望 `HasPtr` 的行为像一个值。即，对于对象所指向的 `string` 成员，每个对象都有一份自己的拷贝。我们将在下一节介绍拷贝控制成员的定义。但是，你已经学习了定义这些成员所需的所有知识。在继续学习下一节之前，为 `HasPtr` 编写拷贝构造函数和拷贝赋值运算符。

解：

```cpp
#include <string>class HasPtr {public:    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0) { }    HasPtr(const HasPtr &hp) : ps(new std::string(*hp.ps)), i(hp.i) { }    HasPtr& operator=(const HasPtr &hp) {        auto new_p = new std::string(*hp.ps);        delete ps;        ps = new_p;        i = hp.i;        return *this;    }    ~HasPtr() {        delete ps;    } private:    std::string *ps;    int i;};
```

## 练习13.23

> 比较上一节练习中你编写的拷贝控制成员和这一节中的代码。确定你理解了你的代码和我们的代码之间的差异。

解：

查看13.22代码。

## 练习13.24

> 如果本节的 `HasPtr` 版本未定义析构函数，将会发生什么？如果未定义拷贝构造函数，将会发生什么？

解：

如果未定义析构函数，将会发生内存泄漏。如果未定义拷贝构造函数，将会拷贝指针的值，指向同一个地址。

## 练习13.25

> 假定希望定义 `StrBlob` 的类值版本，而且希望继续使用 `shared_ptr`，这样我们的 `StrBlobPtr` 类就仍能使用指向`vector`的 `weak_ptr` 了。你修改后的类将需要一个拷贝的构造函数和一个拷贝赋值运算符，但不需要析构函数。解释拷贝构造函数和拷贝赋值运算符必须要做什么。解释为什么不需要析构函数。

解：

拷贝构造函数和拷贝赋值运算符要重新动态分配内存。因为 `StrBlob` 使用的是智能指针，当引用计数为0时会自动释放对象，因此不需要析构函数。

## 练习13.26

> 对上一题中描述的 `strBlob` 类，编写你自己的版本。

解：

头文件：

```cpp
#include <vector>#include <string>#include <initializer_list>#include <memory>#include <exception>using std::vector; using std::string;class ConstStrBlobPtr;class StrBlob {public:    using size_type = vector<string>::size_type;    friend class ConstStrBlobPtr;    ConstStrBlobPtr begin() const;    ConstStrBlobPtr end() const;    StrBlob():data(std::make_shared<vector<string>>()) { }    StrBlob(std::initializer_list<string> il):data(std::make_shared<vector<string>>(il)) { }    // copy constructor    StrBlob(const StrBlob& sb) : data(std::make_shared<vector<string>>(*sb.data)) { }    // copy-assignment operators    StrBlob& operator=(const StrBlob& sb);    size_type size() const { return data->size(); }    bool empty() const { return data->empty(); }    void push_back(const string &t) { data->push_back(t); }    void pop_back() {        check(0, "pop_back on empty StrBlob");        data->pop_back();    }    string& front() {        check(0, "front on empty StrBlob");        return data->front();    }    string& back() {        check(0, "back on empty StrBlob");        return data->back();    }    const string& front() const {        check(0, "front on empty StrBlob");        return data->front();    }    const string& back() const {        check(0, "back on empty StrBlob");        return data->back();    }private:    void check(size_type i, const string &msg) const {        if (i >= data->size()) throw std::out_of_range(msg);    }private:    std::shared_ptr<vector<string>> data;};class ConstStrBlobPtr {public:    ConstStrBlobPtr():curr(0) { }    ConstStrBlobPtr(const StrBlob &a, size_t sz = 0):wptr(a.data), curr(sz) { } // should add const    bool operator!=(ConstStrBlobPtr& p) { return p.curr != curr; }    const string& deref() const { // return value should add const        auto p = check(curr, "dereference past end");        return (*p)[curr];    }    ConstStrBlobPtr& incr() {        check(curr, "increment past end of StrBlobPtr");        ++curr;        return *this;    }private:    std::shared_ptr<vector<string>> check(size_t i, const string &msg) const {        auto ret = wptr.lock();        if (!ret) throw std::runtime_error("unbound StrBlobPtr");        if (i >= ret->size()) throw std::out_of_range(msg);        return ret;    }    std::weak_ptr<vector<string>> wptr;    size_t curr;};
```

主函数：

```cpp
#include "ex_13_26.h"ConstStrBlobPtr StrBlob::begin() const // should add const{    return ConstStrBlobPtr(*this);}ConstStrBlobPtr StrBlob::end() const // should add const{    return ConstStrBlobPtr(*this, data->size());}StrBlob& StrBlob::operator=(const StrBlob& sb){    data = std::make_shared<vector<string>>(*sb.data);    return *this;}int main(){    return 0;}
```

## 练习13.27

> 定义你自己的使用引用计数版本的 `HasPtr`。

解：

```cpp
#include <string>class HasPtr {public:    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0), use(new size_t(1)) { }    HasPtr(const HasPtr &hp) : ps(hp.ps), i(hp.i), use(hp.use) { ++*use; }    HasPtr& operator=(const HasPtr &rhs) {        ++*rhs.use;        if (--*use == 0) {            delete ps;            delete use;        }        ps = rhs.ps;        i = rhs.i;        use = rhs.use;        return *this;    }    ~HasPtr() {        if (--*use == 0) {            delete ps;            delete use;        }    } private:    std::string *ps;    int i;    size_t *use;};
```

## 练习13.28

> 给定下面的类，为其实现一个默认构造函数和必要的拷贝控制成员。

```cpp
(a) class TreeNode {pravite:	std::string value;	int count;	TreeNode *left;	TreeNode *right;	};(b)class BinStrTree{pravite:	TreeNode *root;	};
```

解：

头文件：

```cpp
#include <string>using std::string;class TreeNode {public:    TreeNode() : value(string()), count(new int(1)), left(nullptr), right(nullptr) { }    TreeNode(const TreeNode &rhs) : value(rhs.value), count(rhs.count), left(rhs.left), right(rhs.right) { ++*count; }    TreeNode& operator=(const TreeNode &rhs);    ~TreeNode() {        if (--*count == 0) {            delete left;            delete right;            delete count;        }    }private:    std::string value;    int         *count;    TreeNode    *left;    TreeNode    *right;};class BinStrTree {public:    BinStrTree() : root(new TreeNode()) { }    BinStrTree(const BinStrTree &bst) : root(new TreeNode(*bst.root)) { }    BinStrTree& operator=(const BinStrTree &bst);    ~BinStrTree() { delete root; }private:    TreeNode *root;};
```

实现和主函数：

```cpp
#include "ex_13_28.h"TreeNode& TreeNode::operator=(const TreeNode &rhs){    ++*rhs.count;    if (--*count == 0) {        delete left;        delete right;        delete count;    }    value = rhs.value;    left = rhs.left;    right = rhs.right;    count = rhs.count;    return *this;}BinStrTree& BinStrTree::operator=(const BinStrTree &bst){    TreeNode *new_root = new TreeNode(*bst.root);    delete root;    root = new_root;    return *this;}int main(){    return 0;}
```

## 练习13.29

> 解释 `swap(HasPtr&, HasPtr&)`中对 `swap` 的调用不会导致递归循环。

解：

这其实是3个不同的函数，参数类型不一样，所以不会导致递归循环。

## 练习13.30

> 为你的类值版本的 `HasPtr` 编写 `swap` 函数，并测试它。为你的 `swap` 函数添加一个打印语句，指出函数什么时候执行。

解：

```cpp
#include <string>#include <iostream>class HasPtr {public:    friend void swap(HasPtr&, HasPtr&);    HasPtr(const std::string &s = std::string()) : ps(new std::string(s)), i(0) { }    HasPtr(const HasPtr &hp) : ps(new std::string(*hp.ps)), i(hp.i) { }    HasPtr& operator=(const HasPtr &hp) {        auto new_p = new std::string(*hp.ps);        delete ps;        ps = new_p;        i = hp.i;        return *this;    }    ~HasPtr() {        delete ps;    }         void show() { std::cout << *ps << std::endl; }private:    std::string *ps;    int i;};inlinevoid swap(HasPtr& lhs, HasPtr& rhs){    using std::swap;    swap(lhs.ps, rhs.ps);    swap(lhs.i, rhs.i);    std::cout << "call swap(HasPtr& lhs, HasPtr& rhs)" << std::endl;}
```

## 练习13.31

> 为你的 `HasPtr` 类定义一个 `<` 运算符，并定义一个 `HasPtr` 的 `vector`。为这个 `vector` 添加一些元素，并对它执行 `sort`。注意何时会调用 `swap`。

解：

```cpp
#include <string>#include <iostream>class HasPtr {public:    friend void swap(HasPtr&, HasPtr&);    friend bool operator<(const HasPtr &lhs, const HasPtr &rhs);    HasPtr(const std::string &s = std::string())         : ps(new std::string(s)), i(0)     { }    HasPtr(const HasPtr &hp)         : ps(new std::string(*hp.ps)), i(hp.i)     { }    HasPtr& operator=(HasPtr tmp)     {        this->swap(tmp);        return *this;    }    ~HasPtr()     {        delete ps;    }    void swap(HasPtr &rhs)     {        using std::swap;        swap(ps, rhs.ps);        swap(i, rhs.i);        std::cout << "call swap(HasPtr &rhs)" << std::endl;    }    void show() const    {         std::cout << *ps << std::endl;     }private:    std::string *ps;    int i;};void swap(HasPtr& lhs, HasPtr& rhs){    lhs.swap(rhs);}bool operator<(const HasPtr &lhs, const HasPtr &rhs){    return *lhs.ps < *rhs.ps;}
```

## 练习13.32

> 类指针的 `HasPtr` 版本会从 `swap` 函数收益吗？如果会，得到了什么益处？如果不是，为什么？

解：

不会。类值的版本利用`swap`交换指针不用进行内存分配，因此得到了性能上的提升。类指针的版本本来就不用进行内存分配，所以不会得到性能提升。

## 练习13.33

> 为什么`Message`的成员`save`和`remove`的参数是一个 `Folder&`？为什么我们不能将参数定义为 `Folder` 或是 `const Folder`？

解：

因为 `save` 和 `remove` 操作需要更新指定 `Folder`。

## 练习13.34

> 编写本节所描述的 `Message`。

解：

头文件：

```cpp
#include <string>#include <set>class Folder;class Message {    friend void swap(Message &, Message &);    friend class Folder;public:    explicit Message(const std::string &str = ""):contents(str) { }    Message(const Message&);    Message& operator=(const Message&);    ~Message();    void save(Folder&);    void remove(Folder&);    void print_debug();private:    std::string contents;    std::set<Folder*> folders;    void add_to_Folders(const Message&);    void remove_from_Folders();    void addFldr(Folder *f) { folders.insert(f); }    void remFldr(Folder *f) { folders.erase(f); }};void swap(Message&, Message&);class Folder {    friend void swap(Folder &, Folder &);    friend class Message;public:    Folder() = default;    Folder(const Folder &);    Folder& operator=(const Folder &);    ~Folder();    void print_debug();private:    std::set<Message*> msgs;    void add_to_Message(const Folder&);    void remove_from_Message();    void addMsg(Message *m) { msgs.insert(m); }    void remMsg(Message *m) { msgs.erase(m); }};void swap(Folder &, Folder &);
```

实现和主函数：

```cpp
#include "ex13_34_36_37.h"#include <iostream>void swap(Message &lhs, Message &rhs) {    using std::swap;    lhs.remove_from_Folders(); // Use existing member function to avoid duplicate code.    rhs.remove_from_Folders(); // Use existing member function to avoid duplicate code.        swap(lhs.folders, rhs.folders);    swap(lhs.contents, rhs.contents);        lhs.add_to_Folders(lhs); // Use existing member function to avoid duplicate code.    rhs.add_to_Folders(rhs); // Use existing member function to avoid duplicate code.}// Message Implementationvoid Message::save(Folder &f) {    addFldr(&f); // Use existing member function to avoid duplicate code.    f.addMsg(this);}void Message::remove(Folder &f) {    remFldr(&f); // Use existing member function to avoid duplicate code.    f.remMsg(this);}void Message::add_to_Folders(const Message &m) {    for (auto f : m.folders)        f->addMsg(this);}Message::Message(const Message &m)     : contents(m.contents), folders(m.folders) {    add_to_Folders(m);}void Message::remove_from_Folders() {    for (auto f : folders)        f->remMsg(this);    // The book added one line here: folders.clear(); but I think it is redundant and more importantly, it will cause a bug:    // - In Message::operator=, in the case of self-assignment, it first calls remove_from_Folders() and its folders.clear()     //   clears the data member of lhs(rhs), and there is no way we can assign it back to lhs.    //   Refer to: http://stackoverflow.com/questions/29308115/protection-again-self-assignment    // - Why is it redundant? As its analogous function Message::add_to_Folders(), Message::remove_from_Folders() should ONLY    //   take care of the bookkeeping in Folders but not touch the Message's own data members - makes it much clearer and easier    //   to use. As you can see in the 2 places where we call Message::remove_from_Folders(): in Message::operator=, folders.clear()    //   introduces a bug as illustrated above; in the destructor ~Message(), the member "folders" will be destroyed anyways, why do    //   we need to clear it first?}Message::~Message() {     remove_from_Folders(); }Message &Message::operator=(const Message &rhs) {    remove_from_Folders();    contents = rhs.contents;    folders = rhs.folders;    add_to_Folders(rhs);    return *this;}void Message::print_debug() {     std::cout << contents << std::endl; }// Folder Implementationvoid swap(Folder &lhs, Folder &rhs) {    using std::swap;    lhs.remove_from_Message();    rhs.remove_from_Message();    swap(lhs.msgs, rhs.msgs);        lhs.add_to_Message(lhs);    rhs.add_to_Message(rhs);}void Folder::add_to_Message(const Folder &f) {    for (auto m : f.msgs)        m->addFldr(this);}Folder::Folder(const Folder &f)     : msgs(f.msgs) {     add_to_Message(f); }void Folder::remove_from_Message() {    for (auto m : msgs)        m->remFldr(this);}Folder::~Folder() {     remove_from_Message(); }Folder &Folder::operator=(const Folder &rhs) {    remove_from_Message();    msgs = rhs.msgs;    add_to_Message(rhs);    return *this;}void Folder::print_debug() {    for (auto m : msgs)        std::cout << m->contents << " ";    std::cout << std::endl;}int main() {     return 0;}
```

## 练习13.35

> 如果`Message` 使用合成的拷贝控制成员，将会发生什么？

在赋值后一些已存在的 `Folders` 将会与 `Message` 不同步。

## 练习13.36

> 设计并实现对应的 `Folder` 类。此类应该保存一个指向 `Folder` 中包含  `Message` 的 `set`。

解：

参考13.34。

## 练习13.37

> 为 `Message` 类添加成员，实现向 `folders` 添加和删除一个给定的 `Folder*`。这两个成员类似`Folder` 类的 `addMsg` 和 `remMsg` 操作。

解：

参考13.34。

## 练习13.38

> 我们并未使用拷贝交换方式来设计 `Message` 的赋值运算符。你认为其原因是什么？

对于动态分配内存的例子来说，拷贝交换方式是一种简洁的设计。而这里的 `Message` 类并不需要动态分配内存，用拷贝交换方式只会增加实现的复杂度。

## 练习13.39

> 编写你自己版本的 `StrVec`，包括自己版本的 `reserve`、`capacity` 和`resize`。

解：

头文件：

```cpp
#include <memory>#include <string>// 类vector类内存分配策略的简化实现class StrVec{public:    StrVec() : elements(nullptr), first_free(nullptr), cap(nullptr) { }    StrVec(const StrVec&);  // 拷贝构造函数    StrVec& operator=(const StrVec&);  // 拷贝赋值运算符    ~StrVec();  // 析构函数    void push_back(const std::string&);  // 添加元素时拷贝元素    size_t size() const { return first_free - elements; }    size_t capacity() const { return cap - elements; }    std::string *begin() const { return elements; }    std::string *end() const { return first_free; }    void reserve(size_t new_cap);    void resize(size_t count);    void resize(size_t count, const std::string&);private:    // 工具函数，被拷贝构造函数、赋值运算符和析构函数所使用    std::pair<std::string*, std::string*> alloc_n_copy(const std::string*, const std::string*);    // 销毁元素并释放内存	void free();	// 工具函数，被添加元素的函数使用    void chk_n_alloc() { if (size() == capacity()) reallocate(); }	//获得更多内存并拷贝已有元素    void reallocate();    void alloc_n_move(size_t new_cap);private:    std::string *elements;  // 指向数组首元素的指针    std::string *first_free;  // 指向数组第一个空闲元素的指针    std::string *cap;  // 指向数组第一个空闲元素的指针    std::allocator<std::string> alloc;  // 分配元素};
```

实现和主函数：

```cpp
#include "ex_13_39.h"void StrVec::push_back(const std::string &s){    chk_n_alloc();    alloc.construct(first_free++, s);}// 分配足够的内存来保存给定范围的元素，并将这些元素拷贝到新分配的内存中std::pair<std::string*, std::string*>StrVec::alloc_n_copy(const std::string *b, const std::string *e){    // 分配空间保存给定范围中的元素	auto data = alloc.allocate(e - b);	// 初始化并返回一个pair，该pair由data和uninitialized_copy的返回值构成    return{ data, std::uninitialized_copy(b, e, data) };}void StrVec::free(){    // 不能传递给deallocate一个空指针，如果elements为0，函数什么也不做	if (elements) {		// 逆序销毁元素        for (auto p = first_free; p != elements;)            alloc.destroy(--p);        alloc.deallocate(elements, cap - elements);    }}StrVec::StrVec(const StrVec &rhs){    // 调用alloc_n_copy分配空间以容纳与rhs中一样多的元素	auto newdata = alloc_n_copy(rhs.begin(), rhs.end());    elements = newdata.first;    first_free = cap = newdata.second;}StrVec::~StrVec(){    free();}StrVec& StrVec::operator = (const StrVec &rhs){    // 调用alloc_n_copy分配空间以容纳与rhs中一样多的元素	auto data = alloc_n_copy(rhs.begin(), rhs.end());    free();    elements = data.first;    first_free = cap = data.second;    return *this;}void StrVec::alloc_n_move(size_t new_cap){    auto newdata = alloc.allocate(new_cap);    auto dest = newdata;    auto elem = elements;    for (size_t i = 0; i != size(); ++i)        alloc.construct(dest++, std::move(*elem++));    free();    elements = newdata;    first_free = dest;    cap = elements + new_cap;}void StrVec::reallocate(){    auto newcapacity = size() ? 2 * size() : 1;    alloc_n_move(newcapacity);}void StrVec::reserve(size_t new_cap){    if (new_cap <= capacity()) return;    alloc_n_move(new_cap);}void StrVec::resize(size_t count){    resize(count, std::string());}void StrVec::resize(size_t count, const std::string &s){    if (count > size()) {        if (count > capacity()) reserve(count * 2);        for (size_t i = size(); i != count; ++i)            alloc.construct(first_free++, s);    }    else if (count < size()) {        while (first_free != elements + count)            alloc.destroy(--first_free);    }}int main(){    return 0;}
```

## 练习13.40

> 为你的 `StrVec` 类添加一个构造函数，它接受一个 `initializer_list<string>` 参数。

解：

头文件：

```cpp
StrVec(std::initializer_list<std::string>);
```

实现：

```cpp
void StrVec::range_initialize(const std::string *first, const std::string *last){    auto newdata = alloc_n_copy(first, last);    elements = newdata.first;    first_free = cap = newdata.second;}StrVec::StrVec(std::initializer_list<std::string> il){    range_initialize(il.begin(), il.end());}
```

## 练习13.41

> 在 `push_back` 中，我们为什么在 `construct` 调用中使用后置递增运算？如果使用前置递增运算的话，会发生什么？

解：

会出现 `unconstructed`。

## 练习13.42

> 在你的 `TextQuery` 和 `QueryResult` 类中用你的 `StrVec` 类代替`vector<string>`，以此来测试你的 `StrVec` 类。

解：

略

## 练习13.43

> 重写 `free` 成员，用 `for_each` 和 `lambda` 来代替 `for` 循环 `destroy` 元素。你更倾向于哪种实现，为什么？

解：

**重写**：

```cpp
for_each(elements, first_free, [this](std::string &rhs){ alloc.destroy(&rhs); });
```

更倾向于函数式写法。

## 练习13.44

> 编写标准库 `string` 类的简化版本，命名为 `String`。你的类应该至少有一个默认构造函数和一个接受 C 风格字符串指针参数的构造函数。使用 `allocator` 为你的 `String`类分配所需内存。

解：

头文件：

```cpp
#include <memory>class String{public:    String() : String("") { }    String(const char *);    String(const String&);    String& operator=(const String&);    ~String();    const char *c_str() const { return elements; }    size_t size() const { return end - elements; }    size_t length() const { return end - elements - 1; }private:    std::pair<char*, char*> alloc_n_copy(const char*, const char*);    void range_initializer(const char*, const char*);    void free();private:    char *elements;    char *end;    std::allocator<char> alloc;};
```

实现：

```cpp
#include "ex_13_44_47.h"#include <algorithm>#include <iostream>std::pair<char*, char*>String::alloc_n_copy(const char *b, const char *e){    auto str = alloc.allocate(e - b);    return{ str, std::uninitialized_copy(b, e, str) };}void String::range_initializer(const char *first, const char *last){    auto newstr = alloc_n_copy(first, last);    elements = newstr.first;    end = newstr.second;}String::String(const char *s){    char *sl = const_cast<char*>(s);    while (*sl)        ++sl;    range_initializer(s, ++sl);}String::String(const String& rhs){    range_initializer(rhs.elements, rhs.end);    std::cout << "copy constructor" << std::endl;}void String::free(){    if (elements) {        std::for_each(elements, end, [this](char &c){ alloc.destroy(&c); });        alloc.deallocate(elements, end - elements);    }}String::~String(){    free();}String& String::operator = (const String &rhs){    auto newstr = alloc_n_copy(rhs.elements, rhs.end);    free();    elements = newstr.first;    end = newstr.second;    std::cout << "copy-assignment" << std::endl;    return *this;}
```

测试：

```cpp
#include "ex13_44_47.h"#include <vector>#include <iostream>// Test reference to http://coolshell.cn/articles/10478.htmlvoid foo(String x){    std::cout << x.c_str() << std::endl;}void bar(const String& x){    std::cout << x.c_str() << std::endl;}String baz(){    String ret("world");    return ret;}int main(){    char text[] = "world";    String s0;    String s1("hello");    String s2(s0);    String s3 = s1;    String s4(text);    s2 = s1;    foo(s1);    bar(s1);    foo("temporary");    bar("temporary");    String s5 = baz();    std::vector<String> svec;    svec.reserve(8);    svec.push_back(s0);    svec.push_back(s1);    svec.push_back(s2);    svec.push_back(s3);    svec.push_back(s4);    svec.push_back(s5);    svec.push_back(baz());    svec.push_back("good job");    for (const auto &s : svec) {        std::cout << s.c_str() << std::endl;    }}
```

参考：[A trivial String class that designed for write-on-paper in an interview](https://github.com/chenshuo/recipes/blob/fcf9486f5155117fb8c36b6b0944c5486c71c421/string/StringTrivial.h)

## 练习13.45

> 解释左值引用和右值引用的区别？

解：

定义：

* 常规引用被称为左值引用
* 绑定到右值的引用被称为右值引用。

## 练习13.46

> 什么类型的引用可以绑定到下面的初始化器上？

```cpp
int f();vector<int> vi(100);int? r1 = f();int? r2 = vi[0];int? r3 = r1;int? r4 = vi[0] * f();
```

解：

```cpp
int f();vector<int> vi(100);int&& r1 = f();int& r2 = vi[0];int& r3 = r1;int&& r4 = vi[0] * f();
```

## 练习13.47

> 对你在练习13.44中定义的 `String`类，为它的拷贝构造函数和拷贝赋值运算符添加一条语句，在每次函数执行时打印一条信息。

解：

参考13.44。

## 练习13.48

> 定义一个`vector<String>` 并在其上多次调用 `push_back`。运行你的程序，并观察 `String` 被拷贝了多少次。

解：

```cpp
#include "ex_13_44_47.h"#include <vector>#include <iostream>// Test reference to http://coolshell.cn/articles/10478.htmlvoid foo(String x){    std::cout << x.c_str() << std::endl;}void bar(const String& x){    std::cout << x.c_str() << std::endl;}String baz(){    String ret("world");    return ret;}int main(){    char text[] = "world";    String s0;    String s1("hello");    String s2(s0);    String s3 = s1;    String s4(text);    s2 = s1;    foo(s1);    bar(s1);    foo("temporary");    bar("temporary");    String s5 = baz();    std::vector<String> svec;    svec.reserve(8);    svec.push_back(s0);    svec.push_back(s1);    svec.push_back(s2);    svec.push_back(s3);    svec.push_back(s4);    svec.push_back(s5);    svec.push_back(baz());    svec.push_back("good job");    for (const auto &s : svec) {        std::cout << s.c_str() << std::endl;    }}
```

## 练习13.49

> 为你的 `StrVec`、`String` 和 `Message` 类添加一个移动构造函数和一个移动赋值运算符。

解：

略

## 练习13.50

> 在你的 `String` 类的移动操作中添加打印语句，并重新运行13.6.1节的练习13.48中的程序，它使用了一个`vector<String>`，观察什么时候会避免拷贝。

解：

```cpp
String baz(){    String ret("world");    return ret; // first avoided}String s5 = baz(); // second avoided
```

## 练习13.51

> 虽然 `unique_ptr` 不能拷贝，但我们在12.1.5节中编写了一个 `clone` 函数，它以值的方式返回一个 `unique_ptr`。解释为什么函数是合法的，以及为什么它能正确工作。

解：

在这里是移动的操作而不是拷贝操作，因此是合法的。

## 练习13.52

> 详细解释第478页中的 `HasPtr` 对象的赋值发生了什么？特别是，一步一步描述 `hp`、`hp2` 以及 `HasPtr` 的赋值运算符中的参数 `rhs` 的值发生了什么变化。

解：

左值被拷贝，右值被移动。

## 练习13.53

> 从底层效率的角度看，`HasPtr` 的赋值运算符并不理想，解释为什么？为 `HasPtr` 实现一个拷贝赋值运算符和一个移动赋值运算符，并比较你的新的移动赋值运算符中执行的操作和拷贝并交换版本中的执行的操作。

解：

参考：https://stackoverflow.com/questions/21010371/why-is-it-not-efficient-to-use-a-single-assignment-operator-handling-both-copy-a

## 练习13.54

> 如果我们为 `HasPtr` 定义了移动赋值运算符，但未改变拷贝并交换运算符，会发生什么？编写代码验证你的答案。

解：

```cpp
error: ambiguous overload for 'operator=' (operand types are 'HasPtr' and 'std::remove_reference<HasPtr&>::type { aka HasPtr }')hp1 = std::move(*pH);^
```

## 练习13.55

> 为你的 `StrBlob` 添加一个右值引用版本的 `push_back`。

解：

```cpp
void push_back(string &&s) { data->push_back(std::move(s)); }
```

## 练习13.56

> 如果 `sorted` 定义如下，会发生什么？

```cpp
Foo Foo::sorted() const & {	Foo ret(*this);	return ret.sorted();}
```

解：

会产生递归并且最终溢出。

## 练习13.57

> 如果 `sorted`定义如下，会发生什么：

```cpp
Foo Foo::sorted() const & { return Foo(*this).sorted(); }
```

解：

没问题。会调用移动版本。

## 练习13.58

> 编写新版本的 `Foo` 类，其 `sorted` 函数中有打印语句，测试这个类，来验证你对前两题的答案是否正确。

解：

```cpp
#include <vector>#include <iostream>#include <algorithm>using std::vector; using std::sort;class Foo {public:    Foo sorted() &&;    Foo sorted() const &;private:    vector<int> data;};Foo Foo::sorted() && {    sort(data.begin(), data.end());    std::cout << "&&" << std::endl; // debug    return *this;}Foo Foo::sorted() const & {//    Foo ret(*this);//    sort(ret.data.begin(), ret.data.end());//    return ret;    std::cout << "const &" << std::endl; // debug//    Foo ret(*this);//    ret.sorted();     // Exercise 13.56//    return ret;    return Foo(*this).sorted(); // Exercise 13.57}int main(){    Foo().sorted(); // call "&&"    Foo f;    f.sorted(); // call "const &"}
```



# 第十四章 重载运算与类型转换

## 练习14.1

> 在什么情况下重载的运算符与内置运算符有所区别？在什么情况下重载的运算符又与内置运算符一样？

解：

我们可以直接调用重载运算符函数。重置运算符与内置运算符有一样的优先级与结合性。

## 练习14.2

> 为 `Sales_data` 编写重载的输入、输出、加法和复合赋值运算符。

解：

头文件：

```cpp
#include <string>
#include <iostream>

class Sales_data {
    friend std::istream& operator>>(std::istream&, Sales_data&); // input
    friend std::ostream& operator<<(std::ostream&, const Sales_data&); // output
    friend Sales_data operator+(const Sales_data&, const Sales_data&); // addition

public:
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }
    Sales_data() : Sales_data("", 0, 0.0f){ }
    Sales_data(const std::string &s) : Sales_data(s, 0, 0.0f){ }
    Sales_data(std::istream &is);

    Sales_data& operator+=(const Sales_data&); // compound-assignment
    std::string isbn() const { return bookNo; }

private:
    inline double avg_price() const;

    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

std::istream& operator>>(std::istream&, Sales_data&);
std::ostream& operator<<(std::ostream&, const Sales_data&);
Sales_data operator+(const Sales_data&, const Sales_data&);

inline double Sales_data::avg_price() const
{
    return units_sold ? revenue/units_sold : 0;
}
```

主函数：

```cpp
#include "ex_14_02.h"

Sales_data::Sales_data(std::istream &is) : Sales_data()
{
    is >> *this;
}

Sales_data& Sales_data::operator+=(const Sales_data &rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

std::istream& operator>>(std::istream &is, Sales_data &item)
{
    double price = 0.0;
    is >> item.bookNo >> item.units_sold >> price;
    if (is)
        item.revenue = price * item.units_sold;
    else
        item = Sales_data();
    return is;
}

std::ostream& operator<<(std::ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " " << item.revenue << " " << item.avg_price();
    return os;
}

Sales_data operator+(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum += rhs;
    return sum;
}
```

## 练习14.3

> `string` 和 `vector` 都定义了重载的`==`以比较各自的对象，假设 `svec1` 和 `svec2` 是存放 `string` 的 `vector`，确定在下面的表达式中分别使用了哪个版本的`==`？

```cpp
(a) "cobble" == "stone"
(b) svec1[0] == svec2[0]
(c) svec1 == svec2
(d) svec1[0] == "stone"
```

解：

* (a) 都不是。
* (b) `string` 
* (c) `vector` 
* (d) `string`

## 练习14.4

> 如何确定下列运算符是否应该是类的成员？

```cpp
(a) %
(b) %=
(c) ++
(d) ->
(e) <<
(f) &&
(g) ==
(h) ()
```

解：

* (a) 不需要是成员。
* (b) 是成员。
* (c) 是成员。
* (d) 必须是成员。
* (e) 不需要是成员。
* (f) 不需要是成员。
* (g) 不需要是成员。
* (h) 必须是成员。

## 练习14.5

> 在7.5.1节中的练习7.40中，编写了下列类中某一个的框架，请问在这个类中应该定义重载的运算符吗？如果是，请写出来。

```cpp
(a) Book
(b) Date
(c) Employee
(d) Vehicle
(e) Object
(f) Tree
```

解：

`Book`，应该重载。

头文件：

```cpp
#include <iostream>
#include <string>

class Book
{
	friend std::istream& operator>>(std::istream&, Book&);
	friend std::ostream& operator<<(std::ostream&, const Book&);
	friend bool operator==(const Book&, const Book&);
	friend bool operator!=(const Book&, const Book&);

public:
	Book() = default;
	Book(unsigned no, std::string name, std::string author, std::string pubdate) :no_(no), name_(name), author_(author), pubdate_(pubdate) {}
	Book(std::istream &in) { in >> *this; }

private:
	unsigned no_;
	std::string name_;
	std::string author_;
	std::string pubdate_;
};

std::istream& operator>>(std::istream&, Book&);
std::ostream& operator<<(std::ostream&, const Book&);
bool operator==(const Book&, const Book&);
bool operator!=(const Book&, const Book&);
```

实现：

```cpp
#include "ex_14_5.h"

std::istream& operator>>(std::istream &in, Book &book)
{
	in >> book.no_ >> book.name_ >> book.author_ >> book.pubdate_;
	if (!in)
		book = Book();
	return in;
}

std::ostream& operator<<(std::ostream &out, const Book &book)
{
	out << book.no_ << " " << book.name_ << " " << book.author_ << " " << book.pubdate_;
	return out;
}

bool operator==(const Book &lhs, const Book &rhs)
{
	return lhs.no_ == rhs.no_;
}

bool operator!=(const Book &lhs, const Book &rhs)
{
	return !(lhs == rhs);
}
```

测试：

```cpp
#include "ex_14_5.h"

int main()
{
	Book book1(123, "CP5", "Lippman", "2012");
	Book book2(123, "CP5", "Lippman", "2012");

	if (book1 == book2)
		std::cout << book1 << std::endl;
}
```

## 练习14.6

> 为你的 `Sales_data` 类定义输出运算符。

解：

参考14.2。

## 练习14.7

> 你在13.5节的练习中曾经编写了一个`String`类，为它定义一个输出运算符。

解：

头文件：

```cpp
#include <memory>#include <iostream>class String{	friend std::ostream& operator<<(std::ostream&, const String&);public:	String() : String("") {}	String(const char *);	String(const String&);	String& operator=(const String&);	~String();	const char *c_str() const { return elements; }	size_t size() const { return end - elements; }	size_t length() const { return end - elements - 1; }private:	std::pair<char*, char*> alloc_n_copy(const char*, const char*);	void range_initializer(const char*, const char*);	void free();private:	char *elements;	char *end;	std::allocator<char> alloc;};std::ostream& operator<<(std::ostream&, const String&);
```

实现：

```cpp
#include "ex_14_7.h"#include <algorithm>#include <iostream>std::pair<char*, char*>String::alloc_n_copy(const char *b, const char *e){	auto str = alloc.allocate(e - b);	return{ str, std::uninitialized_copy(b, e, str) };}void String::range_initializer(const char *first, const char *last){	auto newstr = alloc_n_copy(first, last);	elements = newstr.first;	end = newstr.second;}String::String(const char *s){	char *sl = const_cast<char*>(s);	while (*sl)		++sl;	range_initializer(s, ++sl);}String::String(const String& rhs){	range_initializer(rhs.elements, rhs.end);	std::cout << "copy constructor" << std::endl;}void String::free(){	if (elements)	{		std::for_each(elements, end, [this](char &c) { alloc.destroy(&c); });		alloc.deallocate(elements, end - elements);	}}String::~String(){	free();}String& String::operator = (const String &rhs){	auto newstr = alloc_n_copy(rhs.elements, rhs.end);	free();	elements = newstr.first;	end = newstr.second;	std::cout << "copy-assignment" << std::endl;	return *this;}std::ostream& operator<<(std::ostream &os, const String &s){	char *c = const_cast<char*>(s.c_str());	while (*c)		os << *c++;	return os;}
```

测试：

```cpp
#include "ex_14_7.h"int main(){	String str("Hello World");	std::cout << str << std::endl;}
```

## 练习14.8

> 你在7.5.1节中的练习中曾经选择并编写了一个类，为它定义一个输出运算符。

解：

参考14.5。

## 练习14.9

> 为你的 `Sales_data` 类定义输入运算符。

解：

参考14.2。

## 练习14.10

> 对于 `Sales_data` 的输入运算符来说如果给定了下面的输入将发生什么情况？

```cpp
(a) 0-201-99999-9 10 24.95(b) 10 24.95 0-210-99999-9
```

解：

* (a) 格式正确。
* (b) 不合法的输入。因为程序试图将 `0-210-99999-9` 转换为 `float`。

## 练习14.11

> 下面的 `Sales_data` 输入运算符存在错误吗？如果有，请指出来。对于这个输入运算符如果仍然给定上个练习的输入将会发生什么情况？

```cpp
istream& operator>>(istream& in, Sales_data& s){	double price;	in >> s.bookNo >> s.units_sold >> price;	s.revence = s.units_sold >> price;	return in;}
```

解：

没有输入检查，什么也不会发生。

## 练习14.12

> 你在7.5.1节的练习中曾经选择并编写了一个类，为它定义一个输入运算符并确保该运算符可以处理输入错误。

解：

参考14.5。

## 练习14.13

> 你认为 `Sales_data` 类还应该支持哪些其他算术运算符？如果有的话，请给出它们的定义。

解：

没有其他了。

## 练习14.14

> 你觉得为什么调用 `operator+=` 来定义`operator+` 比其他方法更有效？

解：

因为用 `operator+=` 会避免使用一个临时对象，而使得更有效。

## 练习14.15

> 你在7.5.1节的练习7.40中曾经选择并编写了一个类，你认为它应该含有其他算术运算符吗？如果是，请实现它们；如果不是，解释原因。

解：

头文件：

```cpp
#include <iostream>#include <string>class Book{	friend std::istream& operator>>(std::istream&, Book&);	friend std::ostream& operator<<(std::ostream&, const Book&);	friend bool operator==(const Book&, const Book&);	friend bool operator!=(const Book&, const Book&);	friend bool operator<(const Book&, const Book&);	friend bool operator>(const Book&, const Book&);	friend Book operator+(const Book&, const Book&);public:	Book() = default;	Book(unsigned no, std::string name, std::string author, std::string pubdate, unsigned number) :no_(no), name_(name), author_(author), pubdate_(pubdate), number_(number) {}	Book(std::istream &in) { in >> *this; }	Book& operator+=(const Book&);private:	unsigned no_;	std::string name_;	std::string author_;	std::string pubdate_;	unsigned number_;};std::istream& operator>>(std::istream&, Book&);std::ostream& operator<<(std::ostream&, const Book&);bool operator==(const Book&, const Book&);bool operator!=(const Book&, const Book&);bool operator<(const Book&, const Book&);bool operator>(const Book&, const Book&);Book operator+(const Book&, const Book&);
```

实现：

```cpp
#include "ex_14_15.h"std::istream& operator>>(std::istream &in, Book &book){	in >> book.no_ >> book.name_ >> book.author_ >> book.pubdate_ >> book.number_;	return in;}std::ostream& operator<<(std::ostream &out, const Book &book){	out << book.no_ << " " << book.name_ << " " << book.author_ << " " << book.pubdate_ << " " << book.number_ << std::endl;	return out;}bool operator==(const Book &lhs, const Book &rhs){	return lhs.no_ == rhs.no_;}bool operator!=(const Book &lhs, const Book &rhs){	return !(lhs == rhs);}bool operator<(const Book &lhs, const Book &rhs){	return lhs.no_ < rhs.no_;}bool operator>(const Book &lhs, const Book &rhs){	return rhs < lhs;}Book& Book::operator+=(const Book &rhs){	if (rhs == *this)		this->number_ += rhs.number_;	return *this;}Book operator+(const Book &lhs, const Book &rhs){	Book book = lhs;	book += rhs;	return book;}
```

测试：

```cpp
#include "ex_14_15.h"int main(){	Book cp5_1(12345, "CP5", "Lippmen", "2012", 2);	Book cp5_2(12345, "CP5", "Lippmen", "2012", 4);	std::cout << cp5_1 + cp5_2 << std::endl;}
```

## 练习14.16

> 为你的 `StrBlob` 类、`StrBlobPtr` 类、`StrVec` 类和 `String` 类分别定义相等运算符和不相等运算符。

解：

略

## 练习14.17

> 你在7.5.1节中的练习7.40中曾经选择并编写了一个类，你认为它应该含有相等运算符吗？如果是，请实现它；如果不是，解释原因。

解：

参考14.15。

## 练习14.18

> 为你的 `StrBlob` 类、`StrBlobPtr` 类、`StrVec` 类和 `String` 类分别定义关系运算符。

解：

略

## 练习14.19

> 你在7.5.1节的练习7.40中曾经选择并编写了一个类，你认为它应该含有关系运算符吗？如果是，请实现它；如果不是，解释原因。

解：

参考14.15。

## 练习14.20

> 为你的 `Sales_data` 类定义加法和复合赋值运算符。

解：

参考14.2。

## 练习14.21

> 编写 `Sales_data` 类的`+` 和`+=` 运算符，使得 `+` 执行实际的加法操作而 `+=` 调用`+`。相比14.3节和14.4节对这两个运算符的定义，本题的定义有何缺点？试讨论之。

解：

缺点：使用了一个 `Sales_data` 的临时对象，但它并不是必须的。

## 练习14.22

> 定义赋值运算符的一个新版本，使得我们能把一个表示 `ISBN` 的 `string` 赋给一个 `Sales_data` 对象。

解：

头文件：

```cpp
#include <string>#include <iostream>class Sales_data{	friend std::istream& operator>>(std::istream&, Sales_data&);	friend std::ostream& operator<<(std::ostream&, const Sales_data&);	friend Sales_data operator+(const Sales_data&, const Sales_data&);public:	Sales_data(const std::string &s, unsigned n, double p) :bookNo(s), units_sold(n), revenue(n*p) {}	Sales_data() : Sales_data("", 0, 0.0f) {}	Sales_data(const std::string &s) : Sales_data(s, 0, 0.0f) {}	Sales_data(std::istream &is);	Sales_data& operator=(const std::string&);	Sales_data& operator+=(const Sales_data&);	std::string isbn() const { return bookNo; }private:	inline double avg_price() const;	std::string bookNo;	unsigned units_sold = 0;	double revenue = 0.0;};std::istream& operator>>(std::istream&, Sales_data&);std::ostream& operator<<(std::ostream&, const Sales_data&);Sales_data operator+(const Sales_data&, const Sales_data&);inline double Sales_data::avg_price() const{	return units_sold ? revenue / units_sold : 0;}
```

实现：

```cpp
#include "ex_14_22.h"Sales_data::Sales_data(std::istream &is) : Sales_data(){	is >> *this;}Sales_data& Sales_data::operator+=(const Sales_data &rhs){	units_sold += rhs.units_sold;	revenue += rhs.revenue;	return *this;}std::istream& operator>>(std::istream &is, Sales_data &item){	double price = 0.0;	is >> item.bookNo >> item.units_sold >> price;	if (is)		item.revenue = price * item.units_sold;	else		item = Sales_data();	return is;}std::ostream& operator<<(std::ostream &os, const Sales_data &item){	os << item.isbn() << " " << item.units_sold << " " << item.revenue << " " << item.avg_price();	return os;}Sales_data operator+(const Sales_data &lhs, const Sales_data &rhs){	Sales_data sum = lhs;	sum += rhs;	return sum;}Sales_data& Sales_data::operator=(const std::string &isbn){	*this = Sales_data(isbn);	return *this;}
```

测试：

```cpp
#include "ex_4_22.h"int main(){	std::string strCp5("C++ Primer 5th");	Sales_data cp5 = strCp5;	std::cout << cp5 << std::endl;}
```

## 练习14.23

> 为你的`StrVec` 类定义一个 `initializer_list` 赋值运算符。

解：

头文件：

```cpp
#include <memory>#include <string>#include <initializer_list>#ifndef _MSC_VER#define NOEXCEPT noexcept#else#define NOEXCEPT#endifclass StrVec{	friend bool operator==(const StrVec&, const StrVec&);	friend bool operator!=(const StrVec&, const StrVec&);	friend bool operator< (const StrVec&, const StrVec&);	friend bool operator> (const StrVec&, const StrVec&);	friend bool operator<=(const StrVec&, const StrVec&);	friend bool operator>=(const StrVec&, const StrVec&);public:	StrVec() : elements(nullptr), first_free(nullptr), cap(nullptr) {}	StrVec(std::initializer_list<std::string>);	StrVec(const StrVec&);	StrVec& operator=(const StrVec&);	StrVec(StrVec&&) NOEXCEPT;	StrVec& operator=(StrVec&&)NOEXCEPT;	~StrVec();	StrVec& operator=(std::initializer_list<std::string>);	void push_back(const std::string&);	size_t size() const { return first_free - elements; }	size_t capacity() const { return cap - elements; }	std::string *begin() const { return elements; }	std::string *end() const { return first_free; }	std::string& at(size_t pos) { return *(elements + pos); }	const std::string& at(size_t pos) const { return *(elements + pos); }	void reserve(size_t new_cap);	void resize(size_t count);	void resize(size_t count, const std::string&);private:	std::pair<std::string*, std::string*> alloc_n_copy(const std::string*, const std::string*);	void free();	void chk_n_alloc() { if (size() == capacity()) reallocate(); }	void reallocate();	void alloc_n_move(size_t new_cap);	void range_initialize(const std::string*, const std::string*);private:	std::string *elements;	std::string *first_free;	std::string *cap;	std::allocator<std::string> alloc;};bool operator==(const StrVec&, const StrVec&);bool operator!=(const StrVec&, const StrVec&);bool operator< (const StrVec&, const StrVec&);bool operator> (const StrVec&, const StrVec&);bool operator<=(const StrVec&, const StrVec&);bool operator>=(const StrVec&, const StrVec&);
```

实现：

```cpp
#include "ex_14_23.h"#include <algorithm>void StrVec::push_back(const std::string &s){	chk_n_alloc();	alloc.construct(first_free++, s);}std::pair<std::string*, std::string*>StrVec::alloc_n_copy(const std::string *b, const std::string *e){	auto data = alloc.allocate(e - b);	return{ data, std::uninitialized_copy(b, e, data) };}void StrVec::free(){	if (elements)	{		for_each(elements, first_free, [this](std::string &rhs) { alloc.destroy(&rhs); });		alloc.deallocate(elements, cap - elements);	}}void StrVec::range_initialize(const std::string *first, const std::string *last){	auto newdata = alloc_n_copy(first, last);	elements = newdata.first;	first_free = cap = newdata.second;}StrVec::StrVec(const StrVec &rhs){	range_initialize(rhs.begin(), rhs.end());}StrVec::StrVec(std::initializer_list<std::string> il){	range_initialize(il.begin(), il.end());}StrVec::~StrVec(){	free();}StrVec& StrVec::operator = (const StrVec &rhs){	auto data = alloc_n_copy(rhs.begin(), rhs.end());	free();	elements = data.first;	first_free = cap = data.second;	return *this;}void StrVec::alloc_n_move(size_t new_cap){	auto newdata = alloc.allocate(new_cap);	auto dest = newdata;	auto elem = elements;	for (size_t i = 0; i != size(); ++i)		alloc.construct(dest++, std::move(*elem++));	free();	elements = newdata;	first_free = dest;	cap = elements + new_cap;}void StrVec::reallocate(){	auto newcapacity = size() ? 2 * size() : 1;	alloc_n_move(newcapacity);}void StrVec::reserve(size_t new_cap){	if (new_cap <= capacity()) return;	alloc_n_move(new_cap);}void StrVec::resize(size_t count){	resize(count, std::string());}void StrVec::resize(size_t count, const std::string &s){	if (count > size())	{		if (count > capacity()) reserve(count * 2);		for (size_t i = size(); i != count; ++i)			alloc.construct(first_free++, s);	}	else if (count < size())	{		while (first_free != elements + count)			alloc.destroy(--first_free);	}}StrVec::StrVec(StrVec &&s) NOEXCEPT : elements(s.elements), first_free(s.first_free), cap(s.cap){	// leave s in a state in which it is safe to run the destructor.	s.elements = s.first_free = s.cap = nullptr;}StrVec& StrVec::operator = (StrVec &&rhs) NOEXCEPT{	if (this != &rhs)	{		free();		elements = rhs.elements;		first_free = rhs.first_free;		cap = rhs.cap;		rhs.elements = rhs.first_free = rhs.cap = nullptr;	}	return *this;}bool operator==(const StrVec &lhs, const StrVec &rhs){	return (lhs.size() == rhs.size() && std::equal(lhs.begin(), lhs.end(), rhs.begin()));}bool operator!=(const StrVec &lhs, const StrVec &rhs){	return !(lhs == rhs);}bool operator<(const StrVec &lhs, const StrVec &rhs){	return std::lexicographical_compare(lhs.begin(), lhs.end(), rhs.begin(), rhs.end());}bool operator>(const StrVec &lhs, const StrVec &rhs){	return rhs < lhs;}bool operator<=(const StrVec &lhs, const StrVec &rhs){	return !(rhs < lhs);}bool operator>=(const StrVec &lhs, const StrVec &rhs){	return !(lhs < rhs);}StrVec& StrVec::operator=(std::initializer_list<std::string> il){	*this = StrVec(il);	return *this;}
```

测试：

```cpp
#include "ex_14_23.h"#include <vector>#include <iostream>int main(){	StrVec vec;	vec.reserve(6);	std::cout << "capacity(reserve to 6): " << vec.capacity() << std::endl;	vec.reserve(4);	std::cout << "capacity(reserve to 4): " << vec.capacity() << std::endl;	vec.push_back("hello");	vec.push_back("world");	vec.resize(4);	for (auto i = vec.begin(); i != vec.end(); ++i)		std::cout << *i << std::endl;	std::cout << "-EOF-" << std::endl;	vec.resize(1);	for (auto i = vec.begin(); i != vec.end(); ++i)		std::cout << *i << std::endl;	std::cout << "-EOF-" << std::endl;	StrVec vec_list{ "hello", "world", "pezy" };	for (auto i = vec_list.begin(); i != vec_list.end(); ++i)		std::cout << *i << " ";	std::cout << std::endl;	// Test operator==	const StrVec const_vec_list = { "hello", "world", "pezy" };	if (vec_list == const_vec_list)	for (const auto &str : const_vec_list)		std::cout << str << " ";	std::cout << std::endl;	// Test operator<	const StrVec const_vec_list_small = { "hello", "pezy", "ok" };	std::cout << (const_vec_list_small < const_vec_list) << std::endl;}
```

## 练习14.24

> 你在7.5.1节的练习7.40中曾经选择并编写了一个类，你认为它应该含有拷贝赋值和移动赋值运算符吗？如果是，请实现它们。

解：

头文件：

```cpp
#ifndef DATE_H#define DATE_H#ifndef _MSC_VER#define NOEXCEPT noexcept#else#define NOEXCEPT#endif#include <iostream>#include <vector>class Date{	friend  bool            operator ==(const Date& lhs, const Date& rhs);	friend  bool            operator < (const Date &lhs, const Date &rhs);	friend  bool            check(const Date &d);	friend  std::ostream&   operator <<(std::ostream& os, const Date& d);public:	typedef std::size_t Size;	// default constructor	Date() = default;	// constructor taking Size as days	explicit Date(Size days);	// constructor taking three Size	Date(Size d, Size m, Size y) : day(d), month(m), year(y) {}	// constructor taking iostream	Date(std::istream &is, std::ostream &os);	// copy constructor	Date(const Date& d);	// move constructor	Date(Date&& d) NOEXCEPT;	// copy operator=	Date& operator= (const Date& d);	// move operator=	Date& operator= (Date&& rhs) NOEXCEPT;	// destructor  --  in this case, user-defined destructor is not nessary.	~Date() { std::cout << "destroying\n"; }	// members	Size toDays() const;  //not implemented yet.	Date& operator +=(Size offset);	Date& operator -=(Size offset);private:	Size    day = 1;	Size    month = 1;	Size    year = 0;};static const Date::Size YtoD_400 = 146097;    //365*400 + 400/4 -3 == 146097static const Date::Size YtoD_100 = 36524;    //365*100 + 100/4 -1 ==  36524static const Date::Size YtoD_4 = 1461;    //365*4 + 1          ==   1461static const Date::Size YtoD_1 = 365;    //365// normal yearstatic const std::vector<Date::Size> monthsVec_n ={ 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };// leap yearstatic const std::vector<Date::Size> monthsVec_l ={ 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };// non-member operators:  <<  >>  -   ==  !=  <   <=  >   >=//std::ostream&operator <<(std::ostream& os, const Date& d);std::istream&operator >>(std::istream& is, Date& d);intoperator - (const Date& lhs, const Date& rhs);booloperator ==(const Date& lhs, const Date& rhs);booloperator !=(const Date& lhs, const Date& rhs);booloperator < (const Date& lhs, const Date& rhs);booloperator <=(const Date& lhs, const Date& rhs);booloperator  >(const Date& lhs, const Date& rhs);booloperator >=(const Date& lhs, const Date& rhs);Dateoperator - (const Date& lhs, Date::Size  rhs);Dateoperator  +(const Date& lhs, Date::Size  rhs);//  utillities:bool check(const Date &d);inline boolisLeapYear(Date::Size y);// check if the date object passed in is validinline boolcheck(const Date &d){	if (d.month == 0 || d.month >12)		return false;	else	{		//    month == 1 3 5 7 8 10 12		if (d.month == 1 || d.month == 3 || d.month == 5 || d.month == 7 ||			d.month == 8 || d.month == 10 || d.month == 12)		{			if (d.day == 0 || d.day > 31) return false;			else				return true;		}		else		{			//    month == 4 6 9 11			if (d.month == 4 || d.month == 6 || d.month == 9 || d.month == 11)			{				if (d.day == 0 || d.day > 30) return false;				else					return true;			}			else			{				//    month == 2				if (isLeapYear(d.year))				{					if (d.day == 0 || d.day >29)  return false;					else						return true;				}				else				{					if (d.day == 0 || d.day >28)  return false;					else						return true;				}			}		}	}}inline boolisLeapYear(Date::Size y){	if (!(y % 400))	{		return true;	}	else	{		if (!(y % 100))		{			return false;		}		else			return !(y % 4);	}}#endif // DATE_H
```

实现：

```cpp
#include "ex_14_24.h"#include <algorithm>// constructor taking Size as days// the argument must be within (0, 2^32)Date::Date(Size days){	// calculate the year	Size y400 = days / YtoD_400;	Size y100 = (days - y400*YtoD_400) / YtoD_100;	Size y4 = (days - y400*YtoD_400 - y100*YtoD_100) / YtoD_4;	Size y = (days - y400*YtoD_400 - y100*YtoD_100 - y4*YtoD_4) / 365;	Size d = days - y400*YtoD_400 - y100*YtoD_100 - y4*YtoD_4 - y * 365;	this->year = y400 * 400 + y100 * 100 + y4 * 4 + y;	// check if leap and choose the months vector accordingly	std::vector<Size>currYear		= isLeapYear(this->year) ? monthsVec_l : monthsVec_n;	// calculate day and month using find_if + lambda	Size D_accumu = 0, M_accumu = 0;	// @bug    fixed:  the variabbles above hade been declared inside the find_if as static	//                 which caused the bug. It works fine now after being move outside.	std::find_if(currYear.cbegin(), currYear.cend(), [&](Size m)	{		D_accumu += m;		M_accumu++;		if (d < D_accumu)		{			this->month = M_accumu;			this->day = d + m - D_accumu;			return true;		}		else			return false;	});}// construcotr taking iostreamDate::Date(std::istream &is, std::ostream &os){	is >> day >> month >> year;	if (is)	{		if (check(*this)) return;		else		{			os << "Invalid input! Object is default initialized.";			*this = Date();		}	}	else	{		os << "Invalid input! Object is default initialized.";		*this = Date();	}}// copy constructorDate::Date(const Date &d) :day(d.day), month(d.month), year(d.year){}// move constructorDate::Date(Date&& d) NOEXCEPT :day(d.day), month(d.month), year(d.year){ std::cout << "copy moving"; }// copy operator=Date &Date::operator= (const Date &d){	this->day = d.day;	this->month = d.month;	this->year = d.year;	return *this;}// move operator=Date &Date::operator =(Date&& rhs) NOEXCEPT{	if (this != &rhs)	{		this->day = rhs.day;		this->month = rhs.month;		this->year = rhs.year;	}	std::cout << "moving =";	return *this;}// conver to daysDate::Size Date::toDays() const{	Size result = this->day;	// check if leap and choose the months vector accordingly	std::vector<Size>currYear		= isLeapYear(this->year) ? monthsVec_l : monthsVec_n;	// calculate result + days by months	for (auto it = currYear.cbegin(); it != currYear.cbegin() + this->month - 1; ++it)		result += *it;	// calculate result + days by years	result += (this->year / 400)      * YtoD_400;	result += (this->year % 400 / 100)  * YtoD_100;	result += (this->year % 100 / 4)    * YtoD_4;	result += (this->year % 4)        * YtoD_1;	return result;}// member operators:   +=  -=Date &Date::operator +=(Date::Size offset){	*this = Date(this->toDays() + offset);	return *this;}Date &Date::operator -=(Date::Size offset){	if (this->toDays() > offset)		*this = Date(this->toDays() - offset);	else		*this = Date();	return *this;}// non-member operators:  <<  >>  -   ==  !=  <   <=  >   >=std::ostream&operator <<(std::ostream& os, const Date& d){	os << d.day << " " << d.month << " " << d.year;	return os;}std::istream&operator >>(std::istream& is, Date& d){	if (is)	{		Date input = Date(is, std::cout);		if (check(input))    d = input;	}	return is;}int operator -(const Date &lhs, const Date &rhs){	return lhs.toDays() - rhs.toDays();}bool operator ==(const Date &lhs, const Date &rhs){	return (lhs.day == rhs.day) &&		(lhs.month == rhs.month) &&		(lhs.year == rhs.year);}bool operator !=(const Date &lhs, const Date &rhs){	return !(lhs == rhs);}bool operator < (const Date &lhs, const Date &rhs){	return lhs.toDays() < rhs.toDays();}bool operator <=(const Date &lhs, const Date &rhs){	return (lhs < rhs) || (lhs == rhs);}bool operator >(const Date &lhs, const Date &rhs){	return !(lhs <= rhs);}bool operator >=(const Date &lhs, const Date &rhs){	return !(lhs < rhs);}Date operator - (const Date &lhs, Date::Size rhs){                                       //  ^^^ rhs must not be larger than 2^32-1	// copy lhs	Date result(lhs);	result -= rhs;	return result;}Date operator + (const Date &lhs, Date::Size rhs){                                       //  ^^^ rhs must not be larger than 2^32-1	// copy lhs	Date result(lhs);	result += rhs;	return result;}
```

测试：

```cpp
#include "ex_14_24.h"#include <iostream>int main(){	Date lhs(9999999), rhs(1);	std::cout << (lhs -= 12000) << "\n";	return 0;}
```

## 练习14.25

> 上题的这个类还需要定义其他赋值运算符吗？如果是，请实现它们；同时说明运算对象应该是什么类型并解释原因。

解：

是。如上题。

## 练习14.26

> 为你的 `StrBlob` 类、`StrBlobPtr` 类、`StrVec` 类和 `String` 类定义下标运算符。

解：

略

## 练习14.27

> 为你的 `StrBlobPtr` 类添加递增和递减运算符。

解：

只显示添加的代码：

```cpp
class StrBlobPtr {public:    string& deref() const;    StrBlobPtr& operator++();    StrBlobPtr& operator--();    StrBlobPtr operator++(int);    StrBlobPtr operator--(int);    StrBlobPtr& operator+=(size_t);    StrBlobPtr& operator-=(size_t);    StrBlobPtr operator+(size_t) const;    StrBlobPtr operator-(size_t) const;};inline StrBlobPtr& StrBlobPtr::operator++(){    check(curr, "increment past end of StrBlobPtr");    ++curr;    return *this;}inline StrBlobPtr& StrBlobPtr::operator--(){    check(--curr, "decrement past begin of StrBlobPtr");    return *this;}inline StrBlobPtr StrBlobPtr::operator++(int){    StrBlobPtr ret = *this;    ++*this;    return ret;}inline StrBlobPtr StrBlobPtr::operator--(int){    StrBlobPtr ret = *this;    --*this;    return ret;}inline StrBlobPtr& StrBlobPtr::operator+=(size_t n){    curr += n;    check(curr, "increment past end of StrBlobPtr");    return *this;}inline StrBlobPtr& StrBlobPtr::operator-=(size_t n){    curr -= n;    check(curr, "increment past end of StrBlobPtr");    return *this;}inline StrBlobPtr StrBlobPtr::operator+(size_t n) const{    StrBlobPtr ret = *this;    ret += n;    return ret;}inline StrBlobPtr StrBlobPtr::operator-(size_t n) const{    StrBlobPtr ret = *this;    ret -= n;    return ret;}
```

## 练习14.28

> 为你的 `StrBlobPtr` 类添加加法和减法运算符，使其可以实现指针的算术运算。

解：

参考14.27。

## 练习14.29

> 为什么不定义`const` 版本的递增和递减运算符？

解：

因为递增和递减会改变对象本身，所以定义 `const` 版本的毫无意义。

## 练习14.30

> 为你的 `StrBlobPtr` 类和在12.1.6节练习12.22中定义的 `ConstStrBlobPtr` 的类分别添加解引用运算符和箭头运算符。注意：因为 `ConstStrBlobPtr` 的数据成员指向`const vector`，所以`ConstStrBlobPtr` 中的运算符必须返回常量引用。

解：

略。

## 练习14.31

> 我们的 `StrBlobPtr` 类没有定义拷贝构造函数、赋值运算符以及析构函数，为什么？

解：

因为使用合成的足够了。

## 练习14.32

> 定义一个类令其含有指向 `StrBlobPtr` 对象的指针，为这个类定义重载的箭头运算符。

解：

头文件：

```cpp
class StrBlobPtr;class StrBlobPtr_pointer{public:    StrBlobPtr_pointer() = default;    StrBlobPtr_pointer(StrBlobPtr* p) : pointer(p) { }    StrBlobPtr& operator *() const;    StrBlobPtr* operator->() const;private:    StrBlobPtr* pointer = nullptr;};
```

实现：

```cpp
#include "ex_14_32.h"#include "ex_14_30_StrBlob.h"#include <iostream>StrBlobPtr&StrBlobPtr_pointer::operator *() const{    return *pointer;}StrBlobPtr*StrBlobPtr_pointer::operator ->() const{    return pointer;}
```

## 练习14.33

> 一个重载的函数调用运算符应该接受几个运算对象？

解：

一个重载的函数调用运算符接受的运算对象应该和该运算符拥有的操作数一样多。

## 练习14.34

> 定义一个函数对象类，令其执行`if-then-else` 的操作：该类的调用运算符接受三个形参，它首先检查第一个形参，如果成功返回第二个形参值；如果不成功返回第三个形参的值。

解：

```cpp
struct Test{    int operator()(bool b, int iA, int iB)     {        return b ? iA : iB;    }};
```

## 练习14.35

> 编写一个类似于 `PrintString` 的类，令其从 `istream` 中读取一行输入，然后返回一个表示我们所读内容的`string`。如果读取失败，返回空`string`。

解：

```cpp
#include <iostream>#include <string>class GetInput{public:	GetInput(std::istream &i = std::cin) : is(i) {}	std::string operator()() const	{		std::string str;		std::getline(is, str);		return is ? str : std::string();	}private:	std::istream &is;};int main(){	GetInput getInput;	std::cout << getInput() << std::endl;}
```

## 练习14.36

> 使用前一个练习定义的类读取标准输入，将每一行保存为 `vector` 的一个元素。

解：

```cpp
#include <iostream>#include <string>#include <vector>class GetInput{public:	GetInput(std::istream &i = std::cin) : is(i) {}	std::string operator()() const	{		std::string str;		std::getline(is, str);		return is ? str : std::string();	}private:	std::istream &is;};int main(){	GetInput getInput;	std::vector<std::string> vec;	for (std::string tmp; !(tmp = getInput()).empty();) vec.push_back(tmp);	for (const auto &str : vec) std::cout << str << " ";	std::cout << std::endl;}
```

## 练习14.37

> 编写一个类令其检查两个值是否相等。使用该对象及标准库算法编写程序，令其替换某个序列中具有给定值的所有实例。

解：

```cpp
#include <iostream>#include <algorithm>#include <vector>class IsEqual{	int value;public:	IsEqual(int v) : value(v) {}	bool operator()(int elem)	{		return elem == value;	}};int main(){	std::vector<int> vec = { 3, 2, 1, 4, 3, 7, 8, 6 };	std::replace_if(vec.begin(), vec.end(), IsEqual(3), 5);	for (int i : vec) std::cout << i << " ";	std::cout << std::endl;}
```

## 练习14.38

> 编写一个类令其检查某个给定的 `string` 对象的长度是否与一个阈值相等。使用该对象编写程序，统计并报告在输入的文件中长度为1的单词有多少个，长度为2的单词有多少个、......、长度为10的单词有多少个。

解：

```cpp
#include <iostream>#include <string>#include <fstream>#include <vector>#include <map>struct IsInRange{	IsInRange(std::size_t lower, std::size_t upper)	:_lower(lower), _upper(upper)	{}	bool operator()(std::string const& str) const	{		return str.size() >= _lower && str.size() <= _upper;	}	std::size_t lower_limit() const	{		return _lower;	}	std::size_t upper_limit() const	{		return _upper;	}private:	std::size_t _lower;	std::size_t _upper;};int main(){	//create predicates with various upper limits.	std::size_t lower = 1;	auto uppers = { 3u, 4u, 5u, 6u, 7u, 8u, 9u, 10u, 11u, 12u, 13u, 14u };	std::vector<IsInRange> predicates;	for (auto upper : uppers)		predicates.push_back(IsInRange{ lower, upper });	//create count_table to store counts 	std::map<std::size_t, std::size_t> count_table;	for (auto upper : uppers)		count_table[upper] = 0;	//read file and count	std::ifstream fin("storyDataFile.txt");	for (std::string word; fin >> word; /* */)	for (auto is_size_in_range : predicates)	if (is_size_in_range(word))		++count_table[is_size_in_range.upper_limit()];	//print	for (auto pair : count_table)		std::cout << "count in range [1, " << pair.first << "] : " << pair.second << std::endl;	return 0;}
```

## 练习14.39

> 修改上一题的程序令其报告长度在1到9之间的单词有多少个、长度在10以上的单词有多少个。

解：

参考14.38。

## 练习14.40

> 重新编写10.3.2节的`biggies` 函数，使用函数对象替换其中的 `lambda` 表达式。

解：

```cpp
#include <vector>#include <string>#include <iostream>#include <algorithm>using namespace std;class ShorterString{public:	bool operator()(string const& s1, string const& s2) const { return s1.size() < s2.size(); }};class BiggerEqual{	size_t sz_;public:	BiggerEqual(size_t sz) : sz_(sz) {}	bool operator()(string const& s) { return s.size() >= sz_; }};class Print{public:	void operator()(string const& s) { cout << s << " "; }};string make_plural(size_t ctr, string const& word, string const& ending){	return (ctr > 1) ? word + ending : word;}void elimDups(vector<string> &words){	sort(words.begin(), words.end());	auto end_unique = unique(words.begin(), words.end());	words.erase(end_unique, words.end());}void biggies(vector<string> &words, vector<string>::size_type sz){	elimDups(words);	stable_sort(words.begin(), words.end(), ShorterString());	auto wc = find_if(words.begin(), words.end(), BiggerEqual(sz));	auto count = words.end() - wc;	cout << count << " " << make_plural(count, "word", "s") << " of length " << sz << " or longer" << endl;	for_each(wc, words.end(), Print());	cout << endl;}int main(){	vector<string> vec{ "fox", "jumps", "over", "quick", "red", "red", "slow", "the", "turtle" };	biggies(vec, 4);}
```

## 练习14.41

> 你认为 C++ 11 标准为什么要增加 `lambda`？对于你自己来说，什么情况下会使用 `lambda`，什么情况下会使用类？

解：

使用 `lambda` 是非常方便的，当需要使用一个函数，而这个函数不常使用并且简单时，使用`lambda` 是比较方便的选择。

## 练习14.42

> 使用标准库函数对象及适配器定义一条表达式，令其

```
(a) 统计大于1024的值有多少个。 (b) 找到第一个不等于pooh的字符串。(c) 将所有的值乘以2。
```

解：

```cpp
std::count_if(ivec.cbegin(), ivec.cend(), std::bind(std::greater<int>(), _1, 1024));std::find_if(svec.cbegin(), svec.cend(), std::bind(std::not_equal_to<std::string>(), _1, "pooh"));std::transform(ivec.begin(), ivec.end(), ivec.begin(), std::bind(std::multiplies<int>(), _1, 2));
```

## 练习14.43

> 使用标准库函数对象判断一个给定的`int`值是否能被 `int` 容器中的所有元素整除。

解：

```cpp
#include <iostream>#include <string>#include <functional>#include <algorithm>int main(){	auto data = { 2, 3, 4, 5 };	int input;	std::cin >> input;	std::modulus<int> mod;	auto predicator = [&](int i) { return 0 == mod(input, i); };	auto is_divisible = std::any_of(data.begin(), data.end(), predicator);	std::cout << (is_divisible ? "Yes!" : "No!") << std::endl;	return 0;}
```

## 练习14.44

> 编写一个简单的桌面计算器使其能处理二元运算。

解：

```cpp
#include <iostream>#include <string>#include <map> #include <functional> int add(int i, int j) { return i + j; }auto mod = [](int i, int j) { return i % j; };struct Div { int operator ()(int i, int j) const { return i / j; } };auto binops = std::map<std::string, std::function<int(int, int)>>{	{ "+", add },                               // function pointer 	{ "-", std::minus<int>() },                 // library functor 	{ "/", Div() },                             // user-defined functor 	{ "*", [](int i, int j) { return i*j; } },  // unnamed lambda 	{ "%", mod }                                // named lambda object };int main(){	while (std::cout << "Pls enter as: num operator num :\n", true)	{		int lhs, rhs; std::string op;		std::cin >> lhs >> op >> rhs;		std::cout << binops[op](lhs, rhs) << std::endl;	}	return 0;}
```

## 练习14.45

> 编写类型转换运算符将一个 `Sales_data` 对象分别转换成 `string` 和 `double`，你认为这些运算符的返回值应该是什么？

解：

头文件：

```cpp
#include <string>#include <iostream>class Sales_data{	friend std::istream& operator>>(std::istream&, Sales_data&);	friend std::ostream& operator<<(std::ostream&, const Sales_data&);	friend Sales_data operator+(const Sales_data&, const Sales_data&);public:	Sales_data(const std::string &s, unsigned n, double p) :bookNo(s), units_sold(n), revenue(n*p) {}	Sales_data() : Sales_data("", 0, 0.0f) {}	Sales_data(const std::string &s) : Sales_data(s, 0, 0.0f) {}	Sales_data(std::istream &is);	Sales_data& operator=(const std::string&);	Sales_data& operator+=(const Sales_data&);	explicit operator std::string() const { return bookNo; }	explicit operator double() const { return avg_price(); }	std::string isbn() const { return bookNo; }private:	inline double avg_price() const;	std::string bookNo;	unsigned units_sold = 0;	double revenue = 0.0;};std::istream& operator>>(std::istream&, Sales_data&);std::ostream& operator<<(std::ostream&, const Sales_data&);Sales_data operator+(const Sales_data&, const Sales_data&);inline double Sales_data::avg_price() const{	return units_sold ? revenue / units_sold : 0;}
```

## 练习14.46

> 你认为应该为 `Sales_data` 类定义上面两种类型转换运算符吗？应该把它们声明成 `explicit` 的吗？为什么？

解：

上面的两种类型转换有歧义，应该声明成 `explicit` 的。

## 练习14.47

> 说明下面这两个类型转换运算符的区别。

```cpp
struct Integral {	operator const int();	operator int() const;}
```

解：

第一个无意义，会被编译器忽略。第二个合法。

## 练习14.48

> 你在7.5.1节的练习7.40中曾经选择并编写了一个类，你认为它应该含有向 `bool` 的类型转换运算符吗？如果是，解释原因并说明该运算符是否应该是 `explicit`的；如果不是，也请解释原因。

解：

`Date` 类应该含有向 `bool` 的类型转换运算符，并且应该声明为 `explicit` 的。

## 练习14.49

> 为上一题提到的类定义一个转换目标是 `bool` 的类型转换运算符，先不用在意这么做是否应该。

解：

```cpp
 explicit operator bool() { return (year<4000) ? true : false; }
```

## 练习14.50

> 在初始化 `ex1` 和 `ex2` 的过程中，可能用到哪些类类型的转换序列呢？说明初始化是否正确并解释原因。

```cpp
struct LongDouble {	LongDouble(double = 0.0);	operator double();	operator float();};LongDouble ldObj;int ex1 = ldObj;float ex2 = ldObj;
```

解：

`ex1` 转换不合法，没有定义从 `LongDouble` 到 `int` 的转换，从`double`转换还是`float`转换存在二义性。`ex2` 合法。

## 练习14.51

> 在调用 `calc` 的过程中，可能用到哪些类型转换序列呢？说明最佳可行函数是如何被选出来的。

```cpp
void calc(int);void calc(LongDouble);double dval;calc(dval);  // 调用了哪个calc？
```

解：

最佳可行函数是 `void calc(int)`。

转换的优先级如下：

1. 精确匹配
2. `const` 转换。
3. 类型提升
4. 算术转换
5. 类类型转换

## 练习14.52

> 在下面的加法表达式中分别选用了哪个`operator+`？列出候选函数、可行函数及为每个可行函数的实参执行的类型转换：

```cpp
struct Longdouble {	//用于演示的成员operator+;在通常情况下是个非成员	LongDouble operator+(const SmallInt&);	//其他成员与14.9.2节一致};LongDouble operator+(LongDouble&, double);SmallInt si;LongDouble ld;ld = si + ld;ld = ld + si;
```

解：

`ld = si + ld;` 不合法。`ld = ld + si` 两个都可以调用，但是第一个调用更精确一些。

## 练习14.53

> 假设我们已经定义了如第522页所示的`SmallInt`，判断下面的加法表达式是否合法。如果合法，使用了哪个加法运算符？如果不合法，应该怎样修改代码才能使其合法？

```cpp
SmallInt si;
double d = si + 3.14;
```

解：

不合法，存在二义性。

应该该为：

```cpp
SmallInt s1;
double d = s1 + SmallInt(3.14);
```



# 第十五章 面向对象程序设计

## 练习15.1

> 什么是虚成员？

解：

对于某些函数，基类希望它的派生类各自定义适合自身的版本，此时基类就将这些函数声明成虚函数。

## 练习15.2

> `protected` 访问说明符与 `private` 有何区别？

解：

* `protected` ： 基类和和其派生类还有友元可以访问。
* `private` ： 只有基类本身和友元可以访问。

## 练习15.3

> 定义你自己的 `Quote` 类和 `print_total` 函数。

解：

`Quote`:

```cpp
#include <string>

class Quote
{
public:
	Quote() = default;
	Quote(const std::string &b, double p) :
		bookNo(b), price(p){}
	std::string isbn() const { return bookNo; }
	virtual double net_price(std::size_t n) const { return n * price; }

	virtual ~Quote() = default;

private:
	std::string bookNo;

protected:
	double  price = 0.0;

};
```

主函数：

```cpp
#include "ex_15_3.h"
#include <iostream>
#include <string>
#include <map>
#include <functional>

double print_total(std::ostream& os, const Quote& item, size_t n);

int main()
{
	return 0;
}


double print_total(std::ostream &os, const Quote &item, size_t n)
{
	double ret = item.net_price(n);

	os << "ISBN:" << item.isbn()
		<< "# sold: " << n << " total due: " << ret << std::endl;

	return ret;
}

```

## 练习15.4

> 下面哪条声明语句是不正确的？请解释原因。

```cpp
class Base { ... };
(a) class Derived : public Derived { ... };
(b) class Derived : private Base { ... };
(c) class Derived : public Base;
```

解：

* (a) 不正确。类不能派生自身。
* (b) 不正确。这是定义而非声明。
* (c) 不正确。派生列表不能出现在这。

## 练习15.5

> 定义你自己的 `Bulk_quote` 类。

解：

```cpp
#include "ex_15_3.h"

class Bulk_quote : public Quote
{
public:
	Bulk_quote() = default;
	Bulk_quote(const std::string& b, double p, std::size_t q, double disc) :
		Quote(b, p), min_qty(q), discount(disc) {}

	double net_price(std::size_t n) const override;

private:
	std::size_t min_qty = 0;
	double      discount = 0.0;
};
```

## 练习15.6

> 将 `Quote` 和 `Bulk_quote` 的对象传给15.2.1节练习中的 `print_total` 函数，检查该函数是否正确。

解：

```cpp
#include "ex_15_3.h"
#include "ex_15_5.h"

#include <iostream>
#include <string>

double print_total(std::ostream& os, const Quote& item, size_t n);

int main()
{
	// ex15.6
	Quote q("textbook", 10.60);
	Bulk_quote bq("textbook", 10.60, 10, 0.3);

	print_total(std::cout, q, 12);
	print_total(std::cout, bq, 12);

	return 0;
}

double print_total(std::ostream &os, const Quote &item, size_t n)
{
	double ret = item.net_price(n);

	os << "ISBN:" << item.isbn()
		<< "# sold: " << n << " total due: " << ret << std::endl;

	return ret;
}
```

## 练习15.7

> 定义一个类使其实现一种数量受限的折扣策略，具体策略是：当购买书籍的数量不超过一个给定的限量时享受折扣，如果购买量一旦超过了限量，则超出的部分将以原价销售。

解：

```cpp
#include "ex_15_5.h"

class Limit_quote : public Quote
{
public:
	Limit_quote();
	Limit_quote(const std::string& b, double p, std::size_t max, double disc) :
		Quote(b, p), max_qty(max), discount(disc)
	{}

	double net_price(std::size_t n) const override;

private:
	std::size_t max_qty = 0;
	double      discount = 0.0;
};

double Limit_quote::net_price(std::size_t n) const
{
	if (n > max_qty)
		return max_qty * price * discount + (n - max_qty) * price;
	else
		return n * discount *price;
}
```

## 练习15.8

> 给出静态类型和动态类型的定义。

解：

表达式的静态类型在编译时总是已知的，它是变量声明时的类型或表达式生成的类型。动态类型则是变量或表达式表示的内存中的对象的类型。动态类型直到运行时才可知。

## 练习15.9

> 在什么情况下表达式的静态类型可能与动态类型不同？请给出三个静态类型与动态类型不同的例子。

解：

基类的指针或引用的静态类型可能与其动态类型不一致。

## 练习15.10

> 回忆我们在8.1节进行的讨论，解释第284页中将 `ifstream` 传递给 `Sales_data` 的`read` 函数的程序是如何工作的。

解：

`std::ifstream` 是 `std::istream` 的派生基类，因此 `read` 函数能够正常工作。

## 练习15.11

> 为你的 `Quote` 类体系添加一个名为 `debug` 的虚函数，令其分别显示每个类的数据成员。

解：

```cpp
void Quote::debug() const
{
    std::cout << "data members of this class:\n"
              << "bookNo= " <<this->bookNo << " "
              << "price= " <<this->price<< " ";
}
```

## 练习15.12

> 有必要将一个成员函数同时声明成 `override` 和 `final` 吗？为什么？

解：

有必要。`override` 的含义是重写基类中相同名称的虚函数，`final` 是阻止它的派生类重写当前虚函数。

## 练习15.13

> 给定下面的类，解释每个 `print` 函数的机理：

```cpp
class base {
public:
	string name() { return basename;}
	virtual void print(ostream &os) { os << basename; }
private:
	string basename;
};
class derived : public base {
public:
	void print(ostream &os) { print(os); os << " " << i; }
private:
	int i;
};
```

在上述代码中存在问题吗？如果有，你该如何修改它？

解：

有问题。应该改为：

```cpp
	void print(ostream &os) override { base::print(os); os << " derived\n " << i; }
```

## 练习15.14

> 给定上一题中的类以及下面这些对象，说明在运行时调用哪个函数：

```cpp
base bobj; 		base *bp1 = &bobj; 	base &br1 = bobj;derived dobj; 	base *bp2 = &dobj; 	base &br2 = dobj;(a) bobj.print();	(b)dobj.print();	(c)bp1->name();(d)bp2->name();		(e)br1.print();		(f)br2.print();
```

解：

* (a) 编译时。
* (b) 编译时。
* (c) 编译时。
* (d) 编译时。
* (e) 运行时。`base::print()`
* (f) 运行时。`derived::print()`

## 练习15.15

> 定义你自己的 `Disc_quote` 和 `Bulk_quote`。

解：

`Disc_quote`:

```cpp
#include "quote.h"class Disc_quote : public Quote{public:    Disc_quote();    Disc_quote(const std::string& b, double p, std::size_t q, double d) :        Quote(b, p), quantity(q), discount(d)   { }    virtual double net_price(std::size_t n) const override = 0;protected:    std::size_t quantity;    double      discount;};
```

`Bulk_quote`:

```cpp
#include "disc_quote.h"class Bulk_quote : public Disc_quote{public:    Bulk_quote() = default;    Bulk_quote(const std::string& b, double p, std::size_t q, double disc) :        Disc_quote(b, p, q, disc) {   }    double net_price(std::size_t n) const override;    void  debug() const override;};
```

## 练习15.16

> 改写你在15.2.2节练习中编写的数量受限的折扣策略，令其继承 `Disc_quote`。

解：

`Limit_quote`：

```cpp
#include "disc_quote.h"class Limit_quote : public Disc_quote{public:    Limit_quote() = default;    Limit_quote(const std::string& b, double p, std::size_t max, double disc):        Disc_quote(b, p, max, disc)  {   }    double net_price(std::size_t n) const override    { return n * price * (n < quantity ? 1 - discount : 1 ); }    void debug() const override;};
```

## 练习15.17

> 尝试定义一个 `Disc_quote` 的对象，看看编译器给出的错误信息是什么？

解：

`error: cannot declare variable 'd' to be of abstract type 'Disc_quote': Disc_quote d;`

## 练习15.18

> 假设给定了第543页和第544页的类，同时已知每个对象的类型如注释所示，判断下面的哪些赋值语句是合法的。解释那些不合法的语句为什么不被允许：

```cpp
Base *p = &d1;  //d1 的类型是 Pub_Dervp = &d2;		//d2 的类型是 Priv_Dervp = &d3;		//d3 的类型是 Prot_Dervp = &dd1;		//dd1 的类型是 Derived_from_Public	p = &dd2;		//dd2 的类型是 Derived_from_Privatep = &dd3;		//dd3 的类型是 Derived_from_Protected
```

解：

```cpp
Base *p = &d1; 合法p = &d2; 不合法p = &d3; 不合法p = &dd1; 合法p = &dd2; 不合法p = &dd3; 不合法
```

只有在派生类是使用`public`的方式继承基类时，用户代码才可以使用派生类到基类（`derived-to-base`）的转换。

## 练习15.19

> 假设543页和544页的每个类都有如下形式的成员函数：

```cpp
void memfcn(Base &b) { b = *this; }
```

对于每个类，分别判断上面的函数是否合法。

解：

合法：

* Pub_Derv
* Priv_Derv
* Prot_Derv
* Derived_from_Public
* Derived_from_Protected
  不合法：
* Derived_from_Private

这段代码是在成员函数中使用`Base`。`Priv_Drev`中的`Base`部分虽然是`private`的，但其成员函数依然可以访问；`Derived_from_Private`继承自`Priv_Drev`，不能访问`Priv_Drev`中的`private`成员，因此不合法。

## 练习15.20

> 编写代码检验你对前面两题的回答是否正确。

解：

```cpp
#include <iostream>#include <string>#include "exercise15_5.h"#include "bulk_quote.h"#include "limit_quote.h"#include "disc_quote.h"class Base{public:	void pub_mem();   // public memberprotected:	int prot_mem;     // protected memberprivate:	char priv_mem;    // private member};struct Pub_Derv : public    Base{	void memfcn(Base &b) { b = *this; }};struct Priv_Derv : private   Base{	void memfcn(Base &b) { b = *this; }};struct Prot_Derv : protected Base{	void memfcn(Base &b) { b = *this; }};struct Derived_from_Public : public Pub_Derv{	void memfcn(Base &b) { b = *this; }};struct Derived_from_Private : public Priv_Derv{	//void memfcn(Base &b) { b = *this; }};struct Derived_from_Protected : public Prot_Derv{	void memfcn(Base &b) { b = *this; }};int main(){	Pub_Derv d1;	Base *p = &d1;	Priv_Derv d2;	//p = &d2;	Prot_Derv d3;	//p = &d3;	Derived_from_Public dd1;	p = &dd1;	Derived_from_Private dd2;	//p =& dd2;	Derived_from_Protected dd3;	//p = &dd3;	return 0;}
```

## 练习15.21

> 从下面这些一般性抽象概念中任选一个（或者选一个你自己的），将其对应的一组类型组织成一个继承体系：

```cpp
(a) 图形文件格式（如gif、tiff、jpeg、bmp）(b) 图形基元（如方格、圆、球、圆锥）(c) C++语言中的类型（如类、函数、成员函数）
```

解：

```cpp
#include <iostream>#include <string>#include "quote.h"#include "bulk_quote.h"#include "limit_quote.h"#include "disc_quote.h"// just for 2D shapeclass Shape{public:    typedef std::pair<double, double>    Coordinate;    Shape() = default;    Shape(const std::string& n) :        name(n) { }    virtual double area()       const = 0;    virtual double perimeter()  const = 0;    virtual ~Shape() = default;private:    std::string name;};class Rectangle : public Shape{public:    Rectangle() = default;    Rectangle(const std::string& n,              const Coordinate& a,              const Coordinate& b,              const Coordinate& c,              const Coordinate& d) :        Shape(n), a(a), b(b), c(c), d(d) { }    ~Rectangle() = default;protected:    Coordinate  a;    Coordinate  b;    Coordinate  c;    Coordinate  d;};class Square : public Rectangle{public:    Square() = default;    Square(const std::string& n,           const Coordinate& a,           const Coordinate& b,           const Coordinate& c,           const Coordinate& d) :        Rectangle(n, a, b, c, d) { }    ~Square() = default;};int main(){    return 0;}
```

## 练习15.22

> 对于你在上一题中选择的类，为其添加函数的虚函数及公有成员和受保护的成员。

解：

参考15.21。

## 练习15.23

> 假设第550页的 `D1` 类需要覆盖它继承而来的 `fcn` 函数，你应该如何对其进行修改？如果你修改之后 `fcn` 匹配了 `Base` 中的定义，则该节的那些调用语句将如何解析？

解：

移除 `int` 参数。

## 练习15.24

> 哪种类需要虚析构函数？虚析构函数必须执行什么样的操作？

解：

基类通常应该定义一个虚析构函数。

## 练习15.25

> 我们为什么为 `Disc_quote` 定义一个默认构造函数？如果去掉该构造函数的话会对 `Bulk_quote` 的行为产生什么影响？

解：

因为`Disc_quote`的默认构造函数会运行`Quote`的默认构造函数，而`Quote`默认构造函数会完成成员的初始化工作。
如果去除掉该构造函数的话，`Bulk_quote`的默认构造函数而无法完成`Disc_quote`的初始化工作。

## 练习15.26

> 定义 `Quote` 和 `Bulk_quote` 的拷贝控制成员，令其与合成的版本行为一致。为这些成员以及其他构造函数添加打印状态的语句，使得我们能够知道正在运行哪个程序。使用这些类编写程序，预测程序将创建和销毁哪些对象。重复实验，不断比较你的预测和实际输出结果是否相同，直到预测完全准确再结束。

解：

`Quote`:

```cpp
#include <string>#include <iostream>class Quote{	friend bool operator !=(const Quote& lhs, const Quote& rhs);public:	Quote() { std::cout << "default constructing Quote\n"; }	Quote(const std::string &b, double p) :		bookNo(b), price(p)	{		std::cout << "Quote : constructor taking 2 parameters\n";	}	// copy constructor	Quote(const Quote& q) : bookNo(q.bookNo), price(q.price)	{		std::cout << "Quote: copy constructing\n";	}	// move constructor	Quote(Quote&& q) noexcept : bookNo(std::move(q.bookNo)), price(std::move(q.price))	{ std::cout << "Quote: move constructing\n"; }	// copy =	Quote& operator =(const Quote& rhs)	{		if (*this != rhs)		{			bookNo = rhs.bookNo;			price = rhs.price;		}		std::cout << "Quote: copy =() \n";		return *this;	}	// move =	Quote& operator =(Quote&& rhs)  noexcept	{		if (*this != rhs)		{			bookNo = std::move(rhs.bookNo);			price = std::move(rhs.price);		}		std::cout << "Quote: move =!!!!!!!!! \n";		return *this;	}	std::string     isbn() const { return bookNo; }	virtual double  net_price(std::size_t n) const { return n * price; }	virtual void    debug() const;	virtual ~Quote()	{		std::cout << "destructing Quote\n";	}private:	std::string bookNo;protected:	double  price = 10.0;};bool inlineoperator !=(const Quote& lhs, const Quote& rhs){	return lhs.bookNo != rhs.bookNo		&&		lhs.price != rhs.price;}
```

`Bulk_quote`:

```cpp
#include "Disc_quote.h"#include <iostream>class Bulk_quote : public Disc_quote{public:	Bulk_quote() { std::cout << "default constructing Bulk_quote\n"; }	Bulk_quote(const std::string& b, double p, std::size_t q, double disc) :		Disc_quote(b, p, q, disc)	{		std::cout << "Bulk_quote : constructor taking 4 parameters\n";	}	// copy constructor	Bulk_quote(const Bulk_quote& bq) : Disc_quote(bq)	{		std::cout << "Bulk_quote : copy constructor\n";	}	// move constructor	Bulk_quote(Bulk_quote&& bq) : Disc_quote(std::move(bq)) noexcept	{		std::cout << "Bulk_quote : move constructor\n";	}	// copy =()	Bulk_quote& operator =(const Bulk_quote& rhs)	{		Disc_quote::operator =(rhs);		std::cout << "Bulk_quote : copy =()\n";		return *this;	}	// move =()	Bulk_quote& operator =(Bulk_quote&& rhs) noexcept	{		Disc_quote::operator =(std::move(rhs));		std::cout << "Bulk_quote : move =()\n";		return *this;	}	double net_price(std::size_t n) const override;	void  debug() const override;	~Bulk_quote() override	{		std::cout << "destructing Bulk_quote\n";	}};
```

程序输出结果：

```
default constructing Quotedefault constructing Disc_quotedefault constructing Bulk_quoteQuote : constructor taking 2 parametersDisc_quote : constructor taking 4 parameters.Bulk_quote : constructor taking 4 parametersQuote: copy constructingQuote: copy constructingdestructing Quotedestructing QuoteDisc_quote : move =()Bulk_quote : move =()destructing Bulk_quotedestructing Dis_quotedestructing Quotedestructing Bulk_quotedestructing Dis_quotedestructing Quote
```

## 练习15.27

> 重新定义你的 `Bulk_quote` 类，令其继承构造函数。

解：

```cpp
#include "disc_quote.h"#include <iostream>class Bulk_quote : public Disc_quote{public:	Bulk_quote() { std::cout << "default constructing Bulk_quote\n"; }	// changed the below to the inherited constructor for ex15.27.	// rules:  1. only inherit from the direct base class.	//         2. default, copy and move constructors can not inherit.	//         3. any data members of its own are default initialized.	//         4. the rest details are in the section section 15.7.4.	/*	Bulk_quote(const std::string& b, double p, std::size_t q, double disc) :	Disc_quote(b, p, q, disc) { std::cout << "Bulk_quote : constructor taking 4 parameters\n"; }	*/	using Disc_quote::Disc_quote;	// copy constructor	Bulk_quote(const Bulk_quote& bq) : Disc_quote(bq)	{		std::cout << "Bulk_quote : copy constructor\n";	}	// move constructor	Bulk_quote(Bulk_quote&& bq) : Disc_quote(std::move(bq))	{		std::cout << "Bulk_quote : move constructor\n";	}	// copy =()	Bulk_quote& operator =(const Bulk_quote& rhs)	{		Disc_quote::operator =(rhs);		std::cout << "Bulk_quote : copy =()\n";		return *this;	}	// move =()	Bulk_quote& operator =(Bulk_quote&& rhs)	{		Disc_quote::operator =(std::move(rhs));		std::cout << "Bulk_quote : move =()\n";		return *this;	}	double net_price(std::size_t n) const override;	void  debug() const override;	~Bulk_quote() override	{		std::cout << "destructing Bulk_quote\n";	}};
```

## 练习15.28

> 定义一个存放 `Quote` 对象的 `vector`，将 `Bulk_quote` 对象传入其中。计算 `vector` 中所有元素总的 `net_price`。

解：

```cpp
#include <iostream>#include <string>#include <vector>#include <memory>#include "quote.h"#include "bulk_quote.h"#include "limit_quote.h"#include "disc_quote.h"int main(){	/**	* @brief ex15.28   outcome == 9090	*/	std::vector<Quote> v;	for (unsigned i = 1; i != 10; ++i)		v.push_back(Bulk_quote("sss", i * 10.1, 10, 0.3));	double total = 0;	for (const auto& b : v)	{		total += b.net_price(20);	}	std::cout << total << std::endl;	std::cout << "======================\n\n";	/**	* @brief ex15.29   outccome == 6363	*/	std::vector<std::shared_ptr<Quote>> pv;	for (unsigned i = 1; i != 10; ++i)		pv.push_back(std::make_shared<Bulk_quote>(Bulk_quote("sss", i * 10.1, 10, 0.3)));	double total_p = 0;	for (auto p : pv)	{		total_p += p->net_price(20);	}	std::cout << total_p << std::endl;	return 0;}
```

## 练习15.29

> 再运行一次你的程序，这次传入 `Quote` 对象的 `shared_ptr` 。如果这次计算出的总额与之前的不一致，解释为什么;如果一直，也请说明原因。

解：

因为智能指针导致了多态性的产生，所以这次计算的总额不一致。

## 练习15.30

> 编写你自己的 `Basket` 类，用它计算上一个练习中交易记录的总价格。

解：

`Basket h`:

```cpp
#include "quote.h"#include <set>#include <memory>// 购物篮// a basket of objects from Quote hierachy, using smart pointers.class Basket{public:	// Basket使用合成的默认构造函数和拷贝控制成员	// copy verison	void add_item(const Quote& sale)	{		items.insert(std::shared_ptr<Quote>(sale.clone()));	}	// move version	void add_item(Quote&& sale)	{		items.insert(std::shared_ptr<Quote>(std::move(sale).clone()));	}	// 打印每本书的总价和购物篮中所有书的总价	double total_receipt(std::ostream& os) const;private:	// function to compare needed by the multiset member	static bool compare(const std::shared_ptr<Quote>& lhs,		const std::shared_ptr<Quote>& rhs)	{		return lhs->isbn() < rhs->isbn();	}	// hold multiple quotes, ordered by the compare member	std::multiset<std::shared_ptr<Quote>, decltype(compare)*>		items{ compare };};
```

`Basket cpp`:

```cpp
#include "basket.h"double Basket::total_receipt(std::ostream &os) const{	double sum = 0.0;  // 保存实时计算出的总价格	// iter指向ISBN相同的一批元素中的第一个	// upper_bound返回一个迭代器，该迭代器指向这批元素的尾后位置	for (auto iter = items.cbegin(); iter != items.cend();		iter = items.upper_bound(*iter))		//  ^^^^^^^^^^^^^^^^^^^^^^^^^^^		// @note   this increment moves iter to the first element with key		//         greater than  *iter.	{		sum += print_total(os, **iter, items.count(*iter));	}                                   // ^^^^^^^^^^^^^ using count to fetch	// the number of the same book.	os << "Total Sale: " << sum << std::endl;	return sum;}
```

`main`:

```cpp
#include <iostream>#include <string>#include <vector>#include <memory>#include <fstream>#include "quote.h"#include "bulk_quote.h"#include "limit_quote.h"#include "disc_quote.h"#include "basket.h"int main(){	Basket basket;	for (unsigned i = 0; i != 10; ++i)		basket.add_item(Bulk_quote("Bible", 20.6, 20, 0.3));	for (unsigned i = 0; i != 10; ++i)		basket.add_item(Bulk_quote("C++Primer", 30.9, 5, 0.4));	for (unsigned i = 0; i != 10; ++i)		basket.add_item(Quote("CLRS", 40.1));	std::ofstream log("log.txt", std::ios_base::app | std::ios_base::out);	basket.total_receipt(log);	return 0;}
```

## 练习15.31

> 已知 `s1`、`s2`、`s3` 和 `s4` 都是 `string`，判断下面的表达式分别创建了什么样的对象：

```cpp
(a) Query(s1) | Query(s2) & ~Query(s3);(b) Query(s1) | (Query(s2) & ~Query(s3));(c) (Query(s1) & (Query(s2)) | (Query(s3) & Query(s4)));
```

解：

```cpp
(a) OrQuery, AndQuery, NotQuery, WordQuery(b) OrQuery, AndQuery, NotQuery, WordQuery(c) OrQuery, AndQuery, WordQuery
```

## 练习15.32

> 当一个 `Query` 类型的对象被拷贝、移动、赋值或销毁时，将分别发生什么？

解：

* **拷贝**：当被拷贝时，合成的拷贝构造函数被调用。它将拷贝两个数据成员至新的对象。而在这种情况下，数据成员是一个智能指针，当拷贝时，相应的智能指针指向相同的地址，计数器增加1.
* **移动**：当移动时，合成的移动构造函数被调用。它将移动数据成员至新的对象。这时新对象的智能指针将会指向原对象的地址，而原对象的智能指针为 `nullptr`，新对象的智能指针的引用计数为 1。
* **赋值**：合成的赋值运算符被调用，结果和拷贝的相同的。
* **销毁**：合成的析构函数被调用。对象的智能指针的引用计数递减，当引用计数为 0 时，对象被销毁。

## 练习15.33

> 当一个 `Query_base` 类型的对象被拷贝、移动赋值或销毁时，将分别发生什么？

解：

由合成的版本来控制。然而 `Query_base` 是一个抽象类，它的对象实际上是它的派生类对象。

## 练习15.34

> 针对图15.3构建的表达式：

```cpp
(a) 例举出在处理表达式的过程中执行的所有构造函数。(b) 例举出 cout << q 所调用的 rep。(c) 例举出 q.eval() 所调用的 eval。
```

解：

* **a**： `Query q = Query("fiery") & Query("bird") | Query("wind");`

1. `Query::Query(const std::string& s)` where s == "fiery","bird" and "wind"
2. `WordQuery::WordQuery(const std::string& s)` where s == "fiery","bird" and "wind"
3. `AndQuery::AndQuery(const Query& left, const Query& right);`
4. `BinaryQuery(const Query&l, const Query& r, std::string s);`
5. `Query::Query(std::shared_ptr<Query_base> query)` 2times
6. `OrQuery::OrQuery(const Query& left, const Query& right);`
7. `BinaryQuery(const Query&l, const Query& r, std::string s);`
8. `Query::Query(std::shared_ptr<Query_base> query)` 2times


* **b**：

1. `query.rep()` inside the operator <<().
2. `q->rep()` inside the member function rep().
3. `OrQuery::rep()` which is inherited from `BinaryQuery`.
4. `Query::rep()` for `lhs` and `rhs`:
   for `rhs` which is a `WordQuery` : `WordQuery::rep()` where `query_word("wind")` is returned.For `lhs` which is an `AndQuery`.
5. `AndQuery::rep()` which is inherited from `BinaryQuery`.
6. `BinaryQuer::rep()`: for `rhs: WordQuery::rep()`   where query_word("fiery") is returned. For `lhs: WordQuery::rep()` where query_word("bird" ) is returned.


* **c**：

1. `q.eval()`
2. `q->rep()`: where q is a pointer to `OrQuary`.
3. `QueryResult eval(const TextQuery& )const override`: is called but this one has not been defined yet.

## 练习15.35

> 实现 `Query` 类和 `Query_base` 类，其中需要定义`rep` 而无须定义 `eval`。

解：

`Query`:

```cpp
#ifndef QUERY_H#define QUERY_H#include <iostream>#include <string>#include <memory>#include "query_base.h"#include "queryresult.h"#include "textquery.h"#include "wordquery.h"/*** @brief interface class to manage the Query_base inheritance hierachy*/class Query{	friend Query operator~(const Query&);	friend Query operator|(const Query&, const Query&);	friend Query operator&(const Query&, const Query&);public:	// build a new WordQuery	Query(const std::string& s) : q(new WordQuery(s))	{		std::cout << "Query::Query(const std::string& s) where s=" + s + "\n";	}	// interface functions: call the corresponding Query_base operatopns	QueryResult eval(const TextQuery& t) const	{		return q->eval(t);	}	std::string rep() const	{		std::cout << "Query::rep() \n";		return q->rep();	}private:	// constructor only for friends	Query(std::shared_ptr<Query_base> query) :		q(query)	{		std::cout << "Query::Query(std::shared_ptr<Query_base> query)\n";	}	std::shared_ptr<Query_base> q;};inline std::ostream&operator << (std::ostream& os, const Query& query){	// make a virtual call through its Query_base pointer to rep();	return os << query.rep();}#endif // QUERY_H
```

`Query_base`:

```cpp
#ifndef QUERY_BASE_H#define QUERY_BASE_H#include "textquery.h"#include "queryresult.h"/*** @brief abstract class acts as a base class for all concrete query types*        all members are private.*/class Query_base{	friend class Query;protected:	using line_no = TextQuery::line_no; //  used in the eval function	virtual ~Query_base() = default;private:	// returns QueryResult that matches this query	virtual QueryResult eval(const TextQuery&) const = 0;	// a string representation of this query	virtual std::string rep() const = 0;};#endif // QUERY_BASE_H
```

## 练习15.36

> 在构造函数和 `rep` 成员中添加打印语句，运行你的代码以检验你对本节第一个练习中(a)、(b)两小题的回答是否正确。

解：

```cpp
Query q = Query("fiery") & Query("bird") | Query("wind");WordQuery::WordQuery(wind)Query::Query(const std::string& s) where s=windWordQuery::WordQuery(bird)Query::Query(const std::string& s) where s=birdWordQuery::WordQuery(fiery)Query::Query(const std::string& s) where s=fieryBinaryQuery::BinaryQuery()  where s=&AndQuery::AndQuery()Query::Query(std::shared_ptr<Query_base> query)BinaryQuery::BinaryQuery()  where s=|OrQuery::OrQueryQuery::Query(std::shared_ptr<Query_base> query)Press <RETURN> to close this window...
```

```cpp
std::cout << q <<std::endl;Query::rep()BinaryQuery::rep()Query::rep()WodQuery::rep()Query::rep()BinaryQuery::rep()Query::rep()WodQuery::rep()Query::rep()WodQuery::rep()((fiery & bird) | wind)Press <RETURN> to close this window...
```

## 练习15.37

> 如果在派生类中含有 `shared_ptr<Query_base>` 类型的成员而非 `Query` 类型的成员，则你的类需要做出怎样的改变？

解：

参考15.35。

## 练习15.38

> 下面的声明合法吗？如果不合法，请解释原因;如果合法，请指出该声明的含义。

```cpp
BinaryQuery a = Query("fiery") & Query("bird");AndQuery b = Query("fiery") & Query("bird");OrQuery c = Query("fiery") & Query("bird");
```

解：

1. 不合法。因为 `BinaryQuery` 是抽象类。
2. 不合法。`&` 操作返回的是一个 `Query` 对象。
3. 不合法。`&` 操作返回的是一个 `Query` 对象。

## 练习15.39

> 实现 `Query` 类和　`Query_base` 类，求图15.3中表达式的值并打印相关信息，验证你的程序是否正确。

## 练习15.40

> 在 `OrQuery` 的 `eval` 函数中，如果 `rhs` 成员返回的是空集将发生什么？

解：

不会发生什么。代码如下：

```cpp
std::shared_ptr<std::set<line_no>> ret_lines =       std::make_shared<std::set<line_no>>(left.begin(), left.end());
```

如果 `rhs` 成员返回的是空集，在 `set` 当中不会添加什么。

## 练习15.41

> 重新实现你的类，这次使用指向 `Query_base` 的内置指针而非 `shared_ptr`。请注意，做出上述改动后你的类将不能再使用合成的拷贝控制成员。

解：

略

## 练习15.42

> 从下面的几种改进中选择一种，设计并实现它:

```cpp
(a) 按句子查询并打印单词，而不再是按行打印。(b) 引入一个历史系统，用户可以按编号查阅之前的某个查询，并可以在其中添加内容或者将其余其他查询组合。(c) 允许用户对结果做出限制，比如从给定范围的行中跳出匹配的进行显示。
```

解：

略

## TextQuery最终项目

见 cpp_source/cha5/text_query



## 练习16.1

> 给出实例化的定义。

解：

当编译器实例化一个模版时，它使用实际的模版参数代替对应的模版参数来创建出模版的一个新“实例”。

## 练习16.2

> 编写并测试你自己版本的 `compare` 函数。

解：

```cpp
template<typename T>
int compare(const T& lhs, const T& rhs)
{
	if (lhs < rhs) return -1;
	if (rhs < lhs) return 1;
	return 0;
}
```

## 练习16.3

> 对两个 `Sales_data` 对象调用你的 `compare` 函数，观察编译器在实例化过程中如何处理错误。

解：

`error: no match for 'operator<' `

## 练习16.4

> 编写行为类似标准库 `find` 算法的模版。函数需要两个模版类型参数，一个表示函数的迭代器参数，另一个表示值的类型。使用你的函数在一个 `vector<int>` 和一个`list<string>`中查找给定值。


解：

```cpp
template<typename Iterator, typename Value>
Iterator find(Iterator first, Iterator last, const Value& v)
{
	for ( ; first != last && *first != value; ++first);
	return first;
}
```

## 练习16.5

> 为6.2.4节中的`print`函数编写模版版本，它接受一个数组的引用，能处理任意大小、任意元素类型的数组。

解：

```cpp
template<typename Array>
void print(const Array& arr)
{
	for (const auto& elem : arr)
		std::cout << elem << std::endl;
}
```

## 练习16.6

> 你认为接受一个数组实参的标准库函数 `begin` 和 `end` 是如何工作的？定义你自己版本的 `begin` 和 `end`。

解：

```cpp
template<typename T, unsigned N>
T* begin(const T (&arr)[N])
{
	return arr;
}

template<typename T, unsigned N>
T* end(const T (&arr)[N])
{
	return arr + N;
}
```

## 练习16.7

> 编写一个 `constexpr` 模版，返回给定数组的大小。

解：

```cpp
template<typename T, typename N> constexpr
unsigned size(const T (&arr)[N])
{
	return N;
}
```

## 练习16.8

> 在第97页的“关键概念”中，我们注意到，C++程序员喜欢使用 `!=` 而不喜欢 `<` 。解释这个习惯的原因。

解：

因为大多数类只定义了 `!=` 操作而没有定义 `<` 操作，使用 `!=` 可以降低对要处理的类型的要求。

## 练习16.9

> 什么是函数模版，什么是类模版？

解：

一个函数模版就是一个公式，可用来生成针对特定类型的函数版本。类模版是用来生成类的蓝图的，与函数模版的不同之处是，编译器不能为类模版推断模版参数类型。如果我们已经多次看到，为了使用类模版，我们必须在模版名后的尖括号中提供额外信息。

## 练习16.10

> 当一个类模版被实例化时，会发生什么？

解：

一个类模版的每个实例都形成一个独立的类。

## 练习16.11

> 下面 `List` 的定义是错误的。应如何修改它？

```cpp
template <typename elemType> class ListItem;
template <typename elemType> class List {
public:
	List<elemType>();
	List<elemType>(const List<elemType> &);
	List<elemType>& operator=(const List<elemType> &);
	~List();
	void insert(ListItem *ptr, elemType value);
private:
	ListItem *front, *end;
};
```

解：

模版需要模版参数，应该修改为如下：

```cpp
template <typename elemType> class ListItem;  
template <typename elemType> class List{  
public:  
  	List<elemType>();  
  	List<elemType>(const List<elemType> &);  
  	List<elemType>& operator=(const List<elemType> &);  
  	~List();  
  	void insert(ListItem<elemType> *ptr, elemType value);  
private:  
  	ListItem<elemType> *front, *end;  
};
```

## 练习16.12

> 编写你自己版本的 `Blob` 和 `BlobPtr` 模版，包含书中未定义的多个`const`成员。

解：

Blob：

```cpp
#include <memory>
#include <vector>

template<typename T> class Blob
{
public:
	typedef T value_type;
	typedef typename std::vector<T>::size_type size_type;

	// constructors
	Blob();
	Blob(std::initializer_list<T> il);

	// number of elements in the Blob
	size_type size() const { return data->size(); }
	bool      empty() const { return data->empty(); }

	void push_back(const T& t) { data->push_back(t); }
	void push_back(T&& t) { data->push_back(std::move(t)); }
	void pop_back();

	// element access
	T& back();
	T& operator[](size_type i);

	const T& back() const;
	const T& operator [](size_type i) const;

private:
	std::shared_ptr<std::vector<T>> data;
	// throw msg if data[i] isn't valid
	void check(size_type i, const std::string &msg) const;
};

// constructors
template<typename T>
Blob<T>::Blob() : data(std::make_shared<std::vector<T>>())
{}

template<typename T>
Blob<T>::Blob(std::initializer_list<T> il) :
data(std::make_shared<std::vector<T>>(il))
{}

template<typename T>
void Blob<T>::check(size_type i, const std::string &msg) const
{
	if (i >= data->size())
		throw std::out_of_range(msg);
}

template<typename T>
T& Blob<T>::back()
{
	check(0, "back on empty Blob");
	return data->back();
}

template<typename T>
const T& Blob<T>::back() const
{
	check(0, "back on empty Blob");
	return data->back();
}


template<typename T>
T& Blob<T>::operator [](size_type i)
{
	// if i is too big, check function will throw, preventing access to a nonexistent element
	check(i, "subscript out of range");
	return (*data)[i];
}


template<typename T>
const T& Blob<T>::operator [](size_type i) const
{
	// if i is too big, check function will throw, preventing access to a nonexistent element
	check(i, "subscript out of range");
	return (*data)[i];
}

template<typename T>
void Blob<T>::pop_back()
{
	check(0, "pop_back on empty Blob");
	data->pop_back();
}

```

BlobPtr：

```cpp
#include "Blob.h"#include <memory>#include <vector>template <typename> class BlobPtr;template <typename T>bool operator ==(const BlobPtr<T>& lhs, const BlobPtr<T>& rhs);template <typename T>bool operator < (const BlobPtr<T>& lhs, const BlobPtr<T>& rhs);template<typename T> class BlobPtr{	friend bool operator ==<T>	(const BlobPtr<T>& lhs, const BlobPtr<T>& rhs);	friend bool operator < <T>		(const BlobPtr<T>& lhs, const BlobPtr<T>& rhs);public:	BlobPtr() : curr(0) {}	BlobPtr(Blob<T>& a, std::size_t sz = 0) :		wptr(a.data), curr(sz)	{}	T& operator*() const	{		auto p = check(curr, "dereference past end");		return (*p)[curr];	}	// prefix	BlobPtr& operator++();	BlobPtr& operator--();	// postfix	BlobPtr operator ++(int);	BlobPtr operator --(int);private:	// returns  a shared_ptr to the vector if the check succeeds	std::shared_ptr<std::vector<T>>		check(std::size_t, const std::string&) const;	std::weak_ptr<std::vector<T>> wptr;	std::size_t curr;};// prefix ++template<typename T>BlobPtr<T>& BlobPtr<T>::operator ++(){	// if curr already points past the end of the container, can't increment it	check(curr, "increment past end of StrBlob");	++curr;	return *this;}// prefix --template<typename T>BlobPtr<T>& BlobPtr<T>::operator --(){	--curr;	check(curr, "decrement past begin of BlobPtr");	return *this;}// postfix ++template<typename T>BlobPtr<T> BlobPtr<T>::operator ++(int){	BlobPtr ret = *this;	++*this;	return ret;}// postfix --template<typename T>BlobPtr<T> BlobPtr<T>::operator --(int){	BlobPtr ret = *this;	--*this;	return ret;}template<typename T> bool operator==(const BlobPtr<T> &lhs, const BlobPtr<T> &rhs){	if (lhs.wptr.lock() != rhs.wptr.lock())	{		throw runtime_error("ptrs to different Blobs!");	}	return lhs.i == rhs.i;}template<typename T> bool operator< (const BlobPtr<T> &lhs, const BlobPtr<T> &rhs){	if (lhs.wptr.lock() != rhs.wptr.lock())	{		throw runtime_error("ptrs to different Blobs!");	}	return lhs.i < rhs.i;}
```

## 练习16.13

> 解释你为 `BlobPtr` 的相等和关系运算符选择哪种类型的友好关系？

解：

这里需要与类型一一对应，所以就选择一对一友好关系。

## 练习16.14

> 编写 `Screen` 类模版，用非类型参数定义 `Screen` 的高和宽。


解：

Screen

```cpp
#include <string>#include <iostream>template<unsigned H, unsigned W>class Screen{public:	typedef std::string::size_type pos;	Screen() = default; // needed because Screen has another constructor	// cursor initialized to 0 by its in-class initializer	Screen(char c) :contents(H * W, c) {}	char get() const              // get the character at the cursor	{		return contents[cursor];	}       // implicitly inline	Screen &move(pos r, pos c);      // can be made inline later	friend std::ostream & operator<< (std::ostream &os, const Screen<H, W> & c)	{		unsigned int i, j;		for (i = 0; i<c.height; i++)		{			os << c.contents.substr(0, W) << std::endl;		}		return os;	}	friend std::istream & operator>> (std::istream &is, Screen &  c)	{		char a;		is >> a;		std::string temp(H*W, a);		c.contents = temp;		return is;	}private:	pos cursor = 0;	pos height = H, width = W;	std::string contents;};template<unsigned H, unsigned W>inline Screen<H, W>& Screen<H, W>::move(pos r, pos c){	pos row = r * width;	cursor = row + c;	return *this;}
```

## 练习16.15

> 为你的 `Screen` 模版实现输入和输出运算符。`Screen` 类需要哪些友元（如果需要的话）来令输入和输出运算符正确工作？解释每个友元声明（如果有的话）为什么是必要的。

解：

类的 `operator<<` 和 `operator>>` 应该是类的友元。

## 练习16.16

> 将 `StrVec` 类重写为模版，命名为 `Vec`。

解：

Vec:

```cpp
#include <memory>/***  @brief a vector like class*/template<typename T>class Vec{public:	Vec() :element(nullptr), first_free(nullptr), cap(nullptr) {}	Vec(std::initializer_list<T> l);	Vec(const Vec& v);	Vec& operator =(const Vec& rhs);	~Vec();	// memmbers	void push_back(const T& t);	std::size_t size() const { return first_free - element; }	std::size_t capacity()const { return cap - element; }	T* begin() const { return element; }	T* end()   const { return first_free; }	void reserve(std::size_t n);	void resize(std::size_t n);	void resize(std::size_t n, const T& t);private:	// data members	T* element;	T* first_free;	T* cap;	std::allocator<T> alloc;	// utillities	void reallocate();	void chk_n_alloc() { if (size() == capacity()) reallocate(); }	void free();	void wy_alloc_n_move(std::size_t n);	std::pair<T*, T*> alloc_n_copy(T* b, T* e);};// copy constructortemplate<typename T>Vec<T>::Vec(const Vec &v){	/**	* @brief newData is a pair of pointers pointing to newly allocated and copied	*        from range : [b, e)	*/	std::pair<T*, T*> newData = alloc_n_copy(v.begin(), v.end());	element = newData.first;	first_free = cap = newData.second;}// constructor that takes initializer_list<T>template<typename T>Vec<T>::Vec(std::initializer_list<T> l){	// allocate memory as large as l.size()	T* const newData = alloc.allocate(l.size());	// copy elements from l to the address allocated	T* p = newData;	for (const auto &t : l)		alloc.construct(p++, t);	// build data structure	element = newData;	first_free = cap = element + l.size();}// operator =template<typename T>Vec<T>& Vec<T>::operator =(const Vec& rhs){	// allocate and copy first to protect against self_assignment	std::pair<T*, T*> newData = alloc_n_copy(rhs.begin(), rhs.end());	// destroy and deallocate	free();	// update data structure	element = newData.first;	first_free = cap = newData.second;	return *this;}// destructortemplate<typename T>Vec<T>::~Vec(){	free();}/*** @brief   allocate new memeory if nessary and push back the new T* @param t new T*/template<typename T>void Vec<T>::push_back(const T &t){	chk_n_alloc();	alloc.construct(first_free++, t);}/*** @brief   preallocate enough memory for specified number of elements* @param n number of elements required*/template<typename T>void Vec<T>::reserve(std::size_t n){	// if n too small, just return without doing anything	if (n <= capacity()) return;	// allocate new memory and move data from old address to the new one	wy_alloc_n_move(n);}/***  @brief  Resizes to the specified number of elements.*  @param  n  Number of elements the %vector should contain.**  This function will resize it to the specified*  number of elements.  If the number is smaller than the*  current size it is truncated, otherwise*  default constructed elements are appended.*/template<typename T>void Vec<T>::resize(std::size_t n){	resize(n, T());}/***  @brief  Resizes it to the specified number of elements.*  @param  __new_size  Number of elements it should contain.*  @param  __x  Data with which new elements should be populated.**  This function will resize it to the specified*  number of elements.  If the number is smaller than the*  current size the it is truncated, otherwise*  the it is extended and new elements are populated with*  given data.*/template<typename T>void Vec<T>::resize(std::size_t n, const T &t){	if (n < size())	{		// destroy the range [element+n, first_free) using destructor		for (auto p = element + n; p != first_free;)			alloc.destroy(p++);		// update first_free to point to the new address		first_free = element + n;	}	else if (n > size())	{		for (auto i = size(); i != n; ++i)			push_back(t);	}}/*** @brief   allocate new space for the given range and copy them into it* @param b* @param e* @return  a pair of pointers pointing to [first element , one past the last) in the new space*/template<typename T>std::pair<T*, T*>Vec<T>::alloc_n_copy(T *b, T *e){	// calculate the size needed and allocate space accordingly	T* data = alloc.allocate(e - b);	return{ data, std::uninitialized_copy(b, e, data) };	//            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^	// which copies the range[first, last) to the space to which	// the starting address data is pointing.	// This function returns a pointer to one past the last element}/*** @brief   destroy the elements and deallocate the space previously allocated.*/template<typename T>void Vec<T>::free(){	// if not nullptr	if (element)	{		// destroy it in reverse order.		for (auto p = first_free; p != element;)			alloc.destroy(--p);		alloc.deallocate(element, capacity());	}}/*** @brief   allocate memory for spicified number of elements* @param n* @note    it's user's responsibility to ensure that @param n is greater than*          the current capacity.*/template<typename T>void Vec<T>::wy_alloc_n_move(std::size_t n){	// allocate as required.	std::size_t newCapacity = n;	T* newData = alloc.allocate(newCapacity);	// move the data from old place to the new one	T* dest = newData;	T* old = element;	for (std::size_t i = 0; i != size(); ++i)		alloc.construct(dest++, std::move(*old++));	free();	// update data structure	element = newData;	first_free = dest;	cap = element + newCapacity;}/*** @brief   Double the capacity and using std::move move the original data to the newly*          allocated memory*/template<typename T>void Vec<T>::reallocate(){	// calculate the new capacity required	std::size_t newCapacity = size() ? 2 * size() : 1;	// allocate and move old data to the new space	wy_alloc_n_move(newCapacity);}
```

## 练习16.17

> 声明为 `typename` 的类型参数和声明为 `class` 的类型参数有什么不同（如果有的话）？什么时候必须使用`typename`？

解：

没有什么不同。当我们希望通知编译器一个名字表示类型时，必须使用关键字 `typename`，而不能使用 `class`。

## 练习16.18

> 解释下面每个函数模版声明并指出它们是否非法。更正你发现的每个错误。

```cpp
(a) template <typename T, U, typename V> void f1(T, U, V);(b) template <typename T> T f2(int &T);(c) inline template <typename T> T foo(T, unsigned int *);(d) template <typename T> f4(T, T);(e) typedef char Ctype;	template <typename Ctype> Ctype f5(Ctype a);
```

解：

* (a) 非法。应该为 `template <typename T, typename U, typename V> void f1(T, U, V);`。
* (b) 非法。应该为 `template <typename T> T f2(int &t);`
* (c) 非法。应该为 `template <typename T> inline T foo(T, unsigned int*);`
* (d) 非法。应该为 `template <typename T> T f4(T, T);`
* (e) 非法。`Ctype` 被隐藏了。

## 练习16.19

> 编写函数，接受一个容器的引用，打印容器中的元素。使用容器的 `size_type` 和 `size`成员来控制打印元素的循环。

解：

```cpp
template<typename Container>void print(const Container& c){	for (typename Container::size_type i = 0; i != c.size(); ++i)		std::cout << c[i] << " ";}
```

## 练习16.20

> 重写上一题的函数，使用`begin` 和 `end` 返回的迭代器来控制循环。

解：

```cpp
template<typename Container>void print(const Container& c){	for (auto it = c.begin(); it != c.end(); ++it)		std::cout << *it << " ";}
```

## 练习16.21

> 编写你自己的 `DebugDelete` 版本。

解：

DebugDelete

```cpp
#include <iostream>class DebugDelete{public:	DebugDelete(std::ostream& s = std::cerr) : os(s) {}	template<typename T>	void operator() (T* p) const	{		os << "deleting unique_ptr" << std::endl;		delete p;	}private:	std::ostream& os;};
```

## 练习16.22

> 修改12.3节中你的 `TextQuery` 程序，令 `shared_ptr` 成员使用 `DebugDelete` 作为它们的删除器。

解：

略

## 练习16.23

> 预测在你的查询主程序中何时会执行调用运算符。如果你的预测和实际不符，确认你理解了原因。

解：

略

## 练习16.24

> 为你的 `Blob` 模版添加一个构造函数，它接受两个迭代器。

解：

```cpp
template<typename T>    //for classtemplate<typename It>   //for this memberBlob<T>::Blob(It b, It e) :    data(std::make_shared<std::vector<T>>(b, e)){ }
```

## 练习16.25

> 解释下面这些声明的含义。

```cpp
extern template class vector<string>;template class vector<Sales_data>;
```

解：

前者是模版声明，后者是实例化定义。

## 练习16.26

> 假设 `NoDefault` 是一个没有默认构造函数的类，我们可以显式实例化 `vector<NoDefualt>`吗？如果不可以，解释为什么。

解：

不可以。如

```cpp
std::vector<NoDefault> vec(10);
```

会使用 `NoDefault` 的默认构造函数，而 `NoDefault` 没有默认构造函数，因此是不可以的。

## 练习16.27

> 对下面每条带标签的语句，解释发生了什么样的实例化（如果有的话）。如果一个模版被实例化，解释为什么;如果未实例化，解释为什么没有。

```cpp
template <typename T> class Stack {	};void f1(Stack<char>); 		//(a)class Exercise {	Stack<double> &rds;		//(b)	Stack<int> si;			//(c)};int main() {	Stack<char> *sc;		//(d)	f1(*sc);				//(e)	int iObj = sizeof(Stack<string>);	//(f)}
```

解：

(a)、(b)、(c)、(f) 都发生了实例化，(d)、(e) 没有实例化。

## 练习16.28

> 编写你自己版本的 `shared_ptr` 和 `unique_ptr`。

解：

shared_ptr

```cpp
#pragma once#include <functional>#include "delete.h"namespace cp5{	template<typename T>	class SharedPointer;	template<typename T>	auto swap(SharedPointer<T>& lhs, SharedPointer<T>& rhs)	{		using std::swap;		swap(lhs.ptr, rhs.ptr);		swap(lhs.ref_count, rhs.ref_count);		swap(lhs.deleter, rhs.deleter);	}	template<typename T>	class SharedPointer	{	public:		//		//  Default Ctor		//		SharedPointer()			: ptr{ nullptr }, ref_count{ new std::size_t(1) }, deleter{ cp5::Delete{} }		{}		//		//  Ctor that takes raw pointer		//		explicit SharedPointer(T* raw_ptr)			: ptr{ raw_ptr }, ref_count{ new std::size_t(1) }, deleter{ cp5::Delete{} }		{}		//		//  Copy Ctor		//		SharedPointer(SharedPointer const& other)			: ptr{ other.ptr }, ref_count{ other.ref_count }, deleter{ other.deleter }		{			++*ref_count;		}		//		//  Move Ctor		//		SharedPointer(SharedPointer && other) noexcept			: ptr{ other.ptr }, ref_count{ other.ref_count }, deleter{ std::move(other.deleter) }		{			other.ptr = nullptr;			other.ref_count = nullptr;		}		//		//  Copy assignment		//		SharedPointer& operator=(SharedPointer const& rhs)		{			//increment first to ensure safty for self-assignment			++*rhs.ref_count;			decrement_and_destroy();			ptr = rhs.ptr, ref_count = rhs.ref_count, deleter = rhs.deleter;			return *this;		}		//		//  Move assignment		//		SharedPointer& operator=(SharedPointer && rhs) noexcept		{			cp5::swap(*this, rhs);			rhs.decrement_and_destroy();			return *this;		}		//		//  Conversion operator		//		operator bool() const		{			return ptr ? true : false;		}		//		//  Dereference		//		T& operator* () const		{			return *ptr;		}		//		//  Arrow		//		T* operator->() const		{			return &*ptr;		}		//		//  Use count		//		auto use_count() const		{			return *ref_count;		}		//		//  Get underlying pointer		//		auto get() const		{			return ptr;		}		//		//  Check if the unique user		//		auto unique() const		{			return 1 == *refCount;		}		//		//  Swap		//		auto swap(SharedPointer& rhs)		{			::swap(*this, rhs);		}		//		// Free the object pointed to, if unique		//		auto reset()		{			decrement_and_destroy();		}		//		// Reset with the new raw pointer		//		auto reset(T* pointer)		{			if (ptr != pointer)			{				decrement_n_destroy();				ptr = pointer;				ref_count = new std::size_t(1);			}		}		//		//  Reset with raw pointer and deleter		//		auto reset(T *pointer, const std::function<void(T*)>& d)		{			reset(pointer);			deleter = d;		}		//		//  Dtor		//		~SharedPointer()		{			decrement_and_destroy();		}	private:		T* ptr;		std::size_t* ref_count;		std::function<void(T*)> deleter;		auto decrement_and_destroy()		{			if (ptr && 0 == --*ref_count)				delete ref_count,				deleter(ptr);			else if (!ptr)				delete ref_count;			ref_count = nullptr;			ptr = nullptr;		}	};}//namespace
```

unique_ptr:

```cpp
#include "debugDelete.h"// forward declarations for friendshiptemplate<typename, typename> class unique_pointer;template<typename T, typename D> voidswap(unique_pointer<T, D>& lhs, unique_pointer<T, D>& rhs);/***  @brief  std::unique_ptr like class template.*/template <typename T, typename D = DebugDelete>class unique_pointer{	friend void swap<T, D>(unique_pointer<T, D>& lhs, unique_pointer<T, D>& rhs);public:	// preventing copy and assignment	unique_pointer(const unique_pointer&) = delete;	unique_pointer& operator = (const unique_pointer&) = delete;	// default constructor and one taking T*	unique_pointer() = default;	explicit unique_pointer(T* up) : ptr(up) {}	// move constructor	unique_pointer(unique_pointer&& up) noexcept		: ptr(up.ptr) { up.ptr = nullptr; }	// move assignment	unique_pointer& operator =(unique_pointer&& rhs) noexcept;	// std::nullptr_t assignment	unique_pointer& operator =(std::nullptr_t n) noexcept;	// operator overloaded :  *  ->  bool	T& operator  *() const { return *ptr; }	T* operator ->() const { return &this->operator *(); }	operator bool() const { return ptr ? true : false; }	// return the underlying pointer	T* get() const noexcept{ return ptr; }	// swap member using swap friend	void swap(unique_pointer<T, D> &rhs) { ::swap(*this, rhs); }	// free and make it point to nullptr or to p's pointee.	void reset()     noexcept{ deleter(ptr); ptr = nullptr; }	void reset(T* p) noexcept{ deleter(ptr); ptr = p; }	// return ptr and make ptr point to nullptr.	T* release();	~unique_pointer()	{		deleter(ptr);	}private:	T* ptr = nullptr;	D  deleter = D();};// swaptemplate<typename T, typename D>inline voidswap(unique_pointer<T, D>& lhs, unique_pointer<T, D>& rhs){	using std::swap;	swap(lhs.ptr, rhs.ptr);	swap(lhs.deleter, rhs.deleter);}// move assignmenttemplate<typename T, typename D>inline unique_pointer<T, D>&unique_pointer<T, D>::operator =(unique_pointer&& rhs) noexcept{	// prevent self-assignment	if (this->ptr != rhs.ptr)	{		deleter(ptr);		ptr = nullptr;		::swap(*this, rhs);	}	return *this;}// std::nullptr_t assignmenttemplate<typename T, typename D>inline unique_pointer<T, D>&unique_pointer<T, D>::operator =(std::nullptr_t n) noexcept{	if (n == nullptr)	{		deleter(ptr);   ptr = nullptr;	}	return *this;}// relinquish contrul by returnning ptr and making ptr point to nullptr.template<typename T, typename D>inline T*unique_pointer<T, D>::release(){	T* ret = ptr;	ptr = nullptr;	return ret;}
```

## 练习16.29

> 修改你的 `Blob` 类，用你自己的 `shared_ptr` 代替标准库中的版本。

解：

略

## 练习16.30

> 重新运行你的一些程序，验证你的 `shared_ptr` 类和修改后的 `Blob` 类。（注意：实现 `weak_ptr` 类型超出了本书范围，因此你不能将`BlobPtr`类与你修改后的`Blob`一起使用。）

解：

略

## 练习16.31

> 如果我们将 `DebugDelete` 与 `unique_ptr` 一起使用，解释编译器将删除器处理为内联形式的可能方式。

解：

略

## 练习16.32

> 在模版实参推断过程中发生了什么？

解：

在模版实参推断过程中，编译器使用函数调用中的实参类型来寻找模版实参，用这些模版实参生成的函数版本与给定的函数调用最为匹配。

## 练习16.33

> 指出在模版实参推断过程中允许对函数实参进行的两种类型转换。

解：

* `const` 转换：可以将一个非 `const` 对象的引用（或指针）传递给一个 `const` 的引用（或指针）形参。
* 数组或函数指针转换：如果函数形参不是引用类型，则可以对数组或函数类型的实参应用正常的指针转换。一个数组实参可以转换为一个指向其首元素的指针。类似的，一个函数实参可以转换为一个该函数类型的指针。

## 练习16.34

> 对下面的代码解释每个调用是否合法。如果合法，`T` 的类型是什么？如果不合法，为什么？

```cpp
template <class T> int compare(const T&, const T&);(a) compare("hi", "world");(b) compare("bye", "dad");
```

解：

* (a) 不合法。`compare(const char [3], const char [6])`, 两个实参类型不一致。
* (b) 合法。`compare(const char [4], const char [4])`.

## 练习16.35

> 下面调用中哪些是错误的（如果有的话）？如果调用合法，`T` 的类型是什么？如果调用不合法，问题何在？

```cpp
template <typename T> T calc(T, int);tempalte <typename T> T fcn(T, T);double d; float f; char c;(a) calc(c, 'c'); (b) calc(d, f);(c) fcn(c, 'c');(d) fcn(d, f);
```

解：

* (a) 合法，类型为`char`
* (b) 合法，类型为`double`
* (c) 合法，类型为`char`
* (d) 不合法，这里无法确定T的类型是`float`还是`double`

## 练习16.36

> 进行下面的调用会发生什么：

```cpp
template <typename T> f1(T, T);template <typename T1, typename T2) f2(T1, T2);int i = 0, j = 42, *p1 = &i, *p2 = &j;const int *cp1 = &i, *cp2 = &j;(a) f1(p1, p2);(b) f2(p1, p2);(c) f1(cp1, cp2);(d) f2(cp1, cp2);(e) f1(p1, cp1);(f) f2(p1, cp1);
```

解：

```cpp
(a) f1(int*, int*);(b) f2(int*, int*);(c) f1(const int*, const int*);(d) f2(const int*, const int*);(e) f1(int*, const int*); 这个使用就不合法(f) f2(int*, const int*);
```

## 练习16.37

> 标准库 `max` 函数有两个参数，它返回实参中的较大者。此函数有一个模版类型参数。你能在调用 `max` 时传递给它一个 `int` 和一个 `double` 吗？如果可以，如何做？如果不可以，为什么？

解：

可以。提供显式的模版实参：

```cpp
int a = 1;double b = 2;std::max<double>(a, b);
```

## 练习16.38

> 当我们调用 `make_shared` 时，必须提供一个显示模版实参。解释为什么需要显式模版实参以及它是如果使用的。

解：

如果不显示提供模版实参，那么 `make_shared` 无法推断要分配多大内存空间。

## 练习16.39

> 对16.1.1节 中的原始版本的 `compare` 函数，使用一个显式模版实参，使得可以向函数传递两个字符串字面量。

解： 

```cpp
compare<std::string>("hello", "world")  
```

## 练习16.40

> 下面的函数是否合法？如果不合法，为什么？如果合法，对可以传递的实参类型有什么限制（如果有的话）？返回类型是什么？

```cpp
template <typename It>auto fcn3(It beg, It end) -> decltype(*beg + 0){	//处理序列	return *beg;}
```

解：

合法。该类型需要支持 `+` 操作。

## 练习16.41

> 编写一个新的 `sum` 版本，它返回类型保证足够大，足以容纳加法结果。

解：

```cpp
template<typename T>auto sum(T lhs, T rhs) -> decltype( lhs + rhs){    return lhs + rhs;}
```

## 练习16.42

> 对下面每个调用，确定 `T` 和 `val` 的类型：

```cpp
template <typename T> void g(T&& val);int i = 0; const int ci = i;(a) g(i);(b) g(ci);(c) g(i * ci);
```

解：

```cpp
(a) int&(b) const int&(c) int&&
```

## 练习16.43

> 使用上一题定义的函数，如果我们调用`g(i = ci)`,`g` 的模版参数将是什么？

解：

`i = ci` 返回的是左值，因此 `g` 的模版参数是 `int&`

## 练习16.44

> 使用与第一题中相同的三个调用，如果 `g` 的函数参数声明为 `T`（而不是`T&&`），确定T的类型。如果`g`的函数参数是 `const T&`呢？

解：

当声明为`T`的时候，`T`的类型为`int&`。
当声明为`const T&`的时候，T的类型为`int&`。

## 练习16.45

> 如果下面的模版，如果我们对一个像42这样的字面常量调用`g`，解释会发生什么？如果我们对一个`int` 类型的变量调用`g` 呢？

```cpp
template <typename T> void g(T&& val) { vector<T> v; }
```

解：

当使用字面常量，`T`将为`int`。
当使用`int`变量，`T`将为`int&`。编译的时候将会报错，因为没有办法对这种类型进行内存分配，无法创建`vector<int&>`。

## 练习16.46

> 解释下面的循环，它来自13.5节中的 `StrVec::reallocate`:

```cpp
for (size_t i = 0; i != size(); ++i)	alloc.construct(dest++, std::move(*elem++));
```

解：

在每个循环中，对 `elem` 的解引用操作 `*` 当中，会返回一个左值，`std::move` 函数将该左值转换为右值，提供给 `construct` 函数。

## 练习16.47

> 编写你自己版本的翻转函数，通过调用接受左值和右值引用参数的函数来测试它。

解：

```cpp
template<typename F, typename T1, typename T2>void flip(F f, T1&& t1, T2&& t2){    f(std::forward<T2>(t2), std::forward<T1>(t1));}
```

## 练习16.48

> 编写你自己版本的 `debug_rep` 函数。

解：

```cpp
template<typename T> std::string debug_rep(const T& t){    std::ostringstream ret;    ret << t;    return ret.str();}template<typename T> std::string debug_rep(T* p){    std::ostringstream ret;    ret << "pointer: " << p;    if(p)        ret << " " << debug_rep(*p);    else        ret << " null pointer";    return ret.str();}
```

## 练习16.49

> 解释下面每个调用会发生什么：

```cpp
template <typename T> void f(T);template <typename T> void f(const T*);template <typename T> void g(T);template <typename T> void g(T*);int i = 42, *p = &i;const int ci = 0, *p2 = &ci;g(42); g(p); g(ci); g(p2);f(42); f(p); f(ci); f(p2);
```

解：

```cpp
    g(42);    	//g(T )    g(p);     	//g(T*)    g(ci);      //g(T)       g(p2);      //g(T*)        f(42);    	//f(T)    f(p);     	//f(T)    f(ci);    	//f(T)    f(p2);      //f(const T*)
```

## 练习16.50

> 定义上一个练习中的函数，令它们打印一条身份信息。运行该练习中的代码。如果函数调用的行为与你预期不符，确定你理解了原因。

解：

略

## 练习16.51

> 调用本节中的每个 `foo`，确定 `sizeof...(Args)` 和 `sizeof...(rest)`分别返回什么。

解：

```cpp
#include <iostream>using namespace std;template <typename T, typename ... Args>void foo(const T &t, const Args& ... rest){    cout << "sizeof...(Args): " << sizeof...(Args) << endl;    cout << "sizeof...(rest): " << sizeof...(rest) << endl;};void test_param_packet(){    int i = 0;    double d = 3.14;    string s = "how now brown cow";    foo(i, s, 42, d);    foo(s, 42, "hi");    foo(d, s);    foo("hi");}int main(){    test_param_packet();    return 0;}
```

结果：

```
sizeof...(Args): 3sizeof...(rest): 3sizeof...(Args): 2sizeof...(rest): 2sizeof...(Args): 1sizeof...(rest): 1sizeof...(Args): 0sizeof...(rest): 0
```

## 练习16.52

> 编写一个程序验证上一题的答案。

解：

参考16.51。

## 练习16.53

> 编写你自己版本的 `print` 函数，并打印一个、两个及五个实参来测试它，要打印的每个实参都应有不同的类型。 

解：

```cpp
template<typename Printable>std::ostream& print(std::ostream& os, Printable const& printable){    return os << printable;}// recursiontemplate<typename Printable, typename... Args>std::ostream& print(std::ostream& os, Printable const& printable, Args const&... rest){    return print(os << printable << ", ", rest...);}
```

## 练习16.54

> 如果我们对一个没 `<<` 运算符的类型调用 `print`，会发生什么？

解：

无法通过编译。

## 练习16.55

> 如果我们的可变参数版本 `print` 的定义之后声明非可变参数版本，解释可变参数的版本会如何执行。

解：

`error: no matching function for call to 'print(std::ostream&)'`

## 练习16.56

> 编写并测试可变参数版本的 `errorMsg`。

解：

```cpp
template<typename... Args>std::ostream& errorMsg(std::ostream& os, const Args... rest){    return print(os, debug_rep(rest)...);}
```

## 练习16.57

> 比较你的可变参数版本的 `errorMsg` 和6.2.6节中的 `error_msg`函数。两种方法的优点和缺点各是什么？

解：

可变参数版本有更好的灵活性。

## 练习16.58

> 为你的 `StrVec` 类及你为16.1.2节练习中编写的 `Vec` 类添加 `emplace_back` 函数。

解：

```cpp
template<typename T>        //for the class  templatetemplate<typename... Args>  //for the member templateinline voidVec<T>::emplace_back(Args&&...args){    chk_n_alloc();    alloc.construct(first_free++, std::forward<Args>(args)...);}
```

## 练习16.59

> 假定 `s` 是一个 `string`，解释调用 `svec.emplace_back(s)`会发生什么。

解：

会在 `construst` 函数中转发扩展包。

## 练习16.60

> 解释 `make_shared` 是如何工作的。

解：

`make_shared` 是一个可变模版函数，它将参数包转发然后构造一个对象，再然后一个指向该对象的智能指针。

## 练习16.61

> 定义你自己版本的 `make_shared`。

解：

```cpp
template <typename T, typename ... Args>auto make_shared(Args&&... args) -> std::shared_ptr<T>{	return std::shared_ptr<T>(new T(std::forward<Args>(args)...));}
```

## 练习16.62

> 定义你自己版本的 `hash<Sales_data>`, 并定义一个 `Sales_data` 对象的 `unorder_multise`。将多条交易记录保存到容器中，并打印其内容。

解：

略

## 练习16.63

> 定义一个函数模版，统计一个给定值在一个`vecor`中出现的次数。测试你的函数，分别传递给它一个`double`的`vector`，一个`int`的`vector`以及一个`string`的`vector`。

解：

```cpp
#include <iostream>#include <vector>#include <cstring>// templatetemplate<typename T>std::size_t  count(std::vector<T> const& vec, T value) {    auto count = 0u;    for(auto const& elem  : vec)        if(value == elem) ++count;    return count;}// template specializationtemplate<>std::size_t count (std::vector<const char*> const& vec, const char* value){    auto count = 0u;    for(auto const& elem : vec)        if(0 == strcmp(value, elem)) ++count;    return count;}int main(){    // for ex16.63    std::vector<double> vd = { 1.1, 1.1, 2.3, 4 };    std::cout << count(vd, 1.1) << std::endl;        // for ex16.64    std::vector<const char*> vcc = { "alan", "alan", "alan", "alan", "moophy" };    std::cout << count(vcc, "alan") << std::endl;    return 0;}
```

## 练习16.64

> 为上一题的模版编写特例化版本来处理`vector<const char*>`。编写程序使用这个特例化版本。

解：

参考16.64。

## 练习16.65

> 在16.3节中我们定义了两个重载的 `debug_rep` 版本，一个接受 `const char*` 参数，另一个接受 `char *` 参数。将这两个函数重写为特例化版本。

解：

```cpp
#include <iostream>#include <vector>#include <cstring>#include <sstream>// templatetemplate <typename T>std::string debug_rep(T* t);// template specialization T=const char*  ,  char*  respectively.template<>std::string debug_rep(const char* str);template<>std::string debug_rep(      char *str);int main(){    char p[] = "alan";    std::cout << debug_rep(p) << "\n";    return 0;}template <typename T>std::string debug_rep(T* t){    std::ostringstream ret;    ret << t;    return ret.str();}// template specialization// T = const char*template<>std::string debug_rep(const char* str){    std::string ret(str);    return str;}// template specialization// T =       char*template<>std::string debug_rep(      char *str){    std::string ret(str);    return ret;}
```

## 练习16.66

> 重载`debug_rep` 函数与特例化它相比，有何优点和缺点？

解：

重载函数会改变函数匹配。

## 练习16.67

> 定义特例化版本会影响 `debug_rep` 的函数匹配吗？如果不影响，为什么？

解：

不影响，特例化是模板的一个实例，并没有重载函数。