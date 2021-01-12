# 第四章 表达式

> 练习4.1：表达式5+10*20/2的求值结果是多少？

155。

> 练习4.2：根据4.12节中的表，在下述表达式的合理位置添加括号，使得添加括号后运算对象的组合顺序和添加括号前一致。
>
> ```C++
> (a)*vec.begin();
> (b)*vec.begin() + 1;
> ```

```C++
(a)(*(vec.begin()));
(b)((*(vec.begin())) + 1);
```

> 练习4.3：C++语言没有明确规定大多数二元运算符的求值顺序，给编译器优化留下了余地。这种策略实际是在代码生成效率和程序潜在缺陷之间进行平衡，你认为这可以接受吗，说出你的理由。

可以接受，因为C++最大的缺点是速度慢，编译器进行优化可以提高C++的效率。

> 练习4.4：在下面的表达式中添加括号，说明其求值的过程和最终结果，编写程序编译该（不加括号）表达式并输出其结果验证之前的推断。
>
> ```C++
> ((((12 / 3) * 4) + (5 * 15)) + ((24 % 4) / 2))
> ```
>
> 最终结果为：91

> 练习4.5：写出下列表达式的求值结果。
>
> ```C++
> (a)-30 * 3 + 21 / 5;
> (b)-30 + 3 * 21 / 5;
> (c)30 / 3 * 21 % 5;
> (d)-30 / 3 * 21 % 4;
> ```

(a)-86

(b)-18

(c)0

(d)-2

> 练习4.6：写出一条表达式用于确定一个整数是奇数还是偶数。

```C++
i % 2 == 0 ? "even" : "odd"
```

> 练习4.7：溢出是何含义？写出三条将导致溢出的表达式。

```C++
short svalue = 32767; ++svalue; // -32768
unsigned uivalue = 0; --uivalue;  // 4294967295
unsigned short usvalue = 65535; ++usvalue;  // 0
```

> 练习4.8：说明在逻辑与、逻辑或及相等性运算中运算对象求值的顺序。

- 对于逻辑与运算符来说，当且仅当左侧运算对象为真时才对右侧运算对象求值。
- 对于逻辑或运算符来说，当且仅当左侧运算对象为假时才对右侧运算对象求值。
- 当相等性运算符左侧运算对象和右侧运算对象相等时，返回`true`，否则返回`false`。

> 练习4.9：解释在下面的if语句中条件部分的判断过程。
>
> ```C++
> const char *cp = "Hello world";
> if(cp && *cp)
> ```

首先判断`cp`是否不为空指针，判断结果为`true`，结果判断指向常量的指针`cp`指向的变量是否为`true`，返回值为`true`，所以最终结果为`true`。

> 练习4.10：为while循环写一个条件，使其从标准输入中读取整数，遇到42时停止。

```C++
int n = 0;
while(std::cin >> n && n != 42)  // for循环体
```

> 练习4.11：书写一条表达式用于测试4个值a、b、c、d的关系，确保a大于b，b大于c，c大于d。

```C++
(a > b) && (b > c) && (c > d)
```

> 练习4.12：假设i、j和k是三个整数，说明表达式i !=j<k的含义。

首先判断`j < k`是否成立，如果成立，接着判断`i！=1`是否成立，如果不成立，判断`i!=0`是否成立。

> 练习4.13：在下述语句中，当赋值完成后i和d的值分别是多少？
>
> ```C++
> int i;
> double d;
> (a)d = i = 3.5;
> (b)i = d = 3.5;
> ```

(a)i：3  d：3.0

(b)i：3 d：3.5

> 练习4.14：执行下述if语句后将发生什么情况？
>
> ```C++
> (a)if(42 = i) // ...
> (b)if(i = 42) // ...
> ```

(a)出现编译错误，因为赋值运算符左侧只能是左值，不能是字面值常量。

(b)判断条件一直为`true`。

> 练习4.15：下面的赋值是非法的，为什么，应该如何修改？
>
> ```C++
> double dval;
> int ival;
> int *pi;
> dval = ival = pi = 0;
> ```

不合法，不能将指针赋值给变量，修改如下：

```C++
double dval;
int ival;
int *pi;
dval = ival = 0;
pi = 0;
```

> 练习4.16：尽管下面的语句合法，但它们实际执行的行为可能和预期并不一样，为什么，应该如何修改？
>
> ```C++
> (a)if(p = getPtr() != 0)
> (b)if(i = 1024)    
> ```

修改如下：

