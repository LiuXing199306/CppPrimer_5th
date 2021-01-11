# 第三章 字符串、向量和数组

> 练习3.1：使用恰当的using声明重做1.4.1节（第11页）和2.6.2节（第67页）的练习。

略。

> 练习3.2：编写一段程序从标准输入中一次读入一整行，然后修改程序使其一次读入一个词。

```C++
#include <iostream>
#include <string>

int main()
{
    std::string s;
    while (getline(std::cin, s))
    {
        std::cout << s << std::endl;
    }
    return 0;
}
```

```C++
#include <iostream>
#include <string>

int main()
{
    std::string s;
    while (std::cin >> s)
    {
        std::cout << s << std::endl;
    }
    return 0;

```

> 练习3.3：请说明string类的输入运算符和getline函数分别是如何处理空白字符的。

输入运算符遇见空白字符就停止，getline函数遇见空白字符会将其当做字符串的一部分，直到遇见换行符。

> 练习3.4：编写一段程序读入两个字符串，比较其是否相等并输出结果。如果不相等，输出较大的那个字符串。改写上述程序，比较其是否等长，如果不等长，输出长度较大的那个字符串。

```C++
#include <iostream>
#include <string>

int main()
{
    std::string s1, s2;
    std::cout << "Please enter two string: " << std::endl;
    std::cin >> s1 >> s2;
    if (s1 != s2)
    {
        if (s1 > s2)
        {
            std::cout << s1 << std::endl;
        }
        else
        {
            std::cout << s2 << std::endl;
        }
    }
    else
    {
        std::cout << "s1 equals to s2." << std::endl;
    }

    return 0;
}
```

```C++
#include <iostream>
#include <string>

int main()
{
    std::string s1, s2;
    std::cout << "Please enter two string: " << std::endl;
    std::cin >> s1 >> s2;
    if (s1.size() != s2.size())
    {
        if (s1.size() > s2.size())
        {
            std::cout << s1 << std::endl;
        }
        else
        {
            std::cout << s2 << std::endl;
        }
    }
    else
    {
        std::cout << "s1 size equals to s2 size." << std::endl;
    }

    return 0;
}
```

> 练习3.5：编写一段程序从标准输入中读入多个字符串并将它们连接在一起，输出连接成的大字符串。然后修改上述程序，用空格把输入的多个字符串分隔开来。

```C++
#include <iostream>
#include <string>

int main()
{
    std::string currentStr, str;
    while(std::cin >> currentStr)
    {
        str += currentStr;
    }
    std::cout << str<< std::endl;
    return 0;
}
```

```C++
#include <iostream>
#include <string>

int main()
{
    std::string currentStr, str;
    bool first_flag = true;
    while (std::cin >> currentStr)
    {
        if (first_flag)
        {
            str += currentStr;
            first_flag = false;
        }
        else
        {
            str += ' ';
            str += currentStr;
        }
        
    }
    std::cout << str << std::endl;
    return 0;
}
```

> 练习3.6：编写一段程序，使用范围for语句将字符串内所有字符用X代替。

> 

```C++
#include <iostream>
#include <string>

int main()
{
    std::string str = "aj;lajfdl";
    for (auto &c : str)
        c = 'X';

    std::cout << str << std::endl;
    return 0;
}
```

> 练习3.7：就上一题完成的程序而言，如果将循环控制变量的类型设为char将发生什么？先估计一下结果，然后实际编程验证一下。

相同，因为`auto`推断出来的`c`的类型为`char`。

> 练习3.8：分别用while循环和传统的for循环重写第一题的程序，你觉得哪种形式更好呢，为什么？

```C++
// while循环
#include <iostream>
#include <string>

int main()
{
    std::string str = "aj;lajfdl";
    auto size = str.size();
    int i = 0;
    while(i < size)
    {
        str[i] = 'X';
        ++i;
    }
    std::cout << str << std::endl;
    return 0;
}
```

```C++
#include <iostream>
#include <string>

int main()
{
    std::string str = "aj;lajfdl";
    auto size = str.size();
    for (int i = 0; i < size; ++i)
    {
        str[i] = 'X';
    }
    std::cout << str << std::endl;
    return 0;
}
```

