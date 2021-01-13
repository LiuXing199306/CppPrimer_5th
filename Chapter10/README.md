>练习10.1：头文件algorithm中定义了一个名为count的函数，它类似find，接受一对迭代器和一个值作为参数。count返回给定值在序列中出现的次数。编写程序，读取int序列存入vector中，打印有多少个元素的值等于给定值。

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using std::count;
using std::cout;
using std::endl;
using std::string;
using std::vector;

int main()
{
    vector<int> int_vec;
    int value = 2;
    for (auto i = 0; i < 10; ++i)
    {
        int_vec.push_back(i);
        int_vec.push_back(i);
    }
    auto n = count(int_vec.begin(), int_vec.end(), value);
    cout << "n = " << n << endl;
    return 0;
}
```

>练习10.2：重做上一题，但读取string序列存入list中。

```C++
#include <iostream>
#include <string>
#include <list>
#include <algorithm>

using std::count;
using std::cout;
using std::endl;
using std::string;
using std::list;

int main()
{
    list<string> int_str{"abc","def","def","hgh","def","bgh"};
    string value = "def";
   
    auto n = count(int_str.begin(), int_str.end(), value);
    cout << "n = " << n << endl;
    return 0;
}
```

>练习10.3：用accumulate求一个vector中的元素之和。

```C++
#include <iostream>
#include <vector>
#include <numeric>

using std::accumulate;
using std::cout;
using std::endl;
using std::string;
using std::vector;

int main()
{
    vector<int> vec_int{0, 1, 2, 3};
    auto sum = accumulate(vec_int.cbegin(), vec_int.cend(), 0);
    cout << "sum = " << sum << endl;

    return 0;
}
```

>练习10.4：假定v是一个vector<double>, 那么调用accumulate(v.cbegin(), v.cend(), 0)有何错误（如果存在）？

`v`中的元素必须是`int`类型，或者是能转换成`int`类型的数据类型，否则将出现类型不匹配的错误。同时即使是能转换成`int`的数据类型，也存在数据精度丢失导致结果错误的情况，如下例子所示：

```C++
#include <iostream>
#include <vector>
#include <numeric>

using std::accumulate;
using std::cout;
using std::endl;
using std::string;
using std::vector;

int main()
{
    vector<double> vec_double{0, 1.5, 2.2, 3.4};
    auto sum = accumulate(vec_double.cbegin(), vec_double.cend(), 0);
    cout << "sum = " << sum << endl;
    auto sum1 = accumulate(vec_double.cbegin(), vec_double.cend(), 0.0);
    cout << "sum1 = " << sum1 << endl;

    return 0;
}
```

编译器会先判断第三个参数的数据类型，然后将迭代器所指范围内每一个元素转换成第三个参数的数据类型，之后进行求和。

>练习10.5：在本节对名册（roster）调用equal的例子中，如果两个名册中保存的都是C风格字符串而不是string，会发生什么？

```C++
#include <iostream>
#include <list>
#include <vector>
#include <numeric>

using std::equal;
using std::cout;
using std::endl;
using std::begin;
using std::endl;
using std::list;
using std::vector;

int main()
{
    char str1[10] = "abc";
    char str2[10] = "abc";
    vector<char *> vec{str1};
    list<char *> lst{str2};
    auto result = equal(vec.cbegin(),vec.cend(),lst.begin());
    cout << "result = " << result << endl;
    return 0;
}
```

此时比较的是两个C风格字符串的地址，不再是两个C风格字符串的内容。

>练习10.6：编写程序，使用fill_n将一个**序列中**的int都设置为0。

```C++
#include <iostream>
#include <vector>
#include <algorithm>

using std::cout;
using std::endl;
using std::fill_n;
using std::vector;

