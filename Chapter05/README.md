# 第五章 语句

> 练习5.1：什么是空语句？什么时候会用到空语句。

```C++
; // 空语句
```

如果在程序的某个地方，语法上需要一条语句但是逻辑上不需要，此时应该使用空语句。一种常见的情况是，当循环的全部工作在条件部分就可以完成时，通常会用到空语句，在使用空语句的时候应该加上注释，告诉读者是有意省略的，多余的空语句并非总是无害的。

> 练习5.2：什么是块？什么时候会用到块？

复合语句是指用花括号括起来的（可能为空的）语句和声明的序列，复合语句也被称作块，一个块就是一个作用域，在块中引入的名字只能在块的内部以及嵌套在块中的子块中访问。如果在程序的某个地方，语法上需要一条语句但是逻辑上需要多条语句，则应该使用复合语句，也就是块。

> 练习5.3：使用逗号运算符重写1.4.1节（第10页）的while循环。

略。

> 练习5.4：说明下列例子的含义，如果存在问题，试着修改它。
>
> ```C++
> (a)while(string::iterator iter != s.end()) {/*...*/}
> (b)while(bool status = find(word)){/*...*/}
> 	if(!status){/*...*/}
> ```

(a)变量`iter`没有赋初值

(b)变量`status`在while循环外不能被访问

修改如下：

```c++
(a)string::iterator iter == s.begin();
   while(iter != s.end()) {/*...*/}
(b)bool status;
   while(status = find(word)){/*...*/}
	  if(!status){/*...*/}
```

> 练习5.5：写一段自己的程序，使用if else语句实现把数字成绩转换成字母成绩的要求。

```C++
const vector<string> scores = {"F", "D", "C", "B", "A", "A++"};
string letterGrade;
if(grade < 60)
    letterGrade = scores[0];
else
    letterGrade = scores[(grade - 50) / 10];
if(grade % 10 > 7)
    letterGrade += '+';
else if(grade % 10 < 3)
    letterGrede += '-';
    
```

> 练习5.6：改写上一题程序，使用条件运算符（第134页）代替if else语句。

```C++
 vector<string> scores = {"F", "D", "C", "B", "A", "A++"};

    int grade{0};
    while (cin >> grade) {
        string lettergrade = grade < 60 ? scores[0] : scores[(grade - 50) / 10];
        lettergrade += (grade == 100 || grade < 60) ? "" : (grade % 10 > 7) ? "+" : (grade % 10 < 3) ? "-" : "";
```

> 练习5.7：改正下列代码中的错误。
>
> ```C++
> (a)if(ival1 != ival2)
>  ival1 = ival2
> else ival1 = ival2 = 0;
> (b)if(ival < minval)
>  minval = ival;
>  occurs = 1;
> (c)if(int ival = get_value())
>    cout << "ival = " << ival << endl;
> if(!ival)
>     cout << "ival = 0\n";
> (d)if(ival = 0)
>  ival = get_value();
>  
> ```

```C++
(a)if(ival1 != ival2)
    ival1 = ival2;
   else ival1 = ival2 = 0;
(b)if(ival < minval)
	{
    	minval = ival;
   		occurs = 1;    
	}
    
(c)
   int ival;
   if(ival == get_value())
      cout << "ival = " << ival << endl;
   if(!ival)
       cout << "ival = 0\n";
(d)if(ival == 0)
    ival = get_value();
```

> 练习5.8：什么是“悬垂else”？C++语言是如何处理else子句的？

当一个if语句嵌套在另一个if语句内部时，很可能if分支会多于else分支，这种问题被成为悬垂else。

C++规定else语句和离它最近的未匹配的if语句配对。

> 练习5.9：编写一段程序，使用一系列if语句统计从cin读入的文本中有多少元音字母。

