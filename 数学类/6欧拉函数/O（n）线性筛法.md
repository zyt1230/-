#### 1. **变量定义**：

* `primes[N]`：存储所有筛出来的质数。

* `euler[N]`：存储每个数的欧拉函数值。

* `st[N]`：标记数组，`st[x] = true` 表示 `x` 不是质数（被筛掉）。

* `cnt`：记录当前筛出的质数个数。

#### 2. **`get_eulers` 函数**：

* **初始化**：
  
  * `euler[1] = 1`，因为 1 的欧拉函数值为 1。

* **线性筛法**：
  
  * 遍历从 2 到 `n` 的所有数 `i`。
  
  * 如果 `i` 没有被筛掉（`st[i] == false`），则 `i` 是质数，将其加入 `primes` 数组，并设置 `euler[i] = i - 1`（因为质数 `i` 的欧拉函数值为 `i - 1`）。
  
  * 对于每个质数 `primes[j]`，筛掉 `primes[j] * i`，并根据 `i` 和 `primes[j]` 的关系更新 `euler[t]`：
    
    * 如果 `i % primes[j] == 0`，说明 `primes[j]` 是 `i` 的最小质因数，此时 `euler[t] = euler[i] * primes[j]`。
    
    * 否则，`euler[t] = euler[i] * (primes[j] - 1)`。

* **计算结果**：
  
  * 遍历从 1 到 `n` 的所有数，累加它们的欧拉函数值到 `res` 中，最后返回 `res`

#### 3. **`main` 函数**：

* 输入一个整数 `n`，调用 `get_eulers(n)` 计算从 1 到 `n` 的欧拉函数值之和，并输出结果。

### 代码逻辑详解：

#### 1. **线性筛法**：

线性筛法的核心思想是每个合数只会被它的最小质因数筛掉一次，从而保证时间复杂度为 O(n)。

* 对于每个数 `i`，如果它是质数，则直接加入 `primes` 数组。

* 对于每个质数 `primes[j]`，筛掉 `primes[j] * i`。
  
  * 如果 `i % primes[j] == 0`，说明 `primes[j]` 是 `i` 的最小质因数，此时 `primes[j] * i` 的最小质因数也是 `primes[j]`。
  
  * 否则，`primes[j]` 是 `primes[j] * i` 的最小质因数。

#### 2. **欧拉函数的性质**：

* 如果 `p` 是质数，则 `φ(p) = p - 1`。

* 如果 `n = p^k`（`p` 是质数），则 `φ(n) = p^k - p^(k-1)`。

* 如果 `n = p * q`（`p` 和 `q` 互质），则 `φ(n) = φ(p) * φ(q)`。

在代码中，根据 `i` 和 `primes[j]` 的关系，利用上述性质更新 `euler[t]`。

```cpp
#include <iostream>
#include <unordered_set>
using namespace std;
typedef long  long ll;
const int N = 1000010;
int primes[N], cnt;     // primes[]存储所有素数
int euler[N];           // 存储每个数的欧拉函数
bool st[N];         // st[x]存储x是否被筛掉


ll get_eulers(int n)
{
    euler[1] = 1;
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            euler[i] = i - 1;//N-1与N中互质
        }
        for (int j = 0; primes[j] <= n / i; j ++ )
        {
            int t = primes[j] * i;
            st[t] = true;
            if (i % primes[j] == 0)
            {
                euler[t] = euler[i] * primes[j];
                break;
            }
            euler[t] = euler[i] * (primes[j] - 1);
        }
    }
    ll res =0;
    for(int i =1;i<=n;i++)res += euler[i];
    return res;
}

int main()
{
    int n;
    cin>>n;
    cout<<get_eulers(n)<<endl;
    return 0;
}
//输入6
//得到12
```

#### 执行过程：

1. 初始化 `euler[1] = 1`。

2. 遍历 `i` 从 2 到 6：
   
   * `i = 2`：质数，`euler[2] = 1`。
   
   * `i = 3`：质数，`euler[3] = 2`。
   
   * `i = 4`：合数，`euler[4] = 2`。
   
   * `i = 5`：质数，`euler[5] = 4`。
   
   * `i = 6`：合数，`euler[6] = 2`。

3. 计算 `res = euler[1] + euler[2] + euler[3] + euler[4] + euler[5] + euler[6] = 1 + 1 + 2 + 2 + 4 + 2 = 12`。
