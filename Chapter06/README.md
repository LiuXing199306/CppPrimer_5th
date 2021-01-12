# 第六章 函数

> 练习6.1：实参和形参的区别是什么？

形参被声明在函数的形参列表中，当函数被调用的时候实参初始化形参。

> 练习6.2：请指出下列函数哪个有错误，为什么？应该如何修改这些错误呢？
>
> ```C++
> (a)int f(){
>  string s;
>  // ...
>  return s;
> }
> (b)f2(int i){/* ... */}
> (c)int calc(int v1,int v1)/* ... */}
> (d)double square(double x) return x * x;
> ```

修改如下：

```C++
(a)string f(){
 string s;
 // ...
 return s;
}
(b)void f2(int i){/* ... */}
(c)int calc(int v1,int v2){/* ... */}
(d)double square(double x) {return x * x};
```

> 练习6.3：编写你自己的fact函数，上机检查是否正确。

```C++
#include <iostream>

int fact(int _n)
{
    int result = 1;
    if (_n == 0)
        result = 1;
    else if (_n > 1)
    {
        while (_n > 1)
        {
            result *= _n;
            --_n;
        }
    }
    else
    {
        std::cout << "integer must be not smaller than 0." << std::endl;
        result = -1;
    }
    
    return result;
}

int main()
{
    int m;
    std::cin >> m;
    std::cout << fact(m) << std::endl;
    return 0;
}
```

> 练习6.4：编写一个与用户交互的函数，要求用户输入一个数字，计算生成该数字的阶乘。在main函数中调用该函数。

```C++
#include <iostream>

int fact(int _n)
{
    int result = 1;
    if (_n == 0)
        result = 1;
    else if (_n > 1)
    {
        while (_n > 1)
        {
            result *= _n;
            --_n;
        }
    }
    else
    {
        std::cout << "integer must be not smaller than 0." << std::endl;
        result = -1;
    }
    
    return result;
}

int main()
{
    int m;
    std::cin >> m;
    std::cout << fact(m) << std::endl;
    return 0;
}
```

> 练习6.5：编写一个函数输出其实参的绝对值。

```C++
#include <iostream>

int my_abs(int _n)
{
    if (_n < 0)
        return -_n;
    else
        return _n;
}

int main()
{
    int m;
    std::cin >> m;
    std::cout << my_abs(m) << std::endl;
    return 0;
}
```

> 练习6.6：说明形参、局部变量以及局部静态变量的区别。编写一个函数，同时用到这三种形式。

形参和在函数体内部定义的变量都是局部变量，局部静态对象在程序的执行路径第一次经过对象定义语句时初始化，并且直到程序终止时才能被销毁，在此期间即使对象所在的函数结束执行也不会对它有什么影响。如果局部静态变量没有显式的初始值，内置类型的局部静态变量被初始化为0。

> 练习6.7：编写一个函数，当它第一次被调用时返回0，以后每次被调用返回值加1。

```C++
#include <iostream>
size_t count_calls()
{
    static size_t ctr = 0;
    return ctr++;
}

```

> 练习6.8：编写一个名为Chapter6.h的头文件，令其包含6.1节练习（第184页）中的函数声明。

```C++
int fact(int _n);
int my_abs(int _n);
```

> 练习6.9：编写你自己的fact.cc和factMain.cc，这两个文件都应该包含上一小节的练习中编写的Chapter6.h头文件。通过这些文件，理解你的编译器是如何支持分离式编译的。

```C++
// Chapter6.h
#ifndef CHAPTER6_H
#define CHAPTER6_H

int fact(int _n);
int my_abs(int _n);

#endif
```

```C++
// fact.cc
#include <iostream>
#include "Chapter6.h"
int fact(int _n)
{
    int result = 1;
    if (_n == 0)
        result = 1;
    else if (_n > 1)
    {
        while (_n > 1)
        {
            result *= _n;
            --_n;
        }
    }
    else
    {
        std::cout << "integer must be not smaller than 0." << std::endl;
        result = -1;
    }
    
    return result;
}
```

