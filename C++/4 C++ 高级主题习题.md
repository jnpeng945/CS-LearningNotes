## 练习17.1

> 定义一个保存三个`int`值的 `tuple`，并将其成员分别初始化为10、20和30。

解：

```cpp
auto t = tuple<int, int, int>{10, 20, 30};
```

## 练习17.2

> 定义一个 `tuple`，保存一个 `string`、一个`vector<string>` 和一个 `pair<string, int>`。

解：

```cpp
auto t = tuple<string, vector<string>, pair<string, int> >
```

## 练习17.3

> 重写12.3节中的 `TextQuery` 程序，使用 `tuple` 代替 `QueryResult` 类。你认为哪种设计更好？为什么？

解：

程序略。

我认为`tuple`更方便。

## 练习17.4

> 编写并测试你自己版本的 `findBook` 函数。


解：

```cpp
#include <iostream>
#include <tuple>
#include <string>
#include <vector>
#include <algorithm>
#include <utility>
#include <numeric>

#include "ex_17_4_SalesData.h"

using namespace std;

// matches有三个成员：1.一个书店的索引。2.指向书店中元素的迭代器。3.指向书店中元素的迭代器。
typedef tuple<vector<Sales_data>::size_type,
              vector<Sales_data>::const_iterator,
              vector<Sales_data>::const_iterator>
    matches;

// files保存每家书店的销售记录
// findBook返回一个vector，每家销售了给定书籍的书店在其中都有一项
vector<matches> findBook(const vector<vector<Sales_data>> &files,
                         const string &book)
{
    vector<matches> ret; //初始化为空vector
    // 对每家书店，查找给定书籍匹配的记录范围
    for (auto it = files.cbegin; it != files.cend(); ++it)
    {
        // 查找具有相同ISBN的Sales_data范围，found是一个迭代器pair
        auto found = equal_range(it->cbegin(), it->cend(), book, compareIsbn);
        if (found.first != found.second)  // 此书店销售了给定书籍
            // 记住此书店的索引及匹配的范围
            ret.push_back(make_tuple(it - files.cbegin(), found.first, found.second));
    }
    return ret; //如果未找到匹配记录，ret为空
}

void reportResults(istream &in, ostream &os,
                       const vector<vector<Sales_data> > &files){
    string s;  //要查找的书
    while (in >> s){
        auto trans = findBook(files, s);
        if (trans.empty()){
            cout << s << " not found in any stores" << endl;
            continue;  // 获得下一本要查找的书
        }
        for (const auto &store : trans)  // 对每家销售了给定书籍的书店
            // get<n>返回store中tuple的指定的成员
            os << "store " << get<0>(store) << " sales: "
               << accumulate(get<1>(store), get<2>(store), Sales_data(s))
               << endl;
    }
}

int main(){
    return 0;
}
```

## 练习17.5

> 重写 `findBook`，令其返回一个 `pair`，包含一个索引和一个迭代器pair。

解：

```cpp
typedef std::pair<std::vector<Sales_data>::size_type,
                  std::pair<std::vector<Sales_data>::const_iterator,
                            std::vector<Sales_data>::const_iterator>>
                                                                      matches_pair;

std::vector<matches_pair>
findBook_pair(const std::vector<std::vector<Sales_data> > &files,
              const std::string &book)
{
    std::vector<matches_pair> ret;
    for(auto it = files.cbegin(); it != files.cend(); ++it)
    {
        auto found = std::equal_range(it->cbegin(), it->cend(), book, compareIsbn);
        if(found.first != found.second)
            ret.push_back(std::make_pair(it - files.cbegin(),
                                         std::make_pair(found.first, found.second)));
    }
    return ret;
}
```

## 练习17.6

> 重写 `findBook`，不使用`tuple`和`pair`。

解：

```cpp
struct matches_struct
{
    std::vector<Sales_data>::size_type st;
    std::vector<Sales_data>::const_iterator first;
    std::vector<Sales_data>::const_iterator last;
    matches_struct(std::vector<Sales_data>::size_type s,
                   std::vector<Sales_data>::const_iterator f,
                   std::vector<Sales_data>::const_iterator l) : st(s), first(f), last(l) { }
} ;

std::vector<matches_struct>
findBook_struct(const std::vector<std::vector<Sales_data> > &files,
                const std::string &book)
{
    std::vector<matches_struct> ret;
    for(auto it = files.cbegin(); it != files.cend(); ++it)
    {
        auto found = std::equal_range(it->cbegin(), it->cend(), book, compareIsbn);
        if(found.first != found.second)
            ret.push_back(matches_struct(it - files.cbegin(), found.first, found.second));
    }
    return ret;
}
```

## 练习17.7

> 解释你更倾向于哪个版本的`findBook`，为什么。

解：

使用`tuple`的版本。很明显更加灵活方便。

## 练习17.8

> 在本节最后一段代码中，如果我们将`Sales_data()`作为第三个参数传递给`accumulate`，会发生什么？

解：

结果是0，以为`Sales_data`是默认初始化的。

## 练习17.9

> 解释下列每个`bitset` 对象所包含的位模式：