范围`for`语句更方便，可读性更高。

> 练习3.9：下面的程序有何作用？它合法吗？如果不合法，为什么？
>
> ```C++
> string s;
> cout << s[0] << endl;
> ```

不合法，需要先判断字符串`s`是否为空，如果为空，`s[0]`将产生未知的行为。修改如下：

```C++
string s;
if(!s.empty())
    cout << s[0] << endl;
```

> 练习3.10：编写一段程序，读入一个包含标点符号的字符串，将标点符号去除后输出字符串剩余的部分。

```C++
#include <iostream>
#include <string>
#include <cctype>

int main()
{
    std::string str = "aj;lajfdl";
    std::string str1;
    for (auto &c : str)
    {
        if (!ispunct(c))
        {
            str1 += c;
        }
    }
    std::cout << str1 << std::endl;
    return 0;
}
```

> 练习3.11：下面的范围for语句合法吗，如果合法，c的类型是什么？
>
> ```C++
> const string s = "Keep out!";
> for(auto &c : s) {/* */}
> ```

函数体中，如果修改`c`的话，不合法，因为`s`是常量字符串，不能被修改。`c`的类型是`const char &`。

> 练习3.12：下列vector的定义有不正确的吗，如果有，请指出来，对于正确的，描述其执行结果，对于不正确的，说明其错误的原因。
>
> ```C++
> (a)vector<vector<int>> ivec;
> (b)vector<string> svec = ivec;
> (c)vector<string> svec(10, "null");
> ```

(a)正确，创建一个空的vector。

(b)不正确，不能用一种类型的vector去初始化另外一种类型的vector。

(c)正确，创建一个含有10个"null"的字符串。

> 练习3.14：编写一段程序，用cin读入一组整数并把它们存入一个vecotr对象。

```C++
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> intVector;
    int n;
    while(std::cin >> n)
    {
        intVector.push_back(n);
    }
    
    return 0;
}
```

> 练习3.15：改写上题的程序，不过这次读入的是字符串。

```C++
#include <iostream>
#include <vector>
#include <string>

int main()
{
    std::vector<std::string> stringVector;
    std::string str;
    while(std::cin >> str)
    {
        stringVector.push_back(str);
    }
    
    return 0;
}
```

> 练习3.16：编写一段程序，把练习3.13中vector对象的容量和具体内容输出出来。检验你之前的回答是否正确。

略。

> 练习3.17：从cin读入一组词，并把它们存入一个vector对象中，然后设法把所有词都改写成大写形式，输出改变后的结果，每个词占一行。

```C++
#include <iostream>
#include <vector>
#include <string>
#include <cctype>

int main()
{
    std::vector<std::string> stringVector;
    std::string str;
    while (std::cin >> str)
    {
        stringVector.push_back(str);
    }
    for (auto &s : stringVector)
    {
        for (auto &c : s)
        {
            c = toupper(c);
        }
        std::cout << s << std::endl;
    }

    return 0;
}
```

> 练习3.18：下面的程序合法吗？如果不合法，你准备怎么修改？
>
> ```C++
> vector<int> ivec;
> ivec[0] = 42;
> ```

不合法，在使用下表运算符的时候，需要确保vector非空，修改如下：

```C++
vector<int> ivec;
if(!ivec.empty())
    ivec[0] = 42;
```

> 练习3.19：如果想定义一个含有10个元素的vector对象，所有的值都是42，请列举出三种不同的实现方法。哪种方法更好呢，为什么？

```C++
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> intVector1(10, 42);
    std::vector<int> intVector2 = {42, 42, 42, 42, 42, 42, 42, 42, 42, 42};
    std::vector<int> intVector3;
    for (int i = 0; i <= 10; ++i)
    {
        intVector3.push_back(42);
    }
    for (auto c : intVector1)
    {
        std::cout << c << std::endl;
    }
    std::cout << std::endl; 

    for (auto c : intVector2)
    {
        std::cout << c << std::endl;
    }
    std::cout << std::endl;

    for (auto c : intVector3)
    {
        std::cout << c << std::endl;
    }

    return 0;
}
```

