>练习9.1：对于下面的程序任务，vector、deque和list哪种容器最为适合？解释你选择的理由。如果没有哪一种容器优于其他容器，也请解释理由。
>
>（a）读取固定数量的单词，将它们按照**字典序**插入容器中。我们将在下一章中看到，关联容器更适合这个问题。
>
>（b）读取未知数量的单词，总是将新单词插入到末尾。删除操作在头部进行。
>
>（c）从一个文件中读取未知数量的整数。将这些数排序，然后将他们打印标准输出。

（a）list，因为需要从中间插入元素。

（b）deque，因为需要从末尾进行插入操作，从头部进行删除操作。

（c）vector，因为元素总数量未知，可以先在尾部进行插入，之后进行排序。

>练习9.2：定义一个list对象，其元素类型是int的deque。

```C++
list<deque<int>> lst;
```

> 练习9.3：构成迭代器范围的迭代器有何限制？

两个迭代器`begin`和`end`需满足如下的条件，才能构成范围的迭代器：

- 它们指向同一个容器中的元素，或者是容器中最后一个元素之后的位置。
- 可以通过反复递增`begin`来达到`end`，即`end`不能在`begin`之前。

>练习9.4：编写函数，接受一对vector<int>的迭代器和一个int值。在两个迭代器制定的范围内查找给定值，返回一个布尔值来指出是否找到。

```C++
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

bool find_value(vector<int>::iterator begin, vector<int>::iterator end, int value);

int main()
{
    vector<int> vec = {0, 1, 2, 3, 4};
    int value = 3;
    cout << find_value(vec.begin(), vec.end(), value) << endl;
    return 0;
}

bool find_value(vector<int>::iterator begin, vector<int>::iterator end, int value)
{
    bool flag = false;
    while (begin != end)
    {
        if (*begin == value)
            flag = true;
        ++begin;
    }
    return flag;
}

```

>练习9.5：重写上一题的函数，返回一个迭代器指向找到的元素。注意，程序必须处理未找到给定值的情况。

```C++
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

vector<int>::iterator find_value(vector<int>::iterator begin, vector<int>::iterator end, int value);

int main()
{
    vector<int> vec = {0, 1, 2, 3, 4};
    int value = 3;
    find_value(vec.begin(), vec.end(), value);
    return 0;
}

vector<int>::iterator find_value(vector<int>::iterator begin, vector<int>::iterator end, int value)
{
    vector<int>::iterator location = begin;
    while (begin != end)
    {
        if (*begin == value)
            location = begin;
        ++begin;
    }
    if (begin == end)
        location = end;
    return location;
}

```

>练习9.6：下面程序有何错误？你应该如何修改它？
>
>```C++
>list<int> lst1;
>list<int>::iterator iter1 = lst1.begin(),
>					iter2 = lst1.end();
>while(iter1 < iter2) /* ... */
>
>```

修改如下：

```C++
list<int> lst1;
list<int>::iterator iter1 = lst1.begin(),
					iter2 = lst1.end();
while(iter1 != iter2) /* ... */

```

>练习9.7：为了索引int的vector中的元素，应该使用什么类型？

```C++
vector<int>::size_type
```

>练习9.8：为了读取string的list中的元素，应该使用什么类型？如果写入list，又应该使用什么类型？

```C++
list<string>::iterator 或者 list<string>::const_iterator
list<string>::iterator
```

>练习9.9：begin和cbegin两个函数有什么不同？

begin函数返回非常量类型的迭代器iterator。

cbegin函数返回常量类型的迭代器const_iterator。

>练习9.10：下面4个对象分别是什么类型？
>
>```C++
>vector<int> v1;
>const vector<int> v2;
>auto it1 = v1.begin(), it2 = v2.begin();
>auto it3 = v1.cbegin(), it4 = v2.cbegin();
>```

`it1`：vector<int>::iterator

`it2`：vector<int>::const_iterator

`it3`：vector<int>::const_iterator

