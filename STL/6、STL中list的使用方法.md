 

一、了解
----

`list` 是[标准模板库](https://so.csdn.net/so/search?q=%E6%A0%87%E5%87%86%E6%A8%A1%E6%9D%BF%E5%BA%93&spm=1001.2101.3001.7020)（STL）提供的一个**双向链表容器**。它与 std::vector 或 std::array 不同，**不支持随机访问（即通过下标直接访问元素）\[只能用迭代器访问元素\]**，**但支持在任意位置的高效插入和删除操作**。

*   **特性**
    *   **双向链表**：允许2端插入和删除
    *   **不支持随机访问**：【vector和deque支持随机访问】访问list的元素只能用迭代器。
    *   **动态内存管理**：每个节点内存都有它前后2个节点的地址指针，在插入和删除中方便管理内存。
    *   **保持迭代器有效性**：在插入和删除里无迭代器失效。【除了：被删除元素的迭代器】
    *   **高效的插入和删除**：在2段和中间进行插入删除操作都很高效。
*   **性能考虑**
    *   它**适用于在插入和删除操作频繁**的情况。如果需要频繁的访问，vector和dequeue方便更多。
    *   **链表的结构，使得内存上的开销大于vector和dequeue**。  
        ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/75cbbf5d1f70490b93e21451ce97f12a.png)

> *   ### 常见问题
>     
>     *   ### 1、 删除 std::list 中的特定元素
>         
>         *   在C++中，std::**list 提供了 remove(val)** 成员函数来移除所有等于**特定值**的元素。  
>             ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e0d0c7244f594dccbfb1aabfaf9166e7.png)
>     *   ### 2、 list 的迭代器失效情况
>         
>         *   **插入操作**：在 list 中插入操作**不会导致任何现有迭代器失效**，包括指向插入位置的迭代器。插入操作后，原来的迭代器仍然指向它们原来指向的元素。
>         *   **删除操作**：删除操作会导致指向**被删除元素的迭代器失效**。然而，**其他迭代器**，包括指向前一个和后一个元素的迭代器，**仍然有效**。

*   使用的头文件

```cpp
#include<list>
```

二、初始化
-----

```
list<typename> name;//创建空链表
list<typename>name(n);//创建有n个节点的链表
list<typename>name(n,value);//创建有n个节点且每个节点值为value
list<typename>name(other_list);//创建的新链表与other_list的节点和值一样
list<typename>name={初始化列表};//创建一个值为初始化列表的链表
list<typename>name{初始化列表};创建一个值为初始化列表的链表
```

例子：

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3,4};//使用初始化列表
    list<int>list2{1,2,3,4};//使用初始化列表
    list<int>list3;//创建空链表
    list<int>list4(4);//创建有4个节点value为0的链表
    list<int>list5(4,1);//创建4个节点value为1的链表
    list<int>list6(list1);//list6跟list1值一样的链表
}
```

三、函数参数
------

### 1、总结

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2055f32529a84390bbe613644621f068.png#pic_center)

### 2、例子

```cpp
//这里使用到的头文件
#include<iostream>
#include<list>
#include<vector>
#include<queue>
using namespace std;
```

#### 2.1、访问操作

*   只能用迭代器访问。
*   list.end()指向的最后一个元素。而不是最后一个元素的后一个位置。

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3,4};
    cout<<*list1.begin()<<endl;//1
    cout<<*list1.end()<<endl;//4
    //注意
    vector<int>v{1,2,3,4};
    cout<<*v.end()<<endl;//越界
    deque<int>d{1,2,3,4};
    cout<<*d.end()<<endl;//越界
}
```

#### 2.2、插入操作

*   push\_front(val)
    *   在前端插入

```cpp
int main(int argv,char* argc[]){
    list<int>list1={0,0};
    list1.push_front(1);
    for(auto i:list1){
        cout<<i<<" ";//1 0 0 
    }
}
```

*   push\_back(val)
    *   在后端插入

```cpp
int main(int argv,char* argc[]){
    list<int>list1={0,0};
    list1.push_back(2);
    for(auto i:list1){
        cout<<i<<" ";//0 0 2
    }
}
```

*   insert(pos,val)
    *   在pos的位置插入val

```cpp
int main(int argv,char* argc[]){
    list<int>list1={0,0};
    list1.insert(list1.begin(),1);
    for(auto i:list1){
        cout<<i<<" ";//1 0 0 
    }
}
```

*   insert(pos,{val1,val2,val3…初始化列表})
    *   在 pos的位置插入初始化列表

```cpp
int main(int argv,char* argc[]){
    list<int>list1={0,0};
    list1.insert(list1.begin(),{1,2,3});
    for(auto i:list1){
        cout<<i<<" ";//1 2 3 0 0
    }
}
```

*   insert(pos , other\_list\_first ,other\_list\_end )
    *   在pos插入另一个list的first到end的值

```cpp
int main(int argv,char* argc[]){
    list<int>list1={0,0};
    list<int>list2={1,2};
    list1.insert(list1.end(),list2.begin(),list2.end());
    for(auto i:list1){
        cout<<i<<" ";//0 0 1 2
    }
}
```

*   insert(pos , n ,val )
    *   在pos 插入n个值value

