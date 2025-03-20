 

一、了解
----

set 是 C++ 标准模板库（STL）中提供的有序关联容器之一。基于红黑树（Red-Black Tree）实现，用于存储一组唯一的元素，并按照元素的值进行排序。

*   set的特性
    *   **唯一性**
        *   键是唯一的。无重复。
    *   **有序性**
        *   按**升序排列**，因为红黑树的平衡性质。可以**自定义排序规则**。
    *   **插入元素**
        *   如果**元素已存在，就不会进行了**。
    *   **不可修改键值**
        *   键值都是const的。如果要修改，先删除要修改的元素，再插入要修改后的值。

> *   ### 常见问题
>     
> *   ### 1、如何检查 std::set 中是否存在某个值？
>     
>     *   使用`find`，知道，返回值的迭代器，否则返回`end()`  
>         `if (mySet.find(key) != mySet.end()){函数体}`可以用来遍历
> *   ### 2、set的迭代器是什么类型
>     
>     *   `双向迭代器` 。不能随机访问，可以向把迭代器进行后-- 或向前++。
> *   ### 3、set的迭代器失效？
>     
>     *   set主要`与删除有关`，被删除的元素的迭代器会失效。但是，由于红黑树的性质，删除操作不会影响到其他元素的迭代器，所以除了被删除元素的迭代器外，其他迭代器仍然有效。

*   set的头文件

```cpp
#include<set>
```

二、初始化
-----

```
set<typename>name
set<typename,自定义排序规则>name
```

*   例子

```cpp
std::set<int> s1;                   // 默认升序：{1, 2, 3}
std::set<int, std::greater<int>> s2; // 降序：{3, 2, 1}

```

*   自定义排序

```cpp
struct Person {
    std::string name;
    int age;
};

// 定义比较仿函数（按年龄升序）
struct CompareAge {
    bool operator()(const Person& a, const Person& b) const {
        return a.age < b.age;
    }
};

std::set<Person, CompareAge> peopleSet;
peopleSet.insert({"Alice", 25});
peopleSet.insert({"Bob", 30});

```

三、常用函数
------

### 1、总结

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f56355228cb442c6812b40d1f9ad4635.png#pic_center)

### 2、例子

*   首先是这里用到的头文件

```cpp
#include <iostream>
#include<set>
using namespace std;
```

#### 2.1、插入操作

*   insert(x)
    *   插入某一个常数或变量

```cpp
int main(){
    set<int>set1={1,2,3,4};
    set1.insert(3);
    for(auto i: set1){
        cout<<i<<" "<<endl;
    }
/*
1
2
3
4 
 */
}
```

*   insert( other\_set\_first, other\_set\_end)
    *   插入另一个set的范围

```cpp
int main(){
    set<int>set1={1,2,3,4};
    set<int>set2={7,6,5};
    set1.insert(set2.begin(),set2.end());
    for(auto i: set1){
        cout<<i<<" ";
    }
/*
1 2 3 4 5 6 7
 */
}

```

*   insert(pos , x)
    *   在当前set的pos处，插入x

```cpp
int main(){
    set<int>set1={1,2,3,4};
    set<int>set2={7,6,5};
    set1.insert(set1.begin(),2);
    for(auto i: set1){
        cout<<i<<" ";
    }
/*
1 2 3 4
 */
}
```

#### 2.2、删除操作

*   erase(pos)
    *   删除某一个迭代器位置

```cpp
int main(){
    set<int>set1={1,2,3,4};
    set1.erase(set1.begin());
    for(auto i: set1){
        cout<<i<<" ";
    }
/*
2 3 4
 */
}
```

*   erase(key)
    *   删除特定键值

```cpp
int main(){
    set<int>set1={1,2,3,4};
    
    set1.erase(2);
    for(auto i: set1){
        cout<<i<<" ";
    }
/*
1 3 4
 */
}
```

*   erase(first , end)
    *   删除当前set的范围

```cpp
int main(){
    set<int>set1={1,2,3,4};

    set1.erase(set1.begin(),++set1.begin());
    for(auto i: set1){
        cout<<i<<" ";
    }
/*
2 3 4
 */
}
```

#### 2.3、访问操作

*   迭代器访问
    *   begin：指向set第一个位置的指针
    *   end：指向最后一个元素之后的位置

```cpp
int main(){
    set<int>set1={1,2,3,4};
    auto i =set1.begin();
    set<int>::iterator it=set1.end();
    cout<<*i<<endl;//1
    cout<<*it<<endl;//4
}
```

#### 2.4、交换操作

*   swap(other\_set)
    *   交换2个set

```cpp
int main(){
    set<int>set1={1,2,3,4};
    set<int>set2={3,4};
    set1.swap(set2);
    for(auto i: set1){
        cout<<i<<" ";//3 4
    }

}
```

#### 2.5、查找操作

*   count(key)
    *   返回key值的个数

```cpp
int main(){
    set<int>set1={1,2,3,4};
    auto i =set1.count(1);
    cout<<i<<endl;//1

}
```

*   find(key)
    *   返回key位置的迭代器

```cpp
int main(){
    set<int>set1={1,2,3,4};
    auto i =set1.find(1);
    cout<<*i<<endl;//1

}
```

#### 2.6、容量操作

*   size()
    *   返回元素个数

```cpp
int main(){
    set<int>set1={1,2,3,4};
    auto i =set1.size();
    cout<<i<<endl;//4

}
```

*   empty()
    *   检查容器是否为空

```cpp
int main(){
    set<int>set1={1,2,3,4};
   if(set1.empty()==0){
       cout<<"set1有元素"<<endl;//set1有元素
   }else{
       cout<<"无元素"<<endl;
   }
}
```

#### 2.7、清空操作

*   clear
    *   清空所有元素

```cpp
int main(){
    set<int>set1={1,2,3,4};
    set1.clear();
    for(auto i: set1){
        cout<<i<<" ";
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

四、set和map的区别
------------

*   **存储内存**
    *   set**只存储键值**
    *   map存储**键值对**
*   **唯一性**
    *   set的键是唯一的，无重复
    *   map键值对，键也是唯一的，但是**值可以重复**。
*   **访问方式**
    *   set通过迭代器遍历或查找键find(key),并且**不能修改键值**。
    *   map通过键直接访问值（m\[key\] 或 m.at(key)）

本文转自 <https://blog.csdn.net/2401_82911768/article/details/146383612?spm=1001.2014.3001.5502>，如有侵权，请联系删除。