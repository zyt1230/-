 

一、了解
----

`vector`是可变长的数组（[动态数组](https://so.csdn.net/so/search?q=%E5%8A%A8%E6%80%81%E6%95%B0%E7%BB%84&spm=1001.2101.3001.7020)）。

C++中vector的**数组内存通常是在堆上分配的**。当创建一个vector对象时，**对象本身（即vector的控制结构，包括指向数据的指针、大小和容量等）通常存储在栈上**（如果是局部变量）或其他存储区（如全局/静态存储区），但实际的元素数据是在堆上分配的。

> ### 常见问题
> 
> *   ### 1、vector的扩容过程
>     
>     *   当向vector中添加元素时，如果当前元素数量（size()）即将超过预分配的内存容量（capacity()），则会触发扩容。
>     *   那么扩容的过程：
>         *   分配一个更大的内存块，通常是当前大小的两倍（这个**增长因子取决于实现**）。
>         *   将当前所有元素移到新分配的内存中。
>         *   销毁旧元素，并释放旧内存块。
>         *   插入新元素。
>     *   **这个过程中的复制和移动操作可能会导致性能开销**，尤其当元素具有**复杂的拷贝构造函数或移动构造函数**时。
> *   ### 2、解释 vector::push\_back 和vector::emplace\_back 的区别。
>     
>     *   这2个函数都是在vector末尾加元素。但是添加方式不同。
>         *   `push_back()`：**会**对给定的对象进行**拷贝或移动构造**，以将元素添加到 vector 的末尾。
>         *   `emplace_back()`：**不会**对给定的对象进行**拷贝或移动构造**，以将元素添加到 vector 的末尾。
> *   ### 3、什么时候会使用 std::vector::reserve()？
>     
>     *   `vector::reserve`**用于预分配内存**，**以避免在添加新元素时重新分配内存**。当知道将要存储大量元素，但又不想在每次插入时都可能发生内存重新分配时，使用 reserve() 可以提高性能。这样可以减少因扩容导致的不必要的内存分配和元素拷贝。
> *   ### 4、如何减少 std::vector 占用的空间？
>     
>     *   使用`std::vector::shrink_to_fit`方法**来请求移除未使用的容量**，减少 vector 的内存使用。这个函数是 C++11 引入的，**它会尝试压缩 std::vector 的容量，使其等于其大小。但是，这只是一个请求，并不保证容量会减少，因为这依赖于实现。**
> *   ### 5、如何检查 std::vector 是否为空？
>     
>     *   推荐使用`empty`函数，**它的时间复杂度O（1）且与list等等STL容器兼容**，比size和begin()=end()方便。
> *   ### 6、什么是迭代器失效? 如何避免?
>     
>     *   **如果 vector 进行了重新分配**，**所有指向元素的迭代器**都会失效。
>     *   **如果在 vector 中间插入或删除元素**，**从该点到末尾**的所有迭代器都会失效。  
>         \-推荐使用`remove( )或remove_if( )结合erase( )`方法来删除元素。

```
//使用的头文件是
#include<vector>
```

二、初始化
-----

> vector < [typename](https://so.csdn.net/so/search?q=typename&spm=1001.2101.3001.7020) > name;

### 1、一维初始化

*   空vector创建

```cpp
vector<int> v1;//定义了一个名为v1的一维数组,数组存储int类型数据
```

*   指定大小和初始化值

```cpp
vector<int> v3(10);//v3创建10个数据空间
vector<int> v4(10,1);// v4创建10个数据空间，并且每一个数据是1
```

*   初始化多个元素

```cpp
vector<int> v5{1,2,3,4};
```

*   拷贝初始化

```cpp
vector<int> v6(v5);
vector<int> v7=v6;
```

### 2、二维初始化

*   空的vector二维

```cpp
vector<vector<int>> v1;
```

*   push\_back赋值

```cpp
vector<int> v2(2,2);
    vector<vector<int>>v3;
    v3.push_back(v2);
    v3.push_back({1,3});
    for(const auto & v: v3){
        for(int num: v){
            printf("%d ",num);
        }
        printf("\n");
    }
    //打印结果
    //2 2
	//1 3
```

三、方法函数
------

### 1、总结

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d24aaf47bc834bc0acc82a744a2514cf.png#pic_center)

### 2、例子

首先是这里用到的头文件

```
#include<iostream>
#include<cstdio>
#include<vector>
using namespace std;
```

*   v.front（）

```cpp
vector<int>v{1,2,3,4};
    int num = v.front();
    printf("%d",num);//1
```

*   v.back()

```cpp
vector<int>v{1,2,3,4};
    int num = v.back();
    printf("%d",num);//4
```

*   v.at(index);

```cpp
vector<int>v{1,2,3,4};
int num =v.at(1);
printf("%d",num);//2
```

*   v.size()

```cpp
vector<int>v{1,2,3,4};
int num = v.size();
printf("%d",num);//4
```

*   v.capacity()

```cpp
vector<int>v{1,2,3,4};
int num = v.capacity();
printf("%d",num);//4
```

*   v.resize(new\_size)

```cpp
	//原来是4个数量
	//扩大到5个数量
	vector<int>v1{1,2,3,4};
    v1.resize(5);
    for(auto it1 :v1){
        printf("%d ",it1);//1 2 3 4 0
    }
    //缩小到2个数量
    vector<int>v2{1,2,3,4};
    v2.resize(2);
    for(auto it2 :v2){
        printf("%d ",it2);//1 2 
    }
```

*   v.resize(new\_size,value)

```cpp
vector<int>v3{1,2,3,4};
v3.resize(7,0);
for(auto it3 :v3){
	printf("%d ",it3);//1 2 3 4 0 0 0
}
```

*   v.empty()

```cpp
vector<int>v{1,2,3,4};
if(v.empty()== false){
    printf("不是空为 false\n");
}
if(v.empty()==0){
    printf("不是空为 0");
}
```

*   v.begin()

```cpp
vector<int>v{1,2,3,4};
vector<int>::iterator it1 =v.begin();
//或写成
auto it2=v.begin();
cout<<*it1<<endl;//1
cout<<*it2;//1
```

*   v.end()

```cpp
vector<int>v{1,2,3,4};
vector<int>::iterator it1 =v.end();
//或写成
auto it2=v.end();
cout<<*it1<<endl;//0
cout<<*it2;//0
```

*   v.reserve(new\_cap)

```cpp
vector<int>v;
v.reserve(1000);//获取了1000的内存
for(int i=0;i<1000;i++){
    v.push_back(i);//不会频繁的扩容
}
v.shrink_to_fit();//缩减大小
```

*   v.assign(n,val)

```cpp
vector<int>v={10,2,5,2};
v.assign(10,1);
for(auto i =v.begin();i!=v.end();i++)
{
    cout<<*i<<" ";//1 1 1 1 1 1 1 1 1 1
}
```

*   v.assign(first , end)

```cpp
vector<int>tmp{1,2,3,4,5};
vector<int>v={10,2,5,2};
v.assign(tmp.begin(),tmp.end());
for(auto i =v.begin();i!=v.end();i++)
{
    cout<<*i<<" ";//1 2 3 4 5
}
```

*   v.assign( initializer\_list i1)

```cpp
vector<int>v={10,2,5,2};
v.assign({1,2,3,4,5});
for(auto i =v.begin();i!=v.end();i++)
{
    cout<<*i<<" ";//1 2 3 4 5
}
```

*   v.push\_back(val)

```cpp
vector<int>v(3,0);
v.push_back(1);
for(auto i =v.begin();i!=v.end();i++)
{
    cout<<*i<<" ";//0 0 0 1
}
```

*   v.emplace\_back(val)

```cpp
vector<int>v(3,0);
v.emplace_back(1);
for(auto i =v.begin();i!=v.end();i++)
{
    cout<<*i<<" ";//0 0 0 1
}
```

*   v.pop\_back( )

```cpp
vector<int>v(3,0);
v.pop_back();
for(auto i =v.begin();i!=v.end();i++)
{
    cout<<*i<<" ";//0 0 
}
```

*   v.insert( )

```cpp
int main(){
    vector<int>v;//创建一个空数组
    v.insert(v.begin(),1);//在v.begin()的位置插入1
    for(auto i:v){
        cout<<i<<" ";//1
    }
    cout<<endl;
    v.insert(v.end(),{2,3});//在v.end()的位置插入列表{2,3}
    for(auto i:v){
        cout<<i<<" ";//1 2 3
    }
    cout<<endl;
    v.insert(v.begin(),1,0);//在v.begin()的位置插入1个0
    for(auto i:v){
        cout<<i<<" ";//0 1 2 3
    }
    cout<<endl;
    vector<int>tmp{10,20,30,40};
    v.insert(v.end(),tmp.begin(),tmp.end());
    for(auto i:v){
        cout<<i<<" ";// 0 1 2 3 10 20 30 40
    }
    cout<<endl;
}
```

*   v.erase( pos )

```cpp
vector<int>v;//创建一个空数组
v.push_back(1);
v.emplace_back(2);
v.erase(v.begin());
for(auto i:v){
    cout<<i<<" ";//2
}
cout<<endl;
```

*   v.erase(first , end)

```cpp
int main(){
    vector<int>v;//创建一个空数组
    v.push_back(1);
    v.emplace_back(2);
    
    v.erase(v.begin(),v.end());
    for(auto i:v){
        cout<<i<<" ";//
    }
}
```

三、访问
----

*   下标访问

```cpp
int main(){
    vector<int>v={1,2,3,4,5};
    cout<<v[0]<<endl;//1

}
```

*   迭代器访问

```cpp
int main(){
    vector<int>v={1,2,3,4,5};
    vector<int>::iterator it = v.end()-1;
    cout<<*it<<endl;//5

}
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/145947541?spm=1001.2014.3001.5502>，如有侵权，请联系删除。