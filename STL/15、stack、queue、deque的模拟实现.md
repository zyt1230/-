 

一、stack
-------

### 1、stack的使用

[请看这篇文章](https://blog.csdn.net/2401_82911768/article/details/146048404?spm=1001.2014.3001.5501)

### 2、stack的原理

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/05af8adf1dfc4133a34dc66e73243516.png)  
[这篇文章的栈原理讲的不错，并且有链式栈和顺序栈的创建，还有栈常使用的场景，没有数据结构基础的可以看，并且实现一下他的2种栈。](https://blog.csdn.net/qq_67319052/article/details/141072373?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522fa049e4edf1ab166f576e56c454b0f2d%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=fa049e4edf1ab166f576e56c454b0f2d&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-2-141072373-null-null.142%5Ev102%5Epc_search_result_base9&utm_term=stack&spm=1018.2226.3001.4187)

### 3、stack的实现

#### 3.1、成员变量

*   这里使用的deque<typename>类型的底层容器来实现栈。

```cpp
template <class T,class container = deque<T>>

private:
    //定义一个deque<T>data容器
    container data;
```

#### 3.2、压栈

*   向栈放入元素

```cpp
//压入栈顶
    void push(const T&value){
        data.push_back(value);
    }
```

#### 3.3、弹栈

*   从栈中，拿出栈顶元素

```cpp
//弹出栈顶
    void pop(){
        if (!empty()) {
            data.pop_back();
        } else {
            throw std::runtime_error("Stack is empty.");
        }
    }
```

#### 3.4、获取栈顶元素

*   top只能获取栈顶元素值

```cpp
    T& top(){
      if (!empty()) {
            return data.back();
        } else {
            throw std::runtime_error("Stack is empty.");
        }
    }
```

#### 3.5、判断是否为空

*   如果为空就是true，否则为false

```cpp
//判断是否为空
    bool empty() const {
        return data.empty();
    }
```

#### 3.6、大小

*   直接返回size

```cpp
    //返回大小
    size_t size() const {
        return data.size();
    }
```

#### 3.7、完整过程

```cpp
#include<stdexcept>
#include<iostream>
#include<sstream>
#include<list>
#include<deque>
#include<vector>
using namespace std;
template <class T,class container = deque<T>>
//deque 作为底层容器
class Stack{
public:
    //压入栈顶
    void push(const T&value){
        data.push_back(value);
    }
    //弹出栈顶
    void pop(){
        if (!empty()) {
            data.pop_back();
        } else {
            throw std::runtime_error("Stack is empty.");
        }
    }
    //获取栈顶
    T& top(){
      if (!empty()) {
            return data.back();
        } else {
            throw std::runtime_error("Stack is empty.");
        }
    }
    //判断是否为空
    bool empty() const {
        return data.empty();
    }
    //返回大小
    size_t size() const {
        return data.size();
    }
private:
    //定义一个deque<T>data容器
    container data;
};
```

二、queue
-------

### 1、queue的使用

[queue的使用，看这个文章。](https://blog.csdn.net/2401_82911768/article/details/146051834)

### 2、queue的原理

[queu的原理，推荐看这篇文章，讲的很详细](https://blog.csdn.net/chenwang1824/article/details/139899953?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522392bcd407a5ae63e5a4f7133419d326c%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=392bcd407a5ae63e5a4f7133419d326c&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-139899953-null-null.142%5Ev102%5Epc_search_result_base9&utm_term=queue%E7%9A%84%E5%8E%9F%E7%90%86&spm=1018.2226.3001.4187)

### 3、queue的实现

*   这里使用的deque<typename>类型的底层容器来实现单向队列。

#### 3.1、出队

*   出队就是从队头删除元素

```cpp
  void pop() {
    if (!empty()) {
      data.pop_front();
    } else {
      throw runtime_error("Queue is empty.");
    }
  }
```

#### 3.2、入队

*   从队尾插入元素

```cpp
void push(const T &value) { data.push_back(value); }

```

#### 3.3、大小

*   返回数据数量

```cpp
  size_t size() { return data.size(); }
```

#### 3.4、判断为空

*   data数组为空就是true
*   反之，为 false

```cpp
  bool empty() { return data.empty(); }
```

#### 3.5、访问队头

*   先判断是否为空，为空就不能访问了。
*   runtime\_error是运行时错误。
*   返回队头的元素

```cpp
  T &front() {
    if (!empty()) {
      return data.front();
    } else {
      throw runtime_error("Queue is empty.");
    }
  }
```

#### 3.6、访问队尾

*   先判断是否为空，为空就不能访问了。
*   runtime\_error是运行时错误。
*   返回队尾的元素

```cpp
  T &back() {
    if (!empty()) {
      return data.back();
    } else {
      throw runtime_error("Queue is empty.");
    }
  }
```

#### 3.7、完整过程

```cpp
#include <deque>
#include <iostream>
#include <sstream>
#include <stdexcept>
#include <string>
using namespace std;
template <class T, typename container = deque<T>> 
class queue {
public:
  
  void push(const T &value) { data.push_back(value); }

  
  void pop() {
    if (!empty()) {
      data.pop_front();
    } else {
      throw runtime_error("Queue is empty.");
    }
  }
  
  T &front() {
    if (!empty()) {
      return data.front();
    } else {
      throw runtime_error("Queue is empty.");
    }
  }
  
  T &back() {
    if (!empty()) {
      return data.back();
    } else {
      throw runtime_error("Queue is empty.");
    }
  }
  
  bool empty() { return data.empty(); }
  
  size_t size() { return data.size(); }

private:
  container data; 
};
int main(int argc, char *argv[]) {
  queue<int, deque<int>> queue;
  int N;
  cin >> N;
  getchar();
  string line;

  for (int i = 0; i < N; i++) {
    std::getline(std::cin, line);
    std::istringstream iss(line);
    std::string command;
    iss >> command;

    int element;

    if (command == "push") {
      iss >> element;
      queue.push(element);
    }

    if (command == "pop") {
      try {
        queue.pop();
      } catch (const std::runtime_error &e) {
        // 涓嶅仛浠讳綍澶勭悊
        continue;
      }
    }

    if (command == "front") {
      try {
        std::cout << queue.front() << std::endl;
      } catch (const std::runtime_error &e) {
        std::cout << "null" << std::endl;
      }
    }

    if (command == "back") {
      try {
        std::cout << queue.back() << std::endl;
      } catch (const std::runtime_error &e) {
        std::cout << "null" << std::endl;
      }
    }

    if (command == "size") {
      std::cout << queue.size() << std::endl;
    }

    if (command == "empty") {
      std::cout << (queue.empty() ? "true" : "false") << std::endl;
    }
  }
  return 0;
}
```

三、deque
-------

### 1、deque的使用

[deque的使用看这篇文章。](https://blog.csdn.net/2401_82911768/article/details/146057927)

### 2、deque的原理

[deque的原理看这篇文章](https://blog.csdn.net/qq_65207641/article/details/130184194?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522e0a8f806bcccc7cd99440a645a9ad09c%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=e0a8f806bcccc7cd99440a645a9ad09c&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-6-130184194-null-null.142%5Ev102%5Epc_search_result_base9&utm_term=deque%E7%9A%84%E5%8E%9F%E7%90%86&spm=1018.2226.3001.4187)

### 3、deque的实现

#### 3.1、成员变量

*   这里需要的成员
    *   capacity：容器容量
    *   backIndex：指向尾的索引指针
    *   frontIndex：指向头的索引指针
    *   size：数据的数量

```cpp
	T* elements;
	size_t capacity;
	size_t frontIndex;
	size_t backIndex;
	size_t size;
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7082134c47c044788e80f8012f9142bf.png)

#### 3.2、入队

*   先移动到前一个位置
*   再存值

##### 3.2.1、头部

```cpp
//在deque的前端插入元素
	void push_front(const T&value){
		if(size==capacity){
			resize();
		}
		//计算新的前端位置插入元素
		frontIndex = (frontIndex-1+capacity)%capacity;
		//在新的前端位置插入元素 
		elements[frontIndex] = value;
		//增加size
		++size; 
	} 
```

##### 3.2.2、尾部

*   先在当前位置存值
*   再移动到下一个位置

```cpp
//在尾部插入元素 
	void push_back(const T&value){
		if(size==capacity){
			resize();
		}
		elements[backIndex]=value;
        //计算新的后端索引 
		
        backIndex=(backIndex+1)%capacity;
		//增加deque的元素数量
        
		++size; 
	}
```

#### 3.3、出队

##### 3.3.1、头部

*   在头部删除元素
*   如果没有元素就用out\_of\_range()告知。
*   先移动索引，再size减一。

```cpp
void pop_front(){
		//检查deque是否为空
		if(size==0){
			throw out_of_range("deque is empty");
		} 
		//删除不需要显示地删除，以后新追加元素会自动覆盖
		frontIndex = (frontIndex+1)%capacity;
		//减少deque的数量
		--size; 
	}
```

##### 3.3.2、尾部

*   在尾部删除元素
*   如果没有元素就用out\_of\_range()告知。
*   先移动索引，再size减一。

```cpp
void pop_back(){
		if(size==0){
			throw out_of_range("deque is empty");
		}
		backIndex = (backIndex+capacity-1)%capacity;
		--size;	
	} 
```

##### 举例表示front和back的移动过程

```cpp
1、开始的时候（假设容量为5）
索引: [0] [1] [2] [3] [4] [5] [6] [7]  
值:   空  空  空  空  空  空  空  空  
指针: front=0, back=0, size=0

2、操作1:push_back(10)
[0]=10, [1-7]=空  
指针: front=0, back=1, size=1

3、操作2:push_front(20)
[7]=20, [0]=10, [1-6]=空  
指针: front=7, back=1, size=2

4、操作3:push_back(30)
[7]=20, [0]=10, [1]=30, [2-6]=空  
指针: front=7, back=2, size=3

5、操作4：push_front(40)
[6]=40, [7]=20, [0]=10, [1]=30, [2-5]=空  
指针: front=6, back=2, size=4

6、操作5：pop_back()
[6]=40, [7]=20, [0]=10, [1-5]=空  
指针: front=6, back=1, size=3

7、操作6：pop_front()
[7]=20, [0]=10, [1-5]=空  
指针: front=7, back=1, size=2


```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b1d024c6c29046769c9fc9471b637025.png)

#### 3.4、清空

*   直接修改成员变量。

```cpp
	//清空
	void clear() {
        // 更高效的清空方式
        frontIndex = 0;
        backIndex = 0;
        size = 0;
    }
```

#### 3.5、随机访问

*   需要运算符重载 \[ \]

```cpp
	//随机访问[]
	T& operator[](int index){
		if(index<0||index>=size){
			throw out_of_range("index is invalid");
		}
		return elements[(frontIndex+index)%capacity];
	} 
	size_t get_size()const{
		return size;
	}
```

#### 3.6、大小

*   返回数据数量

```cpp
	size_t get_size()const{
		return size;
	}
```

#### 3.7、完整过程

```cpp

#include <iostream>
#include <sstream>
#include <stdexcept>
#include <string>
using namespace std;
template <class T> 
class deque{
private:
	T* elements;
	size_t capacity;
	size_t frontIndex;
	size_t backIndex;
	size_t size;
public:
	//构造函数
	deque():elements(NULL),capacity(0),frontIndex(0),backIndex(0),size(0){
		
	}
	//析构 
	~deque(){
		clear();
		delete[] elements;
	}
	//在deque的前端插入元素
	void push_front(const T&value){
		if(size==capacity){
			resize();
		}
		//计算新的前端位置插入元素
		frontIndex = (frontIndex-1+capacity)%capacity;
		//在新的前端位置插入元素 
		elements[frontIndex] = value;
		//增加size
		++size; 
	} 
	//在尾部插入元素 
	void push_back(const T&value){
		if(size==capacity){
			resize();
		}
		elements[backIndex]=value;
        //计算新的后端索引 
		
        backIndex=(backIndex+1)%capacity;
		//增加deque的元素数量
        
		++size; 
	}
	//从deque的前端移除元素
	void pop_front(){
		//检查deque是否为空
		if(size==0){
			throw out_of_range("deque is empty");
		} 
		//删除不需要显示地删除，以后新追加元素会自动覆盖
		frontIndex = (frontIndex+1)%capacity;
		//减少deque的数量
		--size; 
	}
	//从deque的后端删除
	void pop_back(){
		if(size==0){
			throw out_of_range("deque is empty");
		}
		backIndex = (backIndex+capacity-1)%capacity;
		--size;	
	} 
	//随机访问[]
	T& operator[](int index){
		if(index<0||index>=size){
			throw out_of_range("index is invalid");
		}
		return elements[(frontIndex+index)%capacity];
	} 
	//大小 
	size_t get_size()const{
		return size;
	}
	//清空
	void clear() {
        // 更高效的清空方式
        frontIndex = 0;
        backIndex = 0;
        size = 0;
    }
	//打印deque中的值
	void printElements()const{
		
		size_t index = frontIndex;
		for(size_t i =0;i<size;++i){
			cout<<elements[index]<<" ";
			index = (index+1)%capacity;
		}
		cout<<endl;
	} 
private:
	void resize(){
		size_t newCapacity = (capacity==0)?1:capacity *2;
		//创建新的数组
		T* newElements = new T[newCapacity];
		//复制元素到新的数组
		size_t index = frontIndex;
		for(size_t i =0;i<size;++i){
			newElements[i]=elements[index];
			index = (index+1)%capacity;
		} 
		//释放旧内存 
		delete[] elements;
		//更新
		elements = newElements;
		capacity = newCapacity;
		frontIndex = 0;
		backIndex = size; 
	}
	
};
int main(int argc, char *argv[]) {
  	deque<int>myDeque;
  	int N;
  	cin>>N;
  	getchar();
  	string line;
    for (int i = 0; i < N; i++) {
        std::getline(std::cin, line);
        std::istringstream iss(line);
        std::string command;
        iss >> command;
        int value;

        if (command == "push_back") {
            iss >> value;
            myDeque.push_back(value);
        }

        if (command == "push_front") {
            iss >> value;
            myDeque.push_front(value);
        }

        if (command == "pop_back") {
            if (myDeque.get_size() == 0) {
                continue;
            }
            myDeque.pop_back();
        }

        if (command == "pop_front") {
            if (myDeque.get_size() == 0) {
                continue;
            }
            myDeque.pop_front();
        }

        if (command == "clear") {
            myDeque.clear();
        }

        if (command == "size") {
            std::cout << myDeque.get_size() << std::endl;
        }

        if (command == "get") {
            iss >> value;
            std::cout << myDeque[value] << std::endl;
        }

        if (command == "print") {
            if (myDeque.get_size() == 0) {
                std::cout << "empty" << std::endl;
            } else {
                myDeque.printElements();
            }
        }
    }
}
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/147182735?spm=1001.2014.3001.5501>，如有侵权，请联系删除。