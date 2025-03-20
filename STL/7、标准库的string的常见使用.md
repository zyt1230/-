 

一、了解
----

`string`其实不完全算STL库中的。在 C++ 中，string 是标准库提供的用于处理动态字符序列的类（位于 头文件），相比 C 风格的字符数组（char\[\] 或 char\*），string 提供**更安全、更便捷的操作**。

*   使用的头文件

```cpp
#include<string>
```

二、初始化
-----

```
string name;//创建一个空的string
string name("数据");//创建一个字符串值为“数据”的string
string name="数据";//创建一个字符串值为“数据”的string
string name(n , 'C')//创建一个有n个字符‘C’的string
string name(other_string);//把other_string 的值拷贝到当前string
string name( other_string , pos , n_size_char );//另一个string的索引为pos到pos+n的值都拷贝到name中
string name{"初始化列表"};//name用初始化列表来初始化
string name={"初始化列表"};//name用初始化列表来初始化
```

*   举例

```cpp
#include <string>
using namespace std; // 或直接使用 std::string

string s1;               // 空字符串 ""
string s2("Hello");      // 从 C 字符串初始化：s2 = "Hello"
string s3 = "World";     // 等价于 s3("World")
string s4(5, 'a');       // 重复字符：s4 = "aaaaa"
string s5(s2);           // 拷贝构造：s5 = "Hello"
string s6(s3, 1, 3);     // 子串：从索引1取3字符 → s6 = "orl"
string s7{"1,2,3"};      //s7值为“1,2,3”
string s8={"faj"};       //s8值为"faj"
```

三、常见函数
------

### 1、总结

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f78affac4592442c9fadab2ad2dd6960.png#pic_center)

### 2、例子

首先这里使用的头文件

```cpp
#include<iostream>
#include<string>
using namespace std;
```

#### 2.1、访问操作

##### 2.1.1、迭代器访问

*   begin()
    *   迭代器理解成指针
    *   begin是指向**头部的数据**

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    string::iterator it =s1.begin();
    cout<<*it<<endl;//h
}
```

*   end()
    *   迭代器理解成指针
    *   end()是指向**尾部的后一个位置**【目前为止，写的7篇文章，只有list不是尾部的后一个位置】

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    string::iterator it = s1.end();
    cout<<*(--it);//o
    //--的意思就是把end()指针向后移一位，移到了尾部数据o的位置
}
```

##### 2.1.2、其他访问操作

*   string\[ i \]
    *   可以返回第 i 个位置的下标的**字符值**

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    char num = s1[0];
    cout<<s1[0]<<endl;//h
    cout<<num<<endl;//h
}

```

*   at( i )
    *   可以返回第 i 个位置的**字符**，并且at 会有**越界提醒**。

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    char num = s1.at(1);
    cout<<s1.at(1)<<endl; //e
    cout<<num<<endl;      //e
}
```

*   front( )
    *   返回头部**字符值**

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    char num = s1.front();
    cout<<s1.front()<<endl;//h
    cout<<num<<endl;//h
}
```

*   back( )
    *   返回尾部**字符值**

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    char num = s1.back();
    cout<<s1.back()<<endl;//o
    cout<<num<<endl;//o
}
```

*   data()
    *   打印全部**字符串值**，**包括括号**。

```cpp
int main(int argv,char* argc[]){
    string s1("hello world");
    cout<<s1.data()<<endl;//hello world
}

```

#### 2.2、插入操作

*   “+”
    *   把当前字符串和另一个字符串可以相加起来

```cpp
int main(int argv,char* argc[]){
    string s1("hello world");
    string s2("read");
    s1+=s2;
    cout<<s1<<endl;//hello worldread
    s1+="123";
    cout<<s1<<endl;//hello worldread123
}
```

*   push\_back(‘**C**’)或者push\_back(char\_object)
    *   插入一个字符

```cpp
int main(int argv,char* argc[]){
    string s1("hello world");
    char c = '2';
    s1.push_back('1');
    cout<<s1<<endl;//hello world1
    s1.push_back(c);
    cout<<s1<<endl;//hello world12
}
```

*   append(“字符串”)或者append(other\_string)
    *   在当前string后面加**字符串**

```cpp
int main(int argv,char* argc[]){
    string s1("hello world");
    string s2("read");
    s1.append(s2);
    cout<<s1<<endl;//hello worldread
    s1.append("123");
    cout<<s1<<endl;//hello worldread123
}
```

*   insert(pos , other\_string\_first , other\_string\_end )
    *   在pos的位置插入其他string的范围内字符串
    *   这里放的是**迭代器**。

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    string s2(" world");
    s1.insert(s1.end(),s2.begin(),s2.end());
    cout<<s1<<endl;//hello world
}
```

*   insert( pos , ‘**C**’)
    *   在pos的位置插入字符
    *   pos是**迭代器**

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    s1.insert(s1.end(),'1');
    cout<<s1<<endl;//hello1
}

```