int main()
{
    vector<int> vec_int{0, 1, 2, 3};
    fill_n(vec_int.begin(),vec_int.size(),0);
    for(auto n : vec_int)
    cout << n << endl;
    return 0;
}
```

>练习10.7：下面程序是否有错误？如果有，请改正。 
>
>```C++
>(a)
>vector<int> vec; list<int> lst; int i;
>while (cin >> i) 
>    lst.push_back(i);
>copy(lst.cbegin(), lst.cend(), vec.begin());
>(b)
>vector<int> v;
>v.reserve(10);
>fill_n(v.begin(), 10, 0);
>
>```

修改如下：

```C++
(a)
vector<int> vec;list<int> lst;int i;
while (cin >> i)
    lst.push_back(i);
vec.resize(lst.size());  // 目的序列至少要包含和输入序列一样多的元素个数
copy(lst.cbegin(), lst.cend(), vec.begin());
(b)
vector<int> v;
v.resize(10);  
fill_n(v.begin(), 10, 0);
// 或者
vector<int> v;
v.reserve(10);  
fill_n(back_insert(v), 10, 0);

```

>练习10.8：本节提到过，标准库算法不会改变他们所操作的容器的大小。为什么使用back_inserter不会使这一断言失效？

因为`back_insert`是一种插入迭代器，这种迭代器使用容器本身的成员函数来修改容器的大小。标准库中定义的泛型算法依然不改变容器的大小。

>练习10.9：实现你自己的elimDups。测试你的程序，分别在读取输入后、调用unique后以及调用erase后打印vector的内容（P343有例程）。

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using std::cout;
using std::endl;
using std::sort;
using std::string;
using std::unique;
using std::vector;

void elimDups(vector<string> &vec_str);

int main()
{
    vector<string> vec_str{"the", "quick", "red", "fox", "jumps", "over", "the", "slow", "red", "turtle"};
    elimDups(vec_str);
    return 0;
}
void elimDups(vector<string> &vec_str)
{
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;
    sort(vec_str.begin(), vec_str.end());
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;
    auto unique_iter = unique(vec_str.begin(), vec_str.end());
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;
    vec_str.erase(unique_iter, vec_str.end());
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;
}

```

>练习10.10：你认为算法不改变容器大小的原因是什么？

因为标准库算法是作用在传入的迭代器范围上，而不是直接作用在容器上，这样做的目的是将标准库函数和容器自身的成员函数进行区分。

>练习10.11：编写程序，使用stable_sort和isShorter将传递给你的elimDups版本的vector排序。打印vector的内容，验证你的程序的正确性。

```C++
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using std::cout;
using std::endl;
using std::sort;
using std::stable_sort;
using std::string;
using std::unique;
using std::vector;

void elimDups(vector<string> &vec_str);
bool isShorter(const string &str1, const string &str2)
{
    return (str1.size() < str2.size());
}

int main()
{
    vector<string> vec_str{"the", "quick", "red", "fox", "jumps", "over", "the", "slow", "red", "turtle"};
    elimDups(vec_str);
    stable_sort(vec_str.begin(), vec_str.end(), isShorter);
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;

    return 0;
}
void elimDups(vector<string> &vec_str)
{
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;
    sort(vec_str.begin(), vec_str.end());
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;
    auto unique_iter = unique(vec_str.begin(), vec_str.end());
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;
    vec_str.erase(unique_iter, vec_str.end());
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;
}

```

>练习10.12：编写名为compareIsbn的函数，比较两个Sales_data对象的isbn()。使用这个函数排序一个保存Sales_data对象的vector。

```C++
#include <algorithm>
#include <numeric>
#include <vector>

#include "Sales_data.h" // Sales_data class.

bool compareIsbn(const Sales_data& sd1, const Sales_data& sd2)
{
    return sd1.isbn() < sd2.isbn();
}

int main()
{
    Sales_data d1("CppPrimer"), d2("JavaCore"), d3("PythonCookBook"),
        d4("CppCore"), d5("AwesomeCPP");
    std::vector<Sales_data> v{d1, d2, d3, d4, d5};
    std::sort(v.begin(), v.end(), compareIsbn);

    for (const auto& element : v) std::cout << element.isbn() << " ";
    std::cout << std::endl;
}
```