`it4`：vector<int>::const_iterator

>练习9.11：对6种创建和初始化vector对象的方法，每一种给出一个实例。解释每个vector包含什么值。

```C++
#include <vector>

using std::vector;
int main()
{
    vector<int> vec = {1, 2, 3};
    vector<int> vec1;                //vec1为空
    vector<int> vec2(vec);           //vec2使用vec进行直接初始化，vec2的元素和vec相同
    vector<int> vec3 = vec;          //vec3使用vec进行拷贝初始化，vec2的元素和vec相同
    vector<int> vec4 = {1, 2, 3, 4}; // vec4进行列表初始化，含有4个元素，分别为1,2,3,4
    vector<int> vec5{1, 2, 3, 4};    // vec5进行列表初始化，含有4个元素，分别为1,2,3,4
    vector<int> vec6(3);             // vec6含有3个元素，通过值初始化，全部为0
    vector<int> vec7(3, 2);          // vec7含有3个元素，全部为2
    vector<int> vec8(vec.begin(),vec.end()); // vec8初始化为迭代器指定范围内元素的拷贝，因此含有3个元素，分别为1,2,3
    return 0;
}
```

>练习9.12：对于接受一个容器创建其拷贝的构造函数，和接受两个迭代器创建拷贝的构造函数，解释它们的不同。

接受一个容器创建其拷贝的构造函数，两者的容器类型和元素类型都必须相同。

接受两个迭代器创建拷贝的构造函数，不需要容器的类型完全相同，同时只要被拷贝容器中的元素类型能够转化成该容器中的元素类型即可。

>练习9.13：如何从一个list<int>初始化一个vector<double>？从一个vector<int>又该如何创建？编写代码验证你的答案。

```C++
#include <iostream>
#include <list>
#include <vector>

using std::cout;
using std::endl;
using std::list;
using std::vector;

int main()
{
    list<int> lst = {1, 2, 3, 4};
    vector<double> vec(lst.begin(), lst.end());
    for (auto d : vec)
        cout << d << endl;
}
```

```C++
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<int> vec0 = {1, 2, 3, 4};
    vector<double> vec(vec0.begin(), vec0.end());
    for (auto d : vec)
        cout << d << endl;
}
```

>练习9.14：编写程序，将一个list中的char *指针（指向C风格字符串）元素赋值给一个vector中的string。

```C++
// 编写程序，将一个list中的char *指针（指向C风格字符串）元素赋值给一个vector中的string。
#include <iostream>
#include <list>
#include <string>
#include <vector>

using std::cout;
using std::endl;
using std::list;
using std::string;
using std::vector;

int main()
{
    vector<string> vec_str(1);
    list<const char *> lst_char{"abc", "def"};
    vec_str.assign(lst_char.cbegin(), lst_char.cend());
    for (auto &str : vec_str)
        cout << str << endl;
}
```

>练习9.15：编写程序，判定两个vector是否相等。

```C++
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<int> vec1 = {0, 1, 2, 3};
    vector<int> vec2 = {0, 1, 2, 3};
    if (vec1 == vec2)
        cout << "vec1 is equal to vec2." << endl;
    else
        cout << "vec1 is not equal to vec2." << endl;

    return 0;
}
```

>练习9.16：重写上一题的程序，比较一个list中的一个元素和一个vector中的元素。

```C++
#include <iostream>
#include <vector>
#include <list>

using std::cout;
using std::endl;
using std::list;
using std::vector;

int main()
{
    vector<int> vec1 = {0, 1, 2, 3};
    list<int> lst1 = {1, 2, 3};
    vector<int> vec2(lst1.begin(), lst1.end());
    if (vec1 == vec2)
        cout << "vec1 is equal to lst1." << endl;
    else
        cout << "vec1 is not equal to lst1." << endl;

    return 0;
}
```

>练习9.17：假定c1和c2是两个容器，下面的比较操作有何限制（如果有的话）？
>
>```C++
>if (c1 < c2)
>```

