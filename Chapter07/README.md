# 第七章 类

> 练习7.1：使用2.6.1节练习定义的Sales_data类为1.6节（第21页）的交易程序编写一个新版本。

略。

> 练习7.2：曾在2.6.2节的练习（第67页）中编写了一个Sales_data类，请向这个类添加combine和isbn成员。

```C++
// Sales_data.h
#ifndef SALES_DATA_H
#define SALES_DATA_H
#include <string>

struct Sales_data
{
    std::string isbn() const { return bookNo; }
    Sales_data &combine(const Sales_data &);
    double avg_price() const;

    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

Sales_data add(const Sales_data &, const Sales_data &);
std::ostream &print(std::ostream &, const Sales_data &);
std::istream &read(std::istream &, Sales_data &);

#endif //SLAES_DATA_H

// Sales_data.cpp
#include "Sales_data.h"
double Sales_data::avg_price() const
{
    if (units_sold)
        return revenue / units_sold;
    else
        return 0;
}

Sales_data& Sales_data::combine(const Sales_data&rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

```

> 练习7.3：修改7.11节（第229页）的交易处理程序，令其使用这些成员。

```C++
Salses_data total;
if (read(cin, total))
{
    Sales_data trans;
    while (read(cin, trans))
    {
        if (total.isbn() = trans.isbn())
        {
            total.combine(trans);
        }
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
    cerr << "No data!" << endl;
}

```

> 练习7.4：编写一个名为Person的类，使其表示人员的姓名和住址。使用string对象存放这些元素，接下来的练习将不断充实这个类的其他特征。

```C++
#ifndef PERSON_H
#define PERSON_H
#include <stringZ>
class Person
{
    std::string name;
    std::string address;

    std::string getName() const {return name;}
    std::string getAddress() const (return address;)
};

#endif // PERSON_H
```

> 练习7.5：在你的Person类中提供一些操作使其能够返回姓名和住址，这些函数是否应该是const呢，解释原因。

```C++
#ifndef PERSON_H
#define PERSON_H
#include <stringZ>
class Person
{
    std::string name;
    std::string address;

    std::string getName() const {return name;}
    std::string getAddress() const (return address;)
};

#endif // PERSON_H
```

应该是const，因为这样Person的常量对象、常量指针和常量引用才能调用这些成员函数。

> 练习7.6：对于函数add、read和print，定义你自己的版本。

```C++
#include "Sales_data.h"
double Sales_data::avg_price() const
{
    if (units_sold)
        return revenue / units_sold;
    else
        return 0;
}

Sales_data& Sales_data::combine(const Sales_data&rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}

Sales_data add(const Sales_data &_lhs, const Sales_data &_rhs)
{
    Sales_data sum = _lhs;
    sum.combine(_rhs);
    return sum;

}

std::ostream &print(std::ostream &_os, const Sales_data &_item)
{
    _os << _item.isbn() << " " << _item.units_sold << " " << _item.revenue << " " << _item.avg_price();
    return _os;

}

std::istream &read(std::istream &_is, Sales_data &_item)
{
    double price = 0;
    _is >> _item.bookNo >> _item.units_sold >> price;
    _item.revenue = price * _item.units_sold;
    return _is;

```

> 练习7.7：使用这些函数重写7.12节（第233页）练习中的交易处理程序。

```C++
Salses_data total;
if (read(cin, total))
{
    Sales_data trans;
    while (read(cin, trans))
    {
        if (total.isbn() = trans.isbn())
        {
            total.combine(trans);
        }
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
    cerr << "No data!" << endl;
}

```

> 练习7.8：为什么read函数将其Sales_data参数定义成普通的引用，而print将其参数定义成常量引用？

因为read函数读取对象之后需要修改该参数，而print函数不需要。

> 练习7.9：对于7.1.2节（第233页）练习中的代码，添加读取和打印Person对象的操作。

```C++
// Person.h
#ifndef PERSON_H
#define PERSON_H
#include <iostream>
#include <string>
struct Person
{
    std::string name;
    std::string address;

    std::string getName() const { return name; }
    std::string getAddress() const { return address; }
};

std::istream &read(std::istream &_is, Person &_person);
std::ostream &print(std::ostream &_os, const Person &_pseson);

#endif // PERSON_H
```