```C++
(a)if((p = getPtr()) != 0)
(b)if(i == 1024) 
```

> 练习4.17：说明前置递增运算符和后置递增运算符的区别。

- 前置递增运算符首先将运算对象加1，然后将改变后的对象作为求值结果，后置递增运算符也是将运算对象加1，但是求值结果是运算对象改变之前的那个值的副本。
- 前置版本将对象本身作为左值返回，后置版本将对象初始值的副本作为右值返回。

> 练习4.18：如果第132页那个输出vector对象元素的while循环使用前置递增运算符，将得到什么结果？

将会错过第一个元素。

> 练习4.19：假设ptr的类型是指向int的指针，vec的类型是vector < int >、ival的类型是int，说明下面的表达式是何含义？如果有表达式不正确，为什么，应该如何修改？
>
> ```C++
> (a)ptr != 0 && *ptr++
> (b)ival++ && ival
> (c)vec[ival++] <= vec[ival]
> ```

(a)判断`ptr`是否为空指针，如果不是空指针，接着判断ptr指向的对象是否为0，同时将ptr向后移动。

(b)判断`ival`是否为0，如果不为0，将ival加1，接着判断ival是否为0

(c)不合法，修改为：`vec[ival] <= vec[ival + 1]`

> 练习4.20：假设iter的类型是vector<string>::iterator，说明下面的表达式是否合法，如果合法，表达式的含义是什么，如果不合法，错在何处？
>
> ```C++
> (a)*iter++;
> (b)(*iter)++;
> (c)*iter.empty();
> (d)iter->empty();
> (e)++*iter;
> (f)iter++->empty();
> ```

(a)合法，首先返回迭代器`iter`指向的对象，接着将`iter`加1。

(b)不合法，`*iter`的返回值为`string`对象，不能执行加1操作。

(c)不合法，`.`的优先级高于`*`，迭代器没有成员函数`empty`。

(d)合法，判断迭代器`iter`指向的对象是否为空。

(e)不合法，先执行`*iter`得到迭代器`iter`指向的对象，为string类型对象，不能执行前置递增运算。

(f)合法，返回`iter->empty()`，接着执行`++iter`。

> 练习4.21：编写一段程序，使用条件运算符从vector<int>中找到哪些元素的值是奇数，然后将这些奇数值翻倍。

```C++
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> intVector = {1, 2, 3, 4, 5};
    for (auto &n : intVector)
    {
        n = ((n % 2 != 0) ? 2 * n: n);
        std::cout << n << std::endl;
    }
}
```

> 练习4.22：本节的示例程序将成绩划分为high pass、pass和fail三种，扩展该程序使其进一步将60分到75分之间的成绩设定为low pass。要求程序包含两个版本：一个版本只使用条件运算符；另外一个版本使用1个或者多个if语句。哪个版本的程序更容易理解呢，为什么？

```C++
int grade = 0;
std::cin >> grade;
std::string finalGrade = (grade > 90) ? "high pass" : (grade > 75)? "low pass" : (grade < 60) ? "fail" : "pass";
```

```C++
int grade = 0;
std::cin >> grade;
std::string finalGrade;
if(grade > 90)
    finalGrade = "high pass";
else if(grade > 75)
    finalGrade = "low pass";
else if(grade > 60)
    finalGrade = "pass";
else
    finalGrade = "fail";
```

`if`语句版本更容易理解。

> 练习4.23：因为运算符的优先级问题，下面这条表达式无法通过编译。根据4.12节中的表（第147页）指出它的问题在哪里？应该如何修改？
>
> ```C++
> string s = "world";
> string p1 = s + s[s.size() - 1] == 's' ? "" : "s";
> ```

先执行加法，再执行赋值运算，会导致编译出错，修改如下：

```C++
string s = "world";
string p1 = s + (s[s.size() - 1] == 's' ? "" : "s");
```

> 练习4.24：本节的示例程序将成绩划分为high pass、pass和fail三种，它的依据是条件运算符满足右结合律。假设条件运算符满足的是左结合律，求值过程将是怎样的？

当分数大于90时，结果永远是`fail`，不满足要求。

> 练习4.25：如果一台机器上int占32位，char占8位，用的是Latin-1字符集，其中字符`q`的二进制形式是01110001，那么表达式~'q'<<6的值是什么？

会产生一个未定义的行为。https://stackoverflow.com/questions/19585507/c-not-bitwise-operator-binary-char-conversion

