 

一、理解
----

`multiset`一个有序容器，允许**存储多个相同的元素**，并按照元素的值**进行排序**。**基于红黑树实现**。保证了时间复杂度比较低。

*   特性
    *   **时间复杂度**
        *   插入和删除、查找都是O(log n)
    *   **排序规则**
        *   默认按升序（std::less），支持自定义排序规则。。与set类似。
    *   **重复性**
        *   允许元素重复。相同值可以插多次。
    *   **不可直接修改元素值**
        *   元素为 const，需先删除再插入新值。
    *   **底层实现**
        *   通常基于红黑树（与 set 相同），插入、删除、查找的时间复杂度为 O(log n)。
    *   **内存开销**
        *   因为要**额外存储有序的元素**，就需要更多的内存了。

> *   ### 常见问题
>     
> *   ### 1、multiset和Set的区别是什么？
>     
>     *   可以存储元素，并能够快速检索。主要区别在于multiset允许存储重复的元素，而Set不允许重复元素。**意味着在multiset中，可以有多个相同的键值，每个键值都会被计数**。
> *   ### 2、如果要实现一个multiset，关键的操作有哪些？
>     
>     *   主要是插入、查找和删除操作。要注意处理重复的元素。除此之外，有清空操作，容量 操作等等。

*   multiset使用的头文件

```cpp
#include <set>  // 与 std::set 相同

```

二、初始化
-----

```
multiset<typename> name;//默认升序规则
multiset<typename , 自定义排序规则>//可以自定义排序规则
```

*   例子

```cpp
std::multiset<int> ms1;                 // 默认升序
std::multiset<int, std::greater<int>> ms2; // 降序

```

*   自定义排序规则

```cpp
struct Person {
    std::string name;
    int age;
};

// 按年龄排序（允许重复年龄）
struct CompareAge {
    bool operator()(const Person& a, const Person& b) const {
        return a.age < b.age;
    }
};

std::multiset<Person, CompareAge> people;
people.insert({"Alice", 25});
people.insert({"Bob", 25});  // 允许年龄相同的元素

```

三、常使用的函数
--------

### 1、总结

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/783af9ee1db747ffab5aa9e6a53e0db7.png#pic_center)

### 2、例子

首先这里使用的头文件

```cpp
#include<map>
#include<iostream>
using namespace std;
```

#### 2.1、插入操作

*   insert(key)
    *   插入元素

```cpp
int main() {
    multiset<int>multiset1={2,2,3,3,4,4};
    multiset1.insert(1);
    for(auto i :multiset1){
        cout<<i<<" ";//1 2 2 3 3 4 4
    }
}
```

*   insert(first ， end)
    *   另一个multiset的范围

```cpp
int main() {
    multiset<int>multiset1={2,2,3,3,4,4};
    multiset<int>multiset2={1,5,6};
    multiset1.insert(multiset2.begin(),multiset2.end());
    for(auto i :multiset1){
        cout<<i<<" ";//1 2 2 3 3 4 4 5 6
    }
}
```

*   insert(pos , key)
    *   在pos的迭代器位置插入key

```cpp
int main() {
    multiset<int>multiset1={2,2,3,3,4,4};
    multiset1.insert(multiset1.begin(),1);
    for(auto i :multiset1){
        cout<<i<<" ";//1 2 2 3 3 4 4
    }
}
```

#### 2.2、删除操作

*   erase(pos)
    *   删除特定迭代器位置

```cpp
int main() {
    multiset<int>multiset1={2,2,3,3,4,4};
    multiset1.erase(multiset1.begin());
    for(auto i :multiset1){
        cout<<i<<" ";//2 3 3 4 4
    }
}
```

*   erase(key)
    *   删除特定键值

```cpp
int main() {
    multiset<int>multiset1={2,2,3,3,4,4};
    multiset1.erase(2);
    for(auto i :multiset1){
        cout<<i<<" ";//3 3 4 4
    }
}

```

*   erase(first , end)
    *   删除当前multiset的范围

```cpp
int main() {
    multiset<int>multiset1={2,2,3,3,4,4};
    multiset1.erase(multiset1.begin(),++multiset1.begin());
    for(auto i :multiset1){
        cout<<i<<" ";//2 3 3 4 4
    }
}
```

#### 2.3、访问操作

*   迭代器访问
    *   begin：指向set第一个位置的指针
    *   end：指向最后一个元素之后的位置

```cpp
int main(){
    multiset<int> multiset1={1,2,2,3,3,4};
    auto i =multiset1.begin();
    set<int>::iterator it=multiset1.end();
    cout<<*i<<endl;//1
    cout<<*it<<endl;//6
}
```

#### 2.4、交换操作

*   swap(other\_multiset)
    *   交换2个multiset

```cpp
int main(){
    multiset<int> multiset1={1,2,2,3,3,4};
    multiset<int> multiset2;
    multiset1.swap(multiset2);
    for(auto i:multiset1){
        cout<<i<<" ";//
    }
}
```

#### 2.5、查找操作

*   count(key)
    *   返回key值的个数

```cpp
int main(){
    multiset<int> multiset1={1,2,2,3,3,4};
    auto i =multiset1.count(1);
    cout<<i<<endl;//1
}
```

*   find(key)
    *   返回key位置的迭代器，没找到返回end()迭代器

```cpp
int main(){
    multiset<int> multiset1={1,2,2,3,3,4};
    if(multiset1.find(2)!=multiset1.end()){
        cout<<"找到了"<<endl;//找到了
    }
    auto i = multiset1.find(2);
    cout<<*i<<endl;//2
}
```

*   equal\_range(key)
    *   使用 equal\_range 获取重复元素的区间

```cpp
auto range = ms1.equal_range(2); // 返回一对迭代器 [start, end)
for (auto it = range.first; it != range.second; ++it) {
    std::cout << *it << " "; // 输出所有值为 2 的元素
}

```

#### 2.6、容量操作

*   size()
    *   返回元素个数

```cpp
int main(){
    multiset<int> multiset1={1,2,2,3,3,4};
    int num = multiset1.size();
    cout<<num<<endl;//6
    
}
```

*   empty()
    *   检查容器是否为空

```cpp
int main(){
    multiset<int> multiset1={1,2,2,3,3,4};
    if(multiset1.empty()==0){
        cout<<"multiset1有元素"<<endl;//multiset1有元素
    }else{
        cout<<"无元素"<<endl;
    }
}
```

#### 2.7、清空操作

*   clear（）
    *   清空所有元素

```cpp
int main(){
    multiset<int> multiset1={1,2,2,3,3,4};
    multiset1.clear();
    for(auto i:multiset1){
        cout<<i<<" ";//
    }
}
```

#### 2.8、lower\_bound和upper\_bound

*   lower\_bound(key)
    *   返回第一个不小于（>=） key 的迭代器

```cpp
int main(){
    set<int>set1={1,2,3,4};
    auto i = set1.lower_bound(2);
    cout<<*i<<endl;//2
}
```

*   upper\_bound(key)
    *   返回第一个大于 key 的迭代器

```cpp
int main(){
    set<int>set1={1,2,3,4};
    auto i = set1.upper_bound(2);
    cout<<*i<<endl;//3
}
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/146407430?spm=1001.2014.3001.5502>，如有侵权，请联系删除。