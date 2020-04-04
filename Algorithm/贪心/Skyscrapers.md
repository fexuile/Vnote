# Skyscrapers

[CF1313C]

现在你有n个位置,每一个位置可以放一个数$a_i \in [1,m_i]$,现在要你最大化$\sum_{i=1}^na_i$,而且要满足$\forall i\ \nexists\ j < i < k, a_j > a_i < a_k$.

首先可以想一个贪心,即对于每一个位置,考虑将它作为$m_i$,那么这个序列就是唯一的,因为你一定是左边一段单调不降,右边一段单调不升.这样子很容易做到$O(n^2)$.

现在考虑还是枚举一个最高点,那么我们考虑设 $L_i$ 表示以 i 作为最高点,左边的数的和;同样的设一个 $R_i$ ,不难发现 $L_i$ 和 $R_i$ 都可以单调栈很容易的求出.

```cpp
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<math.h>
#include<algorithm>
#include<queue>
#include<set>
#include<map>
#include<iostream>
using namespace std;
#define re register
#define ll long long
inline int gi()
{
	int f=1,sum=0;char ch=getchar();
	while(ch>'9' || ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0' && ch<='9'){sum=(sum<<3)+(sum<<1)+ch-'0';ch=getchar();}
	return f*sum;
}
const int N=500010;
int n,mx[N],sta[N],top,a[N];
ll L[N],R[N];
int main()
{
	n=gi();ll ans=0;
	for(int i=1;i<=n;i++)mx[i]=gi();
	for(int i=1;i<=n;i++)
	{
		while(top&&mx[sta[top]]>mx[i])top--;
		L[i]=L[sta[top]]+1ll*mx[i]*(i-sta[top]);sta[++top]=i;
	}
	top=0;sta[0]=n+1;
	for(int i=n;i;i--)
	{
		while(top&&mx[sta[top]]>mx[i])top--;
		R[i]=R[sta[top]]+1ll*mx[i]*(sta[top]-i);sta[++top]=i;
	}
	int pos=0;
	for(int i=1;i<=n;i++)
		if(ans<L[i]+R[i]-mx[i])pos=i,ans=L[i]+R[i]-mx[i];
	a[pos]=mx[pos];
	for(int i=pos-1;i;i--)a[i]=min(mx[i],a[i+1]);
	for(int i=pos+1;i<=n;i++)a[i]=min(mx[i],a[i-1]);
	for(int i=1;i<=n;i++)printf("%d%c",a[i],i==n?'\n':' ');
	return 0;
}
```