 

一、了解vector的使用
-------------

[看这篇博客，里面几乎涵盖了，所有常用的，以及一些底层的原理](https://blog.csdn.net/2401_82911768/article/details/145947541)

二、了解工作原理
--------

*   申请的空间
    *   vector数组的元素是在堆上申请内存空间。
    *   但是他申请的对象本身（比如：数据的指针、大小、容量等等控制信息）是在栈上的。
*   扩容过程
    *   当size>capacity时，会发生扩容
        *   扩容的过程：
            *   分配一个更大的内存块，是原来的2倍（一般2倍，由增长因子决定）。
            *   把数据复制到新内存块中。
            *   销毁原来的数据。
            *   释放旧内存块
            *   就可以插入新元素了。

三、实现过程
------

### 1、构造过程

#### 1.1、构造函数

*   需要初始化操作。
*   这里选用3个变量。
    *   elements是开辟的数组空间，存放数据。
    *   capacity用来设置数组空间的容量。
    *   size来确定数组空间的元素数据个数。

```cpp
//构造函数
    Vector():elements(nullptr),capacity(0),size(0){}
```

#### 1.2、拷贝构造

*   这里需要把别的vector数据拷贝到我们新建的vector

```cpp
Vector(const Vector&other):capacity(other.capacity),size(other.size){
        elements = new T[capacity];
        copy(other.emelments,other.elements+size,elements);
    }
```

#### 1.3、赋值拷贝

*   赋值拷贝需要运算符重载
*   然后判断当前对象与别的对象数据是否相同，不同就把当前对象数据重新赋值。

```cpp
Vector& operator=(const Vector &other){
        if(this!=&other){
            delete[] elements;
            capacity = other.capacity;
            size = other.size;
            elements = new T[capacity];
            copy(other.elements,other.elements+size,elements);
        }
        return *this;
    }
```

### 2、析构函数

*   析构函数用来销毁数组elements

```cpp
//析构函数
    ~Vector(){
        delete[] elements;
    }
```

### 3、添加元素

#### 3.1、push\_back()

*   插入一个元素
*   如果数量等于容量，就需要扩容reserve

```cpp
//添加元素到末尾
    void push_back(const T&value){
        if(size==capacity){
            //扩容操作
            reserve(capacity==0?1:2*capacity);
        }
        elesments[size++] = value;
    }
```

#### 3.2、insert插入某个元素

*   找到要插入的位置，如果这个位置大于等于数量
*   就越界了。
*   如果数量等于容量，就需要扩容。
*   把后面的元素，后移。
*   然后进行插入元素

```cpp
/指定位置插入元素insert
    void insert(size_t index,const T&value){
        if(index>=size){
            throw out_of_range("index out of range");
        }
        if(size==capacity){
            reserve(capacity==0?1:capacity*2);
        }
        for(size_t i =size;i>index;--i){
            elements[i]=elements[i-1];
        }
        elements[index]=value;
        ++size;
    }   
```

### 4、删除元素

#### 4.1、删除末尾元素

*   这里直接使用size 减 一解决。
*   优化的话，需要销毁掉元素。

```cpp
//删除末尾元素
    void pop_back(){
        if(size>0){
            --size;
        }
    }
```

#### 4.2、erase删除某一个元素

*   告诉要删除的位置
*   把后面的元素覆盖掉当前要删除元素的位置。
*   再修改size ，维持性质

```cpp
    //erase()删除某一个元素
    T* erase(T* position){
        if (position < begin() || position >= end()) {
            throw std::out_of_range("Invalid erase position");
        }
        if(position!=end()){
            copy(position+1,end(),position);   
            --size; 
        }
        return position;
    }
```

### 5、元素数量

*   直接返回值

```cpp
//元素个数
    size_t getSize()const{
        return size;
    }
```

### 6、容量

*   直接返回值就行

```cpp
/容量
    size_t getCapacity(){
        return capacity;
    }
```

### 7、访问

#### 7.1、\[ \]访问法

*   与at不同
    *   他不会告诉是否越界。
    *   但是我这里是有检查越界的写法。

```cpp
//访问数组中的元素
    T& operator[](size_t index){
        if(index>=size){
            throw out_of_range("index out of range");
        }
        return elements[index];
    }
    // const版本的访问数组中的元素
    const T &operator[](size_t index) const
    {
        // 检查索引是否越界
        if (index >= size)
        {
            throw std::out_of_range("Index out of range");
        }
        return elements[index];
    }
```

#### 7.2、at访问法

*   at
    *   可以判断是否越界，out\_of\_range是访问超出范围的索引 。来自头文件#include<stdexecpt>

```cpp
//at访问
    T at(int index){
        if(index>=size){
            throw out_of_range("vector at out of range");
        }
        return elements[index];
    }
    //at const访问
    const T* at(int index)const{
        if(index>=size){
            throw out_of_range("vector at out of range");
        }
    }
```

### 8、迭代器

*   begin和end迭代器的实现。
*   注意：
    *   end迭代器是最后一个元素的后一个位置。
    *   **并不是最后一个元素**。

```cpp
//迭代器的实现
    T*begin(){
        return elements;
    }

    T*end(){
        return elements+size;
    }

    //利用迭代器的打印数组
    const T*begin()const{
        return elements;
    }

    const T*end()const{
        return elements+size;
    }
```

### 9、清空操作

*   将size =0 说明，没有元素，也可以进行销毁数据，在size =0 ，这是优化方法。

```cpp
//清空数组
    void clear(){
        
        size=0;
    }
```

### 10、扩容操作

*   扩容过程
    *   当size>capacity时，会发生扩容
        *   扩容的过程：
            *   分配一个更大的内存块，是原来的2倍（一般2倍，由增长因子决定）。
            *   把数据复制到新内存块中。
            *   销毁原来的数据。
            *   释放旧内存块
            *   就可以插入新元素了。

```cpp
//扩容操作
    void reserve(size_t newCapacity){
        if(newCapacity>capacity){
            T* new_elements = new T[newCapacity];
            copy(elements,elements+size,new_elements);
            delete[] elements;
            elements= new_elements;
            capacity=newCapacity;
        }
    }
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/147144809?sharetype=blogdetail&sharerId=147144809&sharerefer=PC&sharesource=2401_82911768&spm=1011.2480.3001.8118>，如有侵权，请联系删除。