>练习10.13：标准库定义了名为partition的算法，它接受一个谓词，对容器内容进行划分，使得谓词为true的值对排在容器的前半部分，而使谓词为false的值会排在后半部分。算法返回一个迭代器，指向最后一个使谓词为true的元素之后的位置。编写函数，接受一个string，返回一个bool值，指出string是否有5个或更多字符。使用此函数划分words。打印出长度大于等于5的元素。

```C++
#include <iostream>
#include <string>
#include <algorithm>

using std::cout;
using std::endl;
using std::partition;
using std::string;
using std::vector;

bool compare(const string &str);

int main()
{
    vector<string> words{"afhs;d", "js;l", "fhsd;lasadf", "fah"};
    auto vec_iter = partition(words.begin(), words.end(), compare);
    for (auto iter = words.begin(); iter != vec_iter; ++iter)
        cout << *iter << endl;
    return 0;
}

bool compare(const string &str)
{
    return (str.size() > 5);
}

```

>练习10.14：编写一个lambda，接受两个int，返回它们的和。

```C++
auto add = [](const int a,const int b){return a+b;};
```

>练习10.15：编写一个lambda，捕获**它所在函数的**int，**并接受一个int参数**。lambda应该返回捕获的int和int参数的和。

```C++
int i = 2;
auto sum = [i](const int j){return i +j;};
```

>练习10.16：使用lambda编写你自己版本的biggies。

```C++

#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using std::cout;
using std::endl;
using std::stable_sort;
using std::for_each;
using std::string;
using std::vector;

void elimDups(vector<string> &vec_str);
void biggies(vector<string> &words, vector<string>::size_type sz);

int main()
{
    vector<string> vec_str{"ahfj","fajklfasjk","afsj","s","fa"};
    vector<string>::size_type n = 5;
    biggies(vec_str,n);

    return 0;
}

void elimDups(vector<string> &vec_str)
{
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;
    sort(vec_str.begin(), vec_str.end());
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;
    auto unique_iter = unique(vec_str.begin(), vec_str.end());
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;
    vec_str.erase(unique_iter, vec_str.end());
    for (auto &str : vec_str)
        cout << str << endl;
    cout << endl;
}

void biggies(vector<string> &words, vector<string>::size_type sz)
{
    // 将words按照字典顺序排序，删除重复的单词
    elimDups(words);
    // 将words按照长度进行排序，长度相同的按照字典顺序进行排序
    stable_sort(words.begin(), words.end(), [](const string &str1, const string &str2) { return str1.size() < str2.size(); });
    // 找到words中长度大于sz的string位置
    auto iter = find_if(words.begin(),words.end(),[sz](const string &str){return str.size() >= sz;});
    for_each(iter, words.end(),[](const string &str){cout << str << " ";});
    cout << endl;

}

```

>练习10.17：重新10.3.1节练习10.12（第345页）的程序，在对sort的调用中使用lambda来代替函数compareIsbn。

```C++
#include <algorithm>
#include <numeric>
#include <vector>

#include "Sales_data.h" // Sales_data class.

bool compareIsbn(const Sales_data &sd1, const Sales_data &sd2)
{
    return sd1.isbn() < sd2.isbn();
}

int main()
{
    Sales_data d1("CppPrimer"), d2("JavaCore"), d3("PythonCookBook"),
        d4("CppCore"), d5("AwesomeCPP");
    std::vector<Sales_data> v{d1, d2, d3, d4, d5};
    std::sort(v.begin(), v.end(), [](const Sales_data &sd1, const Sales_data &sd2) { return sd1.isbn() < sd2.isbn(); });

    for (const auto &element : v)
        std::cout << element.isbn() << " ";
    std::cout << std::endl;
}
```

>练习10.18：重写biggies，用partition代替find_if。我们在10.13中介绍了partition算法。

