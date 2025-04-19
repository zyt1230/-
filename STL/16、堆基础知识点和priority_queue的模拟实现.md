 

一、priority\_queue的使用方法
----------------------

[priority\_queue的使用方法看这篇文章](https://blog.csdn.net/2401_82911768/article/details/146077378?spm=1001.2014.3001.5501)

二、堆
---

### 1、介绍

堆（Heap）是一种特殊的完全二叉树数据结构，满足以下性质：

*   堆序性质（Heap Property）：
    *   大顶堆（Max-Heap）：每个节点的值 ≥ 其子节点的值。
    *   小顶堆（Min-Heap）：每个节点的值 ≤ 其子节点的值。
    *   完全二叉树：**除最后一层外**，其他层节点**必须填满**，且**最后一层节点靠左排列**。  
        ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/be567fd4183e451bb5ea9951ab5c8d74.png)

### 2、存储方式

堆的存储方式  
堆通常用数组实现，利用完全二叉树的性质：

*   对于节点 i（从 0 开始）：
    *   父节点：(i - 1) / 2
    *   左子节点：2i + 1
    *   右子节点：2i + 2

### 3、堆的操作过程

*   堆的常用操作
    
    *   1.  插入元素（Push）
    *   步骤：
        *   将新元素添加到数组末尾。
        *   上浮（Percolate Up）：从该节点向上比较并交换，直到满足堆序性质。
    *   时间复杂度：O(log n)
*   2.  删除堆顶（Pop）
    
    *   步骤：
        *   交换堆顶与末尾元素，删除末尾。
        *   下沉（Percolate Down）：从新堆顶向下比较并交换，直到满足堆序性质。
    *   时间复杂度：O(log n)
*   3.  构建堆（Heapify）
    
    *   从**最后一个非叶子节点开始**，逐个**下沉调整**。
    *   时间复杂度：O(n)（非直觉的线性时间）

### 4、使用heap函数\[algorithm头文件\]

*   make\_heap()
    
    *   构建最大堆
*   push\_heap()
    
    *   先使用push\_back插入元素到末尾
    *   再使用push\_heap排序
*   pop\_heap()
    
    *   先使用pop\_heap把堆顶放到最后
    *   再用pop\_back()删除最后一个元素

```cpp
#include <algorithm>
#include <vector>

vector<int> v = {3, 1, 4, 1, 5};

// 构建大顶堆
make_heap(v.begin(), v.end()); // [5, 3, 4, 1, 1]

// 插入元素
v.push_back(6);
push_heap(v.begin(), v.end()); // [6, 3, 5, 1, 1, 4]

// 删除堆顶
pop_heap(v.begin(), v.end()); // 将堆顶移到末尾
v.pop_back(); // [5, 3, 4, 1, 1]
```

三、实现过程
------

### 1、构造

*   默认vector为底层容器

```cpp
	//默认构造函数
    priority_queue(){}
```

*   自定义容器

```cpp
    //构造函数
    priority_queue(const container& c):data(c){
        make_heap(data.begin(),data.end());
    }//可以指定容器

```

### 2、插入

*   在堆的最后插入新元素
*   插入完毕后，重新排序

```cpp
    //插入
    void push(const T& value){
        data.push_back(value);
        push_heap(data.begin(),data.end());
    }
```

### 3、删除

*   删除要先交换堆顶和堆最后一个元素
*   再进行删除最后一个元素
*   最后再次排序

```cpp
    //删除
    void pop(){
        if(!empty()){
            pop_heap(data.begin(),data.end());
            data.pop_back();
        }else{
            throw runtime_error("Priority queue is empty.");
        }
    }
```

### 4、查看

*   访问的堆顶值

```cpp
    T& top(){
        if(!empty()){
            return data.front();
        }
    }
```

### 5、是否为空

*   为空就是true
*   否则为false

```cpp
    bool empty() const{
        return data.empty();
    }
```

### 6、大小

*   返回的数据的个数

```cpp
    size_t size()const{
        return data.size();
    }
```

### 7、完整过程（大顶堆）

```cpp

template<class T,class container = vector<T>>
class priority_queue{
private:
    container data;
public:
    //默认构造函数
    priority_queue(){}
    //构造函数
    priority_queue(const container& c):data(c){
        make_heap(data.begin(),data.end());
    }//可以指定容器

    //插入
    void push(const T& value){
        data.push_back(value);
        push_heap(data.begin(),data.end());
    }
    //删除
    void pop(){
        if(!empty()){
            pop_heap(data.begin(),data.end());
            data.pop_back();
        }else{
            throw runtime_error("Priority queue is empty.");
        }
    }
    //访问
    T& top(){
        if(!empty()){
            return data.front();
        }
    }
    bool empty() const{
        return data.empty();
    }
    size_t size()const{
        return data.size();
    }
};
int main(){
    // 使用默认底层容器vector
    priority_queue<int>q1;
    q1.push(3);
    q1.push(1);
    q1.push(4);
    q1.push(5);
    cout<<"Top element of q1: "<<q1.top()<<endl;
    q1.pop();
    cout << "Priority queue q1 size after pop: " << q1.size() <<endl;
    //自定义vector
    vector<int>v = {9, 5, 7, 2, 6};
    priority_queue<int,vector<int>>q2(v);
    cout<<"Top element of q2: "<<q2.top()<<endl;
    q2.pop();
    cout<<"Priority queue q2 size after pop: "<<q2.size()<<endl;
}
```

### 8、小顶堆

*   如果要完成小顶堆，就需要逆序排列，可以使用less<typename>的方法来实现。
    *   在template里加入less
    *   在push\_heap等等里面，就可以使用less了。

```cpp
template<class T,class container = vector<T>,class compare = less<T>>
        //小顶堆
class priority_queue{
private:
    container data;
public:
    void push(const T& value) {
        data.push_back(value);
        push_heap(data.begin(), data.end(), compare());
    }
};

```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/147353850?spm=1001.2014.3001.5501>，如有侵权，请联系删除。