- `c1`和`c2`两个容器的类型相同，并且容器中元素的类型相同，同时
- `c1`和`c2`两个容器中的元素定义了关于`<`的运算，同时
- `c1`和`c2`两个容器均不是无序关联容器。

>练习9.18：编写程序，从标准输入读取**string序列**，存入一个deque中。编写一个循环，用迭代器打印deque中的元素。

```C++
#include <iostream>
#include <deque>
#include <string>

using std::cin;
using std::cout;
using std::deque;
using std::endl;
using std::string;

int main()
{
    string str;
    deque<string> deque_str;
    while (cin >> str)
        deque_str.push_back(str);
    for (auto iter = deque_str.cbegin(); iter != deque_str.cend(); ++iter)
        cout << *iter << endl;
    return 0;
}
```

>练习9.19：重写上题程序，用list代替deque。列出程序要做出哪些改变。

```C++
#include <iostream>
#include <list>
#include <string>

using std::cin;
using std::cout;
using std::list;
using std::endl;
using std::string;

int main()
{
    string str;
    list<string> list_str;
    while (cin >> str)
        list_str.push_back(str);
    for (auto iter = list_str.cbegin(); iter != list_str.cend(); ++iter)
        cout << *iter << endl;
    return 0;
}
```

>练习9.20：编写程序，从一个list拷贝元素到两个deque中。值为偶数的所有元素都拷贝到一个deque中，而奇数值元素都拷贝到另一个deque中。

```C++
#include <iostream>
#include <list>
#include <deque>

using std::cin;
using std::cout;
using std::deque;
using std::endl;
using std::list;

int main()
{
    list<int> lst = {0, 1, 2, 3};
    deque<int> deque1;
    deque<int> deque2;
    for (auto &i : lst)
    {
        if (i % 2 == 0)
            deque1.push_back(i);
        else
            deque2.push_back(i);
    }

    return 0;
}
```

>练习9.21：如果我们将308页中使用insert返回值将元素添加到list中的循环程序改写为将元素插入到vector中，分析循环将如何工作。

和`list`是相同的。在循环之前先将`iter`初始化为`vec.begin()`，第一次调用`insert`会将刚读入的`string`插入到`iter`所指向的元素之前，同时返回的迭代器刚好指向新插入的元素，之后的每次循环都将新读入的元素添加到`vector`的首元素之前。

>练习9.22：假定iv是一个int的vector，下面的程序存在什么错误？你将如何修改？
>
>```C++
>vector<int>::iterator iter = iv.begin(), mid = iv.begin() + iv.size()/2;
>while (iter != mid)
>if (*iter == some_val)
>   iv.insert(iter, 2 * some_val);
>```

该循环是个死循环，并且运行一次之后`mid`迭代器会失效。

修改如下：

```C++
auto cursor = iv.size() / 2;
auto iter = iv.begin(), mid = iv.begin() + cursor;
while (iter != mid) {
    if (*iter == some_val) {
        iter = iv.insert(iter, 2 * some_val);
        ++iter;
        ++cursor;
        mid = iv.begin() + cursor;
    }
    ++iter;
}
```

>练习9.23：在本节第一个程序（309页）中，若c.size()为1， 则val, val2, val3和val4的值会是什么？

四个变量的值均为`c`中唯一的元素的值。

>练习9.24：编写程序，分别使用at、下标运算符、front和begin提取一个vector中的第一个元素。在一个空vector上测试你的程序。

```C++
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<int> vec;
    cout << vec.at(0) << endl; // terminate called after throwing an instance of 'std::out_of_range'
    cout << vec[0] << endl; // Segmentation fault
    cout << vec.front() << endl; // Segmentation fault
    cout << *(vec.begin()) << endl; // Segmentation fault
    return 0;
}
```

>练习9.25：对于312页中删除一个范围内的元素的程序，如果elem1与elem2相等会发生什么？如果elem2是尾后迭代器，或者elem1和elem2皆为尾后迭代器，又会发生什么？

