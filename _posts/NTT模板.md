---
title:		"NTT模板"
---

```c++
# include<iostream>
# include<cstring>
# include<cstdio>
# include<cmath>
# include<algorithm>
# define LL long long
using namespace std;
const int MAX=5e6+1;
const int mod=998244353;
int n,m,l,lim=1;
int a[MAX],b[MAX],R[MAX];
int read()
{
	int x=0;
	char ch=getchar();
	for(;!isdigit(ch);ch=getchar());
	for(;isdigit(ch);x=x*10+ch-48,ch=getchar());
	return x;
}
int Pow(int x,int b)
{
	int ans=1;
	for(;b;b>>=1,x=1ll*x*x%mod)
	  if(b&1) ans=1ll*ans*x%mod;
	return ans;
}
void NTT(int *A,int tt=1)
{
	for(int i=0;i<lim;++i)
	  if(i<R[i]) swap(A[i],A[R[i]]);
	for(int i=1,w1;i<lim;i<<=1)
	  {
	  	w1=Pow(3,(mod-1)/(i<<1));
	  	for(int l=i<<1,w,j=0;j<lim;j+=l)
	  	  {
	  	  	w=1;
	  	  	for(int k=0,x,y;k<i;++k,w=1ll*w*w1%mod)
	  	  	  {
	  	  	  	x=A[j+k],y=1ll*w*A[j+i+k]%mod,A[j+k]=x+y,A[j+i+k]=x-y+mod;
	  	  	  	if(A[j+k]>=mod) A[j+k]-=mod;
	  	  	  	if(A[i+j+k]>=mod) A[i+j+k]-=mod;
			  }
		  }
	  }
	if(tt==-1)
	{
		reverse(A+1,A+lim);
		for(int i=0,G=Pow(lim,mod-2);i<lim;++i)
		  A[i]=1ll*A[i]*G%mod;
	}
}
int main()
{
	n=read(),m=read();
	for(int i=0;i<=n;++i)
	  a[i]=read();
	for(int i=0;i<=m;++i)
	  b[i]=read();
	while(lim<=n+m) lim<<=1,++l;
	for(int i=0;i<lim;++i)
	  R[i]=(R[i>>1]>>1)|((i&1)<<(l-1));
	NTT(a),NTT(b);
	for(int i=0;i<=lim;++i)
	  a[i]=1ll*a[i]*b[i]%mod;
	NTT(a,-1);
	for(int i=0;i<=n+m;++i)
	  printf("%d ",a[i]);
	return 0;
}
```

