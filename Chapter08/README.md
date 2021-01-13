> 练习8.1：编写函数，接受一个istream&参数，返回值类型也是istream&。此函数须从**给定流中**读取数据，直至遇见**文件结束标识时**停止。它将读取的数据打印在**标准输入**上。完成这些操作后，在返回流之前，**对流进行复位**，使其处于**有效状态**。

```C++
#include <iostream>

using std::istream;
using std::cout;
using std::endl;

istream &test(istream &is)
{
    char a;
    while (is >> a)
    {
        cout << a << endl;
    }
    is.clear();
    return is;
}
```

>练习8.2：测试函数，调用参数为cin。

```C++
#include <iostream>
using std::istream;
using std::cin;
using std::cout;
using std::endl;

istream &test(istream &);

int main()
{
    test(cin);
    return 0;

}

istream &test(istream &is)
{
    char a;
    while (is >> a)
    {
        cout << a << endl;
    }
    is.clear();
    return is;
}
```

>练习8.3：什么情况下，下面的while循环会终止？
>
>```C++
>while (cin >> i) /*...*/
>```

当输入流``cin`处于错误状态流的时候，比如`eofbit` 、`failbit`和`badbit`。

>练习8.4：编写函数，以读模式打开一个文件，将其内容读入到一个**string的vector**中，将**每一行**作为一个独立的元素存在于vector中。

```C++
#include <iostream>
#include <fstream>
#include <string>
#include <vector>

using std::cerr;
using std::cout;
using std::endl;
using std::fstream;
using std::string;
using std::vector;

int main()
{
    string file_path = "/home/lx/Desktop/C++/Chapter10_1/test";
    fstream fstrm(file_path);
    vector<string> file_contents;
    string file_line;
    if (fstrm)
    {
        while (getline(fstrm, file_line))
        {
            cout << file_line << endl;
            file_contents.push_back(file_line);
        }
        cout << file_contents.size() << endl;
    }
    else
    {
        cerr << "file path is wrong!!!" << endl;
        return -1;
    }

    return 0;
}
```

>练习8.5：重写上面的程序，将每个单词作为一个独立的元素进行存储。

```C++
#include <iostream>
#include <fstream>
#include <string>
#include <vector>

using std::cerr;
using std::cout;
using std::endl;
using std::fstream;
using std::string;
using std::vector;

int main()
{
    string file_path = "/home/lx/Desktop/C++/Chapter10_1/test";
    fstream fstrm(file_path);
    vector<string> file_contents;
    string file_word;
    if (fstrm)
    {
        while (fstrm >> file_word)
        {
            cout << file_word << endl;
            file_contents.push_back(file_word);
        }
        cout << file_contents.size() << endl;
    }
    else
    {
        cerr << "file path is wrong!!!" << endl;
        return -1;
    }

    return 0;
}
```

>练习8.6：重写7.1.1节的书店程序（第299页），从一个文件中读取交易记录。将文件名作为一个参数传递给main（参见6.2.5节，第196页）。

```C++
#include <iostream>
#include <fstream>

#include "Sales_data.h"

using std::ifstream;
using std::cout;
using std::endl;
using std::cerr;

int main(int argc, char** argv)
{
    ifstream input(argv[1]); 
    Sales_data total;
    if (read(input, total)) {
        Sales_data trans;
        while (read(input, trans)) {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else {
                print(cout, total) << endl;
                total = trans;
            }
        }
        print(cout, total) << endl;
    }
    else {
        cerr << "No data?!" << endl;
    }
}

```

> 练习8.7：修改上一节的书店程序，将结果保存到一个文件中。将输出文件名作为第二个参数传递给main函数。

```C++
#include <iostream>
#include <fstream>

#include "Sales_data.h"

using std::ifstream;
using std::ostream;
using std::cout;
using std::endl;
using std::cerr;

int main(int argc, char** argv)
{
    ifstream input(argv[1]); 
    ostream output(argv[2]);
    Sales_data total;
    if (read(input, total)) {
        Sales_data trans;
        while (read(input, trans)) {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else {
                print(output, total) << endl;
                total = trans;
            }
        }
        print(output, total) << endl;
    }
    else {
        cerr << "No data?!" << endl;
    }
}