```C++
// Person.cpp
#include <iostream>
#include "Person.h"
std::istream &read(std::istream &_is, Person &_person)
{
    _is >> _person.name >> _person.address;
    return _is;
}

std::ostream &print(std::ostream &_os, const Person &_person)
{
    _os << _person.name << " " << _person.address;
    return _os;
}
```

> 练习7.10：在下面这条if语句中，条件部分的作用是什么？
>
> ```C++
> if(read(read(cin,data1),data2));
> ```

判断读取两次Sales_data对象之后的输入流的状态。

> 练习7.11：在你的Sales_data类中添加构造函数，然后编写一段程序令其用到每个构造函数。

略。

> 练习7.12：把只接受一个istream作为参数的构造函数定义移到类的内部。

略。

> 练习7.13：使用istream构造函数重写第229页的程序。

略。

> 练习7.14：编写一个构造函数，令其用我们提供的类内初始值显式地初始化成员。

```C++
Sales_data::Sales_data(){}
```

> 练习7.15：为你的Person类添加正确的构造函数。

略。

> 练习7.16：在类的定义中对于访问说明符出现的位置和次数有限制吗？如果有，是什么？什么样的成员应该定义在public说明符之后？什么样的成员应该定义在private说明符之后？

在累的定义中对于访问说明符出现的位置和次数没有限制，在整个程序中能够被访问，也就是类的接口定义在public说明符之后，可以被类的成员函数访问，但是不能被使用该类的代码使用的成员应该放在private说明符之后。

> 练习7.17：使用class和struct时有区别吗？如果有，是什么？

默认访问权限不同，class的默认访问权限是private，struct的默认访问权限是public。

> 练习7.18：封装的含义是什么，它有什么用处？

将类的接口和实现分离，使得用户在使用时不用关心类是怎么样实现的。

> 练习7.19：在你的Person类中，你想把哪些成员声明称public的？哪些声明称private的？解释你这样做的原因。

略。

> 练习7.20：友元在什么时候有用，请分别列举出友元的利弊。

当函数不是类的成员函数，但是需要访问类的私有成员变量的时候，需要将该函数定义为类的友元函数。

优点：类外的函数可以访问类的私有成员变量。

缺点：破坏了类的封装性。

> 练习7.21：修改你的Sales_data类使其隐藏实现的细节，你之前编写的关于Sales_data操作的程序应该继续使用，借助类的新定义重新编译该程序，确保其正常工作。

略。

> 练习7.22：修改你的Person类，使其隐藏实现的细节。

略。

> 练习7.23：编写你自己的Screen类。

```C++
// Screen.h
#ifndef CHAPTER07_CLASS_SCREEN_H
#define CHAPTER07_CLASS_SCREEN_H

#include <string>
using std::string;

class Screen
{
public:
    typedef string::size_type pos; 
    Screen() = default;
    Screen(pos width, pos height) : _width(width), _height(height), _contents(width * height, ' ') {}
    Screen(pos width, pos height, char c) : _width(width), _height(height), _contents(width * height, c) {}
    // 获取光标处的字符
    char get() const { return _contents[_cursor]; }
    inline char get(pos width, pos height);
    Screen &move(pos r, pos c);
    void some_member() const;

private:
    pos _width = 0;
    pos _height = 0;
    pos _cursor = 0;
    string _contents; // 屏幕上的内容
    mutable size_t _access_ctr = 0;
};

#endif // CHAPTER07_CLASS_SCREEN_H

```

```C++
// Screen.cpp
#include "Screen.h"

char Screen::get(pos r, pos c)
{
    return _contents[r * _width + c];
}

inline Screen &Screen::move(pos r, pos c)
{
    _cursor = r * _width + c;
    return *this;
}

void Screen::some_member() const
{
    ++ _access_ctr;
}

```

> 练习7.24：给你的Screen类添加三个构造函数：一个默认构造函数；另一个构造函数接受宽和高的值，然后将contents初始化成给定数量的空白；第三个构造函数接受宽和高的值以及一个字符，该字符为初始化之后屏幕的内容。