```C++

#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using std::cout;
using std::endl;
using std::string;
using std::vector;

void elimDups(vector<string> &vec_str);
void biggies(vector<string> &words, vector<string>::size_type sz);

int main()
{
    vector<string> vec_str{"ahfj", "fajklfasjk", "afsj", "s", "fa"};
    vector<string>::size_type n = 5;
    biggies(vec_str, n);

    return 0;
}

void elimDups(vector<string> &vec_str)
{
    sort(vec_str.begin(), vec_str.end());
    auto unique_iter = unique(vec_str.begin(), vec_str.end());
    vec_str.erase(unique_iter, vec_str.end());
}

void biggies(vector<string> &words, vector<string>::size_type sz)
{
    // 将words按照字典顺序排序，删除重复的单词
    elimDups(words);
    // 找到words中长度大于sz的string位置
    auto iter = partition(words.begin(), words.end(), [sz](const string &str) { return str.size() >= sz; });
    for_each(words.begin(),iter, [](const string &str) { cout << str << " "; });
    cout << endl;
}

```

>练习10.19：用stable_partition重写前一题的程序，与stable_sort类似，在划分后的序列中维持原有元素的顺序。

```C++

#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using std::cout;
using std::endl;
// using std::stable_sort;
// using std::for_each;
using std::string;
using std::vector;

void elimDups(vector<string> &vec_str);
void biggies(vector<string> &words, vector<string>::size_type sz);

int main()
{
    vector<string> vec_str{"ahfj", "gklajk", "fajklfasjk", "afsj", "s", "fa"};
    vector<string>::size_type n = 5;
    biggies(vec_str, n);

    return 0;
}

void elimDups(vector<string> &vec_str)
{
    sort(vec_str.begin(), vec_str.end());
    auto unique_iter = unique(vec_str.begin(), vec_str.end());
    vec_str.erase(unique_iter, vec_str.end());
}

void biggies(vector<string> &words, vector<string>::size_type sz)
{
    // 将words按照字典顺序排序，删除重复的单词
    elimDups(words);
    // 找到words中长度大于sz的string位置
    auto iter = stable_partition(words.begin(), words.end(), [sz](const string &str) { return str.size() >= sz; });
    for_each(words.begin(), iter, [](const string &str) { cout << str << " "; });
    cout << endl;
}

```

>练习10.20：标准库定义了一个名为count_if的算法。类似find_if，此函数接受一对迭代器，表示一个输入范围，还接受一个谓词，会对输入范围中每个元素执行。count_if返回一个计数值，表示谓词有多少次为真。使用count_if重写我们程序中统计有多少单词长度超过6的部分。

```C++
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using std::cout;
using std::endl;
using std::string;
using std::vector;

int main()
{
    vector<string> vec_str{"ahfdk", "jafjl;adfj", "jsaf", "fsadhkjadhg"};
    cout << count_if(vec_str.begin(), vec_str.end(), [](const string &str) { return str.size() > 6; });
    cout << endl;
}

```

>练习10.21：编写一个lambda，捕获一个局部int变量，并递减变量值，直至它变为0.一旦变量变为0,再调用lambda应该不再递减变量.lambda应该返回一个bool值,指出捕获的变量是否为0。

```C++
#include <iostream>
using std::cout;
using std::endl;

int main()
{
    int i = 2;
    auto f = [&i]() -> bool {if(i == 0) return true; else{--i;return false;} };
    while(!f())
    cout << f() << endl;
    return 0;
}

```

>练习10.22：重写统计长度小于等于6的单词数量的程序,使用函数代替lambda。

```C++
#include <iostream>
#include <vector>
#include <string>
#include <functional>
#include <algorithm>

using std::cout;
using std::endl;
using std::string;
using std::vector;
using std::placeholders::_1;

bool check_size(const string &str,string::size_type n)
{
    return (str.size() > n);
}
int main()
{
    vector<string> vec_str{"ahsfdkuugh", "fashbc", "bahf", "uhqireiofauih"};
    auto iter = stable_partition(vec_str.begin(),vec_str.end(),bind(check_size,_1,6));
    
    for_each(vec_str.begin(),iter,[](const string &s){cout << s << " ";});
    cout << endl;
    return 0;
}
```

>练习10.23：bind接受几个参数？

如果要被绑定的函数需要`n`个参数，则`bind`接受的参数个数为`n+1`。

>练习10.24：给定一个string, 使用bind和check_size在一个int的vector中查找第一个大于string长度的值。