```C++
#include <iostream>
int main()
{
    char c;
    int aCnt = 0;
    int eCnt = 0;
    int iCnt = 0;
    int oCnt = 0;
    int uCnt = 0;
    while (std::cin >> c)
    {
        if (c == 'a')
            ++aCnt;
        else if (c == 'e')
            ++eCnt;
        else if (c == 'i')
            ++iCnt;
        else if (c == 'o')
            ++oCnt;
        else if (c == 'u')
            ++uCnt;
    }
    std::cout << "aCnt : " << aCnt << " eCnt : " << eCnt << " iCnt : " << iCnt << " oCnt : " << oCnt << " uCnt : " << uCnt << std::endl;
}

```

> 练习5.10：我们之前实现的统计元音字母的程序存在一个问题：如果元音字母以大写形式出现，不会被统计在内。编写一段程序，既统计元音字母的小写形式，也统计大写形式，也就是说，新程序遇到`a`和`A`都应该递增aCnt的值，以此类推。

```C++
#include <iostream>
int main()
{
    char c;
    int aCnt = 0;
    int eCnt = 0;
    int iCnt = 0;
    int oCnt = 0;
    int uCnt = 0;

    while (std::cin >> c)
    {
        switch (c)
        {
        case 'a':
        case 'A':
            ++aCnt;
            break;
        case 'e':
        case 'E':
            ++eCnt;
            break;
        case 'i':
        case 'I':
            ++iCnt;
            break;
        case 'o':
        case 'O':
            ++oCnt;
            break;
        case 'u':
        case 'U':
            ++uCnt;
            break;
        }
    }
    std::cout << "aCnt : " << aCnt << " eCnt : " << eCnt << " iCnt : " << iCnt << " oCnt : " << oCnt << " uCnt : " << uCnt << std::endl;
}

```

> 练习5.11：修改统计元音字母的程序，使其也能统计空格、制表符和换行符的数量。

```C++
#include <iostream>
int main()
{
    char c;
    int aCnt = 0;
    int eCnt = 0;
    int iCnt = 0;
    int oCnt = 0;
    int uCnt = 0;
    int spaceCnt = 0;
    int tabCnt = 0;
    int lineCnt = 0;

    while (std::cin >> c)
    {
        switch (c)
        {
        case 'a':
        case 'A':
            ++aCnt;
            break;
        case 'e':
        case 'E':
            ++eCnt;
            break;
        case 'i':
        case 'I':
            ++iCnt;
            break;
        case 'o':
        case 'O':
            ++oCnt;
            break;
        case 'u':
        case 'U':
            ++uCnt;
            break;
        case ' ':
            ++spaceCnt;
            break;
        case '\t':
            ++tabCnt;
            break;
        case '\n':
            ++lineCnt;
            break;
        }
    }
    std::cout << "aCnt : " << aCnt << " eCnt : " << eCnt << " iCnt : " << iCnt
              << " oCnt : " << oCnt << " uCnt : " << uCnt << " spaceCnt : " << spaceCnt
              << " tabCnt : " << tabCnt << " lineCnt : " << lineCnt << std::endl;
}

```

> 练习5.12：修改统计元音字母的程序，使其能统计以下含有两个字符的字符序列的数量：ff、fl和fi。

```C++
#include <iostream>
int main()
{
    char c;
    char preChar = '\0';
    int aCnt = 0;
    int eCnt = 0;
    int iCnt = 0;
    int oCnt = 0;
    int uCnt = 0;
    int spaceCnt = 0;
    int tabCnt = 0;
    int lineCnt = 0;
    int ffCnt = 0;
    int fiCnt = 0;
    int flCnt = 0;

    while (std::cin >> c)
    {
        switch (c)
        {
        case 'a':
        case 'A':
            ++aCnt;
            break;
        case 'e':
        case 'E':
            ++eCnt;
            break;
        case 'i':
            if(preChar = 'f')
                ++fiCnt;
        case 'I':
            ++iCnt;
            break;
        case 'o':
        case 'O':
            ++oCnt;
            break;
        case 'u':
        case 'U':
            ++uCnt;
            break;
        case ' ':
            ++spaceCnt;
            break;
        case '\t':
            ++tabCnt;
            break;
        case '\n':
            ++lineCnt;
            break;
        case 'f':
            if(preChar = 'f')
                ++ffCnt;
            break;
        case 'l':
            if(preChar = 'f')
                if(preChar = 'f')
                    ++ffCnt;
                break;
        }
        preChar = c;
    }
    std::cout << "aCnt : " << aCnt << " eCnt : " << eCnt << " iCnt : " << iCnt
              << " oCnt : " << oCnt << " uCnt : " << uCnt << " spaceCnt : " <<spaceCnt
              << " fiCnt : " << fiCnt << " ffCnt : " << ffCnt << " flCnt : " << flCnt
              << " tabCnt : " << tabCnt << " lineCnt : " << lineCnt << std::endl;
}

```