```C++
// factMain.cc
#include <iostream>
#include "Chapter6.h"
int main()
{
    int m;
    std::cin >> m;
    std::cout << fact(m) << std::endl;
    return 0;
}
```

> 练习6.10：编写一个函数，使用指针形参交换两个整数的值。在代码中调用该函数并输出交换后的结果，以此验证函数的正确性。

```C++
#include <iostream>
void change_ptr(int *p1, int *p2)
{
    auto temp = *p1;
    *p1 = *p2;    
    *p2 = temp;
}

int main()
{
    int m = 3, n = 5;
    change_ptr(&m, &n);
    std::cout << "m = " << m << " n = " << n << std::endl;
    return 0;
}
```

> 练习6.11：编写并验证你自己的reset函数，使其作用于引用类型的参数。

```C++
#include <iostream>

void reset(int &_i)
{
    _i = 0;
}

int main()
{
    int m = 3;
    reset(m);
    std::cout << "m = " << m << std::endl;
    return 0;
}
```

> 练习6.12：改写6.2.1节中练习6.10（第188页）的程序，使用引用而非指针交换两个整数的值。你觉得哪种方法更易于使用呢？为什么？

```C++
#include <iostream>
void change_by_conference(int &_m, int &_n)
{
    auto temp = _m;
    _m = _n;
    _n = temp;
}

int main()
{
    int m = 3, n = 5;
    change_by_conference(m, n);
    std::cout << "m = " << m << " n = " << n << std::endl;
    return 0;
}
```

> 练习6.13：假设T是某种类型的名字，说明以下两个函数声明的区别，一个是void f(T)，另一个是void f(&T)。

`void f(T)`函数通过值传递。

`void f(&T)` u通过引用传递。

> 练习6.14：举一个形参应该是引用类型的例子，再举一个形参不能是引用类型的例子。

```C++
// 形参应该是引用
void reset(& _i)
{
    _i = 0;
}
```

```C++
// 形参不能是引用
void print(std::vector<int>::iterator begin, std::vector<int>::iterator end)
{
        for (std::vector<int>::iterator iter = begin; iter != end; ++iter)
                std::cout << *iter << std::endl;
}
```

> 练习6.15：说明find_char函数中的三个形参为什么是现在的类型，特别说明为什么s是常量引用而occurs是普通引用？为什么s和occurs是引用类型而c不是？如果令s是普通引用会发生什么情况？如果令occurs是常量引用会发生什么情况？

`s`是常量引用，因为`s`的值在整个函数中不会被改变，同时使用引用传值避免了变量（字符串）的拷贝。

在函数中会改变`occurs`的值，因此`occurs`被定义成普通引用。`c`是一个字符，拷贝字符不会影响运行效率，`s`是普通引用编译能通过，但是`occurs`是常量引用的话，编译失败，因为程序会改变`occurs`的值。

> 练习6.16：下面的这个函数虽然合法，但是不算特别有用，指出他的局限性并设法改善。
>
> ```C++
> bool is_empty(string &s){return s.empty();}
> ```

不能将常量引用传入该函数，应该修改如下：

```C++
bool is_empty(const string &s){return s.empty();}
```

> 练习6.17：编写一个函数，判断string对象中是否含义大写字母，编写另一个函数，把string对象全部改写成小写形式，这两个函数中你使用的形参类型相同吗，为什么？

```C++
#include <iostream>
#include <string>
#include <cctype>

bool hasUpper(const std::string &_str)
{
    for (auto c : _str)
    {
        if (isupper(c))
            return true;
    }
    return false;
}

int main()
{
    std::string str1 = "jfalkjds";
    bool flag = hasUpper(str1);
    std::cout << "flag = " << flag << std::endl;
    return 0;
}
```