见习题7.23。

> 练习7.25：Screen能安全地依赖于拷贝和赋值操作的默认版本吗？如果能，为什么？如果不能，为什么？

Screen能安全地依赖于拷贝和赋值操作的默认版本，因为Screen的成员变量均为内置类型和标准库中的string类型。

> 练习7.26：将Sales_data:avg_price定义成内联函数。

在avg_price函数的定义之前加上inline即可。

> 练习7.27：给你自己的Screen类添加move、set和display函数，通过执行下面的代码检验你的类是否正确。
>
> ```C++
>  Screen myScreen(5, 5, 'X');
>  myScreen.move(4, 0).set('#').dispaly(cout);
>  cout << "\n";
>  myScreen.dispaly(cout);
>  cout << "\n";
> ```
>

```C++
// Screen.h
#ifndef CHAPTER07_CLASS_SCREEN_H
#define CHAPTER07_CLASS_SCREEN_H

#include <string>
#include <iostream>
using std::ostream;
using std::string;

class Screen
{
public:
    typedef string::size_type pos; // 用来定义类型的成员必须先定义后使用，类型成员通常出现在类开始的地方
    Screen() = default;
    Screen(pos width, pos height, char c) : _width(width), _height(height), _contents(width * height, c) {}
    // 获取光标处的字符
    char get() const { return _contents[_cursor]; }
    inline char get(pos width, pos height);
    Screen &move(pos r, pos c);
    void some_member() const;

    // 设置光标所在位置的字符或者任意给定位置的字符
    Screen &set(char c);
    Screen &set(pos r, pos col, char c);

    // 根据对象是否是const重载了display函数
    Screen &dispaly(ostream &os)
    {
        do_display(os);
        return *this;
    }

    const Screen &dispaly(ostream &os) const
    {
        do_display(os);
        return *this;
    }

private:
    pos _width = 0;
    pos _height = 0;
    pos _cursor = 0;
    string _contents; // 屏幕上的内容
    // 一个可变数据成员永远不可能是const，即使它是cosnt对象的数据成员，或者是在常量成员函数中也能够修改可变数据成员
    mutable size_t _access_ctr = 0;
    // 对于公共代码使用私有功能函数
    void do_display(ostream &os) const { os << _contents; }
};

#endif // CHAPTER07_CLASS_SCREEN_H

```

```C++
// Screen.cpp
#ifndef CHAPTER07_CLASS_SCREEN_H
#define CHAPTER07_CLASS_SCREEN_H

#include <string>
#include <iostream>
using std::ostream;
using std::string;

class Screen
{
public:
    typedef string::size_type pos; // 用来定义类型的成员必须先定义后使用，类型成员通常出现在类开始的地方
    Screen() = default;
    Screen(pos width, pos height, char c) : _width(width), _height(height), _contents(width * height, c) {}
    // 获取光标处的字符
    char get() const { return _contents[_cursor]; }
    inline char get(pos width, pos height);
    Screen &move(pos r, pos c);
    void some_member() const;

    // 设置光标所在位置的字符或者任意给定位置的字符
    Screen &set(char c);
    Screen &set(pos r, pos col, char c);

    // 根据对象是否是const重载了display函数
    Screen &dispaly(ostream &os)
    {
        do_display(os);
        return *this;
    }

    const Screen &dispaly(ostream &os) const
    {
        do_display(os);
        return *this;
    }

private:
    pos _width = 0;
    pos _height = 0;
    pos _cursor = 0;
    string _contents; // 屏幕上的内容
    // 一个可变数据成员永远不可能是const，即使它是cosnt对象的数据成员，或者是在常量成员函数中也能够修改可变数据成员
    mutable size_t _access_ctr = 0;
    // 对于公共代码使用私有功能函数
    void do_display(ostream &os) const { os << _contents; }
};

#endif // CHAPTER07_CLASS_SCREEN_H

```

```C++
// main.cpp
#include <iostream>
#include "Screen.h"

using std::cout;

int main()
{
    Screen myScreen(5, 5, 'X');
    myScreen.move(4, 0).set('#').dispaly(cout);
    cout << "\n";
    myScreen.dispaly(cout);
    cout << "\n";
    return 0;
}
```

