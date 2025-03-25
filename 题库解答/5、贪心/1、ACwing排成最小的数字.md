```cpp
#include <cstdio>
#include <string>
#include<iostream>
#include <algorithm>
using namespace std;
#define N 10010
string s[N],ans;
int n;
bool cmp(string& a,string& b){
    return a+b<b+a;
}
int main(){
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>s[i];

    }
    sort(s+1,s+n+1,cmp);
    for(int i =1;i<=n;i++){
        ans+=s[i];
    }
    while(ans[0]=='0'&&ans.size()>1){
        ans=ans.substr(1,ans.size()-1);
    }

    cout<<ans;
    return 0;
}
```