```C++
#include <iostream>
#include <string>
#include <cctype>

void toLower(std::string &_str)
{
    for (auto &c : _str)
    {
        c = tolower(c);
    }
}

int main()
{
    std::string str1 = "jfalkjDs";
    toLower(str1);
    for (auto c : str1)
        std::cout << c << std::endl;
    return 0;
}
```

不相同，第一个函数不会修改传入的参数，所以定义为常量引用，第二个函数会修改传入的参数，定义为普通的引用。

> 练习6.18：为下面的函数编写函数声明，从给定的名字中推断函数具备的功能。
>
> （a）名为compare的函数，返回布尔值，两个参数都是matrix类的引用。
>
> （b）名为change_val的函数，返回vector<int>的迭代器，有两个参数：一个是int，另一个是vector<int>的迭代器。

```C++
bool compare(const matrix & _matrix1,const matrix & _matrix2);
vector<int>::iterator change_val(int _i,vector<int>::iterator _iter);
```

> 练习6.19：假设有如下声明，判断哪个调用合法、哪个调用不合法。对于不合法的函数调用，说明原因。
>
> ```C++
> double calc(double);
> int count(const string &,char);
> int sum(vector<int>::iterator,vector<int>::iterator,int);
> vector<int> vec(10);
> (a)calc(23.4,55.1);
> (b)count("abcda",'a');
> (c)calc(66);
> (d)sum(vec.begin(),vec.end(),3.8);
> ```

(a)不合法，参数个数不对。

(b)合法。

(c)合法，发生隐式类型转换。

(d)合法，发生隐式类型转换。

> 练习6.20：引用形参什么时候应该是常量引用？如果形参应该是常量引用，而我们将其设为了普通引用，会发生什么情况？

当函数不会改变传入的形参时，这样会限制函数的使用场景，将形参设置为普通引用，不能传入常量参数进行调用。

> 练习6.21：编写一个函数，令其接收两个参数：一个是int型的数，另一个是int指针，函数比较int的值和指针所指的值，返回较大的那个，在该函数中指针的类型应该是什么？

```C++
#include <iostream>

int compare(const int _n, const int *const _pn)
{
    if (_n > *_pn)
        return _n;
    else
        return *_pn;
}
int main()
{
    int m = 5, n = 4;
    int *pn = &n;
    std::cout << compare(m, pn) << std::endl;
    return 0;
}
```

> 练习6.22：编写一个函数，令其交换两个int指针。

```C++
#include <iostream>

void change_ptr(int *&_p1, int *&_p2)
{
    int * temp = _p1;
    _p1 = _p2;
    _p2 = temp;
}

int main()
{
    int m = 3, n = 5;
    int *pm = &m, *pn = &n;
    change_ptr(pm, pn);
    std::cout << "*pm = " << *pm << " *pn = " << *pn << std::endl;

    return 0;
}
```

> 练习6.23：参考本节介绍的几个print函数，根据理解编写你自己的版本，依次调用每个函数使其输入下面定义的i和j。
>
> ```C++
> int i = 0, j[2] = {0,1};
> ```

```C++
#include <iostream>
void print1(int *const _a)
{
    if (_a)
        std::cout << *_a << std::endl;
}

void print2(const int *begin, const int *end)
{
    while (begin != end)
        std::cout << *begin++ << std::endl;
}

int main()
{
    int i = 0, j[2] = {0, 1};
    print1(&i);
    print2(std::begin(j), std::end(j));
    return 0;
}
```

> 练习6.24：描述下面这个函数的行为，如果代码中存在问题，请指出并改正。
>
> ```C++
> void print(const int ia[10])
> {
> 	for(size_t i = 0; i!= 10; ++i)
> 	    cout << ia[i] << endl;
> }
> ```

当数组作为函数参数传入的时候，自动转换为常量指针，维数大小失去作用，所以可以传入维数小于10的数组作为参数，这时for循环失去作用，修改如下：

```C++
void print(const int (&ia)[10])
{
	for(size_t i = 0; i!= 10; ++i)
	    cout << ia[i] << endl;
}
```