```cpp
(a) bitset<64> bitvec(32);
// 0000000000000000000000000000000000000000000000000000000000100000
(b) bitset<32> bv(1010101);
// 00000000000011110110100110110101
(c) string bstr; cin >> bstr; bitset<8> bv(bstr);
// 根据输入的str转换成bitset
```

## 练习17.10

> 使用序列1、2、3、5、8、13、21初始化一个`bitset`，将这些位置置位。对另一个`bitset`进行默认初始化，并编写一小段程序将其恰当的位置位。

解：

```cpp
#include <iostream>
#include <bitset>
#include <vector>

int main()
{
    std::vector<int> v = { 1, 2, 3, 5, 8, 13, 21 };
    std::bitset<32> bset;

    for (auto i : v)    bset.set(i);

    std::bitset<32> bset2;
    for (unsigned i = 0; i != 32; ++i)
        bset2[i] = bset[i];

    std::cout <<bset <<std::endl;
    std::cout <<bset2<<std::endl;
}
```

## 练习17.11

> 定义一个数据结构，包含一个整型对象，记录一个包含10个问题的真/假测验的解答。如果测验包含100道题，你需要对数据结构做出什么改变（如果需要的话）？

解：

```cpp
#include <iostream>
#include <bitset>
#include <utility>
#include <string>
#include <iostream>

//class Quiz
template<std::size_t N>
class Quiz
{
public:
    //constructors
    Quiz() = default;
    Quiz(std::string& s) :bitquiz(s){ }

    //generate grade
    template<std::size_t M>
    friend std::size_t grade(Quiz<M> const&, Quiz<M> const&);

    //print
    template<std::size_t M>
    friend std::ostream& operator<<(std::ostream&, Quiz<M> const&);

    //update bitset
    void update(std::pair<std::size_t, bool>);
private:
    std::bitset<N> bitquiz;
};
#endif

template<std::size_t N>
void Quiz<N>::update(std::pair<std::size_t, bool> pair)
{
    bitquiz.set(pair.first, pair.second);
}

template<std::size_t M>
std::ostream& operator<<(std::ostream& os, Quiz<M> const& quiz)
{
    os << quiz.bitquiz;
    return os;
}

template<std::size_t M>
std::size_t grade(Quiz<M> const& corAns, Quiz<M> const& stuAns)
{
    auto result = stuAns.bitquiz ^ corAns.bitquiz;
    result.flip();
    return result.count();
}


int main()
{
    //Ex17_11
    std::string s = "1010101";
    Quiz<10> quiz(s);
    std::cout << quiz << std::endl;

    //EX17_12
    quiz.update(std::make_pair(1, true));
    std::cout << quiz << std::endl;

    //Ex17_13
    std::string answer = "10011";
    std::string stu_answer = "11001";
    Quiz<5> ans(answer), stu_ans(stu_answer);
    std::cout << grade(ans, stu_ans) << std::endl;

    return 0;
}
```

## 练习17.12

> 使用前一题中的数据结构，编写一个函数，它接受一个问题编号和一个表示真/假解答的值，函数根据这两个参数更新测验的解答。

解：

参考17.11。

## 练习17.13

> 编写一个整型对象，包含真/假测验的正确答案。使用它来为前两题中的数据结构生成测验成绩。

解：

参考17.11。

## 练习17.14

> 编写几个正则表达式，分别触发不同错误。运行你的程序，观察编译器对每个错误的输出。

解：

```cpp
#include <iostream>using std::cout;using std::cin;using std::endl;#include <string>using std::string;#include <regex>using std::regex;using std::regex_error;int main(){    // for ex17.14    // error_brack    try{        regex r("[[:alnum:]+\\.(cpp|cxx|cc)$", regex::icase);    }    catch(regex_error e)    {        cout << e.what() << " code: " << e.code() << endl;    }    // for ex17.15    regex r("[[:alpha:]]*[^c]ei[[:alpha:]]*", regex::icase);    string s;    cout << "Please input a word! Input 'q' to quit!" << endl;    while(cin >> s && s != "q")    {        if(std::regex_match(s, r))            cout << "Input word " << s << " is okay!" << endl;        else            cout << "Input word " << s << " is not okay!" <<endl;        cout << "Please input a word! Input 'q' to quit!" << endl;    }    cout << endl;    // for ex17.16    r.assign("[^c]ei", regex::icase);    cout << "Please input a word! Input 'q' to quit!" << endl;    while(cin >> s && s != "q")    {        if(std::regex_match(s, r))            cout << "Input word " << s << " is okay!" << endl;        else            cout << "Input word " << s << " is not okay!" <<endl;        cout << "Please input a word! Input 'q' to quit!" << endl;    }    return 0;}
```

## 练习17.15

> 编写程序，使用模式查找违反“i在e之前，除非在c之后”规则的单词。你的程序应该提示用户输入一个单词，然后指出此单词是否符号要求。用一些违反和未违反规则的单词测试你的程序。

解：

参考17.14。

## 练习17.16

> 如果前一题程序中的`regex`对象用`"[^c]ei"`进行初始化，将会发生什么？用此模式测试你的程序，检查你的答案是否正确。

解：

参考17.14。

## 练习17.17