```C++
#include <iostream>
#include <vector>
#include <string>
#include <functional>
#include <algorithm>

using std::cout;
using std::endl;
using std::string;
using std::vector;
using std::placeholders::_1;

bool check_size(const int i, string::size_type n)
{
    return (i > n);
}
int main()
{
    string str{"sfajjlhs;dfg"};
    vector<int> vec_int{1, 2, 3, 14, 15};
    auto iter = find_if(vec_int.begin(), vec_int.end(), bind(check_size, _1,str.size()));
    cout << iter - vec_int.begin() << endl;
    return 0;
}
```

>练习10.25：在10.3.2节的练习中,编写了一个使用partition的biggies版本.使用check_size和bind重写此函数。

```C++

#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <functional>

using std::cout;
using std::endl;
using std::string;
using std::vector;
using std::placeholders::_1;

void elimDups(vector<string> &vec_str);
void biggies(vector<string> &words, vector<string>::size_type sz);
bool check_size(const string &str, string::size_type n);

int main()
{
    vector<string> vec_str{"ahfj", "fajklfasjk", "afsjnsdnf", "s", "fa"};
    vector<string>::size_type n = 5;
    biggies(vec_str, n);

    return 0;
}

bool check_size(const string &str, string::size_type n)
{
    return (str.size() > n);
}

void elimDups(vector<string> &vec_str)
{
    sort(vec_str.begin(), vec_str.end());
    auto unique_iter = unique(vec_str.begin(), vec_str.end());
    vec_str.erase(unique_iter, vec_str.end());
}

void biggies(vector<string> &words, vector<string>::size_type sz)
{
    // 将words按照字典顺序排序，删除重复的单词
    elimDups(words);
    // 找到words中长度大于sz的string位置
    auto iter = partition(words.begin(), words.end(), bind(check_size, _1, sz));
    for_each(words.begin(), iter, [](const string &str) { cout << str << " "; });
    cout << endl;
}

```

>练习10.26：解释三种插入迭代器的不同之处。

`back_inserter`：创建一个使用`push_back`的迭代器。

`front_inserter`：创建一个使用`push_frint`的迭代器。

`inserter`：创建一个使用`insert`的迭代器，此函数接受第二个参数，这个参数必须是一个指向给定容器的迭代器，元素将被插入到给定迭代器所表示的元素之前。

>练习10.27：除了unique（参见10.2.3节，第343页）之外，标准库还定义了名为unique_copy的函数，它接受第三个迭代器，表示**拷贝不重复元素**的目的位置。编写一个程序，使用unique_copy将一个vector中不重复的元素拷贝到一个初始化为空的list中。

```C++
#include <iostream>
#include <list>
#include <vector>
#include <iterator>
#include <algorithm>

using std::cout;
using std::endl;
using std::list;
using std::vector;

int main()
{
    vector<int> vec_int{0, 1, 2, 3};
    list<int> lst;
    unique_copy(vec_int.begin(),vec_int.end(),inserter(lst,lst.begin()));
    for(auto n : lst)
    cout << n << endl;
    return 0;

}
```

>练习10.28：一个vector中保存1到9，将其拷贝到三个其他容器中。分别使用inster、back_inserter和front_inserter将元素添加到三个容器中。对每种inserter，估计序列是怎样的，运行程序验证你的估计是否正确。

```C++
#include <iostream>
#include <list>
#include <vector>
#include <iterator>
#include <algorithm>

using std::cout;
using std::endl;
using std::list;
using std::vector;

int main()
{
    vector<int> vec_int{1, 2, 3, 4, 5, 6, 7, 8, 9};
    list<int> lst;
    copy(vec_int.begin(),vec_int.end(),inserter(lst,lst.begin()));  // 顺序排列
    // copy(vec_int.begin(),vec_int.end(),front_inserter(lst));     // 逆序排列
    // copy(vec_int.begin(),vec_int.end(),back_inserter(lst));      // 顺序排列
    for(auto n : lst)
    cout << n << endl;
    return 0;

}
```

>练习10.29：编写程序，使用流迭代器读取一个文本文件，存入一个vector中的string里。