> 练习6.25：编写一个main函数，令其接受两个实参，把实参的内容连接成一个string对象并输出出来。

```C++
#include <iostream>
#include <string>
int main(int argc, char **argv)
{
    std::cout << std::string(argv[1]) + std::string(argv[2]) << std::endl;
    return 0;
}
```

> 练习6.26：编写一个程序，使其接受本节所示的选项，输出传递给main函数的实参的内容。

```C++
#include <iostream>
#include <string>
int main(int argc, char **argv)
{
    for (int i = 1; i < argc; ++i)
    {
        std::cout << argv[i] << std::endl;
    }
    return 0;
}
```

> 练习6.27：编写一个函数，它的参数是initializer_list<int>类型的对象，函数的功能是计算列表中所有元素的和。

```C++
#include <iostream>
int sum(std::initializer_list<int> _li)
{
    int result = 0;
    for (auto i : _li)
        result += i;
    return result;
}

int main()
{
    std::initializer_list<int> li = {0, 1, 2};
    std::cout << sum(li) << std::endl;
    return 0;
}
```

>练习6.28：在error_msg函数的第二个版本中包含ErrCode类型的参数，其中循环内的elem是什么类型？

绑定在string上的常量引用。

> 练习6.29：在范围for循环中使用initializer_list对象时，应该将循环控制变量声明成引用类型吗，为什么？

取决于initializer_list对象的类型，如果是内置类型，没必要，其余的最好声明为引用类型，避免变量的拷贝。

> 练习6.30：编译第200页的str_subrange函数，看看你的编译器是如何处理函数中的错误的。

略。

> 练习6.31：什么情况下返回的引用无效？什么情况下返回的常量引用无效？

返回局部对象的指针或者引用是无效的。

> 练习6.32：下面的函数合法吗？如果合法，说明其功能，如果不合法，修改其中的错误并解释原因。
>
> ```C++
> int &get(int *array, int index) {return arry[index];}
> int main()
> {
>  int ia[10];
>  for(int i = 0;i != 10; ++i)
>      get(ia,i) = i;
> }
> ```

合法，将数组`ia`的元素分别赋值为0~9。

>练习6.33：编写一个递归函数，输出vector对象的内容。

```C++
#include <iostream>
#include <vector>

void print(std::vector<int>::const_iterator _beginIter, std::vector<int>::const_iterator _endIter)
{
    if (_beginIter != _endIter)
    {
        std::cout << *_beginIter << std::endl;
        print(std::next(_beginIter), _endIter);
    }
}

int main()
{
    std::vector<int> intVector = {0};
    print(intVector.cbegin(), intVector.cend());
    return 0;
}
```

> 练习6.34：如果factorial函数的停止条件如下所示，将发生什么情况？
>
> ```C++
> if(val != 0)
> ```

如果传入的参数是非负数，则结果均为0，如果传入的参数是负数，则陷入无限死循环。

> 练习6.35：在调用factorial函数时，为什么我们传入的值是val-1而非val--？

函数会陷入无限死循环。

> 练习6.36：编写一个函数的声明，使其返回数组的引用并且该数组包含10个string对象，不要使用尾置返回类型、decltype或者类型别名。

```C++
string(&function())[10];
```

> 练习6.37：为上一题的函数再写三个声明，一个使用类型别名，另一个使用尾置返回类型，最后一个使用decltype关键字，你觉得哪种形式最好，为什么？

```C++
// 类型别名
typedef string ref[10];
using ref = string[10];
ref *function();

// 尾置返回类型
auto function()->string(&)[10];

// decltype关键字
string a[10] = {"a","b","a","b","a","b","a","b","a","b"};
decltype(a) &function(); 
```

> 练习6.38：修改arrPtr函数，使其返回数组的引用。

