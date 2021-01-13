> 练习11.1：描述map和vector的不同。

`map`是一种关联容器，元素为键值对，通过查找关键字可以高效地进行查找和访问。

`vector`是一种顺序容器，且其中的元素在空间中是连续保存的，可以通过索引进行查找和访问。

> 练习11.2：分别给出最适合使用list、vector、deque、map以及set的例子。

`list`：需要从中间插入或删除元素，比如TODO list。

`vector`：需要从末尾追加元素，同时需要使用索引任意访问元素，比如重要的数据。

`deque`：先进先出，比如消息处理。

`map`：键值对，比如字典。

`set`：数据的集合，比如银行统计信用不好的用户的集合。

> 练习11.3：练习11.3:编写你自己的单词记数程序。

```C++
#include <iostream>
#include <string>
#include <map>
#include <set>

using std::cin;
using std::cout;
using std::endl;
using std::map;
using std::set;
using std::string;

int main()
{
    map<string, size_t> word_counts;
    set<string> exclude = {"the", "an", "a"};
    string word;
    while (cin >> word)
    {
        if (exclude.find(word) == exclude.end())
            ++word_counts[word];
    }
    for (auto &w : word_counts)
        cout << w.first << " occurs " << w.second << (w.second > 1 ? " times." : " time.") << endl;
    return 0;
}
```

> 练习11.4：扩展你的程序，忽略大小写和标点。例如，“example.”也要递增相同的计数器。

```C++
#include <iostream>
#include <string>
#include <map>
#include <set>
#include <algorithm>
#include <cctype>

using std::cin;
using std::cout;
using std::endl;
using std::map;
using std::set;
using std::string;

int main()
{
    set<string> exclude{"a", "an", "the"};
    map<string, size_t> word_counts;
    string word;
    while (cin >> word)
    {
        if (exclude.find(word) == exclude.end())
        {
            // to lower
            transform(word.begin(), word.end(), word.begin(), [](char &c) { return tolower(c); });
            // skip symbol
            word.erase(remove_if(word.begin(), word.end(), [](char &c) { return ispunct(c); }), word.end());
            ++word_counts[word];
        }
    }
    for (auto &w : word_counts)
        cout << w.first << " occurs " << w.second << (w.second > 1 ? " times." : " time.") << endl;
    return 0;
}
```

> 练习11.5：解释map和set的区别。你如何选择使用哪个？

`set`：元素类型就是关键字的类型。

`map`：其中的每个元素都是键值对。

当只需要存储关键字的时候使用`set`，当需要存储键值对的时候，使用`map`。

> 练习11.6：解释set和list的区别。你如何选择使用哪个？

`set`：其中的元素是唯一且有序的。

`list`：通过指针指向下一个元素，元素之间不需要满足唯一性和有序性。

如果元素需要进行有序和唯一存储，则使用`set`，否则使用`list`。

> 练习11.7：定义一个map关键字是家庭的姓，值是一个vector，保存家中孩子（们）的名字。编写代码，实现添加新的家庭以及向已有家庭添加新的孩子。

```C++
#include <iostream>
#include <string>
#include <vector>
#include <map>

using std::cin;
using std::cout;
using std::endl;
using std::map;
using std::string;
using std::vector;

int main()
{
    map<string, vector<string>> family;
    string last_name;
    string children_name;
    while ([&]() -> bool {cout << "Please enter last name: " << endl; return cin >> last_name && last_name != "@q"; }())
    {
        cout << "Please enter children name: " << endl;
        while (cin >> children_name && children_name != "@q")
        {
            family[last_name].push_back(children_name);
        }
    }

    for (auto &fam : family)
    {
        cout << fam.first << " : " ;
        for(auto &chil:fam.second)
        cout << chil << " ";

    }
    cout << endl;
}
```

> 练习11.8：编写一个程序，在一个vector而不是一个set中保存不重复的单词。使用set的优点是什么？

```C++
// 使用vector
#include <iostream>
#include <vector>
#include <algorithm>

using std::cin;
using std::cout;
using std::endl;
using std::vector;

int main()
{
    int n;
    vector<int> vec_int;
    while (cin >> n)
    {
        if (find(vec_int.begin(), vec_int.end(), n) == vec_int.end())
            vec_int.push_back(n);
    }
    for (auto &i : vec_int)
        cout << i << endl;
    return 0;
}
```

