散列表
---

### 一、首先了解一下平衡二叉树

增删改查的时间复杂度为lognlognlogn。 平衡的目的是增删后，保证下次搜索能稳定排除一半的数据。比如：10亿个节点，最多比较30次。  
**总结：** 通过比较保证有序，通过每次排除一半的元素达到快速索引的目的。

### 二、散列表

### 1、组成

①哈希函数  
②数组

#### 1.1、hash函数

①：**冲突情况** ：将两个或两个以上的不同key映射到同一地址。  
②：映射函数：Hash（key）=addr； 所以它的作用就是完成映射关系。

##### 1.1.1、选择hash（不需要看源码，明白怎么选择就行）

* 计算速度快
* 强随机分布 （等概率、均匀地分布整个地址空间）
* siphash主要解决字符串接近的强随机分布
* murmurhash2常用
* cityhash常用

##### 1.1.2、hash操作

* 插入
* 查找

##### 1.1.3、hash负载因子

* 数组存储元素的个数/数组长度。
* 用来形容散列表的存储密度；
* 负载因子越小，冲突越小，负载因子越大，冲突越大。
* 主要描述冲突程度

##### 1.1.4、hash冲突解决方案

方法一：拉链法 （链表法） 将具有相同的addr的key，可以用链表链接。  
**但是负载因子要在合理范围内。** ![image.png](https://p6-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/7e4e37b51f5d4ced9c33756c4dd640d5~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAgRWNob3p5dGx4bA==:q75.awebp?rk3s=f64ab15b&x-expires=1746694545&x-signature=qqHTsvbdmJ7kFFDeloQ5VEqOZKQ%3D)  
方法二：开发寻址法

* 将所有的数据直接存储在哈希数组中。如果冲突就采用某种算法来改变位置。  
  算法有多种思路：
* 比如 i+1，i+2，i+3等等 或者 i^1,i^2,i^3等等。  
  但是他们会出现**hash聚集**，也就是近似值的hash值很接近，那么位置也接近。聚集的话，就会在一片区域内，查找，这片区域数据太多了，时间复杂从O（1）变O（n）。所以可以使用**双重哈希**解决。 **但是负载因子要在合理范围内。**

![image.png](https://p6-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/a19294eff13640a3a19770f9571eae4f~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAgRWNob3p5dGx4bA==:q75.awebp?rk3s=f64ab15b&x-expires=1746694545&x-signature=CnegKmvNVD%2BEMvGSHYMRxo%2Fp%2FsE%3D)

> 如果不在合理范围内  
> used：实际存储元素的个数。  
> 方法一：扩容，used > size (避免冲突)  
> 方法二：缩容：used < 0.1size (不能解决冲突) 操作扩容或缩容后，需要创建hash，因为算法会改变。类似顺序表的扩容操作。

### 2、STL中的散列表

#### 2.1、STL中常使用的函数

unordered\_map、unordered\_multimap、 unordered\_set，unordered\_multiset这4个都是用的散列表 。

#### 2.2、实现原理

![image.png](https://p6-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/9c2981ffe6f944bfad472524b704b17e~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAgRWNob3p5dGx4bA==:q75.awebp?rk3s=f64ab15b&x-expires=1746694545&x-signature=ZStBS0SJjz5n22fMtZWgjVxZvxk%3D)

![image.png](https://p6-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/0b09626fe3434e10883d7457c24a6595~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAgRWNob3p5dGx4bA==:q75.awebp?rk3s=f64ab15b&x-expires=1746694545&x-signature=ZxtVP5vo7AUp0uZEL%2BvviN1MHo8%3D)

### 三、布隆过滤器

#### 1、背景（使用情况）

* 只想知道key存在不存在，不想知道内容。

#### 2、构成

位图和多个hash函数。  
首先假设：它是由一个二进制构成的数组，里面存储0或1，所以保密性很好。然后假设有3个哈希函数来计算，数据要存储的数组下标，（不一定是3个哈希，这里只是举例，便于理解），经过3个哈希函数得到3个下标，将③个下标的数组标位1，说明存储好了。  
但是如果我们要删除的话，就会出现问题。比如：hello和你好经过hash运算都要存储在下标2的位置。那么现在我们取出了hello，2的下标改为0，但是没取出你好，就会导致误以为删除了你好。

> a、优点：  
> ①具有安全性，存储的不是数据本身。  
> ②保密性，只看得到0和1  
> ③速度快，哈希计算  
> b、缺点  
> ①删除容易误判，可以多用1个哈希来计算  
> ②还有一种误判，如果存储的hello明明是用另一个位图计算，但是这个位图里，能确定它存在，就会导致误以为存在了。

#### 3、应用分析

在实际应用中，需要但是个hash函数？要分配多少空间的位图？预期存储多少元素？如何控制误差？  
[可以使用该网站求以下内容，bloom计算](https://link.juejin.cn?target=https%3A%2F%2Fhur.st%2Fbloomfilter "https://hur.st/bloomfilter")

> n：预期布隆过滤器中元素的个数。  
> p：可以失误的概率  
> m：位图所占的空间  
> k：hash函数的个数  
> 公式如下：  
> n=ceil（m/(-k/log(1-exp(log(p)/k)))）  
> p=pow(1-exp(-k/(m/n)),k)  
> m=ceil((n\*log(p))/log(1/pow(2,log(2))))  
> k=round((m/n)\*log(2));

### 4、分布式一致性哈希

#### 1、哈希环

* 将节点和数据通过哈希函数映射到一个固定的范围（通常是一个环）。
* 节点和数据的位置由哈希值决定

#### 2、数据定位

* 数据通过哈希函数映射到环上的某个位置。
* 数据存储在顺时针方向找到的第一个节点上。

#### 3、节点增减

* 增加节点时，只有新节点到其前驱节点之间的数据需要迁移。
* 删除节点时，只有被删除节点的数据需要迁移到其后继节点。

#### 4、 哈希迁移

**问题描述**

在分布式缓存系统中，当节点增加或删除时，需要将部分数据从旧节点迁移到新节点。如果使用传统的哈希取模方法，节点的增减会导致大量数据重新映射，影响系统性能。

#### 5、哈希偏移

在一致性哈希中，如果节点数量较少，可能会导致数据分布不均匀，某些节点负载过高，而其他节点负载较低。

##### 5.1、虚拟节点的解决方案

* 通过引入虚拟节点，将一个物理节点映射为多个虚拟节点，分散在哈希环上。
* 虚拟节点的数量可以远大于物理节点的数量，从而使得数据分布更加均匀。

##### 5.2、实现方式

* 为每个物理节点生成多个虚拟节点，例如在节点 IP 和端口后加上虚拟编号（如 `192.168.1.1:8080:1`, `192.168.1.1:8080:2`）。
* 对虚拟节点进行哈希映射，数据定位时找到对应的虚拟节点，再映射到物理节点。

##### 5.3、虚拟节点优点

###### 数据分布均匀

* 虚拟节点将物理节点的负载分散到哈希环的多个位置，避免数据倾斜。

###### 动态扩容

* 增加物理节点时，只需为其分配虚拟节点，**数据迁移量较少**。

###### 容错性

* 删除物理节点时，其虚拟节点对应的数据会迁移到其他物理节点，系统仍然保持平衡。