运行结果如下：

```
XXXXXXXXXXXXXXXXXXXX#XXXX
XXXXXXXXXXXXXXXXXXXX#XXXX
```

> 练习7.28：如果move、set和display函数的返回值类型不是Screen&而是Screen，则在上一个练习中将会发生什么情况？

第10行函数的运行结果相同，但是第12行函数运行结果不同，因为当函数返回值不是引用的时候，函数作用在局部变量上，第10行的的set函数没有作用在myScreen上，而是作用在局部变量上，所以运行结果如下：

```
XXXXXXXXXXXXXXXXXXXX#XXXX
XXXXXXXXXXXXXXXXXXXXXXXXX
```

> 练习7.29：修改你的Screen类，令move、set和displ函数返回Screen并检查程序的运行结果，在上一个练习中你的推测正确吗？

结果如上所述。

> 练习7.30：通过this指针使用成员的做法虽然合法，但是有点多余，讨论显式地使用指针访问成员的优缺点。

优点：

- 减少括号偶的使用
- 成员变量的含义更加明确

- 当函数形参和类型成员变量名称相同的时候，含义更加明确，比如：

```C++
void setAddr(const std::string &addr) {this->addr = addr;}
```

缺点：

- 阅读更加麻烦
- 书写更加繁琐

```C++
std::string getAddr() {return this->addr;}
```

> 练习7.31：定义一对类X和Y，其中X包含一个指向Y的指针，而Y包含一个类型为X的对象。

```C++
class Y;
class X
{
    Y *py = nullptr;
};
class Y
{
    X x;    
};
```

> 练习7.32：定义你自己的Screen和Window_mgr，其中clear是Window_mgr的成员，是Screen的友元。

```C++
// Screen.h
#ifndef CHAPTER07_CLASS_SCREEN_H
#define CHAPTER07_CLASS_SCREEN_H

#include <string>
#include <iostream>
#include "Window_mgr.h"
using std::ostream;
using std::string;

class Window_mgr;

class Screen
{
    friend void Window_mgr::clear(ScreenIndex);
public:
    typedef string::size_type pos; // 用来定义类型的成员必须先定义后使用，类型成员通常出现在类开始的地方
    Screen() = default;
    Screen(pos width, pos height, char c) : _width(width), _height(height), _contents(width * height, c) {}
    // 获取光标处的字符
    char get() const { return _contents[_cursor]; }
    inline char get(pos width, pos height);
    Screen &move(pos r, pos c);
    void some_member() const;

    // 设置光标所在位置的字符或者任意给定位置的字符
    Screen &set(char c);
    Screen &set(pos r, pos col, char c);

    // 根据对象是否是const重载了display函数
    Screen &dispaly(ostream &os)
    {
        do_display(os);
        return *this;
    }

    const Screen &dispaly(ostream &os) const
    {
        do_display(os);
        return *this;
    }

private:
    pos _width = 0;
    pos _height = 0;
    pos _cursor = 0;
    string _contents; // 屏幕上的内容
    // 一个可变数据成员永远不可能是const，即使它是cosnt对象的数据成员，或者是在常量成员函数中也能够修改可变数据成员
    mutable size_t _access_ctr = 0;
    // 对于公共代码使用私有功能函数
    void do_display(ostream &os) const { os << _contents; }
};

#endif // CHAPTER07_CLASS_SCREEN_H

```

```C++
// Screen.cpp
#include "Screen.h"

char Screen::get(pos r, pos c)
{
    return _contents[r * _width + c];
}

Screen &Screen::move(pos r, pos c)
{
    pos row = r * _width;
    _cursor = row + c;
    return *this;
}

void Screen::some_member() const
{
    ++_access_ctr;
}

Screen &Screen::set(char c)
{
    _contents[_cursor] = c;
    return *this;
}
Screen &Screen::set(pos r, pos col, char c)
{
    _contents[r * _width + col] = c;
    return *this;
}
```

