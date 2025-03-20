 

一、了解
----

### 1、[unordered\_map](https://so.csdn.net/so/search?q=unordered_map&spm=1001.2101.3001.7020)(哈希)

`unordered_map`是借用[哈希表实现](https://so.csdn.net/so/search?q=%E5%93%88%E5%B8%8C%E8%A1%A8%E5%AE%9E%E7%8E%B0&spm=1001.2101.3001.7020)的关联容器。

*   访问键值对O（1），最坏情况O（n），例如哈希冲突严重时。【**n是一个哈希桶的元素数量**】
    
*   unordered\_map特性
    
    *   **键值对存储**：（key-value）每一个键对应一个值
    *   **无序：元素顺序取决于哈希函数和元素添加顺序**。
    *   **哈希表表现**：哈希表实现。键用哈希函数生成对应的索引。
    *   **自定义哈希函数和相等函数**：可以自己定义函数。
    *   unordered\_map 的迭代器也是 **前向迭代器**，因此它**只能支持前向遍历**（**即只能使用 ++ 操作，不能使用 – 操作**）。
*   unordered\_map 性能
    
    *   **哈希冲突解决方法**：链表或其他数据结构解决冲突。
        
    *   如下图：  
        ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/99867e897f254b86b4e468a8a173e10c.png)  
        ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8fbff7cdf41d4febb87bfa375dfb99ff.png)
        
    *   **负载因子和重哈希**：
        
        *   **负载因子**：已存储元素数量 / 桶的总数量。。【一般为 1 】**触发哈希表扩容**（rehash）。
        *   **重哈希**：当加载因子超过要求，就要重新分配元素并增加哈希桶数量。以保持高效性。
    *   **内存开销**：哈希表需要**额外内存管理桶**，可能比红黑树占用**更多总内存**。
        

> *   ### 常见问题：
>     
> *   ### 1、如何处理unordered\_map中的哈希冲突？
>     
>     *   unordered\_map处理哈希冲突的一种常见方法是**链地址法**，即在冲突发生时，所有具**有相同哈希值的元素会被存储在同一个哈希桶的链表中**。当查找一个键时，首先会使用**哈希函数计算其哈希值**，**定位到对应的桶**，**然后**通过遍历**链表来查找具有相应键的元素**。
> *   ### 2、unordered\_map的性能瓶颈在哪里？
>     
>     *   重哈希操作成本很高。如果使用的负载因子超出要求。发生重哈希，就容易出现瓶颈。
> *   ### 3、如何优化性能瓶颈？
>     
>     *   可以自定义哈希函数，使用质量更好的哈希函数，减少哈希冲突。**负载因子低了会导致内存消耗大，负载因子打了就容易冲突**。所以，**当知道需要存储的元素时，提前使用reserve预分配空间，减少重哈希**。

*   使用的头文件

```
#include <unordered_map>
```

二、初始化
-----

> unordered\_map<KeyType, ValueType> myMap;  
> 键类型 KeyType：必须支持 < 运算符，或传入自定义比较函数。  
> 值类型 ValueType：任意类型（包括自定义类型）。

```cpp
int main(){
    pair<int,int>pair1={1,2};
    pair<int,int>pair2=make_pair(1,2);
    unordered_map<int ,int>unorderedmap1={{1,2},{1,2}};
    unordered_map<int ,int>unorderedmap2={pair1,pair2};
    unordered_map<int ,int>unorderedmap3(unorderedmap2);
    unordered_map<int ,int>unorderedmap4=unorderedmap3;
    unordered_map<int ,int>unorderedmap5{pair<int,int>(1,2)};
}

```

三、自定义哈希函数
---------

*   首先了解
*   负载因子（load factor）：已存储元素数量 / 桶的总数量。
    *   默认当负载因子超过 max\_load\_factor()（通常为 1.0）时触发哈希表扩容（rehash）。
*   调整桶的数量方法 ，如下：

```cpp
scores.rehash(50);      // 预分配至少容纳 50 个元素的桶
scores.reserve(100);    // 预分配至少 100 个元素的容量（更友好）
```

*   手动设置哈希函数 , 例如：

```cpp
// 示例：自定义类的哈希函数
struct Person {
    string name;
    int id;
};

// 定义哈希函数
struct PersonHash {
    size_t operator()(const Person& p) const {
        return hash<string>()(p.name) ^ hash<int>()(p.id);
    }
};

// 定义相等比较
struct PersonEqual {
    bool operator()(const Person& a, const Person& b) const {
        return a.name == b.name && a.id == b.id;
    }
};

// 使用自定义类型的 unordered_map
unordered_map<Person, int, PersonHash, PersonEqual> customMap;

```

四、常用函数
------