第一种方法最好。

> 练习3.20：读入一组整数并把它们存入一个vector对象，将每队相邻整数的和输出出来。改写你的程序，这次要求先输出第1个和最后1个元素的和，接着输出第2个和倒数第2个元素的和，以此类推。

```C++
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> intVector;
    int n;
    while(std::cin >> n)
    {
        intVector.push_back(n);
    }
    auto size = intVector.size();
    for(int i = 0;i < size-1;++i)
    {
        std::cout << intVector[i] + intVector[i+1] << std::endl;
    }
    std::cout << std::endl;
    
    for(int i = 0;i<size/2;++i)
    {
        std::cout << intVector[i] + intVector[size-1-i] << std::endl;
    }
   

    return 0;
}
```

> 练习3.21：请使用迭代器重做3.3.3节（第94页）的第一个练习。

略。

> 练习3.22：修改之前那个输出text第一段的程序，首先把text第一段全都改写成大写形式，然后再输出它。

略。

> 练习3.23：编写一段程序，创建一个含义10个整数的vector对象，然后使用迭代器将所有元素的值都变成原来的两倍。输出vector对象的内容，检验程序是否正确。

```C++
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> intVector;
    int n;
    while (std::cin >> n)
    {
        intVector.push_back(n);
    }
    for (auto iter = intVector.begin(); iter != intVector.end(); ++iter)
    {
        *iter *= 2;
    }

    for(auto c:intVector)
    {
        std::cout << c << std::endl;
    }

    return 0;
}
```

> 练习3.24：请使用迭代器重做3.3.3节（第94页）的第一个练习。

略。

>练习3.25：3.3.3节（第93页）划分分数段的程序是使用下标运算符实现的，请使用迭代器改写程序并实现完全相同的功能。

略。

>练习3.26：在100页的二分搜索程序中，为什么用的是mid = beg + (end - beg)/2，而非mid = (beg + end)/2？

迭代器没有定义加法。

>练习3.27：假设txt_size是一个无参数的函数，它的返回值是int，请回答下列哪个定义是非法的？为什么？
>
>```C++
>unsigned buf_size = 1024;
>(a)int ia[buf_size];
>(b)int ia[4 * 7 -14];
>(c)int ia[txt_size()];
>(d)char st[11] = "fundamental";
>```

(a)不合法，数组维数必须是整型常量。

(b)合法。

(c)如果txt_size是常量函数，则合法，否则不合法。

(d)不合法，没有空间存放结尾的`\0`。

> 练习3.28：下列数组中元素的值是什么？
>
> ```C++
> string sa[10];
> int ia[10];
> int main()
> {
>  string sa2[10];
>  int ia2[10];
> }
> ```

`sa`中是10个空字符串。

`ia`中是10个0。

`sa2`中是10个空字符串。

`ia2`中是10个未定义的初值。

> 练习3.29：相比于vector来说，数组有哪些缺点，请列举一些。

不能灵活更改数组大小。

> 练习3.30：指出下面代码中的索引错误。
>
> ```C++
> constexpr size_t array_size = 10;
> int ia[array_size];
> for(size_t ix = 1; ix <= array_size; ++ix)
> {
>  ia[ix] = ix;
> }
> ```

数组`ia`的下标是从0开始，到array_size-1结束，会产生缓冲区溢出错误。修改如下：

```C++
constexpr size_t array_size = 10;
int ia[array_size];
for(size_t ix = 0; ix < array_size; ++ix)
{
 ia[ix] = ix;
}
```

> 练习3.31：编写一段程序，定义一个含有10个int的数组，令每个元素的值就是其下标值。

```C++
#include <iostream>
int main()
{
    int intArray[10] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    for(auto n : intArray)
    {
        std::cout << n << std::endl;
    }
    return 0;
}
```

> 练习3.32：将上一题刚刚创建的数组拷贝给另外一个数组。利用vector重写程序，实现类似的功能。