使用`set`的优点是自动忽略`set`中已经存在的元素。

> 练习11.9：定义一个map，将单词与一个行号的list关联，list中保存的是单词所出现的行号。

```C++
map<string,list<size_t>> words;
```

> 练习11.10：可以定义一个vector<int>::iterator 到 int 的map吗？list<int>::iterator 到 int 的map呢？对于两种情况，如果不能，解释为什么。

```C++
#include <iostream>
#include <string>
#include <vector>
#include <list>
#include <map>

using std::cout;
using std::endl;
using std::list;
using std::map;
using std::string;
using std::vector;

int main()
{
    vector<int> vec_int{0, 1, 2, 3};
    list<int> lst_int{4, 5, 6};

    map<string, list<size_t>> words;
    map<vector<int>::iterator, int> map1;
    map<list<int>::iterator, int> map2;
    map1.emplace(vec_int.begin(), 0);
    // map2.emplace(lst_int.begin(), 4);
    return 0;
}
```

可以声明`vector<int>::iterator` 到 `int` 的`map`和`list<int>::iterator` 到` int` 的`map`，可以定义`vector<int>::iterator` 到 `int` 的`map`，但是不能定义`list<int>::iterator` 到` int` 的`map`，因为`list`的迭代器之间没有定义严格弱序的比较运算符，但是`vector`的迭代器之间定义了严格弱序的比较运算符。

> 练习11.11：不使用decltype 重新定义bookstore。

```C++
multiset<Sales_data,&compareIsbn> bookstore(compareIsbn);
```

> 练习11.12：编写程序，读入string和int的序列，将每个string和int存入一个pair中，pair保存在一个vector中。

```C++
#include <iostream>
#include <string>
#include <vector>
#include <utility>

using std::cin;
using std::cout;
using std::endl;
using std::string;
using std::vector;
using std::pair;
using std::make_pair;

int main()
{
    string str;
    int n;
    vector<pair<string,int>> vec_pair;
    while(cin >> str >> n)
    {
        vec_pair.push_back(make_pair(str,n));
    }
    for(auto &p:vec_pair)
    {
        cout << p.first << " " << p.second << endl;
    }

    return 0;
}
```

> 练习11.13：在上一题的程序中，至少有三种创建pair的方法。编写此程序的三个版本，分别采用不同的方法创建pair。解释你认为哪种形式最易于编写和理解，为什么？

```C++
#include <iostream>
#include <string>
#include <vector>

using std::cin;
using std::cout;
using std::endl;
using std::string;
using std::vector;
using std::pair;

int main()
{
    vector<pair<string,int>> vec;
    string str;
    int n;
    while(cin >> str >> n)
    // vec.push_back(make_pair(str,n));
    // vec.emplace_back(str,n); // 该方法最简单
    // vec.push_back({str,n});
    vec.push_back(pair<string,int>{str,n});



    for(auto & v:vec)
    {
        cout << v.first << " " << v.second << endl;
    }
    return 0;

}
```

> 练习11.14：扩展你在11.2.1节练习中编写的孩子姓到名的map，添加一个pair的vector，报错孩子的名和生日。

```C++
#include <iostream>
#include <string>
#include <vector>
#include <map>

using std::cin;
using std::cout;
using std::endl;
using std::map;
using std::string;
using std::vector;
using std::pair;

int main()
{
    map<string, vector<pair<string,string>>> family;
    string last_name;
    string children_name;
    string birthday;
    while ([&]() -> bool {cout << "Please enter last name: " << endl; return cin >> last_name && last_name != "@q"; }())
    {
        cout << "Please enter children name and birthday: " << endl;
        while (cin >> children_name >> birthday && children_name != "@q"&& birthday != "@q")
        {
            family[last_name].push_back({children_name,birthday});
        }
    }

    for (auto &fam : family)
    {
        cout << fam.first << " : " ;
        for(auto &chil:fam.second)
        cout << chil.first << " " << chil.second << " ";

    }
    cout << endl;
}
```

> 练习11.15：对一个int到vector<int>的map，其mapped_type、key_type和value_type分别是什么？