> 更新你的程序，令它查找输入序列中所有违反"ei"语法规则的单词。

解：

```cpp
#include <iostream>using std::cout;using std::cin;using std::endl;#include <string>using std::string;#include <regex>using std::regex;using std::sregex_iterator;int main(){	string s;	cout << "Please input a sequence of words:" << endl;	getline(cin, s);	cout << endl;	cout << "Word(s) that violiate the \"ei\" grammar rule:" << endl;	string pattern("[^c]ei");	pattern = "[[:alpha:]]*" + pattern + "[[:alpha:]]*";	regex r(pattern, regex::icase);	for (sregex_iterator it(s.begin(), s.end(), r), end_it; it != end_it; ++it)		cout << it->str() << endl;	return 0;}
```

## 练习17.18

> 修改你的程序，忽略包含“ei`但并非拼写错误的单词，如“albeit”和“neighbor”。

解：

参考17.17。

## 练习17.19

> 为什么可以不先检查`m[4]`是否匹配了就直接调用`m[4].str()`？

解：

如果不匹配，则`m[4].str()`返回空字符串。

## 练习17.20

> 编写你自己版本的验证电话号码的程序。

解：

```cpp
#include <iostream>using std::cout;using std::cin;using std::endl;#include <string>using std::string;#include <regex>using std::regex;using std::sregex_iterator;using std::smatch;bool valid(const smatch& m);int main(){	string phone = "(\\()?(\\d{ 3 })(\\))?([-. ])?(\\d{ 3 })([-. ]?)(\\d{ 4 })";	regex r(phone);	smatch m;	string s;	bool valid_record;	// read each record from the input file	while (getline(cin, s))	{		valid_record = false;		// for each matching phone number		for (sregex_iterator it(s.begin(), s.end(), r), end_it; it != end_it; ++it)		{			valid_record = true;			// check whether the number's formatting is valid			if (valid(*it))				cout << "valid phone number: " << it->str() << endl;			else				cout << "invalid phone number: " << it->str() << endl;		}		if (!valid_record)			cout << "invalid record!" << endl;	}	return 0;}bool valid(const smatch& m){	// if there is an open parenthesis before the area code	if (m[1].matched)		// the area code must be followed by a close parenthesis		// and followed immediately by the rest of the number or a space		return m[3].matched && (m[4].matched == 0 || m[4].str() == " ");	else		// then there can't be a close after the area code		// the delimiters between the other two components must match		return !m[3].matched && m[4].str() == m[6].str();}
```

## 练习17.21

> 使用本节定义的`valid` 函数重写8.3.2节中的电话号码程序。

解：

```cpp
#include <iostream>using std::cerr;using std::cout;using std::cin;using std::endl;using std::istream;using std::ostream;#include <fstream>using std::ifstream;using std::ofstream;#include <sstream>using std::istringstream;using std::ostringstream;#include <string>using std::string;#include <vector>using std::vector;#include <regex>using std::regex;using std::sregex_iterator;using std::smatch;struct PersonInfo{    string name;    vector<string> phones;};bool valid(const smatch& m);bool read_record(istream& is, vector<PersonInfo>& people);void format_record(ostream& os, const vector<PersonInfo>& people);// fake function that makes the program compilestring format(const string &num) { return num; }int main(){    vector<PersonInfo> people;    string filename;    cout << "Please input a record file name: ";    cin >> filename;    cout << endl;    ifstream fin(filename);    if (read_record(fin, people))    {        ofstream fout("data\\result.txt", ofstream::trunc);        format_record(fout, people);    }    else    {        cout << "Fail to open file " << filename << endl;    }    return 0;}bool valid(const smatch& m){    // if there is an open parenthesis before the area code    if (m[1].matched)        // the area code must be followed by a close parenthesis        // and followed immediately by the rest of the number or a space        return m[3].matched && (m[4].matched == 0 || m[4].str() == " ");    else        // then there can't be a close after the area code        // the delimiters between the other two components must match        return !m[3].matched && m[4].str() == m[6].str();}bool read_record(istream& is, vector<PersonInfo>& people){    if (is)    {        string line, word; // will hold a line and word from input, respectively                           // read the input a line at a time until cin hits end-of-file (or another error)        while (getline(is, line))        {            PersonInfo info; // create an object to hold this record's data            istringstream record(line); // bind record to the line we just read            record >> info.name; // read the name            while (record >> word) // read the phone numbers                info.phones.push_back(word); // and store them            people.push_back(info); // append this record to people        }        return true;    }    else        return false;}void format_record(ostream& os, const vector<PersonInfo>& people){    string phone = "(\\()?(\\d{ 3 })(\\))?([-. ])?(\\d{ 3 })([-. ]?)(\\d{ 4 })";    regex r(phone);    smatch m;    for (const auto &entry : people)    {        // for each entry in people        ostringstream formatted, badNums; // objects created on each loop        for (const auto &nums : entry.phones)        {            for (sregex_iterator it(nums.begin(), nums.end(), r), end_it; it != end_it; ++it)            {                // for each number                // check whether the number's formatting is valid                if (!valid(*it))                    // string in badNums                    badNums << " " << nums;                else                    // "writes" to formatted's string                    formatted << " " << format(nums);            }        }        if (badNums.str().empty()) // there were no bad numbers            os << entry.name << " " // print the name            << formatted.str() << endl; // and reformatted numbers        else // otherwise, print the name and bad numbers            cerr << "input error: " << entry.name            << " invalid number(s) " << badNums.str() << endl;    }}
```

## 练习17.22

> 重写你的电话号码程序，使之允许在号码的三个部分之间放置任意多个空白符。


解：

参考17.21。

## 练习17.23

> 编写查找邮政编码的正则表达式。一个美国邮政编码可以由五位或九位数字组成。前五位数字和后四位数字之间可以用一个短横线分隔。

解：

```cpp
#include <iostream>using std::cout;using std::cin;using std::endl;#include<string>using std::string;#include <regex>using std::regex;using std::sregex_iterator;using std::smatch;bool valid(const smatch& m);int main(){	string zipcode =		"(\\d{5})([-])?(\\d{4})?\\b";	regex r(zipcode);	smatch m;	string s;		while (getline(cin, s))	{		//! for each matching zipcode number		for (sregex_iterator it(s.begin(), s.end(), r), end_it;			it != end_it; ++it)		{			//! check whether the number's formatting is valid			if (valid(*it))				cout << "valid zipcode number: " << it->str() << endl;			else				cout << "invalid zipcode number: " << s << endl;		}	}	return 0;}bool valid(const smatch& m){		if ((m[2].matched)&&(!m[3].matched))		return false;	else		return true;}
```

## 练习17.24

> 编写你自己版本的重拍电话号码格式的程序。

解：

```cpp
#include <iostream>#include <regex>#include <string>using namespace std;string pattern = "(\\()?(\\d{3})(\\))?([-. ])?(\\d{3})([-. ])?(\\d{4})";string format = "$2.$5.$7";regex r(pattern);string s;int main(){    while(getline(cin,s))    {        cout<<regex_replace(s,r,format)<<endl;    }    return 0;}
```

## 练习17.25

> 重写你的电话号码程序，使之只输出每个人的第一个电话号码。

解：

```cpp
#include <iostream>#include <regex>#include <string>using namespace std;string pattern = "(\\()?(\\d{3})(\\))?([-. ])?(\\d{3})([-. ])?(\\d{4})";string fmt = "$2.$5.$7";regex r(pattern);string s;int main(){    while(getline(cin,s))    {        smatch result;        regex_search(s,result,r);        if(!result.empty())        {        cout<<result.prefix()<<result.format(fmt)<<endl;        }        else        {            cout<<"Sorry, No match."<<endl;        }    }    return 0;}
```

## 练习17.26

> 重写你的电话号码程序，使之对多于一个电话号码的人只输出第二个和后续号码。

解：

略

## 练习17.27

> 编写程序，将九位数字邮政编码的格式转换为 `ddddd-dddd`。

解：

```cpp
#include <iostream>#include <regex>#include <string>using namespace std;string pattern = "(\\d{5})([.- ])?(\\d{4})";string fmt = "$1-$3";regex r(pattern);string s;int main(){    while(getline(cin,s))    {        smatch result;        regex_search(s,result, r);        if(!result.empty())        {            cout<<result.format(fmt)<<endl;        }        else        {            cout<<"Sorry, No match."<<endl;        }    }    return 0;}
```

## 练习17.28

> 编写函数，每次调用生成并返回一个均匀分布的随机`unsigned int`。

解：

```cpp
#include <iostream>#include <random>#include<string>// default versionunsigned random_gen();// with seed spicifiedunsigned random_gen(unsigned seed);// with seed and range spicifiedunsigned random_gen(unsigned seed, unsigned min, unsigned max);int main(){    std::string temp;    while(std::cin >> temp)    std::cout << std::hex << random_gen(19, 1, 10) << std::endl;    return 0;}unsigned random_gen(){    static std::default_random_engine e;    static std::uniform_int_distribution<unsigned> ud;    return ud(e);}unsigned random_gen(unsigned seed){    static std::default_random_engine e(seed);    static std::uniform_int_distribution<unsigned> ud;    return ud(e);}unsigned random_gen(unsigned seed, unsigned min, unsigned max){    static std::default_random_engine e(seed);    static std::uniform_int_distribution<unsigned> ud(min, max);    return ud(e);}
```

## 练习17.29

> 修改上一题中编写的函数，允许用户提供一个种子作为可选参数。

解：

参考17.28。

## 练习17.30

> 再次修改你的程序，此次增加两个参数，表示函数允许返回的最小值和最大值。

解：

参考17.28。

## 练习17.31

> 对于本节中的游戏程序，如果在`do`循环内定义`b`和`e`，会发生什么？

解：

由于引擎返回相同的随机数序列，因此眉不循环都会创建新的引擎，眉不循环都会生成相同的值。

## 练习17.32

> 如果我们在循环内定义`resp`，会发生什么？

解：

会报错，`while`条件中用到了`resp`。

## 练习17.33

> 修改11.3.6节中的单词转换程序，允许对一个给定单词有多种转换方式，每次随机选择一种进行实际转换。

解：

```cpp
#include <iostream>using std::cout;using std::endl;#include <fstream>using std::ifstream;#include <string>using std::string;#include <vector>using std::vector;#include <random>using std::default_random_engine;using std::uniform_int_distribution;#include <ctime>using std::time;#include <algorithm>using std::sort;using std::find_if;#include <utility>using std::pair;int main() {	typedef pair<string, string> ps;	ifstream i("d.txt");	vector<ps> dict;	string str1, str2;	// read wirds from dictionary	while (i >> str1 >> str2) {		dict.emplace_back(str1, str2);	}	i.close();	// sort words in vector	sort(dict.begin(), dict.end(), [](const ps &_ps1, const ps &_ps2){ return _ps1.first < _ps2.first; });	i.open("i.txt");	default_random_engine e(unsigned int(time(0)));	// read words from text	while (i >> str1) {	  // find word in dictionary		vector<ps>::const_iterator it = find_if(dict.cbegin(), dict.cend(),		  [&str1](const ps &_ps){ return _ps.first == str1; });		// if word doesn't exist in dictionary		if (it == dict.cend()) {		  // write it itself			cout << str1 << ' ';		}		else {		  // get random meaning of word 			uniform_int_distribution<unsigned> u (0, find_if(dict.cbegin(), dict.cend(),			 [&str1](const ps &_ps){ return _ps.first > str1; }) - it - 1);			// write random meaning			cout << (it + u(e))->second << ' ';		}	}	return 0;}
```

## 练习17.34

> 编写一个程序，展示如何使用表17.17和表17.18中的每个操作符。

解：

略

## 练习17.35

> 修改第670页中的程序，打印2的平方根，但这次打印十六进制数字的大写形式。

解：

```cpp
#include <iostream>#include<iomanip>#include <math.h>using namespace std;int main(){	cout <<"default format: " << 100 * sqrt(2.0) << '\n'		<< "scientific: " << scientific << 100 * sqrt(2.0) << '\n'		<< "fixed decimal: " << fixed << 100 * sqrt(2.0) << '\n'		<< "hexidecimal: " << uppercase << hexfloat << 100 * sqrt(2.0) << '\n'		<< "use defaults: " << defaultfloat << 100 * sqrt(2.0)		<< "\n\n";}//17.36//Modify the program from the previous exercise to print the various floating-point values so that they line up in a column.#include <iostream>#include<iomanip>#include <math.h>using namespace std;int main(){	cout <<left<<setw(15) << "default format:" <<setw(25)<< right<< 100 * sqrt(2.0) << '\n'	<< left << setw(15) << "scientific:" << scientific << setw(25) << right << 100 * sqrt(2.0) << '\n'	<< left << setw(15) << "fixed decimal:" << setw(25) << fixed << right << 100 * sqrt(2.0) << '\n'	<< left << setw(15) << "hexidecimal:" << setw(25) << uppercase << hexfloat << right << 100 * sqrt(2.0) << '\n'	<< left << setw(15) << "use defaults:" << setw(25) << defaultfloat << right << 100 * sqrt(2.0)	<< "\n\n";}
```

## 练习17.36

> 修改上一题中的程序，打印不同的浮点数，使它们排成一列。

解：

参考17.36。

## 练习17.37

> 用未格式化版本的`getline` 逐行读取一个文件。测试你的程序，给定一个文件，既包含空行又包含长度超过你传递给`geiline`的字符数组大小的行。

解：

```cpp
//17.37//Use the unformatted version of getline to read a file a line at a time.//Test your program by giving it a file that contains empty lines as well as lines that are//longer than the character array that you pass to getline.#include <iostream>#include <fstream>#include <iomanip>using namespace std;//int main () {//  ifstream myfile("F:\\Git\\Cpp-Primer\\ch17\\17_37_38\\test.txt");//  if (myfile) cout << 1 << endl;//  char sink [250];////  while(myfile.getline(sink,250))//  {//    cout << sink << endl;//  }//  return 0;//}//17.38//Extend your program from the previous exercise to print each word you read onto its own line.//#include <iostream>//#include <fstream>//#include <iomanip>////using namespace std;////int main () {//  ifstream myfile ("F:\\Git\\Cpp-Primer\\ch17\\17_37_38\\test.txt");//  char sink [250];////  while(myfile.getline(sink,250,' '))//  {//    cout << sink << endl;//  }//  return 0;//}int main(){	std::cout << "Standard Output!\n";	std::cerr << "Standard Error!\n";	std::clog << "Standard Log??\n";}
```

## 练习17.38

> 扩展上一题中你的程序，将读入的每个单词打印到它所在的行。

解：

参考17.37。

## 练习17.39

> 对本节给出的 `seek`程序，编写你自己的版本。

解：

略



## 练习18.1

> 在下列 `throw` 语句中异常对象的类型是什么？

```cpp
(a) range_error r("error");
	throw r;
