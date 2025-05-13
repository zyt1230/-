 

一、map
-----

### 1、了解

[map的使用和常考面试题等等，看这篇文章](https://blog.csdn.net/2401_82911768/article/details/146291948)

*   map的`key`是有序的 ，值**不可重复** 。
*   插入使用 `insert`的效率更高，而在"更新map的键值对时，使用 \[ \]运算符效率更高 。"

> **注意**
> 
> *   map 的lower和upper那2个函数，经常用在算法里。
> *   直接修改某一个键的值，用 运算符\[ \]

### 2、map的复现

*   可以使用红黑树代码 （可以放在.h文件里，然后.h放入cpp文件中，分文件编程）。
*   直接调用红黑树 。
*   剩下的部分与之前写stack、queue等等的是一样的。

```cpp
#include <cstddef>
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <sstream>
#include <stdexcept>
#include <string>
#include <utility>
using namespace std;
enum class Color{BLACK,RED};
template<class Key,class Value>
class RedBlackTree{
    class RBNode{
    public:
        RBNode* left;
        RBNode* right;
        RBNode* parent;
        Key key;
        Value value;
        Color color;
        RBNode(const Key& key,const Value& value)
                :left(nullptr),right(nullptr),parent(nullptr),
                 color(Color::RED),key(key),value(value){

        }
        RBNode():left(nullptr),right(nullptr),parent(nullptr),
                 color(Color::BLACK){

        }
    };
    RBNode* nil;
    RBNode* root;
    size_t size;
public:
    RedBlackTree():nil(new RBNode()),root(nil),size(0){

    }
    ~RedBlackTree(){
        if(root!=nil)destroy(root);
        delete nil;
    }
private:
    void inorderPrint(RBNode* node){
        if(node!=nil){
            inorderPrint(node->left);
            cout<<node->key<<" "<<node->value<<" ";
            inorderPrint(node->right);
        }
    }
    void destroy(RBNode* node){
        if(node!=nil){
            destroy(node->left);
            destroy(node->right);
            delete node;
            size--;
        }
    }
    RBNode* searchRBNode(const Key& key){
        RBNode* cur = root;
        while(cur!=nil){
            if(cur->key>key){
                cur = cur->left;
            }else if(cur->key<key){
                cur = cur->right;
            }else{
                return cur;
            }
        }
        return nil;
    }
    void leftRotate(RBNode* x){
        RBNode* y = x->right;
        x->right = y->left;
        if(y->left!=nil){
            y->left->parent = x;
        }
        y->parent = x->parent;
        if(x->parent!=nil){
            if(x->parent->left==x){
                x->parent->left = y;
            }else{
                x->parent->right = y;
            }
        }else{
            root = y;
        }
        y->left =x;
        x->parent = y;
    }
    void rightRotate(RBNode* y){
        RBNode* x = y->left;
        y->left = x->right;
        if(x->right!=nil){
            x->right->parent = y;
        }
        x->parent = y->parent;
        if(y->parent!=nil){
            if(y->parent->right==y){
                y->parent->right = x;
            }else{
                y->parent->left = x;
            }
        }else{
            root = x;
        }
        x->right = y;
        y->parent = x;
    }
    RBNode* minimum(RBNode* node){
        while(node->left!=nil){
            node=node->left;
        }
        return node;
    }
    void transPlant(RBNode* u,RBNode* v){
        if(u->parent==nil){
            root = v;
        }else if(u->parent->left==u){
            u->parent->left=v;
        }else{
            u->parent->right=v;
        }
        if(v!=nil)v->parent = u->parent;
    }
    void insertFixup(RBNode* node){
        RBNode* parent = node->parent;
        RBNode* grandparent;
        RBNode* uncle;
        RBNode* tmp;
        while(parent!=nil&&parent->color==Color::RED){
            grandparent = parent->parent;
            if(parent->parent->left==parent){
                uncle = grandparent->right;
            }else{
                uncle = grandparent->left;
            }
            if(uncle->color==Color::RED){
                parent->color=Color::BLACK;
                uncle->color=Color::BLACK;
                grandparent->color=Color::RED;
                node = grandparent;
                parent = node->parent;
                grandparent=parent->parent;
                continue;
            }
            if(grandparent->left==parent){//L
                if(parent->right==node){//R
                    leftRotate(parent);
                    tmp = parent;
                    parent = node;
                    node = parent;
                }
                rightRotate(grandparent);
                grandparent->color = Color::RED;
                parent->color = Color::BLACK;
            }else{//R
                if(parent->left==node){//L
                    rightRotate(parent);
                    tmp = parent;
                    parent = node;
                    node = parent;
                }
                leftRotate(grandparent);
                grandparent->color = Color::RED;
                parent->color = Color::BLACK;
            }
        }
        root->color=Color::BLACK;
    }
    void insertRBNode(const Key& key,const Value& value){
        RBNode* node = new RBNode(key,value);
        node->left = nil;
        node->right = nil;
        node->parent = nil;
        if(node==nil){
            return ;
        }

        RBNode* cur = root;
        RBNode* pre = nullptr;
        while(cur!=nil){
            pre = cur;
            if(key>cur->key){
                cur = cur->right;
            }else if(cur->key>key){
                cur = cur->left;
            }else{
                delete node;
                return ;
            }
        }
        if(pre==nullptr){
            root = node;
        }else{
            node->parent = pre;
            if(pre->key>key){
                pre->left = node;
            }else{
                pre->right = node;
            }
        }
        size++;
        insertFixup(node);
    }
    void deleteFixup(RBNode* x){
        RBNode* w;
        while((x!=root)&&(x!=nil)&&(x->color==Color::BLACK)){
            if(x->parent->left == x){
                w = x->parent->right;
                if(w->color==Color::RED){
                    w->color=Color::BLACK;
                    x->parent->color = Color::RED;
                    leftRotate(x->parent);
                    w = x->parent->right;
                }
                if(w->left->color==Color::BLACK&&w->right->color==Color::BLACK){
                    w->color = Color::RED;
                    x = x->parent;
                }else{
                    if(w->right->color==Color::BLACK){//RL
                        w->left->color = Color::BLACK;
                        w->color = Color::RED;
                        w->parent->color =Color::BLACK;
                        rightRotate(w);
                        w = x->parent->right;
                    }
                    w->right->color = w->color;
                    w->color= x->parent->color;
                    x->parent->color = Color::BLACK;
                    leftRotate(x->parent);
                    x = root;
                }
            }else{
                w = x->parent->left;
                if(w->color==Color::RED){
                    w->color=Color::BLACK;
                    x->parent->color = Color::RED;
                    rightRotate(x->parent);
                    w = x->parent->left;
                }
                if(w->left->color==Color::BLACK&&w->right->color==Color::BLACK){
                    w->color = Color::RED;
                    x = x->parent;
                }else{
                    if(w->left->color==Color::BLACK){//LR
                        w->right->color = Color::BLACK;
                        w->color = Color::RED;
                        x->parent->color = Color::BLACK;
                        leftRotate(w);
                        w = x->parent->left;
                    }
                    w->left->color = w->color;
                    w->color = x->parent->color;
                    w->parent->color = Color::BLACK;
                    rightRotate(x->parent);
                    x = root;
                }
            }
        }
        if(x!=nil)x->color = Color::BLACK;
    }

    void deleteRBNode(RBNode* z){
        RBNode* x;//替换
        RBNode* y= z;//删除
        Color y_origin_color = y->color;
        if(y->left==nil){
            x = y->right;
            transPlant(y, y->right);
        }else if(y->right==nil){
            x = y->left;
            transPlant(y, y->left);
        }else{
            y = minimum(z->right);
            y_origin_color = y->color;
            x = y->right;
            if(y->parent!=z){
                transPlant(y, x);
                y->right =z->right;
                z->right->parent = y;
            }
            transPlant(z, y);
            y->left = z->left;
            z->left->parent = y;
            y->color = z->color;
        }

        if(y_origin_color==Color::BLACK){
            deleteFixup(x);
        }
        delete z;
        size--;
    }
public:
    void print(){
        if(root!=nil)inorderPrint(root);
        cout<<endl;
    }
    size_t getSize(){
        return size;
    }
    void clear(){
        if(root!=nil)destroy(root);

    }
    void remove(const Key& key){
        RBNode* node = searchRBNode(key);
        if(node==nil){
            return;
        }
        deleteRBNode(node);
    }
    bool empty() const {
        return size == 0;
    }
    void insert(const Key& key,const Value& value){
        insertRBNode(key,value);
    }
    Value* at(const Key& key){
        RBNode* node = searchRBNode(key);
        if(node!=nil){
            return &node->value;
        }else{

            return nullptr;
        }
    }
};

template <typename Key, typename Value> 

class Map {
public:
  Map() : rbTree() {}

  void insert(const Key &key, const Value &value) { rbTree.insert(key, value); }

  void erase(const Key &key) { rbTree.remove(key); }

  size_t size() { return rbTree.getSize(); }

  bool empty() { return rbTree.empty(); }

  bool contains(const Key &key) { return rbTree.at(key) != nullptr; }

  Value &at(const Key &key) {
    Value *foundVal = rbTree.at(key);
    if (foundVal) {
      return *foundVal;
    } else {
      throw std::out_of_range("Key not found");
    }
  }

  Value &operator[](const Key &key) {
    Value *foundVal = rbTree.at(key);
    if (foundVal) {
      // 如果键存在，返回关联的值
      return *foundVal; // 或者，返回与找到的键关联的值
    } else {
      // 如果键不存在，插入新键值对，并返回新插入的值的引用
      Value defaultValue;
      rbTree.insert(key, defaultValue);
      return *rbTree.at(key);
    }
  }

private:
  RedBlackTree<Key, Value> rbTree;
};
// main函数
int main() {
  Map<int, int> map;

  int N;
  std::cin >> N;
  getchar();

  std::string line;
  for (int i = 0; i < N; i++) {
    std::getline(std::cin, line);
    std::istringstream iss(line);
    std::string command;
    iss >> command;
    
    int key;
    int value;

    if (command == "insert") {
      iss >> key >> value;
      map.insert(key, value);
    }

    if (command == "erase") {
      iss >> key;
      map.erase(key);
    }

    if (command == "contains") {
      iss >> key;
      if (map.contains(key)) {
        std::cout << "true" << std::endl;
      } else {
        std::cout << "false" << std::endl;
      }
    }

    if (command == "at") {
      iss >> key;
      try {
        std::cout << map.at(key) << std::endl;
      } catch (const std::out_of_range& e) {
        std::cout << "not exist" << std::endl;
      }
    }

    // size 命令
    if (command == "size") {
      std::cout << map.size() << std::endl;
    }

    // empty 命令
    if (command == "empty") {
      if (map.empty()) {
        std::cout << "true" << std::endl;
      } else {
        std::cout << "false" << std::endl;
      }
    }
  }
  return 0;
}
```

二、set
-----

### 1、了解

[看这篇文章就够了](https://blog.csdn.net/2401_82911768/article/details/146383612)

### 2、代码

```cpp

#include <cstddef>
#include <iostream>
#include <sstream>
#include <stdexcept>
#include <string>
#include <utility>
using namespace std;
enum class Color{BLACK,RED};
template<class Key,class Value>
class RBTree{
    class RBNode{
    public:
        RBNode* left;
        RBNode* right;
        RBNode* parent;
        Key key;
        Value value;
        Color color;
        RBNode(const Key& key,const Value& value)
                :left(nullptr),right(nullptr),parent(nullptr),
                 color(Color::RED),key(key),value(value){

        }
        RBNode():left(nullptr),right(nullptr),parent(nullptr),
                 color(Color::BLACK){

        }
    };
    RBNode* nil;
    RBNode* root;
    size_t size;
public:
    RBTree():nil(new RBNode()),root(nil),size(0){

    }
    ~RBTree(){
        if(root!=nil)destroy(root);
        delete nil;
    }
private:
    void inorderPrint(RBNode* node){
        if(node!=nil){
            inorderPrint(node->left);
            cout<<node->key<<" "<<node->value<<" ";
            inorderPrint(node->right);
        }
    }
    void destroy(RBNode* node){
        if(node!=nil){
            destroy(node->left);
            destroy(node->right);
            delete node;
        }
        size = 0;
    }
    RBNode* searchRBNode(const Key& key){
        RBNode* cur = root;
        while(cur!=nil){
            if(cur->key>key){
                cur = cur->left;
            }else if(cur->key<key){
                cur = cur->right;
            }else{
                return cur;
            }
        }
        return nil;
    }
    void leftRotate(RBNode* x){
        RBNode* y = x->right;
        x->right = y->left;
        if(y->left!=nil){
            y->left->parent = x;
        }
        y->parent = x->parent;
        if(x->parent!=nil){
            if(x->parent->left==x){
                x->parent->left = y;
            }else{
                x->parent->right = y;
            }
        }else{
            root = y;
        }
        y->left =x;
        x->parent = y;
    }
    void rightRotate(RBNode* y){
        RBNode* x = y->left;
        y->left = x->right;
        if(x->right!=nil){
            x->right->parent = y;
        }
        x->parent = y->parent;
        if(y->parent!=nil){
            if(y->parent->right==y){
                y->parent->right = x;
            }else{
                y->parent->left = x;
            }
        }else{
            root = x;
        }
        x->right = y;
        y->parent = x;
    }
    RBNode* minimum(RBNode* node){
        while(node->left!=nil){
            node=node->left;
        }
        return node;
    }
    void transPlant(RBNode* u,RBNode* v){
        if(u->parent==nil){
            root = v;
        }else if(u->parent->left==u){
            u->parent->left=v;
        }else{
            u->parent->right=v;
        }
        v->parent = u->parent;
    }
    void insertFixup(RBNode* node){
        RBNode* parent = node->parent;
        RBNode* grandparent;
        RBNode* uncle;
        RBNode* tmp;
        while(parent!=nil&&parent->color==Color::RED){
            grandparent = parent->parent;
            if(parent->parent->left==parent){
                uncle = grandparent->right;
            }else{
                uncle = grandparent->left;
            }
            if(uncle->color==Color::RED){
                parent->color=Color::BLACK;
                uncle->color=Color::BLACK;
                grandparent->color=Color::RED;
                node = grandparent;
                parent = node->parent;
                grandparent=parent->parent;
                continue;
            }
            if(grandparent->left==parent){//L
                if(parent->right==node){//R
                    leftRotate(parent);
                    tmp = parent;
                    parent = node;
                    node = parent;
                }
                rightRotate(grandparent);
                grandparent->color = Color::RED;
                parent->color = Color::BLACK;
            }else{//R
                if(parent->left==node){//L
                    rightRotate(parent);
                    tmp = parent;
                    parent = node;
                    node = parent;
                }
                leftRotate(grandparent);
                grandparent->color = Color::RED;
                parent->color = Color::BLACK;
            }
        }
        root->color=Color::BLACK;
    }
    void insertRBNode(const Key& key,const Value& value){
        RBNode* node = new RBNode(key,value);
        node->left = nil;
        node->right = nil;
        node->parent = nil;
        if(node==nil){
            return ;
        }

        RBNode* cur = root;
        RBNode* pre = nullptr;
        while(cur!=nil){
            pre = cur;
            if(key>cur->key){
                cur = cur->right;
            }else if(cur->key>key){
                cur = cur->left;
            }else{
                delete node;
                return ;
            }
        }
        if(pre==nullptr){
            root = node;
        }else{
            node->parent = pre;
            if(pre->key>key){
                pre->left = node;
            }else{
                pre->right = node;
            }
        }
        size++;
        insertFixup(node);
    }
    void deleteFixup(RBNode* x){
        RBNode* w;
        while((x!=root)&&(x->color==Color::BLACK)){
            if(x->parent->left == x){
                w = x->parent->right;
                if(w->color==Color::RED){
                    w->color=Color::BLACK;
                    x->parent->color = Color::RED;
                    leftRotate(x->parent);
                    w = x->parent->right;
                }
                if(w->left->color==Color::BLACK&&w->right->color==Color::BLACK){
                    w->color = Color::RED;
                    x = x->parent;
                }else{
                    if(w->right->color==Color::BLACK){//RL
                        w->left->color = Color::BLACK;
                        w->color = Color::RED;
                        w->parent->color =Color::BLACK;
                        rightRotate(w);
                        w = x->parent->right;
                    }
                    w->right->color = w->color;
                    w->color= w->parent->color;
                    w->parent->color = Color::BLACK;
                    leftRotate(x->parent);
                    x = root;
                }
            }else{
                w = x->parent->left;
                if(w->color==Color::RED){
                    w->color=Color::BLACK;
                    x->parent->color = Color::RED;
                    rightRotate(x->parent);
                    w = x->parent->left;
                }
                if(w->left->color==Color::BLACK&&w->right->color==Color::BLACK){
                    w->color = Color::RED;
                    x = x->parent;
                }else{
                    if(w->left->color==Color::BLACK){//LR
                        w->right->color = Color::BLACK;
                        w->color = Color::RED;
                        x->parent->color = Color::BLACK;
                        rightRotate(w);
                        w = x->parent->left;
                    }
                    w->left->color = w->color;
                    w->color= w->parent->color;
                    w->parent->color = Color::BLACK;
                    leftRotate(x->parent);
                    x = root;
                }
            }
        }
        x->color = Color::BLACK;
    }

    void deleteRBNode(RBNode* z){
        RBNode* x;//替换
        RBNode* y= z;//删除
        Color y_origin_color = y->color;
        if(y->left==nil){
            x = y->right;
            transPlant(y, x);
        }else if(y->right==nil){
            x = y->left;
            transPlant(y, x);
        }else{
            y = z->right;
            y = minimum(y);
            y_origin_color = y->color;
            x = y->right;
            if(y->parent!=z){
                transPlant(y, x);
                y->right =z->right;
                z->right->parent = y;
            }
            transPlant(z, y);
            y->left = z->left;
            z->left->parent = y;
            y->color = z->color;
        }

        if(y_origin_color==Color::BLACK){
            deleteFixup(x);
        }
        delete z;
        size--;
    }
public:
    void print(){
        if(root!=nil)inorderPrint(root);
        cout<<endl;
    }
    size_t getSize(){
        return size;
    }
    void clear(){
        if(root!=nil)destroy(root);

    }
    void remove(const Key& key){
        RBNode* node = searchRBNode(key);
        if(node==nil){
            return;
        }
        deleteRBNode(node);
    }
    bool empty() const {
        return size == 0;
    }
    void insert(const Key& key,const Value& value){
        insertRBNode(key,value);
    }
    Value* at(const Key& key){
        RBNode* node = searchRBNode(key);
        if(node!=nil){
            return &node->value;
        }else{

            return nullptr;
        }
    }
};

template <typename Key>
class Set {
public:
    Set() : rbTree() {}

    void insert(const Key &key) { rbTree.insert(key, key); }

    void erase(const Key &key) { rbTree.remove(key); }

    size_t size() { return rbTree.getSize(); }

    bool empty() { return rbTree.empty(); }

    bool contains(const Key &key) { return rbTree.at(key) != nullptr; }

private:
    RBTree<Key, Key> rbTree;
};

int main() {
    Set<int> mySet;

    int N;
    std::cin >> N;
    getchar();

    std::string line;
    for (int i = 0; i < N; i++) {
        std::getline(std::cin, line);
        std::istringstream iss(line);
        std::string command;
        iss >> command;

        int element;

        if (command == "insert") {
            iss >> element;
            mySet.insert(element);
        }

        if (command == "earse") {
            iss >> element;
            mySet.erase(element);
        }

        if (command == "contains") {
            iss >> element;
            std::cout << (mySet.contains(element) ? "true" : "false") << std::endl;
        }

        if (command == "size") {
            std::cout << mySet.size() << std::endl;
        }

        if (command == "empty") {
            std::cout << (mySet.empty() ? "true" : "false") << std::endl;
        }
    }
    return 0;
}
```

三、unordered\_map的理解
-------------------

```cpp
#include <cstddef>
#include <algorithm>
#include <cstddef>
#include <functional>
#include <iostream>
#include <list>
#include <utility>
#include <vector>
#include <sstream>
#include <string>

template <typename Key, typename Value, typename Hash = std::hash<Key>>
class HashTable {

  class HashNode {
  public:
    Key key;
    Value value;

    // 从Key构造节点，Value使用默认构造
    explicit HashNode(const Key &key) : key(key), value() {}

    // 从Key和Value构造节点
    HashNode(const Key &key, const Value &value) : key(key), value(value) {}

    // 比较算符重载，只按照key进行比较
    bool operator==(const HashNode &other) const { return key == other.key; }

    bool operator!=(const HashNode &other) const { return key != other.key; }

    bool operator<(const HashNode &other) const { return key < other.key; }

    bool operator>(const HashNode &other) const { return key > other.key; }

    bool operator==(const Key &key_) const { return key == key_; }

    void print() const {
      std::cout << "key: " << key << " value: " << value << " ";
    }
  };

private:
  using Bucket = std::list<HashNode>; // 定义桶的类型为存储键的链表
  std::vector<Bucket> buckets;        // 存储所有桶的动态数组
  Hash hashFunction;                  // 哈希函数对象
  size_t tableSize;                   // 哈希表的大小，即桶的数量
  size_t numElements;                 // 哈希表中元素的数量

  float maxLoadFactor = 0.75; // 默认的最大负载因子

  // 计算键的哈希值，并将其映射到桶的索引
  size_t hash(const Key &key) const { return hashFunction(key) % tableSize; }

  // 当元素数量超过最大负载因子定义的容量时，增加桶的数量并重新分配所有键
  void rehash(size_t newSize) {
    std::vector<Bucket> newBuckets(newSize); // 创建新的桶数组

    for (Bucket &bucket : buckets) {      // 遍历旧桶
      for (HashNode &hashNode : bucket) { // 遍历桶中的每个键
        size_t newIndex =
            hashFunction(hashNode.key) % newSize; // 为键计算新的索引
        newBuckets[newIndex].push_back(hashNode); // 将键添加到新桶中
      }
    }
    buckets = std::move(newBuckets); // 使用移动语义更新桶数组
    tableSize = newSize;             // 更新哈希表大小
  }

public:
  // 构造函数初始化哈希表
  HashTable(size_t size = 10, const Hash &hashFunc = Hash())
      : buckets(size), hashFunction(hashFunc), tableSize(size), numElements(0) {
  }

  // 插入键到哈希表中
  void insert(const Key &key, const Value &value) {
    if ((numElements + 1) > maxLoadFactor * tableSize) { // 检查是否需要重哈希
      rehash(tableSize * 2); // 重哈希，桶数量翻倍
    }
    size_t index = hash(key);                     // 计算键的索引
    std::list<HashNode> &bucket = buckets[index]; // 获取对应的桶
    // 如果键不在桶中，则添加到桶中
    if (std::find(bucket.begin(), bucket.end(), key) == bucket.end()) {
      bucket.push_back(HashNode(key, value));
      ++numElements; // 增加元素数量
    }
  }

  void insertKey(const Key &key) { insert(key, Value{}); }

  // 从哈希表中移除键
  void erase(const Key &key) {
    size_t index = hash(key);      // 计算键的索引
    auto &bucket = buckets[index]; // 获取对应的桶
    auto it = std::find(bucket.begin(), bucket.end(), key); // 查找键
    if (it != bucket.end()) {                               // 如果找到键
      bucket.erase(it); // 从桶中移除键
      numElements--;    // 减少元素数量
    }
  }

  // 查找键是否存在于哈希表中
  Value *find(const Key &key) {
    size_t index = hash(key);      // 计算键的索引
    auto &bucket = buckets[index]; // 获取对应的桶
    // 返回键是否在桶中
    auto ans = std::find(bucket.begin(), bucket.end(), key);
    if (ans != bucket.end()) {
      return &ans->value;
    };
    return nullptr;
  }

  // 获取哈希表中元素的数量
  size_t size() const { return numElements; }

  // 打印哈希表中的所有元素
  void print() const {
    std::cout << "HashTable elements:" << std::endl;
    for (size_t i = 0; i < buckets.size(); ++i) {
      std::cout << "Bucket " << i << ": ";
      for (const HashNode &element : buckets[i]) {
        element.print();
      }
      std::cout << std::endl;
    }
  }
  void clear() {
    this->buckets.clear();
    this->numElements = 0;
  }
};

template <typename Key, typename Value> class Unordered_map {
private:
  HashTable<Key, Value> hashtable;

public:
  Unordered_map() : hashtable(){};

  ~Unordered_map() {}

  bool empty() const noexcept { return hashtable.size() == 0; }

  size_t size() const noexcept { return hashtable.size(); }

  void clear() noexcept { hashtable.clear(); }

  void insert(const Key &key, const Value &value) {
    hashtable.insert(key, value);
  }

  void erase(const Key &key) { hashtable.erase(key); }

  bool find(const Key &key) { return hashtable.find(key) != nullptr; }

  Value &operator[](const Key &key) {
    Value *ans = hashtable.find(key);
    if (ans != nullptr) {
      return *ans;
    }
    hashtable.insertKey(key);
    ans = hashtable.find(key);
    return *ans;
  }
};

int main() {
  Unordered_map<int, int> map;

  int N;
  std::cin >> N;
  getchar();

  std::string line;
  
  for (int i = 0; i < N; i++) {
    std::getline(std::cin, line);
    std::istringstream iss(line);
    std::string command;
    iss >> command;
    
    int key;
    int value;

    if (command == "insert") {
      iss >> key >> value;
      map.insert(key, value);
    }

    if (command == "erase") {
      iss >> key;
      map.erase(key);
    }

    if (command == "find") {
      iss >> key;
      if (map.find(key)) {
        std::cout << "true" << std::endl;
      } else {
        std::cout << "false" << std::endl;
      }
    }

    // size 命令
    if (command == "size") {
      std::cout << map.size() << std::endl;
    }

    // empty 命令
    if (command == "empty") {
      if (map.empty()) {
        std::cout << "true" << std::endl;
      } else {
        std::cout << "false" << std::endl;
      }
    }
  }
  return 0;
}
```

四、unordered\_set的理解
-------------------

```cpp
#include <algorithm>
#include <cstddef>
#include <functional>
#include <iostream>
#include <list>
#include <utility>
#include <vector>
#include <sstream>
#include <string>

template <typename Key, typename Value, typename Hash = std::hash<Key>>
class HashTable {

  class HashNode {
  public:
    Key key;
    Value value;

    // 从Key构造节点，Value使用默认构造
    explicit HashNode(const Key &key) : key(key), value() {}

    // 从Key和Value构造节点
    HashNode(const Key &key, const Value &value) : key(key), value(value) {}

    // 比较算符重载，只按照key进行比较
    bool operator==(const HashNode &other) const { return key == other.key; }

    bool operator!=(const HashNode &other) const { return key != other.key; }

    bool operator<(const HashNode &other) const { return key < other.key; }

    bool operator>(const HashNode &other) const { return key > other.key; }

    bool operator==(const Key &key_) const { return key == key_; }

    void print() const {
      std::cout << key << " "<< value << " ";
    }
  };

private:
  using Bucket = std::list<HashNode>; // 定义桶的类型为存储键的链表
  std::vector<Bucket> buckets;        // 存储所有桶的动态数组
  Hash hashFunction;                  // 哈希函数对象
  size_t tableSize;                   // 哈希表的大小，即桶的数量
  size_t numElements;                 // 哈希表中元素的数量

  float maxLoadFactor = 0.75; // 默认的最大负载因子

  // 计算键的哈希值，并将其映射到桶的索引
  size_t hash(const Key &key) const { return hashFunction(key) % tableSize; }

  // 当元素数量超过最大负载因子定义的容量时，增加桶的数量并重新分配所有键
  void rehash(size_t newSize) {
    std::vector<Bucket> newBuckets(newSize); // 创建新的桶数组

    for (Bucket &bucket : buckets) {      // 遍历旧桶
      for (HashNode &hashNode : bucket) { // 遍历桶中的每个键
        size_t newIndex =
            hashFunction(hashNode.key) % newSize; // 为键计算新的索引
        newBuckets[newIndex].push_back(hashNode); // 将键添加到新桶中
      }
    }
    buckets = std::move(newBuckets); // 使用移动语义更新桶数组
    tableSize = newSize;             // 更新哈希表大小
  }

public:
  // 构造函数初始化哈希表
  HashTable(size_t size = 10, const Hash &hashFunc = Hash())
      : buckets(size), hashFunction(hashFunc), tableSize(size), numElements(0) {
  }

  // 插入键到哈希表中
  void insert(const Key &key, const Value &value) {
    if ((numElements + 1) > maxLoadFactor * tableSize) { // 检查是否需要重哈希
      if (tableSize == 0) tableSize = 1;
      rehash(tableSize * 2); // 重哈希，桶数量翻倍
    }
    size_t index = hash(key);                     // 计算键的索引
    std::list<HashNode> &bucket = buckets[index]; // 获取对应的桶
    // 如果键不在桶中，则添加到桶中
    if (std::find(bucket.begin(), bucket.end(), key) == bucket.end()) {
      bucket.push_back(HashNode(key, value));
      ++numElements; // 增加元素数量
    }
  }

  void insertKey(const Key &key) { insert(key, Value{}); }

  // 从哈希表中移除键
  void erase(const Key &key) {
    size_t index = hash(key);      // 计算键的索引
    auto &bucket = buckets[index]; // 获取对应的桶
    auto it = std::find(bucket.begin(), bucket.end(), key); // 查找键
    if (it != bucket.end()) {                               // 如果找到键
      bucket.erase(it); // 从桶中移除键
      numElements--;    // 减少元素数量
    }
  }

  // 查找键是否存在于哈希表中
  Value *find(const Key &key) {
    size_t index = hash(key);      // 计算键的索引
    auto &bucket = buckets[index]; // 获取对应的桶
    // 返回键是否在桶中
    auto ans = std::find(bucket.begin(), bucket.end(), key);
    if (ans != bucket.end()) {
      return &ans->value;
    };
    return nullptr;
  }

  // 获取哈希表中元素的数量
  size_t size() const { return numElements; }

  // 打印哈希表中的所有元素
  void print() const {
    for (size_t i = 0; i < buckets.size(); ++i) {
      for (const HashNode &element : buckets[i]) {
        element.print();
      }
      std::cout << std::endl;
    }
  }
  void clear() {
    this->buckets.clear();
    this->numElements = 0;
    this->tableSize = 0;
  }
};

template <typename Key> class Unordered_set {
public:
  Unordered_set() : hashtable(){};

  ~Unordered_set() {}

  bool empty() const noexcept { return hashtable.size() == 0; }

  size_t size() const noexcept { return hashtable.size(); }

  void clear() noexcept { hashtable.clear(); }

  void insert(Key key) { hashtable.insertKey(key); }

  void erase(Key key) { hashtable.erase(key); }

  bool find(const Key &key) { return hashtable.find(key) != nullptr; }

private:
  HashTable<Key, Key> hashtable;
};

int main() {
  Unordered_set<int> mySet;
  int N;
  std::cin >> N;
  getchar();

  std::string line;
  for (int i = 0; i < N; i++) {
    std::getline(std::cin, line);
    std::istringstream iss(line);
    std::string command;
    iss >> command;

    int element;

    if (command == "insert") {
        iss >> element;
        mySet.insert(element);
    }

    if (command == "erase") {
        iss >> element;
        mySet.erase(element);
    }

    if (command == "find") {
        iss >> element;
        std::cout << (mySet.find(element) ? "true" : "false") << std::endl;
    }

    if (command == "size") {
        std::cout << mySet.size() << std::endl;
    }

    if (command == "empty") {
        std::cout << (mySet.empty() ? "true" : "false") << std::endl;
    }
  }
  return 0;
}
```

本文转自 <https://blog.csdn.net/2401_82911768/article/details/147926683>，如有侵权，请联系删除。