如果`elem1`和`elem2`相等，则什么都不会发生。

如果`elem2`是尾后指针，删除`elem1`指向的元素及其之后所有的元素，同时`elem1`是尾后迭代器。

如果`elem1`和`elem2`都是尾后指针，则什么都不会发生。

验证代码：

```C++
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<int> vec = {1, 2, 3};
    auto iter1 = vec.begin();
    auto iter2 = iter1;
    iter1 = vec.erase(iter1,iter2);
    cout << vec.size() << endl;

    return 0;
}
```

```C++
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<int> vec = {1, 2, 3};
    auto iter1 = vec.end();
    auto iter2 = iter1;
    iter1 = vec.erase(iter1,iter2);
    cout << vec.size() << endl;

    return 0;
}
```

>练习9.26：使用下面代码定义的ia，将ia拷贝到一个从vector和一个list中。使用单迭代器版本的erase从list中删除奇数元素，从vector中删除偶数元素。
>
>```C++
>int ia[] = {0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89};
>```

```C++
#include <iostream>
#include <vector>
#include <list>

using std::begin;
using std::cout;
using std::end;
using std::endl;
using std::list;
using std::vector;

int main()
{
    int ia[] = {0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89};

    vector<int> vec(begin(ia), end(ia));
    list<int> lst(begin(ia), end(ia));

    auto iter = vec.begin();
    while (iter != vec.end())
    {
        if ((*iter) % 2 == 0)
            iter = vec.erase(iter);
        else
            ++iter;
    }
    for (auto &i : vec)
        cout << i << endl;

    auto iter1 = lst.begin();
    while (iter1 != lst.end())
    {
        if ((*iter1) % 2)
            iter1 = lst.erase(iter1);
        else
            ++iter1;
    }
    for (auto &i : lst)
        cout << i << endl;

    return 0;
}
```

>练习9.27：编写程序，查找并删除forward_list中的奇数元素。

```C++
#include <iostream>
#include <forward_list>

using std::cout;
using std::endl;
using std::forward_list;

int main()
{
    forward_list<int> flst = {0, 1, 2, 3, 4, 5};
    auto pre = flst.before_begin();
    auto cur = flst.begin();
    while (cur != flst.end())
    {
        if ((*cur) % 2)
        {
            cur = flst.erase_after(pre);
        }
        else
        {
            ++pre;
            ++cur;
        }
    }

    for (auto &i : flst)
        cout << i << endl;
    return 0;
}

```

>练习9.28：编写函数，接受一个forward_list和两个string共三个参数。函数应在链表中查找第一个string，并将第二个string插入到紧接着第一个string之后的位置。若第一个string未在链表中，则将第二个string插入到链表末尾。

```C++
#include <iostream>
#include <forward_list>
#include <string>

using std::cout;
using std::endl;
using std::forward_list;
using std::string;

void insertString(forward_list<string> &flst, const string &str1, string &str2);

int main()
{
    forward_list<string> flst = {"abc", "def", "ghi"};
    string str1 = "deh";
    string str2 = "abs";
    insertString(flst, str1, str2);
    for (auto &str : flst)
        cout << str << endl;
    return 0;
}

void insertString(forward_list<string> &flst, const string &str1, string &str2)
{
    auto cur = flst.begin();
    auto prev = flst.before_begin();

    while (cur != flst.end())
    {
        if (*cur == str1)
        {
            cur = flst.insert_after(cur, str2);
            break;
        }
        else
        {
            ++cur;
            ++prev;
        }
        }
    if (cur == flst.end())
    {
        flst.insert_after(prev, str2);
    }
}

```

>练习9.29：假定vec包含25个元素，那么vec.resize(100)会做什么？如果接下来调用vec.resize(10)会做什么？

调用`vec.resize(100)`会向`vec`最后追加75个0。

