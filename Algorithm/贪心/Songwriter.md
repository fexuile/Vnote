# Songwriter

[CF1252E]

给你一个A数组,要你求出一个B数组,满足B中的相邻两个数的大小关系和A中相同,而且满足B中所有的数在[L,R]中,且相邻两个数的差不能超过K,问字典序最小的B数组.

考虑可以求出来每一个数的范围,不如令它为$[low_i,upp_i]$,那么显然这个是可以倒推的.此时不难发现第一个数必然取$low_1$,然后再将这个贪心的顺推即可.

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
const int N=200010;
int low[N],upp[N],a[N],n,L,R,K,b[N];
int main()
{
	n=gi();L=gi();R=gi();K=gi();
	for(int i=1;i<=n;i++)a[i]=gi();
	low[n]=L;upp[n]=R;
	for(int i=n-1;i;i--)
	{
		if(a[i]==a[i+1])low[i]=low[i+1],upp[i]=upp[i+1];
		else if(a[i]<a[i+1])low[i]=max(L,low[i+1]-K),upp[i]=upp[i+1]-1;
		else low[i]=low[i+1]+1,upp[i]=min(R,upp[i+1]+K);
	}
	for(int i=1;i<=n;i++)
		if(low[i]>R||upp[i]<L)return puts("-1")&0;
	b[1]=low[1];
	for(int i=2;i<=n;i++)
	{
		if(a[i]==a[i-1])b[i]=b[i-1];
		else if(a[i]<a[i-1])b[i]=b[i-1]-K;
		else b[i]=b[i-1]+1;
		b[i]=min(b[i],upp[i]);b[i]=max(b[i],low[i]);
	}
	for(int i=1;i<=n;i++)printf("%d%c",b[i],i==n?'\n':' ');
	return 0;
}
```