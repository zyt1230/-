```cpp
#include <cstdio>
#include <cstring>
#include<iostream>
#include <algorithm>
using namespace std;
#define N 100100
int h[N],w[N];
int n,k;

int get_sum(int mid){
    int sum =0;
    for(int i=1;i<=n;i++){
        sum+= (w[i]/mid)*(h[i]/mid);
    }
    return sum;
}
int main(){
    cin>>n;
    cin>>k;
    for(int i =1;i<=n;i++){
        cin>>h[i]>>w[i];
    }
    int l = 1;
    int r = N;
    while(l<=r){
        int mid= (l+r)/2;
        if(get_sum(mid)>=k){
            l= mid+1;
        }else{
            r=mid-1;
        }
    }
    printf("%d",l-1);
}

```