```

>练习8.8：修改上一题的程序，将结果追加到给定的文件末尾。对同一个输出文件，运行程序至少两次，检验数据是否得以保留。

```C++
#include <iostream>
#include <fstream>

#include "Sales_data.h"

using std::ifstream;
using std::ostream;
using std::cout;
using std::endl;
using std::cerr;

int main(int argc, char** argv)
{
    ifstream input(argv[1]); 
    ostream output(argv[2], ostream::app);
    
    Sales_data total;
    if (read(input, total)) {
        Sales_data trans;
        while (read(input, trans)) {
            if (total.isbn() == trans.isbn())
                total.combine(trans);
            else {
                print(output, total) << endl;
                total = trans;
            }
        }
        print(output, total) << endl;
    }
    else {
        std::cerr << "No data?!" << std::endl;
    }
}

```

>练习8.9：使用你为8.1.2节（第281页）第一个练习所编写的函数打印一个istringstream对象的内容。

```C++
#include <iostream>
#include <sstream>
#include <string>

using std::cout;
using std::endl;
using std::istringstream;
using std::string;

istringstream &test(istringstream &stringstrm);

int main(int argc,char ** argv)
{
    istringstream stringstrm("hello");
    test(stringstrm);

    return 0;
}

istringstream &test(istringstream &stringstrm)
{
    string str;
    while (stringstrm >> str)
    {
        cout << str << endl;
    }
    stringstrm.clear();
    return stringstrm;
}
```

>练习8.10：编写程序，将来自一个文件中的行保存在一个vector<string>中。然后使用一个istringstream从vector读取数据元素，每次读取一个单词。

```C++
#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <vector>

using std::cerr;
using std::cout;
using std::endl;

using std::fstream;
using std::istringstream;

using std::string;
using std::vector;

int main(int argc, char **argv)
{
    fstream fstr(argv[1]);
    string file_line;
    vector<string> file_contents;
    if (!fstr)
    {
        cerr << "file is not exit!!!" << endl;
        return -1;
    }
    
    while (getline(fstr, file_line))
    {
        file_contents.push_back(file_line);
    }

    for (auto &str : file_contents)
    {
        istringstream istrstrm(str);
        string word;
        while (istrstrm >> word)
        {
            cout << word << endl;
        }
    }

    return 0;
}

```

>练习8.11：本节的程序在外层while循环中定义了一个istringstream对象。如果record对象定义在循环之外，你需要对程序进行怎样的修改？重写程序，将record的定义转移到while循环之外，验证你想的修改方法是否正确。

```C++
#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <vector>

using std::cerr;
using std::cout;
using std::endl;
using std::fstream;
using std::string;
using std::stringstream;
using std::vector;

struct PersonInfo
{
    string name;
    vector<string> phones;
};

int main(int argc, char **argv)
{
    string file_path(argv[1]);
    string file_line;
    string phone;

    vector<PersonInfo> all_person;
    fstream strm(file_path);
    if (!strm)
    {
        cerr << "file is not exit!!!" << endl;
        return -1;
    }
    stringstream record;
    while (getline(strm, file_line))
    {
        record.clear();
        record.str(file_line);
        PersonInfo person;
        record >> person.name;
        while (record >> phone)
        {
            person.phones.push_back(phone);
        }
        all_person.push_back(person);
    }
    for (auto &p : all_person)
    {
        cout << p.name << " ";
        for (auto &s : p.phones)
            cout << s << " ";
        cout << endl;
    }
    return 0;
}
```

在每次使用istringstream之前需要clear。

>练习8.12：我们为什么没有在PersonInfo中使用类内初始化？

因为这里的PersonInfo是一个聚合类，需要从文件中读取内容写入到PersonInfo中。

>练习8.13：重写本节的电话号码程序，从一个命名文件而非cin读取数据。

见练习8.11。

>练习8.14：我们为什么将entry和nums定义为const auto&？

1. 因为不会更改entry和nums的值，所以将其定义为`const`的。
2. entry和nums均是自定义的类类型，而不是内置的基本数据类型，所以将二者定义成引用可以避免变量的拷贝，有利于提升代码效率。