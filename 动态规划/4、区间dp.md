# 区间dp：

 就是对于区间的一种动态规划，它将问题划分为若干个子区间，并通过定义状态和状态转移方程来求解每个子区间的最优解，最终得到整个区间的最优解。

对于某个区间，它的合并方式可能有很多种，我们需要去枚举所有的方式，通常是去枚举区间的分割点，找到最优的方式(一般是找最少消耗)。

# 区间 DP 有以下特点：

**合并**：即将两个或多个部分进行整合，当然也可以反过来；

**特征**：能将问题分解为能两两合并的形式；

**求解**：对整个问题设最优值，枚举合并点，将问题分解为左右两个部分，最后合并两个部分的最优值得到原问题的最优值。

# 核心思路：

状态：dp[i][j]表示【i,j】的最小消耗

通常都是先枚举区间长度，区间长度为1就不用合并，所以从2开始枚举，然后枚举左端点，那么右端点就为左端点加区间长度-1，再枚举分割点k（即子区间的终点和起点），最后计算不同分割点 k 的情况下，合并区间的消耗，dp[i][j]选择其中的最小消耗。（需要注意的是要记得根据题意给上初值）

长区间肯定由短区间转移得到，所以先算短区间。（**分割点k:是从i到j-1,因为k+1≤j**)。

- 固定模版

![1d4d0966-1540-4342-9f02-dfa7ef4b455e](file:///C:/Users/LEGION/Pictures/1d4d0966-1540-4342-9f02-dfa7ef4b455e.png)

-------

[P1775 石子合并（弱化版） - 洛谷](https://www.luogu.com.cn/problem/P1775)

2               5 3 1

2 5            3  1

2 5 3         1

[视频讲解](https://www.bilibili.com/video/BV1ov4y1y7et/?spm_id_from=333.337.search-card.all.click&vd_source=c6d25b8c148a15b1c45b8da5d613a042)

```cpp
const int INF = 0x3f3f3f3f;  // 无穷大
const int NEG_INF = -0x3f3f3f3f;  // 无穷小
```

- 答案

```cpp
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
#define N 305
int n;
int a[N];
int dp[N][N];
int sum[N];
int main(){
    scanf("%d",&n);
    //给dp赋值成较大的值
    memset(dp,0x3f,sizeof(dp)); 
    for(int i =1;i<=n;i++){
        scanf("%d",&a[i]);
        sum[i]=sum[i-1]+a[i];
        dp[i][i]=0;
    }
    int j;
    for(int  len =2;len<=n;len++){//直接从2 开始，所以上面会初始化1
    //1 的操作，看情况初始化，一般值是固定的。 
        for(int i =1;i+len-1<=n;i++){
            j=i+len-1;
            for(int k =i;k+1<=j;k++){
                dp[i][j]=min(dp[i][j],dp[i][k]+dp[k+1][j]+sum[j]-sum[i-1]);
            }//下标 i 到 k 的数 都加起来。
             //k+1到 j的数 都加起来。 
        }
    }
    printf("%d\n",dp[1][n]);
    return 0;
}
```



----



[P4170 [CQOI2007] 涂色 - 洛谷](https://www.luogu.com.cn/problem/P4170)

- 木板长度为1：
  
  - 涂1次

- 木板长度为2：
  
  - AA类型：涂1次，就一次涂完。
  
  - AB类型：涂2次，分成2个区间涂，挨个涂每个区间。

- 木板长度为3：
  
  - AAA类型：三个颜色相同，就涂1次。答案1 次。
  
  - ABA类型：如果首尾颜色一样，可以把首尾一次性涂掉。[ i , j-1 ] , [ i+1 , j] , 就是指把 i 到 j-1 下标的位置 和 i+1 到 j 下标的位置涂成 一样。答案2 次。
  
  - AAB 或 ABB类型：分成2个区间  。例如AAB：要么涂AA后再涂B，要么涂B后涂AA。答案2 次。
  
  - ABC类型：也分成2个区间。例如：要么先涂AB类型【涂A后涂B】，再涂C ；要么先涂B后涂C，再涂A。答案3 次。

- 所以我们**看出大区间的次数是由小区间来的，所以是区间dp做法**。

![215c90dd-d6e7-4da0-ba46-420b4f98559f](file:///C:/Users/LEGION/Pictures/215c90dd-d6e7-4da0-ba46-420b4f98559f.png)

```cpp
#include <cstdio>
#include <cstring>
#include <algorithm>
using namespace std;
#define N 305
int n;
char s[55]; 
int dp[N][N];
int main(){
	scanf("%s",s+1);
	n = strlen(s+1);
	memset(dp,0x3f,sizeof(dp));
	for(int i =1;i<=n;i++){
		dp[i][i]=1;//单个就初始化为 1 
	}
	int j;
	for(int len=2;len<=n;len++){
		for(int i =1;i+len-1<=n;i++){
			j = i+len-1;
			if(s[i]==s[j]){
				dp[i][j]=min(dp[i+1][j],dp[i][j-1]);
			}else{
				for(int k =i;k+1<=j;k++){
					dp[i][j]=min(dp[i][j],dp[i][k]+dp[k+1][j]);
				}
			}
		}
	}
	printf("%d",dp[1][n]);
}
```