> 练习5.13：下面显示的每个程序都含有一个常见的编程错误，指出错误在哪里，然后修改它们。
>
> ```C++
> (a)
> unsigned aCnt = 0, eCnt = 0, iouCnt = 0;
> char ch = bext_text();
> switch(ch){
>  case 'a':aCnt++;
>  case 'e':eCnt++;
>  default:iouCnt++;
> }
> (b)
> unsigned index = some_value();
> switch(index)
> {
>  case 1:
>      int ix = get_value();
>      ivec[ix] = index;
>      break;
>  default:
>      ix = ivec.size() - 1;
>      ivec[ix] = index;
> }
> (c)
> unsigned evenCnt = 0,oddCnt = 0;
> int digit = get_num() % 10;
> switch(digit)
> {
>  case 1, 3, 5, 7, 9:
>      oddCnt++;
>      break;
>  case 2, 4, 6, 8, 10:
>      evenCnt++;
>      break;
>      
> }
> (d)
> unsigned ival = 512; jval = 1024; kval = 4096;
> unsigned bufsize();
> unsigned swt = get_bufCnt();
> switch(swt)
> {
>  case ival:
>      bufsize = ival * sizeof(int);
>      break;
>  case jval:
>      bufsize = jval * sizeof(int);
>      break;
>  case kval:
>      bufsize = kval * sizeof(int);
>      break;
> }
> 
> ```

修改如下：

```C++
(a)
unsigned aCnt = 0, eCnt = 0, iouCnt = 0;
char ch = bext_text();
switch(ch){
 case 'a':aCnt++; break;
 case 'e':eCnt++; break;
 default:iouCnt++; break;
}
(b)
unsigned index = some_value();
int ix;
switch(index)
{
 case 1:
     ix = get_value();
     ivec[ix] = index;
     break;
 default:
     ix = static_cast<int>(ivec.size()) - 1;
     ivec[ix] = index;
}
(c)
unsigned evenCnt = 0,oddCnt = 0;
int digit = get_num() % 10;
switch(digit)
{
    case 1:
    case 3:
    case 5:
    case 7:
    case 9:
        oddCnt++;
        break;
    case 2:
    case 4:
    case 6:
    case 8:
    case 10:
        evenCnt++;
        break; 	     
}
(d)
unsigned ival = 512; jval = 1024; kval = 4096;
unsigned bufsize;
constexpr unsigned swt = get_bufCnt();
switch(swt)
{
 case ival:
     bufsize = ival * sizeof(int);
     break;
 case jval:
     bufsize = jval * sizeof(int);
     break;
 case kval:
     bufsize = kval * sizeof(int);
     break;
}

```

> 练习5.14：编写一段程序，从标准输入中读取若干string对象并查找连续重复出现的单词。所谓连续重复出现的意思是：一个单词后面紧跟着这个单词本身。要求记录连续重复出现的最大次数以及对应的单词。如果这样的单词存在，输出重复出现的最大次数；如果不存在，输出一条信息说明任何单词都没有连续出现过。例如，如果输入是：how now now brown cow cow ，那么输出应该表明单词now连续出现了3次。