```C++
// Window_mgr.h
#ifndef CHAPTER07_CLASS_WINDOW_MGR_H
#define CHAPTER07_CLASS_WINDOW_MGR_H

#include <vector>
#include "Screen.h"

using std::vector;

class Window_mgr
{
public:
    // 窗口中每个屏幕的编号
    using ScreenIndex = vector<Screen>::size_type;
    // 按照编号将制定的Screen重置为空白
    void clear(ScreenIndex i)
    {
        Screen &screen = _screens[i];
        screen._contents = string(screen._width * screen._height, ' ');
    }

private:
    vector<Screen> _screens{Screen(24, 80, ' ')};
};

#endif // CHAPTER07_CLASS_WINDOW_MGR_H

```

```C++
// Window_mgr.cpp
#include "Window_mgr.h"

```

> 练习7.33：如果我们给Screen添加一个如下所示的size成员将发生什么情况？如果出现了问题，请尝试修改它。
>
> ```C++
> pos Screen::size() const
> {
>     return height * width;
> }
> ```

编译器会报错：pos未被定义。

修改如下：

```C++
Screen::pos Screen::size() const
{
    return height * width;
}
```

>练习7.34：如果我们把第256页Screen类的pos的typedef放在类的最后一行，会发生什么情况？

编译器会报错：`pos`未定义。

> 练习7.35：解释下面代码的含义，说明其中Type和initVal分别使用了哪个定义。如果代码存在错误，尝试修复它。
>
> ```C++
> typedef string Type;
> Type initVal();
> class Exercise {
> public:
>     typedef double Type;
>     Type setVal(Type);
>     Type initVal();
> private:
>     int val;
> };
> 
> Type Exercise::setVal(Type parm) {  
>     val = parm + initVal();
>     return val;
> }
> ```

```C++
typedef string Type;
Type initVal();  // 此处Type使用的是上一行的string的别名
class Exercise {
public:
    typedef double Type;
    Type setVal(Type);  // 此处Type使用的是上一行的double的别名
    Type initVal();     // 此处Type使用的是double的别名
private:
    int val;
};

Type Exercise::setVal(Type parm) {  // 此处第一个Type使用的是类外定义的string的别名，此处第二个Type使用的是double的别名
    val = parm + initVal();  // 此处initVal使用的是类内成员函数
    return val;
}
```

修改如下：

```C++
typedef string Type;
Type initVal();  
class Exercise {
public:
    typedef double Type;
    Type setVal(Type);  
    Type initVal();    
private:
    int val;
};

Exercise::Type Exercise::setVal(Type parm) {  
    val = parm + initVal();
    return val;
}
```

> 练习7.36：下面的初始值是错误的，请找出问题并尝试修复它。
>
> ```C++
> struct X {
>   X (int i, int j): base(i), rem(base % j) { }
>   int rem, base;
> };
> ```

修改如下：

```C++
struct X {
  X (int i, int j): base(i), rem(base % j) { }
  int base, rem;
};
```

>练习7.37：使用本节提供的Sales_data类，确定初始化下面的变量时分别使用了哪个构造函数，然后罗列出每个对象所有数据成员的值。
>
>```C++
>Sales_data first_item(cin);   
>
>int main() {
>  Sales_data next;  
>  Sales_data last("9-999-99999-9"); 
>}
>
>```

```C++
Sales_data first_item(cin);   // 使用了Sales_data(std::istream &is);该所有成员变量的值取决于输入。

int main() {
  Sales_data next;  // 使用了Sales_data(std::string s = ""); 其中，bookNo = "", cnt = 0, revenue = 0.0。
  Sales_data last("9-999-99999-9"); // 使用了Sales_data(std::string s = "");其中bookNo = "9-999-99999-9", cnt = 0, revenue = 0.0。
}

```

> 练习7.38：有些情况下我们希望提供cin作为接受istream&参数的构造函数的默认实参，请声明这样的构造函数。

```C++
#include <iostream>
using std::cin;
using std::istream;

Sales_data(istream & is = cin);
```

> 练习7.39：如果接受string的构造函数和接受istream&的构造函数都使用默认实参，这种行为合法吗，如果不，为什么？

不合理，因为这样会引起调用的二义性。