(b) exception *p = &r;
	throw *p;
```

解：

- (a): `range_error`
- (b): `exception`


## 练习18.2

> 当在指定的位置发生了异常时将出现什么情况？

```cpp
void exercise(int *b, int *e)
{
	vector<int> v(b, e);
	int *p = new int[v.size()];
	ifstream in("ints");
	//此处发生异常
}
```

解：

指针`p`指向的内容不会被释放，将造成内存泄漏。

## 练习18.3

> 要想让上面的代码在发生异常时能正常工作，有两种解决方案。请描述这两种方法并实现它们。

解：

方法一：不使用指针，使用对象：

```cpp
struct intArray
{
    intArray() : p(nullptr) { }
    explicit    intArray(std::size_t s):
        p(new int[s])       { }


    ~intArray()
    {
        delete[] p;
    }

    // data meber
    int *p;
};

intArray p(v.size());
```

方法二：使用智能指针：

```cpp
std::shared_ptr<int> p(new int[v.size()], [](int *p) { delete[] p; });
```

## 练习18.4

> 查看图18.1所示的继承体系，说明下面的 `try` 块有何错误并修改它。

```cpp
try {
	// 使用 C++ 标准库
} catch (exception) {
	// ...
} catch (const runtime_error &re) {
	// ...
} catch (overflow_error eobj) { /* ... */ }
```

解：

细化的异常类型应该写在前面：

```cpp
try {
	// 使用 C++ 标准库
} catch (overflow_error eobj) {
	// ...
} catch (const runtime_error &re) {
	// ...
} catch (exception) { /* ... */ }
```

## 练习18.5

> 修改下面的`main`函数，使其能捕获图18.1所示的任何异常类型：

```cpp
int main(){
	// 使用 C++标准库
}
```

处理代码应该首先打印异常相关的错误信息，然后调用 `abort` 终止函数。

解：

略

## 练习18.6

> 已知下面的异常类型和 `catch` 语句，书写一个 `throw` 表达式使其创建的异常对象能被这些 `catch` 语句捕获：

```cpp
(a) class exceptionType { };
	catch(exceptionType *pet) { }
