 

一、了解
----

`multimap`是一个**允许键（key）重复**的[关联容器](https://so.csdn.net/so/search?q=%E5%85%B3%E8%81%94%E5%AE%B9%E5%99%A8&spm=1001.2101.3001.7020)。适合用于**一对多的更新**。 允许多个键拥有相同的值。基于`红黑树`。

*   multimap特性
    *   **键允许重复**：允许多个键有相同的值。
    *   **无 \[ \] 运算法**：禁止用 下标访问，因为键不唯一。
    *   **排序**：默认升序规则，可以自定义。
    *   **性能**：基于红黑树的实现。
    *   **时间复杂度**：插入 / 删除 / 查找是**O（logn）**
    *   **不支持直接修改键**：键是排序好的。直接修改会改变顺序。如果要修改，**先删除要修改的键，再插入新元素**。

> *   ### 常见问题
>     
> *   ### 1、如何在 multimap中搜索一个特定键对应的所哟值？
>     
> *   使用`equal_range`获得一个迭代器，他包含给定键的第一个元素和超过最后一个元素的迭代器。遍历这个范围来找到相对应的值。
> *   ### 2、如何从 multimap 中删除一个特定的键值对？
>     
> *   使用`erase`找到特定的键值对。键是排序好的。直接修改会改变顺序。如果要修改，**先删除要修改的键，再插入新元素**
> *   ### 3、在 multimap 中插入数据有什么特别之处？
>     
>     *   不需要检查键是否已经存在。由于 multimap 允许键的重复，插入操作会简单地添加一个新的键值对到容器中，即使该键已经有一个或多个关联值。您可以使用 insert 执行插入操作。
> *   ### 4、multimap 是如何保证元素顺序的？
>     
>     *   使用平衡二叉树红黑树来存储元素。

*   使用的头文件

```
#include < map >
```

二、初始化
-----

> multimap<KeyType, ValueType> myMap;  
> 键类型 KeyType：必须支持 < 运算符，或传入自定义比较函数。  
> 值类型 ValueType：任意类型（包括自定义类型）。

```cpp
int main(){
    pair<int,int>pair1={1,2};
    pair<int,int>pair2=make_pair(1,2);
    multimap<int ,int>multimap1={{1,2},{1,2}};
    multimap<int ,int>multimap2={pair1,pair2};
    multimap<int ,int>multimap3(multimap2);
    multimap<int ,int>multimap4=multimap3;
    multimap<int ,int>multimap5{pair<int,int>(1,2)};
}
```

*   自定义排序

```cpp
// 自定义键类型的比较规则（假设 Key 是结构体）
struct Key { int id; };
struct Compare {
    bool operator()(const Key& a, const Key& b) const {
        return a.id < b.id;
    }
};
multimap<Key, string, Compare> customMultimap;
```

三、常见使用函数
--------

### 1、总结

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/32c0f9fd799f4704807faa5c445d4fcc.png#pic_center)

### 2、例子

*   首先是这里用到的头文件

```cpp
#include<iostream>
#include<map>
#include<utility>
using namespace std;
```

#### 2.1、插入操作

*   insert(key-value)
    *   插入一个键值对

```cpp
int main(){
    multimap<int,int>m;
    m.insert(pair<int,int>(1,2));
    for(auto i:m){
        cout<<"first:"<<i.first<<" "<<"second:"<<i.second<<endl;
        //first:1 second:2
    }
}
```

*   insert(first , end )
    *   插入另一个multimap的范围

```cpp
int main(){
    multimap<int,int>m;
    multimap<int,int>multimap1={{2,3},{1,2}};
    m.insert(multimap1.begin(),multimap1.end());
    for(auto i:m){
        cout<<"first:"<<i.first<<" "<<"second:"<<i.second<<endl;
        /*
        first:1 second:2
        first:2 second:3
        */
    }
    
}
```

*   insert({初始化列表})
    *   插入初始化列表

```cpp
int main(){
    multimap<int,int>m;
    multimap<int,int>multimap1={{2,3},{1,2}};
    m.insert({{1,2},{2,3}});
    for(auto i:m){
        cout<<"first:"<<i.first<<" "<<"second:"<<i.second<<endl;
        /*
        first:1 second:2
        first:2 second:3

        */
    }
}
```

*   insert(pos , {初始化列表})
    *   插入初始化列表值

```cpp
int main(){
    multimap<int,int>m;
    m.insert(m.begin(),{1,2});
    for(auto i:m){
        cout<<"first:"<<i.first<<" "<<"second:"<<i.second<<endl;
        /*
        first:1 second:2

        */
    }
}
```

#### 2.2、删除操作

*   erase(key)
    *   删除key的节点

```cpp
int main(){
    multimap<int,int>m={{1,2},{2,3}};
    m.erase(1);
    for(auto i:m){
        cout<<"first:"<<i.first<<" "<<"second:"<<i.second<<endl;
        /*
        first:2 second:3

        */
    }
}
```

*   erase(first ,end )
    *   删除范围内的multimap

```cpp
int main(){
    multimap<int,int>m={{1,2},{2,3}};

    m.erase(m.begin(),m.end());
    for(auto i:m){
        cout<<"first:"<<i.first<<" "<<"second:"<<i.second<<endl;
        //
    }
}
```

*   erase(pos )
    *   删除pos位置的节点

```cpp
int main(){
    multimap<int,int>m={{1,2},{2,3}};
    
    m.erase(m.begin());
    for(auto i:m){
        cout<<"first:"<<i.first<<" "<<"second:"<<i.second<<endl;
        //first:2 second:3
    }
}
```

#### 2.4、查询操作

*   ### equal\_range( key )
    

```cpp
int main(){
    multimap<int,int>m={{1,2},{2,3},{2,4}};
    auto j = m.equal_range(2);
    auto start = j.first;
    auto end = j.second;
    for(multimap<int,int>::iterator it=start;it!=end;it++){
        cout<<"first:"<<it->first<<" "<<"second:"<<it->second<<endl;
        //first:2 second:3
        //first:2 second:4
    }
}
```

*   find(key)
    *   返回找到的第一个位置的迭代器

```cpp
int main(){
    multimap<int,int>m={{1,2},{2,3},{2,4}};
    auto i = m.find(2);
    cout<<i->first<<" "<<i->second;
    // 2 3
}
```

*   count(key)
    *   计数这个key有多少个

```cpp
int main(){
    multimap<int,int>m={{1,2},{2,3},{2,4}};
    auto i = m.count(2);
    cout<<i;
    //2
}
```

#### 2.5、容量操作

*   size()
    *   当前元素数量

```cpp
int main(){
    multimap<int,int>m={{1,2},{2,3},{2,4}};
    int num = m.size();
    cout<<num<<endl;//3
}
```

*   empty()
    *   当前multimap是否为空

```cpp
int main(){
    multimap<int,int>m={{1,2},{2,3},{2,4}};
    if(m.empty()==1){
        cout<<"m是空的"<<endl;
    }else{
        cout<<"m不是空的"<<endl;
    }
    //m不是空的
}
```

#### 2.6、交换操作

*   swap(other\_multimap)
    *   交换2个multimap

```cpp
int main(){
    multimap<int,int>m1={{1,2},{2,3},{2,4}};
    multimap<int,int>m2={{2,3}};
    m1.swap(m2);
    auto i = m1.begin();
    cout<<"second :"<<i->second<<endl;
    cout<<"first : "<<i->first;
    //second :3
    //first : 2
}
```

#### 2.7、访问操作

*   迭代器访问
    *   迭代器访问。存在双向迭代器
    *   一般用正向迭代器就够用了。

```cpp
int main(){
    multimap<int,int>m={{1,2},{2,3},{2,4}};
    auto i = m.begin();
    auto j =m.end();
    cout<<"begin :"<<i->first<<endl;
    cout<<"end: "<<j->first;
    //begin :1
    //end: 3
}
```

#### 2.8、lower\_bound(key)

*   lower\_bound(key)
    *   返回第一个比大于等于(>=)key的迭代器

```cpp
int main(){
    multimap<int,int>m={{1,2},{2,3},{2,4}};
    auto i = m.lower_bound(1);
    cout<<i->first<<" "<<i->second;//1 2
}
```

#### 2.9、upper\_bound(key)

*   upper\_bound(key)
    *   返回第一个大于（>） key的迭代器

```cpp
int main(){
    multimap<int,int>m={{1,2},{2,3},{2,4}};
    auto i = m.upper_bound(1);
    cout<<i->first<<" "<<i->second;//2 3
}
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/146332463?spm=1001.2014.3001.5501>，如有侵权，请联系删除。