>  练习7.40：从下面的抽象概念中选择一个（或者你自己指定一个），思考这样的类需要哪些数据成员，提供一组合理的构造函数并阐明这样做的原因。
>
> (a)Book (b)Date (c)Employee (d)Vehicle (e)Object (f)Tree

```C++
class Book {
public:
    Book() = default;
    Book(unsigned no, std::string name, std::string author, std::string pubdate) : no_(no), name_(name), author_(author), pubdate_(pubdate) { }
    Book(std::istream &is) { is >> no_ >> name_ >> author_ >> pubdate_; }

private:
    unsigned no_;
    std::string name_;
    std::string author_;
    std::string pubdate_;
};
```

>练习7.41：使用委托构造函数重新编写你的Sales_data类，给每个构造函数体添加一条语句，令其一旦执行就打印一条信息。用各种可能的方式分别创建Sales_data对象，认真研究每次输出的信息直到你确实理解了委托构造函数的执行顺序。

略。

> 练习7.42：对于你在练习7.40（参见7.5.1节，第261页）中编写的类，确定哪些构造函数可以使用委托。如果可以的话，编写委托构造函数，如果不可以，从抽象概念列表中重新选择一个你认为可以使用委托构造函数的，为挑选出的这个概念编写类定义。

```C++
class Book {
public:
  Book(unsigned no, std::string name, std::string author, std::string pubdate):no_(no), name_(name), author_(author), pubdate_(pubdate) { }
  Book() : Book(0, "", "", "") { }
  Book(std::istream &in) : Book() { in >> no_ >> name_ >> author_ >> pubdate_; }

private:
  unsigned no_;
  std::string name_;
  std::string author_;
  std::string pubdate_;
};
```

> 练习7.43：假设有一个名为NoDefault的类，它有一个接受int的构造函数，但是没有默认构造函数。定义类C，C有一个NoDefault类型的成员，定义C的默认构造函数。

```C++
class NoDefault
{
    NoDefault(int){}
    
};

class C
{
    C(int i = 0):NoDefault(i){}
    NoDefault nd;  
};
```

> 练习7.44：下面这条声明合法吗？如果不，为什么？
>
> ```C++
> vector<NoDefault> vec(10);
> ```

不合法，因为vec在初始化的时候需要执行NoDefault的默认构造函数，但是NoDefault类没有默认构造函数。

> 练习7.45：如果在上一个练习中定义的vector的元素类型是C，则声明合法吗？为什么？

合法，因为类C有默认构造函数。

> 练习7.46：下面哪些论断是不正确的？为什么？
>
> （a）一个类必须至少提供一个构造函数。
>
> （b）默认构造函数是参数列表为空的构造函数。
>
> （c）如果对于类来说不存在有意义的默认值，则类不应该提供默认构造函数。
>
> （d）如果类没有定义默认构造函数，则编译器将为其生成一个并把每个数据成员初始化成相应类型的默认值。

（a）不正确，因为如果不提供构造函数的情况下，编译器会自动生成默认的构造函数。

（b）不正确，默认含有默认参数的构造函数也被称为默认构造函数。默认构造函数是指当该构造函数被调用的时候，没有类内数据成员被初始化。

（c）不正确，如果类中定义了其他的构造函数，最好直接定义一个默认构造函数。

（d）不正确，只要类中明确定义了一个构造函数，编译器将不再自动生成默认构造函数。

> 练习7.47：说明一个接收string参数的Sales_data构造函数是否应该是explicit的，并解释这样做的优缺点。

是否将接收string参数的Sales_data的构造函数定义成explicit取决于我们的需要，如果想要使用string到Sales_data的隐式类型转换，就不要将构造函数定义成explicit的，否则将构造函数定义成explicit的。

优点：

- 能够防止隐式类型转换；
- 可以定义一个只有一个参数的构造函数，该函数只能用于直接初始化，而不能用于拷贝初始化。

缺点：

- explicit只能用于只有一个参数的构造函数。

> 练习7.48：假定Sales_data的构造函数不是explicit的，则下述定义将执行什么样的操作？
>
> ```C++
> string null_isbn("9-999-99999-9");
> Sales_data item1(null_isbn);
> Sales_data item2("9-999-99999-9");
> ```
>
> 如果Sales_data的构造函数是explicit的，又会发生什么呢？

