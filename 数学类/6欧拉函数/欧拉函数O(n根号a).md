![09b87f89-0908-4abd-bd86-8f42c216bb61](file:///C:/Users/LEGION/Pictures/09b87f89-0908-4abd-bd86-8f42c216bb61.png)

这段代码的主要功能是计算给定整数的欧拉函数值（Euler's Totient Function），通常记作 φ(n)。欧拉函数 φ(n) 表示小于或等于 n 的正整数中与 n 互质的数的个数。

### 代码分析：

1. **输入部分**：
   
   * 首先输入一个整数 `n`，表示接下来要处理的数的个数。
   
   * 然后通过 `while(n--)` 循环，依次处理每个数。

2. **欧拉函数计算**：
   
   * 对于每个数 `a`，初始化 `res` 为 `a`。
   
   * 通过 `for` 循环遍历从 2 到 `sqrt(a)` 的所有整数 `i`，检查 `i` 是否是 `a` 的因数。
   
   * 如果 `i` 是 `a` 的因数，则更新 `res` 为 `res / i * (i - 1)`，并将 `a` 中的所有 `i` 因子去除（即 `while(a % i == 0) a /= i`）。
   
   * 如果最后 `a` 仍然大于 1，说明 `a` 本身是一个质数，此时更新 `res` 为 `res / a * (a - 1)`。

3. **输出部分**：
   
   * 对于每个数 `a`，输出计算得到的 `res`。

### 解释：

* 对于 `a = 3`，小于等于 3 且与 3 互质的数有 1 和 2，所以 φ(3) = 2。

* 对于 `a = 6`，小于等于 6 且与 6 互质的数有 1 和 5，所以 φ(6) = 2。

* 对于 `a = 4`，小于等于 4 且与 4 互质的数有 1 和 3，所以 φ(4) = 2。

```cpp
#include <iostream>
using namespace std;

int main()
{
    int n;
    cin >> n;
    while (n--) {
        int a;
        cin >> a;
        int res = a;
        for (int i = 2; i <= a / i; i++) {
            if (a % i == 0) {
                res = res / i * (i - 1);
                while (a % i == 0) a /= i;//去除因子
            }
        }
        if (a > 1) res = res / a * (a - 1);//a本身也是质数
        cout << res << endl;
    }
    return 0;
}
//输入 3 3 6 4
//res 2 2 4
```