接下来调用`vec.resize(10)`，`vec`中会只包含10个元素，这10个元素为`vec`原始的前10个元素。

>练习9.30：接受单个参数的resize版本对元素类型有什么限制（如果有的话）？

如果容器中元素为类类型，该类需要定义默认构造函数。

>练习9.31：第316页中删除偶数值元素并复制奇数值元素的程序不能用于list或forward_list。为什么？修改程序，使之也能用于这些类型。

`list`的迭代器没有定义`+`运算，定义了`advance`函数来移动`iter`。

```C++
#include <iostream>
#include <list>

using std::cout;
using std::endl;
using std::list;

int main()
{
    list<int> lst = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    auto iter = lst.begin();
    while (iter != lst.end())
    {
        if (*iter % 2)
        {
            iter = lst.insert(iter, *iter);
            advance(iter, 2);
        }
        else
        {
            iter = lst.erase(iter);
        }
    }
    for (auto &i : lst)
        cout << i << endl;

    return 0;
}
```

`forward_list`没有定义`insert`、`erase`函数，而是定义了`insert_after`、`erase_after`函数，`forward_list`的迭代器没有定义`+`运算，定义了`advance`函数来移动`iter`。

```C++
#include <iostream>
#include <forward_list>

using std::cout;
using std::endl;
using std::forward_list;

int main()
{
    forward_list<int> flst = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    auto iter = flst.before_begin();
    auto cur = flst.begin();

    while (cur != flst.end())
    {
        if (*cur % 2)
        {
            iter = flst.insert_after(iter, *cur);
            advance(iter, 1);
            advance(cur, 1);
        }
        else
        {
            cur = flst.erase_after(iter);
        }
    }
    for (auto &i : flst)
        cout << i << endl;

    return 0;
}
```

>练习9.32：在316页的程序中，向下面语句这样调用insert是否合法？如果不合法，为什么？
>
>```C++
>iter = vi.insert(iter, *iter++);
>```

不而发，因为函数参数的赋值顺序是未定义的。

>练习9.33：在本节最后一个例子中，如果不将insert的结果赋予begin，将会发生什么？编写程序，去掉此赋值语句，验证你的答案。

代码运行出现未定义的行为，因为向容器中插入新的元素，使得之前的迭代器、指针和引用失效。

```C++
#include <iostream>
#include <vector>
using namespace std;
int main()
{
    vector<int> vec = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    auto begin = vec.begin();
    while (begin != vec.end())
    {
        ++begin;
        vec.insert(begin, 42);
        ++begin;
    }
    for (auto i : vec)
        cout << i << " ";
    cout << endl;    
    return 0;
}
```

>练习9.34：假定vi是一个保存int的容器，其中有偶数值也有奇数值，分析下面循环的行为，然后编写程序验证你的分析是否正确。
>
>```C++
>iter = vi.begin();
>while (iter != vi.end())
>    if (*iter % 2)
>        iter = vi.insert(iter, *iter);
>    ++iter;
>```

该程序形成一个无限死循环，在`vi`的第一个奇数前一直添加该奇数。

```
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<int> vi = {2,3};
    auto iter = vi.begin();
    while (iter != vi.end())
        if (*iter % 2)
            iter = vi.insert(iter, *iter);
    ++iter;

    for (auto i : vi)
        cout << i << " ";
    cout << endl;
    return 0;
}
```

>练习9.35：解释一个vector的capacity和size有何区别？

`size`是指容器中已经保存的元素的数目。

`capacity`是指容器在不扩张内存空间的情况下容器可以保存多少个元素。

>练习9.36：一个容器的capacity可能小于它的size吗？

不可能。

>练习9.37：为什么list或array没有capacity成员函数？

`array`中的元素可以不连续存储。

`array`中元素个数是固定的，不能向其中添加元素。

>练习9.38：编写程序，探究在你的标准库实现中，vector是如何增长的？