当Sales_data的构造函数不是explicit时：

1. string的直接初始化
2. Sales_data的直接初始化
3. 字符串字面值常量隐式转换成string类型，之后进行Sales_data的直接初始化。

当Sales_data的构造函数不是explicit时，情况相同，因为上述三种定义都没有使用Sales_data的隐士类型转换。

> 练习7.49：对于combine函数的三种不同声明，当我们调用i.combine(s)时分别发生什么情况？其中i是一个Sales_data，而s是一个string对象。
>
> ```C++
> (a)Sales_data &combine(Sales_data);
> (b)Sales_data &combine(Sales_data &);
> (c)Sales_data &combine(const Sales_data) const;
> ```

（a）编译成功，调用Sales_data的隐式类型转换，将string转换成Sales_data类型。

（b）编译失败，因为调用Sales_data的隐式类型转换，将string转换成的Sales_data对象是个临时量，该临时量只能被绑定在常量const上，而combine的形参不是常量const。

（c）编译失败，不应该将combine函数定义成常量成员函数，因为combine函数会修改数据成员的值，但是const成员函数是不允许修改调用该函数的对象的成员变量的值。

> 练习7.50：确定在你的Person类中是否有一些构造函数应该是explicit的。

略。

>练习7.51：vector将其单参数的构造函数定义成explicit的，而string则不是，你觉得原因何在？
>

string接受的单参数是const char*类型，如果我们得到了一个常量指针，则把它看做string对象是自然而然的过程，编译器自动把参数类型转换成类类型也非常符合逻辑，因此我们无须指定为explicit。

与string相反，vector接受的单参数是int类型，这个参数的原意是指定vector的容量。如果我们在本来需要vector的地方提供一个int值并且希望这个int值自动转换成vector，则这个过程显得比较牵强，因此把vector的单参数构造函数定义成explicit的更加合理。

> 练习7.52：使用2.6.1节（第64页）的Sales_data类，解释下面的初始化过程。如果存在问题，尝试修改它。
>
> ```C++
> Sales_data item = {"978-0590353403",25,15.99};
> ```
>

能直接使用列表进行初始化的聚合类不能有类内初始值，因此修改如下：

```C++
struct Sales_data {
    std::string bookNo;
    unsigned units_sold;
    double revenue;
};
```

> 练习7.53：定义你自己的Debug。

略。

> 练习7.54：Debug中以set_开头的成员应该被声明为constexpr吗？如果不，为什么？

不应该，因为constexpr函数函数体只包含一条return语句。

> 练习7.55：7.5.5节（第266页）的Data类是字面值常量吗？请解释原因。

不是，因为std::string不是字面值类型。

> 练习7.56：什么是类的静态成员？它有何优缺点？静态成员与普通成员有何区别？

类的静态成员和类自身有关，而和类的对象无关。

优缺点：

静态成员不需要每个对象都维护，只会在类中维护一个静态成员。

区别：

- 静态成员是类共享的，与类的对象无关，而普通成员是从属于具体的对象的。

- 静态成员可以作为成员函数的默认实参，而普通成员则不行。

> 练习7.57：编写你自己的Account类。

```C++
class Account
{
 public:
    void calculate(){amount += amount * interestRate;}
    static double rate(){return interestRate;}
    static void rate(double);
 private:
    std::string owner;
    double amount;
    static double interestRate;
    static double initRate();
    
};
```

> 练习7.58：下面的静态成员的声明和定义有错误吗？请解释原因。
>
> ```C++
> // example.h
> class Example{
> public:
>     static double rate = 6.5;
>     static const int vecSize = 20;
>     static vector<double> vec(vecSize);
> };
> 
> //examlpe.c
> #include "example.h"
> double Example::rate;
> vector<double> Eaxmple::vec;
> ```
>

`rate`错误，在类内初始化的静态成员必须是const整数类型。

`vecSize`的声明正确，但是还需要在类外进行定义：

```C++
const int Example::vecSize;
```

`vec`错误，不应该在类内进行初始化，声明修改为：

```C++
static vector<double> vec;
```

定义修改为：

```C++
vector<double> Example::vec(Example::vecSize);
```