```C++
#include <iostream>
#include <string>

int main()
{
    std::string str;
    std::string lastStr;
    std::string maxStr;
    int maxCnt = 0;
    if (std::cin >> str)
    {
        int cnt = 1;
        lastStr = str;
        while (std::cin >> str)
        {
            if (str == lastStr)
            {
                ++cnt;
            }
            else
            {
                lastStr = str;
                cnt = 1;
            }
            if (maxCnt < cnt)
            {
                maxCnt = cnt;
                maxStr = lastStr;
            }
        }
    }
    if (maxCnt > 1)
        std::cout << "maxCnt = " << maxCnt << " maxStr = " << maxStr << std::endl;
    else
        std::cout << "no word was repeated" << std::endl;

    return 0;
}
```

> 练习5.15：说明下列循环的含义并改正其中错误。
>
> ```C++
> (a)for(int ix = 0; ix != sz; ++ix) {/* ... */}
> if(ix != sz)
>     // ...
> (b)int ix;
> for(ix != sz; ++ix) {/* ... */}
> (c)for(int ix = 0; ix != sz; ++ix, ++sz){/* ... */}
> ```

修改如下：

```C++
(a)int ix = 0;
   for(; ix != sz; ++ix) {/* ... */}
   if(ix != sz)
       // ...
(b)int ix;
   for(;ix != sz; ++ix) {/* ... */}
(c)for(int ix = 0; ix != sz; ++ix){/* ... */}
```

> 练习5.16：while循环特别适用于那种条件保持不变、反复执行操作的情况，例如，当未达到文件末尾时不断读取下一个值。for循环则更像是在按步骤迭代，它的索引值在某个范围内一次变化。根据每种循环的习惯用法各自编写一段程序，然后分别用另一种循环改写，如果只能使用一种循环，你倾向于使用哪种呢？为什么？

略。

> 练习5.17：假设有两个包含整数的vector对象，编写一段程序，检验其中一个vector对象是否是另一个的前缀。为了实现这一目标，对于两个不等长的vector对象，只需挑出长度较短的那个，把它的所有元素和另一个vector对象比较即可。例如，如果两个vector对象的元素分别是0、1、1、2和0、1、1、2、3、5、8，则程序的返回结果应该为真。

```C++
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> intVec1 = {0, 1, 1, 2};
    std::vector<int> intVec2 = {0, 1, 1, 2, 3, 5, 8};
    std::vector<int> temp;

    if (intVec1.size() >= intVec2.size())
    {
        temp = intVec1;
        intVec1 = intVec2;
        intVec2 = temp;
    }
    auto i = 0;
    for (; i < intVec1.size(); ++i)
    {
        if (intVec1[i] != intVec2[i])
        {
            std::cout << "false" << std::endl;
            return false;
        }
    }
    if (i == intVec1.size())
    {
        std::cout << "true" << std::endl;
        return true;
    }
}
```

> 练习5.18：说明下列循环的含义并改正其中的错误。

> ```C++
> (a)
> do 
>  int v1,v2;
>  cout << "Please enter two numbers to sum : ";
>  if(cin >> v1 >> v2)
>      cout << "Sum is: " << v1 + v2 << endl;
> while(cin);
> (b)
> do{
>  // ...
> }while(int ival = get_response());
> (c)
> do{
>  int ival = get_response();
> }while(ival);
> ```

```C++
(a)
do 
{
    int v1,v2;
    cout << "Please enter two numbers to sum : ";
    if(cin >> v1 >> v2)
        cout << "Sum is: " << v1 + v2 << endl;
}
while(cin);
(b)
int ival;
do{
    // ...
}while(ival = get_response());
(c)
int ival;
do{
    ival = get_response();
}while(ival);
```

> 练习5.19：编写一段程序，使用do while循环重复执行下述任务：首先提示用户输入两个string对象，然后挑出较短的那个并输出它。