`mapped_type`：vector<int>

`key_type`：int

`value_type`：pair<const int,vector<int>>

> 练习11.16：使用一个map迭代器编写一个表达式，将一个值赋予一个元素。

```C++
std::map<int, std::string> map;
map[LiuXing] = "1993";
std::map<int, std::string>::iterator it = map.begin();
it->second = "Henan";
```

> 练习11.17：假定 c 是一个string的multiset，v是一个string的vector，解释下面的调用。指出每个调用是否合法：
>
> ```C++
> copy(v.begin(), v.end(), inserter(c, c.end()));
> copy(v.begin(), v.end(), back_inserter(c));
> copy(c.begin(), c.end(), inserter(v, v.end()));
> copy(c.begin(), c.end(), back_inserter(v));
> ```
>

```C++
// 合法，通过调用insert将v中的元素顺序插入c的末尾
copy(v.begin(), v.end(), inserter(c, c.end())); 
// 不合法，因为set中没有定义push_back
copy(v.begin(), v.end(), back_inserter(c));
// 合法，通过调用insert将c中的元素插入v中
copy(c.begin(), c.end(), inserter(v, v.end()));
// 合法，通过调用push_back将c中的元素插入v中
copy(c.begin(), c.end(), back_inserter(v));
```

> 练习11.18：写出第382页循环中map_it的类型，不要使用auto或decltype。

```C++
map<string,size_t>::iterator
```

> 练习11.19：定义一个变量，通过对11.2.2节（第378页）中的名为 bookstore 的multiset调用begin()来初始化这个变量。写出变量的类型，不要使用auto或decltype。

```C++
using compare_type = bool(*)(const Sales_data &lhs, const Sales_data &rhs);
std::multiset<Sales_data, compareType> bookstore(compareIsbn);
std::multiset<Sales_data, compareType>::iterator c_it = bookstore.begin();
```

> 练习11.20：重写单词计数程序，使用insert代替下标。

```C++
#include <iostream>
#include <string>
#include <map>

using std::cin;
using std::cout;
using std::endl;
using std::map;
using std::string;

int main()
{
    map<string,size_t> word_counts;
    auto ret = word_counts.insert({"aa",3});
    if(!ret.second)
    {
        ++(ret.first)->second;
    }
    for(auto &w:word_counts)
    cout << w.first << " " << w.second << endl;
    return 0;
}
```

> 练习11.21：假定word_count是一个string到size_t的map，word是一个string，解释下面循环的作用。
>
> ```C++
> while (cin >> word)
>     ++word_count.insert({word, 0}).first->second;
> ```

执行顺序是：

```C++
while (cin >> word)
    ++((word_count.insert({word, 0}).first)->second);
```

> 练习11.22：给定一个map<string,vector<int>>，对此容器的插入一个元素的insert版本，写出其**参数类型**和返回类型。

参数类型：

```C++
pair<string, vector<int>>
```

返回类型：

```C++
pair<map<string, vector<int>>::iterator, bool>
```

> 练习11.23：11.2.1节练习（第378页）中的map以孩子的姓为关键字，保存他们的名的vector，用multimap重写此map。

```C++
#include <iostream>
#include <string>
#include <map>

using std::cin;
using std::cout;
using std::endl;
using std::multimap;
using std::string;

int main()
{
    multimap<string, string> family;
    string last_name;
    string child_name;
    while (cin >> last_name >> child_name)
    {
        family.emplace(last_name, child_name);
    }
    for (auto &fam : family)
    {
        cout << fam.first << " " << fam.second << endl;
    }
    return 0;
}
```

> 练习11.24：下面的程序完成什么功能？
>
> ```C++
>map<int, int> m;
> m[0] = 1;
>```
> 

向map插入{0,1}这个键值对。

> 练习11.25：对比下面的程序与上一题程序
>
> ```C++
>vector<int> v;
> v[0] = 1;
>```
> 

编译出错。

> 练习11.26：可以用什么类型来对一个map进行下标操作？下标运算符返回的类型是什么？请给出一个具体例子———即，定义一个map，然后写出一个可以用来对map进行下标操作的类型以及下标运算符将会返会的类型。