```C++
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<int> vec;
    cout << "vec size = " << vec.size() << ' ' << "vec capacity = " << vec.capacity() << endl;
    for (int i = 0; i < 1; ++i)
    {
        vec.push_back(i);
    }
    cout << "vec size = " << vec.size() << ' ' << "vec capacity = " << vec.capacity() << endl;
    for (int i = 1; i < 5; ++i)
    {
        vec.push_back(i);
    }
    cout << "vec size = " << vec.size() << ' ' << "vec capacity = " << vec.capacity() << endl;
    vec.reserve(100);
    cout << "vec size = " << vec.size() << ' ' << "vec capacity = " << vec.capacity() << endl;
    while (vec.size() != vec.capacity())
        vec.push_back(0);
    cout << "vec size = " << vec.size() << ' ' << "vec capacity = " << vec.capacity() << endl;
    vec.resize(10);
    cout << "vec size = " << vec.size() << ' ' << "vec capacity = " << vec.capacity() << endl;

    return 0;
}

```

>练习9.39：解释下面程序片段做了什么？
>
>```C++
>vector<string> svec;
>svec.reserve(1024);
>string word;
>while (cin >> word)
>    svec.push_back(word);
>svec.resize(svec.size()+svec.size()/2);
>```

该段代码从终端输入`string`保存在`svec`中，如果`string`的个数超过1024，`svec`的`capacity`自动增加，同时如果`resize`之后`svec`的大小超过1024，`svec`的`capacity`也会自动增加。

>练习9.40：如果上一题的程序读入了256个词，在resize之后容器的capacity可能是多少？如果读入了512个、1000个或1048个词呢？

| read | size | capacity |
| ---- | ---- | -------- |
| 256  | 384  | 1024     |
| 512  | 768  | 1024     |
| 1000 | 1500 | 2000     |
| 1048 | 1572 | 2048     |

>练习9.41：编写程序，从一个vector初始化一个string。

```C++
#include <iostream>
#include <vector>
#include <string>

using std::vector;
using std::cout;
using std::endl;
using std::string;

int main()
{
    vector<char> vec{'a', 'b', 'c', 'd'};
    string str(vec.begin(), vec.end());

    cout << str << endl;

    return 0;
}
```

>练习9.42：假定你希望每次读取一个字符存入一个string中，而且知道最少需要读取100个字符，应该如何提高性能。

使用`reserve(100)`提前为该`string`开辟足够的空间。

>练习9.43：编写一个函数，接受三个string参数是s、oldVal 和newVal。使用迭代器及insert和erase函数将s中所有oldVal替换为newVal。测试你的程序，用它替换通用的简写形式，如，将"tho"替换为"though",将"thru"替换为"through"。

```C++
#include <iostream>
#include <string>

using std::cout;
using std::distance;
using std::endl;
using std::string;

void change(string &s, const string &oldVal, const string &newVal);

int main()
{
    string str{"To drive straight thru is a foolish, tho courageous act."};
    change(str, "thru", "through");
    change(str, "tho", "though");
    cout << str << endl;
    return 0;
}

void change(string &s, const string &oldVal, const string &newVal)
{
    for (auto begin = s.begin(); distance(begin, s.end()) >= distance(oldVal.begin(), oldVal.end());)
    {
        if (string{begin, begin + oldVal.size()} == oldVal)
        {
            begin = s.erase(begin, begin + oldVal.size());
            begin = s.insert(begin, newVal.begin(), newVal.end());
            begin += newVal.size();
        }
        else
        {
            ++begin;
        }
    }
}

```

>练习9.44：重写上一题的函数，这次使用一个**下标和replace**。

```C++
#include <iostream>
#include <string>

using std::cout;
using std::distance;
using std::endl;
using std::string;

void change(string &s, const string &oldVal, const string &newVal);

int main()
{
    string str{"To drive straight thru is a foolish, tho courageous act."};
    change(str, "thru", "through");
    change(str, "tho", "though");
    cout << str << endl;
    return 0;
}

void change(string &s, const string &oldVal, const string &newVal)
{
    for (auto i = 0; i != s.size(); ++i)
    {
        if (s.substr(i, oldVal.size()) == oldVal)
        {
            s.replace(i, oldVal.size(), newVal);
            i += oldVal.size() - 1;
        }
    }
}

```