```C++
#include <iostream>
#include <vector>
int main()
{
    std::vector<int> intVector1 = {0, 1, 2, 3, 4, 5, 6, 7, 9};
    std::vector<int> intVector2 = intVector1;
    for(auto c:intVector2)
    {
        std::cout << c << std::endl;
    }

    return 0;
}
```

> 练习3.33：对于104页的程序来说，如果不初始化scores将发生什么？

初始值将是未定义的。

> 练习3.34：假定p1和p2指向同一个数组中的元素，则下面程序的功能是什么，什么情况下该程序是非法的？
>
> ```C++
> p1 += p2 - p2;
> ```

将p2赋值给p1，只要`p1`和`p2`是合法的，结果就是合法的。

> 练习3.35：编写一段程序，利用指针将数组中的元素置为0。

```C++
#include <iostream>
#include <iterator>

int main()
{
    int intArray[10] = {0, 1, 2, 3, 4, 5, 6, 7, 9};
    auto pBegin = std::begin(intArray);
    auto pEnd = std::end(intArray);

    while(pBegin != pEnd)
    {
        *pBegin = 0;
        ++pBegin;
    }

    for(auto n : intArray)
    {
        std::cout << n << std::endl;
    }

    return 0;
}
```

> 练习3.36：编写一段程序，比较两个数组是否相等。再写一段程序，比较两个vector对象是否相等。

```C++
#include <iostream>
#include <iterator>

int main()
{
    int intArray1[10] = {0, 1, 2, 3, 4, 5, 6, 7, 9};
    int intArray2[11] = {0, 1, 2, 3, 4, 5, 6, 7, 9, 10};
    auto pBegin1 = std::begin(intArray1);
    auto pEnd1 = std::end(intArray1);
    auto size1 = pEnd1 - pBegin1;
    std::cout << size1 << std::endl;

    auto pBegin2 = std::begin(intArray2);
    auto pEnd2 = std::end(intArray2);
    auto size2 = pEnd2 - pBegin2;
    std::cout << size2 << std::endl;

    if (size1 != size2)
    {
        std::cout << "intArray1 is not equal to intArray2" << std::endl;
    }
    else
    {
        while (*pBegin1 == *pBegin2)
        {
            ++pBegin1;
            ++pBegin2;
        }

        if (pBegin1 != pEnd1)
        {
            std::cout << "intArray1 is not equal to intArray2" << std::endl;
        }
        else
        {
            std::cout << "intArray1 equals to intArray2" << std::endl;
        }
    }

    return 0;
}
```

> 练习3.37：下面程序是何含义，程序的输出结果是什么？
>
> ```C++
> const char ca[] = {'h','e','l','l','o'};
> const char *cp = ca;
> while(*cp)
> {
>  cout << *cp << endl;
>  ++cp;
> }
> ```

输出字符串中所有的字符，但是直到遇到空字符`\0`才会停止。

> 练习3.38：在本节中我们提到，将两个指针相加不但是非法的，而且也没什么意义。请问为什么两个指针相加没有什么意义？

两个指针相加是两个指针中保存的地址相加，得到的结果应该是一个地址，但是可能得到一个无法解释或者无意义的地址。

> 练习3.39：编写一段程序，比较两个string对象，再编写一段程序，比较两个C风格字符串的内容。

```C++
#include <iostream>
#include <string>
#include <cstring>
int main()
{
    std::string s1 = "jad;hsfkj";
    std::string s2 = "sdljafiojreo";
    if (s1 == s2)
    {
        std::cout << "s1 is equal to s2." << std::endl;
    }
    else if (s1 > s2)
    {
        std::cout << "s1 is larger than s2." << std::endl;
    }
    else
    {
        std::cout << "s1 is smaller than s2." << std::endl;
    }

    const char a1[] = "hello";
    const char a2[] = "world";

    if (strcmp(a1,a2) == 0)
    {
        std::cout << "a1 is equal to a2." << std::endl;
    }
    else if (strcmp(a1,a2) > 0)
    {
        std::cout << "a1 is larger than a2." << std::endl;
    }
    else
    {
        std::cout << "a1 is smaller than a2." << std::endl;
    }

    return 0;
}
```