> 练习4.26：在本节关于测试成绩的例子中，如果使用unsigned int作为quiz1的类型会发生什么情况？

会产生一个未定义的行为。

> 练习4.27：下列表达式的结果是什么？
>
> ```C++
> unsigned long ul1 = 3, ul2 = 7;
> (a)ul1 & ul2
> (b)ul1 | ul2
> (c)ul1 && ul2
> (d)ul1 || ul2
> ```

(a)3

(b)7

(c)true

(d)true

> 练习4.28：编写一段程序，输出每一种内置类型所占空间的大小。

略。

> 练习4.29：推断下面代码的输出结果并说明理由，并实际运行这段代码。
>
> ```C++
> int x[10];
> int *p = x;
> cout << sizeof(x)/sizeof(*x) << endl;
> cout << sizeof(p)/sizeof(*p) << endl;
> ```

10

结果依赖于机器，如果是64位机器，结果为2，如果是32位机器，结果为1。

> 练习4.30：在下述表达式的适当位置加上括号，使得加上括号之后表达式的含义和原来的含义相同。
>
> ```C++
> (a)sizeof x + y
> (b)sizeof p->mem[i]
> (c)sizeof a < b
> (d)sizeof f()
> ```

```C++
(a)sizeof (x + y)
(b)sizeof (p->mem[i])
(c)(sizeof a) < b
(d)(sizeof (f()))  // 但是如果函数f返回void类型，则产生无定义的行为
```

> 练习4.31：本节的程序使用了前置版本的递增运算符和递减运算符，解释为什么要用前置版本而不是后置版本。要想使用后置版本的递增递减运算符需要做哪些改动？使用后置版本重写本节的程序。

```C++
vector<int>::size_type cnt = ivec.size();
for(vector<int>::size_type ix = 0; ix != ivec.size(); ix++, cnt--)
    ivec[ix] = cnt;
```

> 练习4.32：解释下面这个循环的含义。
>
> ```C++
> constexpr int size = 5;
> int ia[size] = {1,2,3,4,5};
> for(int *ptr = ia,ix = 0;ix != size && ptr != ia + size;++ix,++ptr)
> {/*...*/}
> ```

遍历整个数组。

> 练习4.33：根据4.12节中表（第147页）说明下面这条表达式的含义。
>
> ```C++
> someValue ? ++x,++y:--x,--y
> ```

```C++
(someValue ? ++x,++y:--x),--y
```

如果some为真，结果为y加1，同时将x加1，否则的话，表达式的结果为y减1，同时将x减1，等价于：

```C++
someValue ? (++x,++y):(--x,--y)
```

> 练习4.34：根据本节给出的变量定义，说明在下面的表达式中将发生什么样的类型转换：
>
> ```C++
> (a)if(fval)
> (b)dval = fval + ival;
> (c)dval + ival * cval;
> ```
>
> 需要注意每组运算符遵循的是左结合律还是右结合律。

(a)float->bool

(b)float->int->double

(c)char->int->double

> 练习4.35：假设有如下的定义，
>
> ```C++
> char cval;
> int ival;
> unsugned int ui;
> float fval;
> double dval;
> ```
>
> 请回答在下面的表达式中发生了隐式转换吗？如果有，请指出来。
>
> ```C++
> (a)cval = 'a' + 3;
> (b)fval = ui - ival * 1.0;
> (c)dval = ui + fval;
> (d)cval = ival +fval +dval;
> ```

(a)发生：char->int

(b)发生：int->double unsigned int->double double->float

(c)发生：unsigned int->float->double

(d)发生：int->float->double->char

> 练习4.36：假设i是int类型，d是double类型，书写表达式i*=d使其执行整数类型的乘法而非浮点类型的乘法。

```C++
i *= sattic_cast<int> d
```

> 练习4.37：用命名的强制类型转换改写下列旧式的转换语句。
>
> ```C++
> int i;
> double d;
> const string *ps;
> char *pc;
> void *pv;
> (a)pv = (void *)ps;
> (b)i = int (*)pc;
> (c)pv = &d;
> (d)pc = (chat*)pv;
> ```

(a)pv = const_cast<string*>(ps);

(b)i = static_cast<int>(*pc);

(c)pv = static_cast<void*>(&d);

(d)pc = reinterpret_cast<char*>(pv);

> 练习4.38：说明下面这条表达式的含义。
>
> ```C++
> double slope = static_cast<double>(j/i);
> ```

j/i的结果为int，将int显式转换成double赋值给slope。