```C++
int (&func(int i))[10];
// 类型别名
typedef int arr[10];
using arr = int[10];
arr &fun(int i);
// 尾置返回类型
auto fun(int i)->int(&)[10];
// decltype关键字
int a[10] = {0,1,2};
decltype(a) &arrPtr(int i);
```

> 练习6.39：说明下面的每组声明中第二条声明语句是何含义，如果有非法的声明请指出来。
>
> ```C++
> (a)int calc(int,int);
> int calc(const int, const int);
> (b)int get();
> double get();
> (c)int *reset(int *);
> double *reset(double *);
> ```

(a)不合法，因为顶层const不能构成重载。

(b)不合法，只有返回值类型不同时，不能构成重载，函数名不能相同。

(c)合法，函数构成重载。

> 练习6.40：下面的哪个声明是错误的，为什么？
>
> ```C++
> (a)int ff(int a, int b = 0, int c = 0);
> (b)char *init(int ht = 24, int wd, char bckgrnd);
> ```

(a)正确。

(b)错误，因为一旦一个参数被设置为默认参数，之后所有的参数都必须是默认参数。

> 练习6.41：下面的哪个调用是非法的，为什么？哪个调用虽然合法但是显然与程序员的初衷不符，为什么？
>
> ```C++
> char *init(int ht, int wd = 80, char bckgrnd = ' ');
> (a)init();
> (b)init(24,10);
> (c)init(14,'*');
> ```

(a)非法，第一个参数没有相应的实参来初始化。

(b)合法。

(c)合法，但是和程序员初衷不符，字符能转换成整型数。

> 练习6.42：给make_plural函数（参见6.3.2节，第201页）的第二个形参赋予默认实参‘s’，利用新版本的函数输出单词success和failure的单数和复数形式。

```C++
string make_plural(size_t ctr, const string &word,const string &ending = 's')
{
    return (ctr > 1) ? word+ending : word;
}
```

> 练习6.43：你会把下面的哪个声明和定义放在头文件中？哪个放在源文件中？为什么？
>
> ```C++
> (a)inline bool eq(const BigInt&, const BigInt&){...}
> (b)void putValues(int *arr,int size);
> ```

(a)声明和定义都放在头文件中。

(b)声明放在头文件中，定义放在源文件中。

> 练习6.44：将6.2.6节（第189页）的isShorter函数改写成内联函数。

```C++
inline bool isShorter(const string &s1,const string &s2)
{
    return s1.size() < s2.size();
}
```

> 练习6.45：回顾在前面的联系中你编写的那些函数，他们应该是内联函数吗？如果是将它们改写成内联函数，如果不是说明原因。

略。

> 练习6.46：能把isShort函数定义成constexpr函数吗，如果能，将它改写成constexpr函数，如果不能请说明原因。

不能，因为要成为constexpr函数必须满足的要求为：参数值列表和返回值类型必须为字面值类型，同时函数体有且只要一条return语句，而isShort函数的参数值列表和返回值类型都不是字面值类型。

> 练习6.47：改写6.3.2节（第205页）练习中使用递归输出vector内容的程序，使其有条件地输出与执行过程相关的信息。例如，每次调用输出 vector对象的大小，分别在打开和关闭调试器的情况下编译并执行这个程序。

```C++
#include <iostream>
#include <vector>
#define DEBUG  // comment

void print(std::vector<int>::const_iterator _beginIter, std::vector<int>::const_iterator _endIter)
{
    if (_beginIter != _endIter)
    {
        std::cout << *_beginIter << std::endl;
        auto nextIter = std::next(_beginIter);
        print(nextIter, _endIter);
        #ifndef DEBUG
        std::cout << "vector size = " << _endIter - nextIter << std::endl;
        #endif
    }
}

int main()
{
    std::vector<int> intVector = {0};
    print(intVector.cbegin(), intVector.cend());
    return 0;
}
```

> 练习6.48：说明下面这个循环的含义，它对assert的使用合理吗？
>
> ```C++
> string s;
> while(cin >> s && s != sought) {} // 空函数体
> assert(cin);
> ```