> 练习3.40：编写一段程序，定义两个字符数组并用字符串字面值初始化它们，接着再定义一个字符数组存放前两个数组连接后的结果。使用strcpy和strcat把前两个数组的内容拷贝到第三个数组中。

```C++
#include <iostream>
#include <cstring>
int main()
{
    const char a1[] = "hello";
    const char a2[] = "world";
    auto size = strlen(a1) + strlen(" ") + strlen(a2) + 1;
    char *a3 = new char[size];
    strcpy(a3,a1);
    strcat(a3," ");
    strcat(a3,a2);
    std::cout << a3 << std::endl;
    delete[] a3;

    return 0;
}
```

> 练习3.41：编写一段程序，用整型数组初始化一个vector对象。

```C++
#include <iostream>
#include <vector>
int main()
{
    int a[5] = {0, 1, 2, 3, 4};
    std::vector<int> intVector(std::begin(a),std::end(a));
    for(auto n:intVector)
    {
        std::cout << n << std::endl;
    }
    return 0;
}
```

> 练习3.42：编写一段程序，将含有整数元素的vector对象拷贝给一个整型数组。

```C++
#include <iostream>
#include <vector>
int main()
{
    std::vector<int> intVector = {0, 1, 2, 3, 4};
    int a[5];
    for (int i = 0; i < 5; ++i)
    {
        a[i] = intVector[i];
    }
    for (auto c : a)
    {
        std::cout << c << std::endl;
    }
    return 0;
}
```

> 练习3.43：编写三个不同版本的程序，令其均能输出ia的元素。版本1使用范围for语句管理迭代过程；版本2和版本3都使用普通的for语句 其中版本2要求使用下标运算符，	版本3要求使用指针，此外，在所有3个版本的程序中都要直接写出数据类型，而不能使用类型别名、auto关键字或者decltype关键字。

```C++
#include <iostream>

using std::cout;
using std::endl;

int main()
{
    int ia[3][4] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11};

    // a range for to manage the iteration
    for (const int(&p)[4] : ia)
        for (int q : p) cout << q << " ";
    cout << endl;

    // ordinary for loop using subscripts
    for (size_t i = 0; i != 3; ++i)
        for (size_t j = 0; j != 4; ++j) cout << ia[i][j] << " ";
    cout << endl;

    // using pointers.
    for (int(*p)[4] = ia; p != ia + 3; ++p)
        for (int* q = *p; q != *p + 4; ++q) cout << *q << " ";
    cout << endl;

    return 0;
}
```

> 练习3.44：改写上一个练习中的程序，使用类型别名来代替循环控制变量的类型。

```C++
#include <iostream>

using std::cout;
using std::endl;

int main()
{
    int ia[3][4] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11};

    // a range for to manage the iteration
    // use type alias
    using int_array = int[4];
    for (int_array& p : ia)
        for (int q : p) cout << q << " ";
    cout << endl;

    // ordinary for loop using subscripts
    for (size_t i = 0; i != 3; ++i)
        for (size_t j = 0; j != 4; ++j) cout << ia[i][j] << " ";
    cout << endl;

    // using pointers.
    // use type alias
    for (int_array* p = ia; p != ia + 3; ++p)
        for (int* q = *p; q != *p + 4; ++q) cout << *q << " ";
    cout << endl;

    return 0;
}
```

> 练习3.45：再一次改写程序，这次使用auto关键字。

```C++
#include <iostream>

using std::cout;
using std::endl;

int main()
{
    int ia[3][4] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11};

    // a range for to manage the iteration
    for (auto& p : ia)
        for (int q : p) cout << q << " ";
    cout << endl;

    // ordinary for loop using subscripts
    for (size_t i = 0; i != 3; ++i)
        for (size_t j = 0; j != 4; ++j) cout << ia[i][j] << " ";
    cout << endl;

    // using pointers.
    for (auto p = ia; p != ia + 3; ++p)
        for (int* q = *p; q != *p + 4; ++q) cout << *q << " ";
    cout << endl;

    return 0;
}
```