(b) catch(...) { }
(c) typedef int EXCPTYPE;
	catch(EXCPTYPE) { }
```

解：

```cpp
(a): throw exceptionType();(b): throw expection();(c): EXCPTYPE e = 1; throw e;
```

## 练习18.7

> 根据第16章的介绍定义你自己的 `Blob` 和 `BlobPtr`，注意将构造函数写成函数`try`语句块。

解：

略

## 练习18.8

> 回顾你之前编写的各个类，为它们的构造函数和析构函数添加正确的异常说明。如果你认为某个析构函数可能抛出异常，尝试修改代码使得该析构函数不会抛出异常。

解：

略

## 练习18.9

> 定义本节描述的书店程序异常类，然后为 `Sales_data` 类重新编写一个复合赋值运算符并令其抛出一个异常。

## 练习18.10

> 编写程序令其对两个 `ISBN` 编号不相同的对象执行 `Sales_data` 的加法运算。为该程序编写两个不同的版本：一个处理异常，另一个不处理异常。观察并比较这两个程序的行为，用心体会当出现了一个未被捕获的异常时程序会发生什么情况。

解：

略

## 练习18.11

> 为什么 `what` 函数不应该抛出异常？

解：

略

## 练习18.12

> 将你为之前各章练习编写的程序放置在各自的命名空间中。也就是说，命名空间chapter15包含`Query`程序的代码，命名空间chapter10包含`TextQuery`的代码；使用这种结构重新编译`Query`代码实例。

解：

略

## 练习18.13

> 什么时候应该使用未命名的命名空间？

解：

需要定义一系列静态的变量的时候。

参考：https://stackoverflow.com/questions/154469/unnamed-anonymous-namespaces-vs-static-functions

## 练习18.14

> 假设下面的 `operator*` 声明的是嵌套的命名空间 `mathLib::MatrixLib` 的一个成员：

```cpp
namespace mathLib {	namespace MatrixLib {		class matrix { /* ... */ };		matrix operator* (const matrix &, const matrix &);		// ...	}}
```

请问你应该如何在全局作用域中声明该运算符？

解：

```cpp
mathLib::MatrixLib::matrix mathLib::MatrixLib::operator* (const mathLib::MatrixLib::matrix &, const mathLib::MatrixLib::matrix &); 
```

## 练习18.15

> 说明 `using` 指示与 `using` 声明的区别。

解：

- 一条`using`声明语句一次只引入命名空间的一个成员。
- `using` 指示使得某个特定的命名空间中所有的名字都可见。

有点像python中的`import`:

```python
from lib import funcfrom lib import *
```

## 练习18.16

> 假定在下面的代码中标记为“位置1”的地方是对命名空间 Exercise 中所有成员的`using`声明，请解释代码的含义。如果这些`using`声明出现在“位置2”又会怎样呢？将`using`声明变为`using`指示，重新回答之前的问题。

```cpp
namespace Exercise {	int ivar = 0;	double dvar = 0;	const int limit = 1000;}int ivar = 0;//位置1void main() {	//位置2	double dvar = 3.1416;	int iobj = limit + 1;	++ivar;	++::ivar;}
```

解：

略

## 练习18.17

> 实际编写代码检验你对上一题的回答是否正确。

解：

略

## 练习18.18

> 已知有下面的 `swap` 的典型定义，当 `mem1` 是一个 `string` 时程序使用 `swap` 的哪个版本？如果 `mem1` 是 `int` 呢？说明在这两种情况下名字查找的过程。

```cpp
void swap(T v1, T v2){	using std::swap;	swap(v1.mem1, v2.mem1);	//交换类型的其他成员}
```

解：

`std::swap`是一个模板函数，如果是`string`会找到`string`版本；反之如果是`int`会找到`int`版本。

## 练习18.19

> 如果对 `swap` 的调用形如 `std::swap(v1.mem1, v2.mem1)` 将会发生什么情况？

解：

会直接调用`std`版的`swap`，但对后面的调用无影响。

## 练习18.20

> 在下面的代码中，确定哪个函数与`compute`调用匹配。列出所有候选函数和可行函数，对于每个可行函数的实参与形参的匹配过程来说，发生了哪种类型转换？

```cpp
namespace primerLib {	void compute();	void compute(const void *);}using primerLib::compute;void compute(int);void compute(double, double = 3.4);void compute(char*, char* = 0);void f(){	compute(0);}
```

解：

略

## 练习18.21

> 解释下列声明的含义，在它们当作存在错误吗？如果有，请指出来并说明错误的原因。

```cpp
(a) class CADVehicle : public CAD, Vehicle { ... };(b) class DbiList : public List, public List { ... };(c) class iostream : public istream, public ostream { ... };
```

## 练习18.22

> 已知存在如下所示的类的继承体系，其中每个类都定义了一个默认构造函数：

```cpp
class A { ... };class B : public A { ... };class C : public B { ... };class X { ... };class Y { ... };class Z : public X, public Y { ... };class MI : public C, public Z { ... };
```

对于下面的定义来说，构造函数的执行顺序是怎样的？

```cpp
MI mi;
```

## 练习18.23

> 使用练习18.22的继承体系以及下面定义的类 `D`，同时假定每个类都定义了默认构造函数，请问下面的哪些类型转换是不被允许的？

```cpp
class D : public X, public C { ... };p *pd = new D;(a) X *px = pd;(b) A *pa = pd;(c) B *pb = pd;(d) C *pc = pd;
```

## 练习18.24

> 在第714页，我们使用一个指向 `Panda` 对象的 `Bear` 指针进行了一系列调用，假设我们使用的是一个指向 `Panda` 对象的 `ZooAnimal` 指针将会发生什么情况，请对这些调用语句逐一进行说明。

## 练习18.25

> 假设我们有两个基类 `Base1` 和 `Base2` ，它们各自定义了一个名为 `print` 的虚成员和一个虚析构函数。从这两个基类中文名派生出下面的类，它们都重新定义了 `print` 函数：

```cpp
class D1 : public Base1 { /* ... */};class D2 : public Base2 { /* ... */};class MI : public D1, public D2 { /* ... */};
```

通过下面的指针，指出在每个调用中分别使用了哪个函数：

```cpp
Base1 *pb1 = new MI;Base2 *pb2 = new MI;D1 *pd1 = new MI;D2 *pd2 = new MI;(a) pb1->print();(b) pd1->print();(c) pd2->print();(d) delete pb2;(e) delete pd1;(f) delete pd2;
```

```cpp
struct Base1 {	void print(int) const;protected:	int ival;	double dval;	char cval;private:	int *id;};struct Base2 {	void print(double) const;protected:	double fval;private:	double dval;};struct Derived : public Base1 {	void print(std::string) const;protected:	std::string sval;	double dval;};struct MI : public Derived, public Base2 {	void print(std::vector<double>);protected:	int *ival;	std::vector<double> dvec;};
```

## 练习18.26

> 已知如上所示的继承体系，下面对`print`的调用为什么是错误的？适当修改`MI`，令其对`print`的调用可以编译通过并正确执行。

```cpp
MI mi;mi.print(42);
```

## 练习18.27

> 已知如上所示的继承体系，同时假定为MI添加了一个名为`foo`的函数：

```cpp
int ival;double dval;void MI::foo(double cval){	int dval;	//练习中的问题发生在此处}(a) 列出在MI::foo中可见的所有名字。(b) 是否存在某个可见的名字是继承自多个基类的？(c) 将Base1的dval成员与Derived 的dval 成员求和后赋给dval的局部实例。(d) 将MI::dvec的最后一个元素的值赋给Base2::fval。(e) 将从Base1继承的cval赋给从Derived继承的sval的第一个字符。
```

## 练习18.28

> 已知存在如下的继承体系，在 `VMI` 类的内部哪些继承而来的成员无须前缀限定符就能直接访问？哪些必须有限定符才能访问？说明你的原因。

```cpp
struct Base {	void bar(int);protected:	int ival;};struct Derived1 : virtual public Base {	void bar(char);	void foo(char);protected:	char cval;};struct Derived2 : virtual public Base {	void foo(int);protected:	int ival;	char cval;};class VMI : public Derived1, public Derived2 { };
```

## 练习18.29

> 已知有如下所示的类继承关系：

```cpp
class Class { ... };class Base : public Class { ... };class D1 : virtual public Base { ... };class D2 : virtual public Base { ... };class MI : public D1, public D2 { ... };class Final : public MI, public Class { ... };(a) 当作用于一个Final对象时，构造函数和析构函数的执行次序分别是什么？(b) 在一个Final对象中有几个Base部分？几个Class部分？(c) 下面的哪些赋值运算符将造成编译错误？Base *pb; Class *pc; MI *pmi; D2 *pd2;(a) pb = new Class;(b) pc = new Final;(c) pmi = pb;(d) pd2 = pmi;
```

## 练习18.30

> 在`Base`中定义一个默认构造函数、一个拷贝构造函数和一个接受`int`形参的构造函数。在每个派生类中分别定义这三种构造函数，每个构造函数应该使用它的形参初始化其`Base`部分。



## 练习19.1

> 使用 malloc 编写你自己的 operator new(sizt_t)函数，使用 free 编写operator delete(void *)函数。

## 练习19.2

> 默认情况下，allocator 类使用 operator new 获取存储空间，然后使用 operator delete 释放它。利用上一题中的两个函数重新编译并运行你的 StrVec 程序。

## 练习19.3

> 已知存在如下的类继承体系，其中每个类分别定义了一个公有的默认构造函数和一个析构函数：

```cpp
class A { /* ... */};
class B : public A { /* ... */};
class C : public B { /* ... */};
class D : public B, public A { /* ... */};
```

下面哪个 dynamic_cast 将失败？

```cpp
(a) A *pa = new C;
	B *pb = dynamic_cast<B*>(pa);
(b) B *pb = new B;
	C *pc = dynamic_cast<C*>(pb);
(c) A *pa = new D;
	B *pb = dynamic_cast<B*>(pa);
```

## 练习19.4

> 使用上一个练习定义的类改写下面的代码，将表达式*pa 转换成类型C&：

```cpp
if (C *pc = dynamic_cast<C*>(pa))
{
	//使用C的成员
} else {
	//使用A的成员
}
```

## 练习19.5

> 在什么情况下你应该用 dynamic_cast 替代虚函数？

## 练习19.6

> 编写一条表达式将 Query_base 指针动态转换为 AndQuery 指针。分别使用 AndQuery 的对象以及其他类型的对象测试转换是否有效。打印一条表示类型转换是否成功的信息，确保实际输出的结果与期望的一致。

## 练习19.7

> 编写与上一个练习类似的转换，这一次将 Query_base 对象转换为 AndQuery 的引用。重复上面的测试过程，确保转换能正常工作。

## 练习19.8

> 编写一条 typeid 表达式检查两个 Query_base 对象是否指向同一种类型。再检查该类型是否是 AndQuery。

## 练习19.9

> 编写与本节最后一个程序类似的代码，令其打印你的编译器为一些常见类型所起的名字。如果你得到的输出结果与本书类似，尝试编写一个函数将这些字符串翻译成人们更容易读懂的形式。

## 练习19.10

> 已知存在如下的类继承体系，其中每个类定义了一个默认公有的构造函数和一个虚析构函数。下面的语句将打印哪些类型名字？

```cpp
class A { /* ... */ };
class B : public A { /* ... */ };
class C : public B { /*...*/ };
(a) A *pa = new C;
	cout << typeid(pa).name() << endl;
(b) C cobj;
	A& ra = cobj;
	cout << typeid(&ra).name() << endl;
(c) B *px = new B;
	A& ra = *px;
	cout << typeid(ra).name() << endl;
```

## 练习19.11

> 普通的数据指针和指向数据成员的指针有何区别？

## 练习19.12

> 定义一个成员指针，令其可以指向 Screen 类的 cursor 成员。通过该指针获得 Screen::cursor 的值。

## 练习19.13

> 定义一个类型，使其可以表示指向 Sales_data 类的 bookNo 成员的指针。

## 练习19.14

> 下面的代码合法吗？如果合法，代码的含义是什么？如果不合法，解释原因。

```cpp
auto pmf = &Screen::get_cursor;
pmf = &Screen::get;
```

## 练习19.15

> 普通函数指针和指向成员函数的指针有何区别？

## 练习19.16

> 声明一个类型别名，令其作为指向 Sales_data 的 avg_price 成员的指针的同义词。

## 练习19.17

> 为 Screen 的所有成员函数类型各定义一个类型别名。

## 练习19.18

> 编写一个函数，使用 count_if 统计在给定的 vector 中有多少个空 string。

## 练习19.19

> 编写一个函数，令其接受vector<Sales_data>并查找平均价格高于某个值的第一个元素。

## 练习19.20

> 将你的 QueryResult 类嵌套在 TextQuery 中，然后重新运行12.3.2节中使用了 TextQuery 的程序。

## 练习19.21

> 编写你自己的 Token 类。

## 练习19.22

> 为你的 Token 类添加一个 Sales_data 类型的成员。

## 练习19.23

> 为你的 Token 类添加移动构造函数和移动赋值运算符。

## 练习19.24

> 如果我们将一个 Token 对象付给它自己将发生什么情况？

## 练习19.25

> 编写一系列赋值运算符，令其分别接收 union 中各种类型的值。

## 练习19.26

> 说明下列声明语句的含义并判断它们是否合法：

```cpp
extern "C" int compute(int *, int);
extern "C" double compute(double *, double);
```