>练习9.45：编写一个函数，接受一个**表示名字的string参数**和两个分别表示**前缀**（如”Mr.”或MS.）和**后缀**（如“Jr”或“3”）的字符串。使用**迭代器**及**insert**和**append函数**将**前缀和后缀**添加到给定的名字中，将生成的新string返回。

```C++
#include <iostream>
#include <string>

using std::cout;
using std::distance;
using std::endl;
using std::string;

void add(string &name, const string &prefix, const string &suffix);

int main()
{
    string name = "lx";
    string prefix = "Ms.";
    string suffix = "6";
    add(name, prefix, suffix);
    cout << name << endl;
    return 0;
}

void add(string &name, const string &prefix, const string &suffix)
{
    name.insert(name.begin(),prefix.begin(),prefix.end());
    name.append(suffix);

}

```

>练习9.46：重写上一题的函数，这次使用**位置和长度**来管理string，并**只使用insert**。

```C++
#include <iostream>
#include <string>

using std::cout;
using std::distance;
using std::endl;
using std::string;

void add(string &name, const string &prefix, const string &suffix);

int main()
{
    string name = "lx";
    string prefix = "Ms.";
    string suffix = "6";
    add(name, prefix, suffix);
    cout << name << endl;
    return 0;
}

void add(string &name, const string &prefix, const string &suffix)
{
    name.insert(0, prefix);
    name.insert(name.size(), suffix);
}

```

>练习9.47：编写程序，首先查找string "ab2c3d7R4E6"中的**每个**数字字符，然后查找其中每个字母字符。编写两个版本的程序，第一个要使用find_fisrt_of，第二个要使用find_first_not_of。

```C++
#include <iostream>
#include <string>

using std::cout;
using std::endl;
using std::string;

int main()
{
    string str = "ab2c3d7R4E6";
    string numbers = "0123456789";
    string alphabet{"abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"};
    string::size_type position = 0,position1 = 0;
    while((position = str.find_first_of(numbers,position))!=string::npos)
    {
        cout << position << endl;
        ++position;
    }
    while((position1 = str.find_first_of(alphabet,position1))!=string::npos)
    {
        cout << position1 << endl;
        ++position1;
    }
    return 0;
}

```

```C++
#include <iostream>
#include <string>

using std::cout;
using std::endl;
using std::string;

int main()
{
    string str = "ab2c3d7R4E6";
    string numbers = "0123456789";
    string alphabet{"abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"};
    string::size_type position = 0,position1 = 0;
    while((position = str.find_first_not_of(alphabet,position))!=string::npos)
    {
        cout << position << endl;
        ++position;
    }
    while((position1 = str.find_first_not_of(numbers,position1))!=string::npos)
    {
        cout << position1 << endl;
        ++position1;
    }
    return 0;
}

```

>练习9.48：假定name和numbers的定义如325页所示，number.find(name)返回什么？

```C++
string::npos
```

>练习9.49：如果一个字母延伸要中线之上，如d或f，则称其有上出头部分。如果延伸到中线之下，则称其有下出头部分。编写程序，读入一个**单词文件**，输出最长的既不包含上出头部分，也不包含下出头部分的单词。

```C++
#include <string>
#include <fstream>
#include <iostream>

using std::string;
using std::ifstream;
using std::cout;
using std::endl;

int main()
{
    ifstream ifs("../data/letter.txt");
    if (!ifs) return -1;
    string longest_word;
    for (string word; ifs >> word;)
        if (word.find_first_not_of("aceimnorsuvwxz") == string::npos &&
            word.size() > longest_word.size())
            longest_word = word;

    cout << longest_word << endl;
}
```

