 

一、了解
----

`unordered_set`是一个**无序关联容器**，其内部基于**哈希表**实现。**核心特性是 唯一性、无序性 和 快速查找**。

*   核心特性
    *   **元素的唯一性**
        *   与 std::set 类似，**不允许多个相同元素**（需唯一性确保的容器）。
    *   **无序存储**
        *   元素**不按特定顺序排列，存储顺序由哈希函数决定**。
    *   **底层实现**
        *   **基于哈希表**，查找、插入、删除的平均时间复杂度为 **O(1)**（**最坏情况 O(n)**）。
    *   **哈希依赖**
        *   需要为键类型提供哈希函数（内置类型自动支持，**自定义类型需手动定义**）。
        *   **不可修改值**
            *   如果修改值，会影响哈希。

> *   ### 常见问题
>     
> *   ### 1、如何处理 std::unordered\_set 中的冲突？
>     
>     *   **链地址法处理哈希冲突**。[可以看一下我之前写的unordered\_map文章。如何处理冲突的部分](https://blog.csdn.net/2401_82911768/article/details/146331662?spm=1001.2014.3001.5501)当两个元素的**哈希值相同并映射到同一存储位置时**，这些元素会被存储在**同一存储桶中的链表**里。查找元素时，首先**计算其哈希值**，然后**遍历对应存储桶中的链表**。
> *   ### 2、unordered\_set的迭代器是什么类型？如何避免哈希表影响？
>     
>     *   **支持前向遍历（只能使用 + + 操作，不能使用 - - 操作）**。
>     *   可以使用**调整负载因子**，可以通过 `max_load_factor()` 调整负载因子，以减少扩容的频率。  
>         如果知道元素的大致数量，可以提前**分配足够的**桶，避免频繁扩容。使用 `reserve()` 预分配桶的数量。或者使用 std::set 替代。

*   unordered\_set使用的头文件

```cpp
#include <unordered_set>
```

二、初始化
-----

```
unordered_set<typename> name;//默认哈希函数构造
```

*   例子

```cpp
std::unordered_set<int> us1;                // 默认构造
std::unordered_set<std::string> us2 = {"apple", "banana", "cherry"};

```

三、自定义哈希构造
---------

*   对于自定义类型，需提供 **哈希函数** 和 **键相等比较** 的规则：

```cpp
struct Person {
    std::string name;
    int age;
};

// 自定义哈希函数（需进入 std 命名空间特例化）
namespace std {
template <>
struct hash<Person> {
    size_t operator()(const Person& p) const {
        return hash<string>()(p.name) ^ hash<int>()(p.age);
    }
};
} // namespace std

// 特例化相等运算符
bool operator==(const Person& a, const Person& b) {
    return a.name == b.name && a.age == b.age;
}

// 使用自定义类型的 unordered_set
std::unordered_set<Person> people;
people.insert({"Alice", 25});
people.insert({"Bob", 30});

```

三、常用函数
------

### 1、总结

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d605cbba0ec84e9eb1e0f44caf59b474.png#pic_center)

### 2、例子

#### 2.1、插入操作

*   insert(key)
    *   插入元素

```cpp
int main() {
    unordered_set<int>set1{1,2,3,4};
    set1.insert(5);
    for(auto i:set1){
        cout<<i<<" ";//5 4 3 2 1
    }
}
```

*   insert(pos , key)
    *   在pos插入key

```cpp
int main() {
    unordered_set<int>set1{1,2,3,4};
    set1.insert(set1.begin(),{5});
    for(auto i:set1){
        cout<<i<<" ";//5 4 3 2 1
    }
}
```

*   inserrt(other\_first , other\_end )
    *   插入另一个unordered\_set的范围

```cpp
int main() {
    unordered_set<int>set1{1,2,3,4};
    unordered_set<int>set2{5,6};
    set1.insert(set2.begin(),set2.end());
    for(auto i:set1){
        cout<<i<<" ";//5 6 4 3 2 1
    }
}
```

#### 2.2、删除操作

*   erase(pos)
    *   删除pos的值

```cpp
int main() {
    unordered_set<int>set1{1,2,4};
    set1.erase(set1.begin());

    for(auto i:set1){
        cout<<i<<" ";// 2 1
    }
}

```

*   erase(key)
    *   删除key

```cpp
int main() {
    unordered_set<int>set1{1,2,4};
    set1.erase(1);

    for(auto i:set1){
        cout<<i<<" ";//4 2
    }
}
```

*   erase(first , end )
    *   删除这个范围内的值

```cpp
int main() {
    unordered_set<int>set1{1,2,4};
    set1.erase(set1.begin(),set1.end());

    for(auto i:set1){
        cout<<i<<" ";//
    }
}
```

#### 2.3、访问操作

*   迭代器访问
    *   begin：返回第一个
    *   end：返回最后一个的后一位

```cpp
int main() {
    unordered_set<int>set1{1,2,4};
    auto i = set1.begin();
    auto j = set1.end();
    cout<<*i<<endl;//1
    cout<<*j<<endl;//这个值无法得到
   
}
```

#### 2.4、交换操作

*   swap(other\_unordered\_set)
    *   交换2个unordered\_set

```cpp
int main() {
    unordered_set<int>set1{1,2,3,4};
    unordered_set<int>set2{5,6};
    set1.swap(set2);
    for(auto i:set1){
        cout<<i<<" ";//6 5
    }
}
```

#### 2.5、查找操作

*   find(key)
    *   返回key位置的迭代器，没找到返回end()迭代器

```cpp
int main() {
    unordered_set<int>set1{1,2,4};
    auto i = set1.find(1);
    cout<<*i<<endl;//1
}
```

*   count(key)
    *   返回key的个数

```cpp
int main() {
    unordered_set<int>set1{1,2,3,4};
    int num = set1.count(1);
    cout<<num<<endl;//1
    
}
```

#### 2.6、容量操作

*   size()
    *   返回元素个数

```cpp
int main() {
    unordered_set<int>set1{1,2,3,4};
    int num = set1.size();
    cout<<num<<endl;//4
}
```

*   empty()
    *   检查容器是否为空

```cpp
int main() {
    unordered_set<int>set1{1,2,3,4};
    
    if(set1.empty()==0){
        cout<<"set1是有值的"<<endl;//set1是有值的
    }else{
        cout<<"set1是无值的"<<endl;
    }
    
}
```

#### 2.7、清空操作

*   clear()
    *   清空所有元素

```cpp
int main() {
    unordered_set<int>set1{1,2,3,4};
    set1.clear();
    for(auto i:set1){
        cout<<i<<" ";//
    }
}
```

#### 2.8、哈希操作（这里的s是unordered\_set的一个节点来算的）

*   bucket\_count()
    *   返回哈希表中桶（Bucket）的总数

```cpp
s.bucket_count();//返回
```

*   load\_factor()
    *   返回平均每个桶存放的元素数量（负载因子）

```cpp
s.load_factor();//返回
```

*   rehash(n)
    *   设置哈希表的桶数为 n，优化性能

```cpp
s.rehash(100);//哈希表桶设置为100
```

*   max\_load\_factor()
    *   调整负载因子

```cpp
s.max_load_facotr(0.75)//负载因子设置为0.75
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/146408882?spm=1001.2014.3001.5501>，如有侵权，请联系删除。