```cpp
int main(int argv,char* argc[]){
    list<int>list1={0,0};
    list1.insert(list1.end(),10,1);
    for(auto i:list1){
        cout<<i<<" ";//0 0 1 1 1 1 1 1 1 1 1 1
    }
}
```

#### 2.3、删除操作

*   remove(val)
    *   删除所有值为 val 的元素

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3,0,0};
    list1.remove(0);
    for(auto i:list1){
        cout<<i<<" ";//1 2 3
    }
}
```

*   pop\_front()
    *   删除头首元素

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3,0,0};
    list1.pop_front();
    for(auto i:list1){
        cout<<i<<" ";//2 3 0 0
    }
}
```

*   pop\_back()
    *   删除队尾元素

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3,0,0};
    list1.pop_back();
    for(auto i:list1){
        cout<<i<<" ";//1 2 3 0
    }
}
```

*   unique()
    *   删除重复元素

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3,0,0};
    list1.unique();
    for(auto i:list1){
        cout<<i<<" ";//1 2 3 0
    }
}
```

*   erase(pos)
    *   删除pos位置的元素

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3,0,0};
    list1.erase(list1.begin());
    for(auto i:list1){
        cout<<i<<" ";//2 3 0 0
    }
}
```

*   erase(first , end )
    *   删除当前list特定范围内的元素

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3,0,0};
    list1.erase(list1.begin(),list1.end());
    for(auto i:list1){
        cout<<i<<" ";//
    }
}
```

#### 2.4、容量操作

*   size()
    *   返回当前list有的元素个数

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3,0,0};
    int num =list1.size();
    cout<<num<<endl;//5

}
```

*   resize(new\_size)
    *   把当前list的size改为new\_size的大小。多的元素就删除

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3,0,0};
    list1.resize(4);
    for(auto i:list1){
        cout<<i<<" ";//1 2 3 0
    }
}
```

*   resize(new\_size,value)
    *   把当前list的size改为new\_size的大小。多的就填加value

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3,0,0};
    list1.resize(6,1);
    for(auto i:list1){
        cout<<i<<" ";//1 2 3 0 0 1
    }
}
```

#### 2.5、合并操作

*   merge(other\_list)
    *   把另一个list和当前list的值合并。但是合并后的值顺序是不会自动排序的。

```cpp
int main(int argv,char* argc[]){
   list<int>list1={1,2,3};
   list<int>list2={6,4,5};
   list1.merge(list2);
   for(auto i:list1){
       cout<<i<<" ";//1 2 3 6 4 5
   }
}
```

*   splice(pos , other\_list)
    *   在当前list的pos位置插入另一个list。

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3};
    list<int>list2={6,4,5};
    list1.splice(list1.begin(),list2);
    for(auto i:list1){
        cout<<i<<" ";//6 4 5 1 2 3
    }
}
```

*   splice(pos , other\_list , i )
    *   把other\_list的第`i`位置的元素插入当前list的pos位置

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3};
    list<int>list2={6,4,5};
    list1.splice(list1.begin(),list2,list2.begin());
    for(auto i:list1){
        cout<<i<<" ";//6 1 2 3
    }
}
```

*   splice(pos , other\_list , other\_list\_first ,other\_list\_end )
    *   把other\_list的特定的某一段范围插入到pos里

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3};
    list<int>list2={6,4,5};
    list1.splice(list1.begin(),list2,list2.begin(),list2.end());
    for(auto i:list1){
        cout<<i<<" ";//6 4 5 1 2 3
    }
}
```

#### 2.6、清空操作

*   clear()
    *   清除所有元素

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3};
    list1.clear();
    for(auto i:list1){
        cout<<i<<" ";//
    }
}

```

#### 2.7、交换操作

*   swap(other\_list)
    *   把other\_list与当前list交换

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3};
    list<int>list2={4,5,6};
    list1.swap(list2);
    for(auto i:list1){
        cout<<i<<" ";//4 5 6
    }
}
```

#### 2.8、替换操作

*   assign（other\_list\_first ,other\_list\_end）
    *   把当前这个list替换成另一个list的范围内的值

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3};
    list<int>list2={4,5,6};
    list1.assign(list2.begin(), list2.end());
    for(auto i:list1){
        cout<<i<<" ";//4 5 6
    }
}
```

*   assign(n , val )
    *   把当前list替换成n个value

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3};
    list<int>list2={4,5,6};
    list1.assign(10,0);
    for(auto i:list1){
        cout<<i<<" ";//0 0 0 0 0 0 0 0 0 0
    }
}
```

*   assign({初始化列表})
    *   把当前list替换成这个初始化列表

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3};
    list1.assign({4,5,6});
    for(auto i:list1){
        cout<<i<<" ";//4 5 6
    }
}
```

#### 2.9、倒序操作

*   reverse()
    *   把list的值倒序

```cpp
int main(int argv,char* argc[]){
    list<int>list1={1,2,3,0,0};
    list1.reverse();
    for(auto i:list1){
        cout<<i<<" ";//0 0 3 2 1
    }
}
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/146256378?sharetype=blogdetail&sharerId=146256378&sharerefer=PC&sharesource=2401_82911768&spm=1011.2480.3001.8118>，如有侵权，请联系删除。