>练习9.50：编写程序处理一个vector,其元素都表示整型值。计算vector中所有元素之和。修改程序，使之计算表示浮点值的string之和。

```C++
#include <iostream>
#include <string>
#include <vector>

using std::cout;
using std::endl;
using std::string;
using std::vector;

int main()
{
    string str_sum;
    double double_sum = 0.0;
    vector<string> vec={"12","13","14"};
    for(auto &n:vec)
    {
        str_sum += n;
        double_sum += stod(n);
    }
    cout << str_sum << endl;
    cout << double_sum << endl;


}
```

>练习9.51：设计一个类，它有三个unsigned成员，分别表示年、月和日。为其编写构造函数，接受一个表示日期的string参数。你的构造函数应该能处理不同数据格式，如January 1，1900、1/1/1990、Jan 1 1900等。

```C++
#include <array>
#include <iostream>
#include <string>

class Date {
public:
    explicit Date(const std::string& str = "");
    void Print();
    unsigned year = 1970;
    unsigned month = 1;
    unsigned day = 1;

private:
    std::array<std::string, 12> month_names{"Jan", "Feb", "Mar", "Apr", "May", "Jun",
                                            "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"};
    unsigned MonthFromName(const std::string& str);
};

Date::Date(const std::string& str)
{
    if (str.empty()) return;
    std::string delimiters{" ,/"};
    auto month_day_delim_pos = str.find_first_of(delimiters);
    if (month_day_delim_pos == std::string::npos)
        throw std::invalid_argument("This format is not supported now.");
    month = MonthFromName(str.substr(0, month_day_delim_pos));
    auto day_year_delim_pos = str.find_first_of(delimiters, month_day_delim_pos + 1);
    auto day_len = day_year_delim_pos - month_day_delim_pos - 1;
    day = std::stoi(str.substr(month_day_delim_pos + 1, day_len));
    year = std::stoi(str.substr(day_year_delim_pos + 1));
}

void Date::Print()
{
    std::cout << year << "-" << month << "-" << day << "\n";
}

unsigned Date::MonthFromName(const std::string& str)
{
    if (str.empty()) return 0;
    if (std::isdigit(str[0])) return std::stoi(str);
    for (size_t i = 0; i != 12; ++i) {
        if (str.find(month_names[i]) != std::string::npos) return i + 1;
    }
    return 0; //  not found
}

int main()
{
    { //  default case
        auto date = Date();
        date.Print();
    }
    { //  case 0: January 1, 1900
        auto date = Date("January 1, 1900");
        date.Print();
    }
    { //  case 1: 1/1/1900
        auto date = Date("1/1/1900");
        date.Print();
    }
    { //  case 2: Jan 1, 1900
        auto date = Date("Jan 1, 1900");
        date.Print();
    }
}
```

>练习9.52：使用stack处理括号化的表达式。当你看到一个左括号，将其记录下来。当在一个左括号之后看到一个右括号，从stack中pop对象，直至遇到左括号，将左括号也一起弹出栈。然后将一个值push到栈中，表示一个括号化的表达式已经处理完毕，被其运算结果所替代。

```C++
#include <iostream>
#include <string>
#include <stack>

using std::cout;
using std::endl;
using std::stack;
using std::string;

int main()
{
    auto &expr = "This is (Mooophy(awesome)((((wooooooooo))))) and (ocxs) over";  // reference
    auto repl = '#';
    auto seen = 0;

    stack<char> stk;

    for (auto c : expr)
    {
        stk.push(c);
        if (c == '(')
            ++seen; // open
        if (seen && c == ')')
        { // pop elements down to the stack
            while (stk.top() != '(')
                stk.pop();
            stk.pop();      // including the open parenthesis
            stk.push(repl); // push a value indicate it was replaced
            --seen;         // close
        }
    }

    // Test
    string output;
    for (; !stk.empty(); stk.pop())
        output.insert(output.begin(), stk.top());
    cout << output << endl; // "This is # and # over"
}
```

