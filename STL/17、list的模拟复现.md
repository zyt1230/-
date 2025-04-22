 

一、list的使用方法
-----------

[list的使用方法，看这个文章就行](https://blog.csdn.net/2401_82911768/article/details/146256378)

二、list的原理【了解就行，主要看list使用的文章，都是重点】
---------------------------------

### 1、[STL](https://so.csdn.net/so/search?q=STL&spm=1001.2101.3001.7020)中list的原理

STL中的`list`是一个[双向链表](https://so.csdn.net/so/search?q=%E5%8F%8C%E5%90%91%E9%93%BE%E8%A1%A8&spm=1001.2101.3001.7020)容器，它的实现原理主要包括以下几个方面：

### 2、基本结构

1.  **节点结构**：每个元素存储在一个独立的节点中，每个节点包含：
    
    *   数据部分（存储实际元素）
    *   前驱指针（指向前一个节点）
    *   后继指针（指向后一个节点）
2.  **环形结构**：STL的list通常实现为带[哨兵](https://so.csdn.net/so/search?q=%E5%93%A8%E5%85%B5&spm=1001.2101.3001.7020)节点(sentinel)的环形双向链表
    
    *   有一个不存储数据的头节点（也称为哨兵节点）
    *   头节点的前驱指向最后一个元素
    *   最后一个元素的后继指向头节点

### 3、关键特性

1.  **非连续存储**：元素在内存中不是连续存储的，而是分散在各处
    
2.  **迭代器稳定性**：
    
    *   插入和删除操作不会使其他元素的迭代器失效
    *   只有被删除元素的迭代器会失效
3.  **时间复杂度**：
    
    *   插入/删除：O(1)（已知位置时）
    *   随机访问：O(n)（不支持随机访问）

### 4、实现细节

1.  **内存分配**：通常使用分配器(allocator)来管理节点的内存分配和释放
    
2.  **常用操作实现**：
    
    *   `push_front`：在头节点后插入新节点
    *   `push_back`：在尾节点后插入新节点
    *   `insert`：在指定位置前插入新节点
    *   `erase`：移除指定节点并调整前后节点的指针
3.  **特殊成员函数**：
    
    *   `splice`：在常数时间内将元素从一个list移动到另一个list
    *   `merge`：合并两个已排序的list
    *   `sort`：使用归并排序算法对list进行排序

### 5、与vector的对比

1.  **优点**：
    
    *   任意位置插入删除高效
    *   不需要大规模数据搬移
    *   内存使用更灵活
2.  **缺点**：
    
    *   不支持随机访问
    *   内存局部性差，缓存命中率低
    *   每个元素需要额外空间存储前后指针

三、list的复现过程
-----------

### 1、定义节点

*   结点包含
    *   指向下一个节点的指针
    *   指向上一个节点的指针
    *   数据

```cpp
struct Node{
        T data;//数据
        Node* next;//指向下一个节点的指针
        Node* prev;//指向上一个节点的指针
        //构造函数
        Node(const T&value,Node* nextNode = nullptr,Node* prevNode = nullptr):data(value),next(nextNode),prev(prevNode){

        }
    };
```

### 2、头节点的定义

*   头节点包含
    *   头结点指针
    *   尾节点指针
    *   链表中节点的数量

```cpp
    Node* head;//头结点指针
    Node* tail;//尾节点指针
    size_t size;//链表中节点的数量
```

### 3、清空链表

*   从头节点开始遍历，直到为空。
*   把每一个节点都清空。

```cpp
    //清空链表
    void clear(){
        while(head){
            //从头节点开始，一次删除节点
            Node* temp = head;
            head = head->next;
            delete temp;
        }
        //更新尾节点和链表大小
        tail = nullptr;
        size = 0;
    }
```

### 4、尾部插入元素

*   创建新节点，并初始化
*   如果尾节点存在，就直接插入。尾节点不存在，就需要重新定义头节点是新插入的节点，而不是空指针。
*   更新尾节点指针指向的节点。

```cpp
    //链表末尾添加元素
    void push_back(const T&value){
        //创建新节点
        Node* newNode =new Node(value, nullptr,tail);
        if(tail){
            // 如果链表非空，将尾节点的 next 指针指向新节点
            tail->next = newNode;
        }else{
            // 如果链表为空，新节点同时也是头节点
            head = newNode;
        }
        //尾部进行更新
        tail = newNode;
        ++size;
    }
```

### 5、头部插入元素

*   创建新节点，并初始化
*   如果头节点存在，就直接插入。头节点不存在，就需要重新定义尾节点是新插入的节点，而不是空指针。
*   更新头结点

```cpp
    //头部插入元素
    void push_front(const T& value){
        //创建新的节点
        Node* newNode = new Node(value,head, nullptr);
        if(head){
            // 如果链表非空，将头节点的 prev 指针指向新节点
            head->prev = newNode;
        }else{
            // 如果链表为空，新节点同时也是尾节点
            tail = newNode;
        }
        //更新头节点指针和链表大小
        head = newNode;
        ++size;
    }
```

### 6、尾部删除元素

*   如果size大于0 ，说明有节点才能删除，否则不能。
*   获取尾结点的上一个节点
*   删除尾结点
*   更新尾结点到删除尾结点的上一个节点。
*   新尾结点的后面为空，需要更新它的next指针
*   如果尾节点不存在了，也需要重新定义。
*   更新size大小

```cpp
    //删除链表末尾的元素
    void pop_back(){
        if(size>0){
            //获取尾节点的前一个节点
            Node* newTail = tail->prev;
            //删除尾节点
            delete tail;
            //更新尾节点和大小
            tail = newTail;
            if(tail){
                tail->next = nullptr;
            }else{
                head = nullptr;
            }
            --size;
        }
    }
```

### 7、头部删除元素

*   如果size大于0 ，说明有节点才能删除，否则不能。
*   获取头结点的下一个节点
*   删除头结点
*   更新头结点到删除头结点的下一个节点。
*   新头结点的前面为空，需要更新它的prev指针
*   如果头节点不存在了，也需要重新定义。
*   更新size大小

```cpp
    //删除链表开头的元素
    void pop_front(){
        if(size>0){
            //获取头节点的下一个节点
            Node* newHead = head->next;
            //删除头节点
            delete head;
            //更新头节点指针和链表大小
            head = newHead;
            if(head){
                head->prev= nullptr;
            }else{
                tail = nullptr;//如果链表为空，尾节点也置空
            }
            --size;
        }
    }
```

### 8、获取节点的大小

*   直接返回节点的大小size成员变量就行

```cpp
	//获取链表的大小
    size_t getSize()const{return size;}
```

### 9、\[ \]访问元素

*   重载运算符\[ \]，返回值

```cpp
    //访问链表中的元素
    T& operator[](size_t index){
        //从头节点开始遍历链表，找到index个节点
        Node* current = head;
        for(size_t i =0;i<index;++i){
            if(!current){//current不存在时
                //如果index超出了链表长度，则抛出异常
                throw out_of_range("Index out of range");
            }
            current=current->next;
        }
        //返回数据
        return current->data;
    }
    // const版本的访问链表中的元素
    const T &operator[](size_t index) const
    {
        // 从头节点开始遍历链表，找到第 index 个节点
        Node *current = head;
        for (size_t i = 0; i < index; ++i)
        {
            if (!current)
            {
                // 如果 index 超出链表长度，则抛出异常
                throw std::out_of_range("Index out of range");
            }
            current = current->next;
        }

        // 返回节点中的数据
        return current->data;
    }
```

### 11、删除指定节点的值

*   从head开始找，直到找到要删除的值与节点的值相同位止。
    *   没有找到就返回空。
*   如果待删除的节点既不是头也不是尾
    *   需要重新改变它前后节点的next和prev指针指向。
*   如果是头和尾
    *   说明只有一个节点，被删除后，就没有了。头和尾指针需要改为NULL
*   如果是头
    *   需要更新头的指向
*   如果是尾
    *   需要更新尾的指向

```cpp
    //删除指定值的节点
    void remove(const T&val){
        Node* node = head;
        while(node!=nullptr&&node->data!=val){
            node=node->next;
        }
        if(node== nullptr){
            //没有找到
            return;
        }
        if(node!=head&&node!=tail){
            //既不是头节点也不是尾节点
            node->prev->next=node->next;
            node->next->prev=node->prev;
        }else if(node==head&&node==tail){
            //既是头节点也是尾节点
            head = nullptr;
            tail = nullptr;
        }else if(node==head){
            //是头结点
            head = node->next;
            head->prev = nullptr;
        }else{
            //尾节点
            tail = node->prev;
            tail->next = nullptr;
        }
        --size;
    }
```

### 12、判断是否为空

*   如果size为0，就是空

```cpp
    //判断是否为空
    bool empty(){return size==0;}
```

### 13、迭代器的实现

*   begin迭代器是头指针

```cpp
    //使用迭代器访问开头
    Node* begin(){return head;}
    // 使用迭代器遍历链表的开始位置（const版本）
    const Node *begin() const { return head; }
```

*   end迭代器是尾指针

```cpp
    //使用迭代器访问尾部
    Node* end(){return nullptr;}
    // 使用迭代器遍历链表的结束位置（const版本）
    const Node *end() const { return nullptr; }
```

### 、总结

```cpp

#include <iostream>
#include <algorithm>
#include <string>
#include <stdexcept>
#include <sstream>
using namespace std;
template <class T>
class List{
public:
    template<class L>
    friend ostream& operator<<(ostream& os,const List<L> &pt);
private:
    //定义节点结构
    struct Node{
        T data;//数据
        Node* next;//指向下一个节点的指针
        Node* prev;//指向上一个节点的指针
        //构造函数
        Node(const T&value,Node* nextNode = nullptr,Node* prevNode = nullptr):data(value),next(nextNode),prev(prevNode){

        }
    };
    Node* head;//头结点指针
    Node* tail;//尾节点指针
    size_t size;//链表中节点的数量
public:
    //构造函数
    List():head(nullptr),tail(nullptr),size(0){}
    //析构函数
    ~List(){clear();}
    //清空链表
    void clear(){
        while(head){
            //从头节点开始，一次删除节点
            Node* temp = head;
            head = head->next;
            delete temp;
        }
        //更新尾节点和链表大小
        tail = nullptr;
        size = 0;
    }
    //链表末尾添加元素
    void push_back(const T&value){
        //创建新节点
        Node* newNode =new Node(value, nullptr,tail);
        if(tail){
            // 如果链表非空，将尾节点的 next 指针指向新节点
            tail->next = newNode;
        }else{
            // 如果链表为空，新节点同时也是头节点
            head = newNode;
        }
        //尾部进行更新
        tail = newNode;
        ++size;
    }
    //头部插入元素
    void push_front(const T& value){
        //创建新的节点
        Node* newNode = new Node(value,head, nullptr);
        if(head){
            // 如果链表非空，将头节点的 prev 指针指向新节点
            head->prev = newNode;
        }else{
            // 如果链表为空，新节点同时也是尾节点
            tail = newNode;
        }
        //更新头节点指针和链表大小
        head = newNode;
        ++size;
    }
    //获取链表的大小
    size_t getSize()const{return size;}
    //访问链表中的元素
    T& operator[](size_t index){
        //从头节点开始遍历链表，找到index个节点
        Node* current = head;
        for(size_t i =0;i<index;++i){
            if(!current){//current不存在时
                //如果index超出了链表长度，则抛出异常
                throw out_of_range("Index out of range");
            }
            current=current->next;
        }
        //返回数据
        return current->data;
    }
    // const版本的访问链表中的元素
    const T &operator[](size_t index) const
    {
        // 从头节点开始遍历链表，找到第 index 个节点
        Node *current = head;
        for (size_t i = 0; i < index; ++i)
        {
            if (!current)
            {
                // 如果 index 超出链表长度，则抛出异常
                throw std::out_of_range("Index out of range");
            }
            current = current->next;
        }

        // 返回节点中的数据
        return current->data;
    }
    //删除链表末尾的元素
    void pop_back(){
        if(size>0){
            //获取尾节点的前一个节点
            Node* newTail = tail->prev;
            //删除尾节点
            delete tail;
            //更新尾节点和大小
            tail = newTail;
            if(tail){
                tail->next = nullptr;
            }else{
                head = nullptr;
            }
            --size;
        }
    }
    //删除链表开头的元素
    void pop_front(){
        if(size>0){
            //获取头节点的下一个节点
            Node* newHead = head->next;
            //删除头节点
            delete head;
            //更新头节点指针和链表大小
            head = newHead;
            if(head){
                head->prev= nullptr;
            }else{
                tail = nullptr;//如果链表为空，尾节点也置空
            }
            --size;
        }
    }
    //获取指定节点的值
    Node* getNode(const T& val){
        Node* node = head;
        while(node!=nullptr&&node->data!=val){
            node = node->next;
        }
        return node;
    }
    T* find(const T&val){
        Node* node = getNode(val);
        if(node==nullptr){
            return nullptr;
        }
        return &node->data;
    }
    //删除指定值的节点
    void remove(const T&val){
        Node* node = head;
        while(node!=nullptr&&node->data!=val){
            node=node->next;
        }
        if(node== nullptr){
            //没有找到
            return;
        }
        if(node!=head&&node!=tail){
            //既不是头节点也不是尾节点
            node->prev->next=node->next;
            node->next->prev=node->prev;
        }else if(node==head&&node==tail){
            //既是头节点也是尾节点
            head = nullptr;
            tail = nullptr;
        }else if(node==head){
            //是头结点
            head = node->next;
            head->prev = nullptr;
        }else{
            //尾节点
            tail = node->prev;
            tail->next = nullptr;
        }
        --size;
    }
    //判断是否为空
    bool empty(){return size==0;}
    //使用迭代器访问开头
    Node* begin(){return head;}
    //使用迭代器访问尾部
    Node* end(){return nullptr;}
    // 使用迭代器遍历链表的开始位置（const版本）
    const Node *begin() const { return head; }
    // 使用迭代器遍历链表的结束位置（const版本）
    const Node *end() const { return nullptr; }
    // 打印链表中的元素
    void printElements() const
    {
        for (Node *current = head; current; current = current->next)
        {
            std::cout << current->data << " ";
        }
        std::cout << std::endl;
    }
};
//重载<<运算符
template <class T>
std::ostream &operator<<(std::ostream &os, const List<T> &pt)
{
    //访问私有成员变量，需要友元
    for (typename List<T>::Node *current = pt.head; current;
         current = current->next)
    {
        os << " " << current->data;
    }
    os << std::endl;
    return os;
}
int main() {
    // 创建一个 List 对象
    List<int> myList;

    int N;
    std::cin >> N;
    // 读走回车
    getchar();
    std::string line;
    // 接收命令
    for (int i = 0; i < N; i++) {
        std::getline(std::cin, line);
        std::istringstream iss(line);
        std::string command;
        iss >> command;
        int value;

        if (command == "push_back") {
            iss >> value;
            myList.push_back(value);
        }

        if (command == "push_front") {
            iss >> value;
            myList.push_front(value);
        }

        if (command == "pop_back") {
            myList.pop_back();
        }

        if (command == "pop_front") {
            myList.pop_front();
        }

        if (command == "remove") {
            iss >> value;
            myList.remove(value);
        }

        if (command == "clear") {
            myList.clear();
        }

        if (command == "size") {
            std::cout << myList.getSize() << std::endl;
        }

        if (command == "get") {
            iss >> value;
            std::cout << myList[value] << std::endl;
        }

        if (command == "print") {
            if (myList.getSize() == 0) {
                std::cout << "empty" << std::endl;
            } else {
                myList.printElements();
            }
        }
    }
    List<int> myList2;
    myList2.push_back(10);
    myList2.push_back(20);
    myList2.push_back(30);

// 使用重载的 << 运算符输出
    std::cout << myList2;  // 输出: 10 20 30
    return 0;
}

```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/147373994?spm=1001.2014.3001.5501>，如有侵权，请联系删除。