```C++
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <iterator>
#include <algorithm>

using std::cerr;
using std::cout;
using std::endl;
using std::fstream;
using std::istream_iterator;
using std::ostream_iterator;
using std::string;
using std::vector;

int main()
{
    vector<string> vec_str;
    fstream fstrm("./test.txt");
    if (!fstrm)
    {
        cerr << "file is not exit!!!" << endl;
        return -1;
    }
    istream_iterator<string> str_iter(fstrm);
    istream_iterator<string> str_iter_eof;

    copy(str_iter,str_iter_eof,back_inserter(vec_str));
    copy(vec_str.begin(),vec_str.end(),ostream_iterator<string>(cout ," "));
}
```

>练习10.30：使用流迭代器、sort和copy从标准输入读取一个整数序列，将其排序，并将结果写到标准输出。

```C++
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>

using std::cin;
using std::cout;
using std::endl;
using std::istream_iterator;
using std::ostream_iterator;
using std::vector;

int main()
{
    vector<int> vec_int;
    istream_iterator<int> int_iter(cin);
    istream_iterator<int> int_iter_eof;
    ostream_iterator<int> int_iter_output(cout, " ");
    copy(int_iter, int_iter_eof, back_inserter(vec_int));
    sort(vec_int.begin(), vec_int.end());
    copy(vec_int.begin(), vec_int.end(), int_iter_output);
    return 0;
}

```

>练习10.31：修改前一题的程序，使其只打印不重复的元素。你的程序应使用unique_copy。

```C++
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>

using std::cin;
using std::cout;
using std::endl;
using std::istream_iterator;
using std::ostream_iterator;
using std::vector;

int main()
{
    vector<int> vec_int;
    istream_iterator<int> int_iter(cin);
    istream_iterator<int> int_iter_eof;
    ostream_iterator<int> int_iter_output(cout, " ");
    copy(int_iter, int_iter_eof, back_inserter(vec_int));
    sort(vec_int.begin(), vec_int.end());
    unique_copy(vec_int.begin(), vec_int.end(), int_iter_output);
    cout << endl;
    return 0;
}

```

>练习10.32：重写1.6节的书店程序，使用一个vector保存交易记录，使用不同算法完成处理。使用sort和10.3.1节中的compareIsbn函数来排序交易记录，然后使用find和accumulate求和。

```C++
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>
#include <numeric>
#include "../include/Sales_item.h"

int main()
{
    std::istream_iterator<Sales_item> in_iter(std::cin), in_eof;
    std::vector<Sales_item> vec;

    while (in_iter != in_eof) vec.push_back(*in_iter++);
    sort(vec.begin(), vec.end(),
         [](Sales_item const& lhs, Sales_item const& rhs) {
             return lhs.isbn() < rhs.isbn();
         });
    for (auto beg = vec.cbegin(), end = beg; beg != vec.cend(); beg = end) {
        end = find_if(beg, vec.cend(), [beg](const Sales_item& item) {
            return item.isbn() != beg->isbn();
        });
        std::cout << std::accumulate(beg, end, Sales_item(beg->isbn()))
                  << std::endl;
    }
}
```

>练习10.33：编写程序，接受三个参数：一个输入文件和两个输出文件的文件名。输入文件保存的应该是整数。使用istream_iterator读取输入文件。使用ostream_iterator将奇数写入第一个输出文件，每个值之后都跟一个空格。将偶数写入第二个输出文件，每个值都独占一行。

```C++
#include <fstream>
#include <iterator>
#include <algorithm>

int main(int argc, char** argv)
{
    if (argc != 4) return -1;

    std::ifstream ifs(argv[1]);
    std::ofstream ofs_odd(argv[2]), ofs_even(argv[3]);

    std::istream_iterator<int> in(ifs), in_eof;
    std::ostream_iterator<int> out_odd(ofs_odd, " "), out_even(ofs_even, "\n");

    std::for_each(in, in_eof, [&out_odd, &out_even](const int i) {
        *(i & 0x1 ? out_odd : out_even)++ = i;
    });

    return 0;
}
```

>练习10.34：使用reverse_iterator逆序打印一个vector。