```C++
#include <iostream>
#include <string>

int main()
{
    std::string str1;
    std::string str2;
    std::string response;

    do
    {
        std::cout << "please enter two strings: ";
        std::string str1, str2;
        std::cin >> str1 >> str2;
        if (str1.size() > str2.size())
            std::cout << str2 << std::endl
                      << "More? Enter yes or no:" << std::endl;
        else
            std::cout << str1 << std::endl
                      << "More? Enter yes or no:" << std::endl;
        std::cin >> response;
    } while (!response.empty() && response[0] != 'n');
    return 0;
}
```

> 练习5.20：编写一段程序，从标准输入中读取string对象的序列直到连续出现两个相同的单词或者所有单词都读完为止。使用while循环一次读取一个单词，当一个单词连续出现两次时使用break语句终止循环。输出连续出现的单词，或者输出一个消息说明没有任何单词是连续重复出现的。

```C++
#include <iostream>
#include <string>

int main()
{
    std::string str;
    std::string lastStr;
    bool flag = false;
    if (std::cin >> str)
    {
        lastStr = str;
        while (std::cin >> str)
        {
            if (str == lastStr)
            {
                flag = true;
                std::cout << str << " occurs two times." << std::endl;
                break;
            }
            else
            {
                lastStr = str;
            }
        }
    }
    if (!flag)
        std::cout << "no same word" << std::endl;
    return 0;
}
```

> 练习5.21：修改5.5.1节（第171页）练习题的程序，使其找到的重复单词必须以大写字母开头。

```C++
#include <iostream>
#include <string>

int main()
{
    std::string str;
    std::string lastStr;
    bool flag = false;
    if (std::cin >> str)
    {
        while (std::cin >> str)
        {
            if (!isupper(str[0]))
            {
                lastStr = "";
                continue;
            }
            if (str == lastStr)
            {
                flag = true;
                std::cout << str << " occurs two times." << std::endl;
                break;
            }
            else
            {
                lastStr = str;
            }
        }
    }
    if (!flag)
        std::cout << "no same word" << std::endl;
    return 0;
}
```

> 练习5.22：本节的最后一个例子跳回到begin，其实使用循环能更好地完成该任务。重写这段代码注意不再使用goto语句。

```C++
for(int sz = get_size();sz <= 0; sz = get_size())
    ;
```

> 练习5.23：编写一段程序，从标准输入读取两个整数，输出第一个数除以第二个数的结果。

```C++
#include <iostream>

int main()
{
    int value1, value2;
    std::cout << "Please enter two numbers:" << std::endl;
    std::cin >> value1 >> value2;
    if (value2 != 0)
    {
        int result = value1 / value2;
        std::cout << "result = " << result << std::endl;
    }

    return 0;
}
```

> 练习5.24：修改你的程序，使得当第二个数是0时抛出异常。先不要设定catch子句，运行程序并真的为除数输入0，看看会发生什么？

```C++
#include <iostream>
#include <stdexcept>

int main()
{
    int value1, value2;
    std::cout << "Please enter two numbers:" << std::endl;
    std::cin >> value1 >> value2;
    if(value2 == 0)
    {
        throw std::runtime_error("divisor is equal to 0!");

    }
    else
    {
        int result = value1 / value2;
        std::cout << "result = " << result << std::endl;
    }

    return 0;
}
```

> 练习5.25：修改上一题的程序，使用try语句块去捕获异常，catch子句应该为用户输出一条提示消息，询问其是否输入新数并重新执行try语句块的内容。

```C++
#include <iostream>
#include <stdexcept>

int main()
{
    int value1, value2;
    std::cout << "Please enter two numbers:" << std::endl;
    while (std::cin >> value1 >> value2)
    {
        try
        {
            if (value2 == 0)
            {
                throw std::runtime_error("divisor is equal to 0!");
            }
            else
            {
                std::cout << "result = " << value1 / value2 << std::endl;
            }
        }
        catch (std::runtime_error error)
        {
            std::cout << error.what() << "\nTry Again? Enter y or n" << std::endl;
            char c;
            std::cin >> c;
            if (!std::cin || c == 'n')
                break;
        }
    }
    return 0;
}
```