不合理，因为assert常常用来检查“不能发生”的条件，但是while循环结束之后，通常cin会是无效的，所以修改为如下比较好：assert(!cin || s == sought)

> 练习6.49：什么是候选函数，什么是可行函数？

函数匹配的第一步是选定本次调用对应的重载函数集，集合中的函数称为候选函数，候选函数具备两个特征：一是与被调用的函数同名，二是其声明在调用点可见。第二步考察本次调用提供的实参，然后从候选函数中选出能被这组实参调用的函数，这些选出的函数称为可行函数，可行函数也是有两个特征：一是其形参数量与本次调用提供的实参数量相等，二是每个实参的类型与对应的形参类型相同，或者能转换为形参的类型。

> 练习6.50：已知有第217页对函数的声明，对于下面的每一个调用列出**可行函数**，其中哪个函数是最佳匹配？如果调用不合法，是因为没有可匹配的函数还是因为调用具有**二义性**？
>
> ```C++
> (a)f(2.56,42);
> (b)f(42);
> (c)f(42,0);
> (d)f(2.56,3.14);
> ```

(a)调用不合法，调用具有二义性。

(b)可行函数：void f(int); void f(double,double = 3.14);最佳匹配：void f(int);

(c)可行函数：void f(int,int); void f(double,double = 3.14);最佳匹配： void f(int,int);

(d)可行函数：void f(int,int); void f(double,double = 3.14);最佳匹配： void f(double,double = 3.14);

> 练习6.51：编写函数f的四个版本，令其各输出一条可以区分的消息。验证上一个练习的答案，如果你回答错了，反复研究本节的内容直到你弄清自己错在何处。

略。

> 练习6.52：已知有如下声明
>
> ```C++
> void manip(int,int);
> double dobj;
> ```
>
> 请指出下列调用中每个类型转换的等级（参见6.6.1节，第219页）。
>
> ```C++
> (a)manip(`a`,`z`);
> (b)manip(55.4,dobj);
> ```

(a)通过类型提升实现的匹配。

(b)通过算数类型转换实现的匹配。

> 练习6.53：说明下列每组声明中的第二条语句会产生什么影响，并指出那些不合法（如果有的话）。
>
> ```C++
> (a)int calc(int &, int &);
> int calc(const int &, const int &);
> (b)int calc(char *, char *);
> int calc(const char *, const char *);
> (c)int calc(char *, char *);
> int calc(char *const, char* const);
> ```

(a)函数重载。

(b)函数重载。

(c)不合法，顶层const不能构成函数重载。

> 练习6.54：编写函数的声明，令其接受两个int形参并且返回类型也是int；然后声明一个vector对象，令其元素是指向该函数的指针。

```C++
int f(int _n1,int _n2);
int (*pf)(int _n1,int _n2) = f;
vector<pf> pfVector;
```

> 练习6.55：编写4个函数，分别对两个int值执行加、减、乘、除运算；在上一题创建的vector对象中保存指向这些函数的指针。

```C++
int add(int _n1,int _n2){return _n1 + _n2;}
int subtract(int _n1,int _n2){return _n1 - _n2;}
int multiply(int _n1,int _n2){return _n1 * _n2;}
int divide(int _n1,int _n2){return _n1 / _n2;}

int (*pfAdd)(int _n1,int _n2);
int (*pfSubtract)(int _n1,int _n2);
int (*pfMultiply)(int _n1,int _n2);
int (*pfDivide)(int _n1,int _n2);

pfVector = {pfAdd,pfSubtract,pfMultiply,pfDivide};
```

> 练习6.56：调用上述vector对象中的每个元素并输出其结果。

```C++
std::cout << pfAdd(3,6) << std::endl;
std::cout << pfSubtrct(3,6) << std::endl;
std::cout << pfMultiply(3,6) << std::endl;
std::cout << pfDivide(3,6) << std::endl;
```