```C++
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<int> vec_int{2, 3, 4};
    for (auto iter = vec_int.crbegin(); iter != vec_int.crend(); ++iter)
        cout << *iter << endl;
    return 0;
}
```

>练习10.35：使用普通迭代器逆序打印一个vector。

```C++
#include <iostream>
#include <vector>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<int> vec_int{2, 3, 4};
    for (auto iter = vec_int.end() - 1; iter != vec_int.cbegin(); --iter)
        cout << *iter << endl;
    cout << *(vec_int.begin()) << endl;
    return 0;
}
```

>练习10.36：使用find在一个int的list中查找最后一个值为0的元素。

```C++
#include <iostream>
#include <list>
#include <algorithm>

using std::cout;
using std::endl;
using std::list;

int main()
{
    list<int> lst_int{1, 0, 2, 3, 4, 3, 2, 1};
    auto iter = find(lst_int.crbegin(), lst_int.crend(), 0);
    cout << *iter << endl;
    cout << *iter.base() << endl;
    return 0;
}
```

>练习10.37：给定一个包含10个元素的vector，将位置3到7之间的元素按逆序拷贝到一个list中。

```C++
#include <iostream>
#include <vector>
#include <algorithm>

using std::cout;
using std::endl;
using std::vector;

int main()
{
    vector<int> vec_int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    vector<int> vec_int_copy(7 - 3 + 1);
    auto iter1 = vec_int.begin() + 3;
    auto iter2 = vec_int.begin() + 8;
    reverse_copy(iter1, iter2, vec_int_copy.begin());
    for (auto i : vec_int_copy)
        cout << i << endl;
    return 0;
}
```

>练习10.38：列出5个迭代器类别，以及每类迭代器所支持的操作。

|   迭代器类别   |              支持的操作              |
| :------------: | :----------------------------------: |
|   输入迭代器   |    只读、不写；单遍扫描，只能递增    |
|   输出迭代器   |    只写、不读；单遍扫描，只能递增    |
|   前向迭代器   |      可读写；多遍扫描，只能递增      |
|   双向迭代器   |     可读写；多遍扫描，可递增递减     |
| 随机访问迭代器 | 可读写；多遍扫描，支持全部迭代器操作 |

>练习10.39：list上的迭代器属于哪类？vector呢？

`list`上的迭代器属于双向迭代器。

`vector`上的迭代器属于随机访问迭代器。

>练习10.40：你认为copy要求哪类迭代器？reverse和unique呢？

`copy`要求输出迭代器。

`reverse`要求双向迭代器。

`unique`要求前向迭代器。

>练习10.41：仅根据算法和参数的名字，描述下面每个标准库算法执行什么操作：
>
>```C++
>repalce(beg,end,old_val,new_val);
>replace_if(beg,end,pred,new_val);
>replace_copy(beg,end,dest,old_val,new_val);
>replace_copy_if(beg,end,dest,pred,new_val);
>```

```C++
// 查找[beg,end)中等于`old_val`的值并将其替换为`new_val`。
repalce(beg,end,old_val,new_val);
// 查找[beg,end)中使`pred`为真的值并将其替换为`new_val`。
replace_if(beg,end,pred,new_val);
// 查找[beg,end)之间等于`old_val`的值并将其替换为`new_val`，然后将替换后的值拷贝到`dest`目标位置。
replace_copy(beg,end,dest,old_val,new_val);
// 查找[beg,end)之间使`pred`为真的值并将其替换为`new_val`，然后将替换后的值拷贝到`dest`目标位置。
replace_copy_if(beg,end,dest,pred,new_val);
```

>练习10.42：使用list代替vector重新实现10.2.3节中的去除重复单词的程序。

```C++
#include <iostream>
#include <string>
#include <list>

using std::string;
using std::list;

void elimDups(list<string>& words)
{
    words.sort();
    words.unique();
}

int main()
{
    list<string> l = {"aa", "aa", "aa", "aa", "aasss", "aa"};
    elimDups(l);
    for (const auto& e : l) std::cout << e << " ";
    std::cout << std::endl;
}
```