*   insert( pos , n , ‘C’)
    *   在pos插入n个字符 ‘C’
    *   pos可以是**下标或迭代器**

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    s1.insert(0,10,'1');
    cout<<s1<<endl;//hello1111111111
}
```

*   insert（p，{初始化列表}）
    *   在**下标p**处插入字符串【初始化列表】

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    s1.insert(0,{"123"});
    cout<<s1<<endl;//123hello
}
```

#### 2.3、删除操作

*   pop\_back()
    *   删除尾部

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    s1.pop_back();
    cout<<s1<<endl;//hell
}
```

*   erase(pos或下表)
    *   删除**第p【下标】个位置以后得所有字符**
    *   删除pos迭代器的字符

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    s1.erase(1);
    cout<<s1<<endl;//h
    
}
```

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    s1.erase(++s1.begin());
    cout<<s1<<endl;//hllo

}
```

*   erase(other\_string\_first , other\_string\_end )
    *   删除first到end的值

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    s1.erase(s1.begin(),s1.end());
    cout<<s1<<endl;//

}
```

*   erase( p , n )
    *   删除下标p及其以后的n-1个值

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    s1.erase(0,2);
    cout<<s1<<endl;//llo

}
```

#### 2.4、比较操作

*   “> < =”
    *   直接用2个字符串比较

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    string s2("world");
    if(s1<s2){
        cout<<"true";
    }else{
        cout<<"false";
    }
    //true
}
```

*   compare(other\_string)
    *   当前字符串与另一个string比较

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    string s2("world");
    if(s1.compare(s2)){
        cout<<"true";
    }else{
        cout<<"false";
    }
    //true
}
```

*   compare(p , n , other\_string )
    *   当前字符从下标p开始往后n个值与另一个string比较

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    string s2("world");
    if(s1.compare(0,1,s2)){//h<world
        cout<<"true";
    }else{
        cout<<"false";
    }
    //true
}
```

*   compare(p1 , n1 , other\_string ,p2 , n2)
    *   当前字符从下标p开始往后n个值与另一个string从下标p开始往后n个值比较

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    string s2("world");
    if(s1.compare(0,1,s2,0,1)){//h w
        cout<<"true";
    }else{
        cout<<"false";
    }
    //true
}
```

#### 2.5、容量操作

*   length和size
    *   都是返回当前数据数量

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    string s2("world");
    int num1 = s1.size();
    int num2 = s2.length();
    cout<<num1<<endl;//5
    cout<<num2<<endl;//5
}
```

*   reserve( n )
    *   预分配内存

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    s1.reserve(10);
    int num1 = s1.size();
    cout<<num1<<endl;//5
   
}
```

*   shrink\_to\_fit()
    *   减少内存

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    s1.reserve(10);
    int num1 = s1.size();
    cout<<num1<<endl;//5
    s1.shrink_to_fit();
}
```

#### 2.6、子串操作

*   substr( p , n )
    *   \[**必须用一个新的string获取子串**\] 返回当前string从pos到pos+n位置的子串

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    string s2 =s1.substr(0,1);
    cout<<s1<<endl;//hello
    cout<<s2<<endl;//h
}
```

#### 2.7、查找操作

*   find(“字符串”)
    *   返回出现的位置，没有返回 -1
    *   string::npos来查找是否找到。

```cpp
size_t pos = s2.find("ll");    // 返回首次出现的位置 → 2
if (pos != string::npos) {     // npos表示未找到（值为-1）
    cout << "Found at " << pos;
}

pos = s2.rfind("l");           // 反向查找 → 3

```

```cpp
string text = "C++ is powerful. C++ is fast.";

// 查找 "C++" 的第一次出现
size_t first = text.find("C++");          // first = 0

// 从索引 5 开始查找 "C++" 的下一次出现
size_t next = text.find("C++", first + 1); // next = 14

```

#### 2.8、替换操作

*   replace(p , n1 , “字符串”)
    *   从下标 p 开始的n1个字符替换成 “字符串”

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    string s2 =s1.substr(0,1);
    s1.replace(1,2,"1");
    cout<<s1<<endl;//h1lo
}
```

#### 2.9、交换操作

*   swap(other\_string)
    *   当前string和另一个string交换

```cpp
int main(int argv,char* argc[]){
    string s1("hello");
    string s2 =s1.substr(0,1);
    s1.swap(s2);
    cout<<s1<<endl;//h
}
```

### 3、空格输入

*   getline(cin ,string )

```cpp
int main(int argv,char* argc[]){
    string string1={"423 42"};
    string string2;
    cout<<string1<<endl;//423 42
    getline(cin,string2);//fsd sdf
    cout<<string2<<endl;//fsd sdf
}
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/146269559?sharetype=blogdetail&sharerId=146269559&sharerefer=PC&sharesource=2401_82911768&spm=1011.2480.3001.8118>，如有侵权，请联系删除。