### 1、总结

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d6a50420c0a2444e8826bdc191a123d4.png#pic_center)

### 2、例子

*   首先是这里用的头文件

```cpp
#include <iostream>
#include<unordered_map>
#include <utility>
using namespace std;
```

#### 2.1、插入操作

*   insert({key-value})
    *   插入键值

```cpp
int main(){
    unordered_map<int,int>m;
    m.insert({1,2});
    for(auto i:m){
        cout<<i.first<<" "<<i.second<<" "<<endl;//1 2
    }
}
```

*   insert(pair)
    *   插入pair

```cpp
int main(){
    pair<int,int>p={1,2};
    unordered_map<int,int>m;
    m.insert(p);
    for(auto i:m){
        cout<<i.first<<" "<<i.second<<" "<<endl;//1 2
    }
}
```

*   insert(other\_unordered\_map\_first，other\_unordered\_map\_end)
    *   插入另一个哈希map

```cpp
int main(){
    unordered_map<int,int>m;
    unordered_map<int,int>tmp{{2,3},{2,3}};
    m.insert(tmp.begin(),tmp.end());
    for(auto i:m){
        cout<<i.first<<" "<<i.second<<" "<<endl;//2 3
    }
}
```

*   inserrt(pos , {key-value})
    *   在pos插入键值

```cpp
int main(){
    unordered_map<int,int>m={{1,2}};
    m.insert(m.begin(),{3,4});
    for(auto i:m){
        cout<<i.first<<" "<<i.second<<" "<<endl;
        //3 4
        //1 2
    }
}
```

#### 2.2、删除操作

*   erase(first , end)
    *   删除当前这个map 在这个范围内的键值对

```cpp
int main(){
    unordered_map<int,int>m={{1,2},{2,3},{3,4}};
    m.erase(m.begin(),++m.begin());
    for(auto i:m){
        cout<<i.first<<" "<<i.second<<" "<<endl;
        //2 3
        //1 2
    }
}
```

*   erase(pos)
    *   删除pos的键值对

```cpp
int main(){
    unordered_map<int,int>m={{1,2},{2,3},{3,4}};
    m.erase(m.begin());
    for(auto i:m){
        cout<<i.first<<" "<<i.second<<" "<<endl;
        //2 3
        //1 2
    }
}
```

#### 2.3、访问操作

*   \[key\]运算符
    *   查key对应的值

```cpp
int main(){
    unordered_map<int,int>m={{1,2},{2,3},{3,4}};
    cout<<m[1]<<endl;//2
    
}
```

*   at(key)
*   查key对应的值

```cpp
int main(){
    unordered_map<int,int>m={{1,2},{2,3},{3,4}};
    cout<<m.at(1)<<endl;//2

}
```

*   begin()
    *   返回第一个

```cpp
int main(){
    unordered_map<int,int>m={{1,2},{2,3},{3,4}};
    cout<<m.begin()->first<<endl;//3
    cout<<m.begin()->second<<endl;//4
}

```

*   end()
    *   返回最后一个

```cpp
int main(){
    unordered_map<int,int>m={{1,2},{2,3},{3,4}};
    cout<<m.end()->first<<endl;//
    cout<<m.end()->second<<endl;//
}
```

#### 2.4、查询操作

*   find(key)
    *   找key键值的位置

```cpp
int main(){
    unordered_map<int,int>m={{1,2},{2,3},{3,4}};
    auto i =m.find(1);
    cout<<i->first<<endl; //1
    cout<<i->second<<endl;//2
}
```

*   count(key)
    *   找key的键值数量

```cpp
int main(){
    unordered_map<int,int>m={{1,2},{2,3},{3,4}};
    auto i =m.count(1);
    cout<<i<<endl; //1

}
```

#### 2.5、容量操作

*   size()
    *   查找map的数量

```cpp
int main(){
    unordered_map<int,int>m={{1,2},{2,3},{3,4}};
    int num = m.size();
    cout<<num<<endl; //3

}
```

*   empty
    *   当前map是否为空

```cpp
int main(){
    unordered_map<int,int>m={{1,2},{2,3},{3,4}};
    if(m.empty()==1){
        cout<<"m是空的"<<endl;
    }else{
        cout<<"m不是空的"<<endl;
    }
    //m不是空的

}
```

#### 2.6、交换操作

*   swap(other\_unordered\_map)
    *   交换2个map

```cpp
int main(){
    unordered_map<int,int>m={{1,2},{2,3},{3,4}};
    unordered_map<int,int>s={{3,4}};
    m.swap(s);

    for(auto i:m){
        cout<<i.first<<" "<<i.second<<" "<<endl;
        //3 4
    }
}
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/146331662?spm=1001.2014.3001.5502>，如有侵权，请联系删除。