可以使用map中关键字类型数据对map进行下标操作，如果关键字在map中存在，下标运算符的返回类型为mapped_type，但是如果关键字在map中不存在，向map中插入一个新的元素。

```
#include <iostream>
#include <string>
#include <map>

using std::cin;
using std::cout;
using std::endl;
using std::map;
using std::string;

int main()
{
    map<string, int> word_counts;
    word_counts.insert({"aa", 0});
    auto n = word_counts["aa"];
    cout << n << endl;
    return 0;
}
```

> 练习11.27：对于什么问题你会使用count来解决？什么时候你又会选择find呢？

- 当要查找的关键字不在关联容器中，但是我们不希望向其中插入这个不存在的关键字的键值对时，使用count，否则使用find。
- 当对允许存在重复关键字的关联容器查找关键字时，使用count可以返回关联容器中关键字的数目。

> 练习11.28：对一个string到int的vector的map，**定义并初始化一个变量**来保存在其上调用find所返回的结果。

```c++
#include <iostream>
#include <vector>
#include <string>
#include <map>

using std::cin;
using std::cout;
using std::endl;
using std::map;
using std::string;
using std::vector;

int main()
{
    map<string, vector<int>> map_str_vec{
        {"aa", {0, 2}},
        {"bb", {1}},
    };
    string value = "aa";
    map<string, vector<int>>::iterator iter = map_str_vec.find(value);
    for (auto &n : (iter->second))
        cout << n << endl;

    return 0;
}
```

> 练习11.29：如果给定的关键字不在容器中，upper_bound、lower_bound 和 equal_range 分别会返回什么？

- 如果给定的关键字不在容器中，同时关键字又比容器中最大的元素还要大，则upper_bound和lower_bound 返回尾置迭代器，equal_range返回pair，pair中的两个迭代器均为尾置迭代器。
- 否则，如果给定的关键字不在容器中，则upper_bound和lower_bound 返回该迭代器可以插入的位置。equal_range返回pair，pair中的两个迭代器均指向迭代器可以插入的位置。

> 练习11.30：对于本节最后一个程序中的输出表达式，解释运算对象pos.first->second的含义。

pos是一个pair数据类型，pos.first是pai中第一个迭代器，类型为map<string,string>::iterator，pos.first->second是作者对应的书籍的名称。

> 练习 11.31：编写程序，定义一个作者以及其作品的**multimap**。使用find在multimap中查找一个元素并用erase删除它。确保你的程序在元素不在map中时也能正常运行。

```C++
#include <iostream>
#include <string>
#include <map>

using std::cin;
using std::cout;
using std::endl;
using std::multimap;
using std::string;

int main()
{
    multimap<string, string> authors{{"aa", "bb"}, {"aa", "cc"}, {"dd", "ee"}};
    string author = "aa";
    // string author = "bb";
    string book = "cc";

    auto iter = authors.find(author);
    while (iter != authors.end())
    {
        if ((iter->first == author) && (iter->second == book))
        {
            authors.erase(iter);
            iter = authors.find(author);
        }
        else
        {
            ++iter;
        }
    }

    for (auto &auth : authors)
        cout << auth.first << " " << auth.second << endl;

    return 0;
}
```

> 练习11.32：使用上一题定义的multimap编写一个程序，按字典序打印作者列表和他们的作品。

```C++
#include <iostream>
#include <string>
#include <map>
#include <set>

using std::cin;
using std::cout;
using std::endl;
using std::map;
using std::multimap;
using std::multiset;
using std::string;

int main()
{
    multimap<string, string> authors{{"dd", "ee"}, {"aa", "cc"}, {"aa", "bb"}};
    map<string, multiset<string>> ordered_authors;
    for (auto &auth : authors)
        ordered_authors[auth.first].insert(auth.second);

    for (auto &auth : ordered_authors)
    {
        cout << auth.first << " " << endl;
        for (auto &book : auth.second)
        {
            cout << book << endl;
        }

        cout << endl;
    }

    return 0;
}
```

> 练习11.33：实现你自己版本的单词转换程序。

```C++
#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <map>
#include <stdexcept>

using std::cout;
using std::endl;
using std::ifstream;
using std::istringstream;
using std::map;
using std::runtime_error;
using std::string;

// 建立转换映射
map<string, string> buildMap(ifstream &map_file)
{
    map<string, string> transformation_map;
    string key;
    string value;
    // 读取第一个单词存入key，行中剩余部分存入value
    while (map_file >> key && getline(map_file, value))
    {
        if (value.size() > 1) // 存在要转换成的词汇
        {
            transformation_map[key] = value.substr(1).substr(0,value.find_last_not_of(' '));
        }
        else
        {
            throw runtime_error("no rule for " + key);
        }
    }
    return transformation_map;
}

// 生成转换文本
const string & transform(const string &s,const map<string, string> transformation_map)
{
    auto iter = transformation_map.find(s);
    if(iter != transformation_map.end())
    {
        return iter->second;
    }
    else
    {
        return s;
    }
    
}

void word_transformation(ifstream &map_file, ifstream &input)
{
    auto transformation_map = buildMap(map_file); // 保存转换规则
    string text;                                  // 保存输入中的每一行
    while (getline(input, text))
    {
        istringstream strm(text);
        string word;
        while (strm >> word)
        {
            bool first_word = true;
            if (first_word)
                first_word = false;
            else
            {
                cout << " ";
            }
            cout << transform(word, transformation_map);
        }
        cout << endl;
    }
}
```

> 练习11.34：如果你将transform函数中的find替换为下标运算符，会发生什么情况？

如果被查找的单词不在map中，会尝试向map中添加一个新的键值对{word,""}，但是map被声明为const，不能被修改，所有编译出错。

> 练习11.35：在buildMap中，如果进行如下改写，会有什么效果？
>
> ```C++
> trans_map[key] = value.substr(1);
> 改为trans_map.insert({key,value.substr(1)});
> ```

在map中已经存在当前key的情况下，使用下标运算会在map中保存最后一次赋值的value.substr(1)，但是使用insert的话，会保存第一次输入的value.substr(1)。

> 练习11.36：我们的程序并没检查输入文件的合法性。特别是，它假定转换规则文件中的规则都是有意义的。如果文件中的某一行包含一个关键字、一个空格，然后就结束了，会发生什么？预测程序的行为并进行验证，再与你的程序进行比较。

原程序会将单词替换成空。

> 练习11.37：一个无序容器与其有序版本相比有何优势？有序版本有何优势？

无序版本的优势：

- 关键字之间没有顺序关系的时候只能使用无序版本，无法使用有序版本。
- 维护关键字之间的有序性消耗比较大的情况下，使用无序版本，而不是用有序版本。

有序版本的优势：

- 有序版本可以通过关键字的顺序来对容器进行顺序访问。
- 可以通过自定义类的关键字之间的比较方法来定义有序容器。

> 练习11.38：用unnordered_map重写单词计数程序与单词转换程序。

```C++
#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <unordered_map>
#include <stdexcept>

using std::cout;
using std::endl;
using std::ifstream;
using std::istringstream;
using std::unordered_map;
using std::runtime_error;
using std::string;

// 建立转换映射
unordered_map<string, string> buildMap(ifstream &map_file)
{
    unordered_map<string, string> transformation_map;
    string key;
    string value;
    // 读取第一个单词存入key，行中剩余部分存入value
    while (map_file >> key && getline(map_file, value))
    {
        if (value.size() > 1) // 存在要转换成的词汇
        {
            transformation_map[key] = value.substr(1).substr(0,value.find_last_not_of(' '));
        }
        else
        {
            throw runtime_error("no rule for " + key);
        }
    }
    return transformation_map;
}

// 生成转换文本
const string & transform(const string &s,const map<string, string> transformation_map)
{
    auto iter = transformation_map.find(s);
    if(iter != transformation_map.end())
    {
        return iter->second;
    }
    else
    {
        return s;
    }
    
}

void word_transformation(ifstream &map_file, ifstream &input)
{
    auto transformation_map = buildMap(map_file); // 保存转换规则
    string text;                                  // 保存输入中的每一行
    while (getline(input, text))
    {
        istringstream strm(text);
        string word;
        while (strm >> word)
        {
            bool first_word = true;
            if (first_word)
                first_word = false;
            else
            {
                cout << " ";
            }
            cout << transform(word, transformation_map);
        }
        cout << endl;
    }
}
```

