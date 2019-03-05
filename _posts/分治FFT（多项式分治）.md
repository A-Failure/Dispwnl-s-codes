---
title:		"分治FFT（多项式分治）"
---

```c++
# include<iostream>
# include<cstring>
# include<cstdio>
# include<algorithm>
using namespace std;
const int MAX=4e5+5,mod=998244353;
int n,lim=1,L;
int R[MAX],a[MAX],b[MAX],c[MAX],d[MAX],inv[MAX];
int read()
{
	int x(0);
	char ch=getchar();
	for(;!isdigit(ch);ch=getchar());
	for(;isdigit(ch);x=x*10+ch-48,ch=getchar());
	return x;
}
int Pow(int a,int b)
{
	int ans=1;
	for(;b;b>>=1,a=1ll*a*a%mod)
	  if(b&1) ans=1ll*a*ans%mod;
	return ans;
}
void NTT(int *A,int tt=1)
{
	for(int i=0;i<lim;++i)
	  if(i<R[i]) swap(A[i],A[R[i]]);
	for(int i=1,w1;i<lim;i<<=1)
	  {
	  	w1=inv[i<<1];
	  	for(int l=i<<1,j=0,w;j<lim;j+=l)
	  	  {
	  	  	w=1;
	  	  	for(int k=0,x,y;k<i;++k,w=1ll*w*w1%mod)
	  		  {
	  			x=A[j+k],y=1ll*w*A[i+j+k]%mod,A[j+k]=x+y,A[i+j+k]=x-y+mod;
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
void Solve(int l=0,int r=n-1)
{
	if(l>=r) return;
	int mid(l+r>>1);
	Solve(l,mid);
	lim=1,L=0;
	while(lim<=(r-l+1)) lim<<=1,++L;
	for(int i=0;i<=lim;++i)
	  c[i]=d[i]=0,R[i]=(R[i>>1]>>1)|((i&1)<<L-1);
	for(int i=l;i<=mid;++i)
	  c[i-l]=b[i];
	for(int i=0;i<=r-l;++i)
	  d[i]=a[i+1];
	NTT(c),NTT(d);
	for(int i=0;i<=lim;++i)
	  c[i]=1ll*c[i]*d[i]%mod;
	NTT(c,-1);
	for(int i=mid+1,j=mid-l;i<=r;++i,++j)
	  {
	  	b[i]+=c[j];
	  	if(b[i]>=mod) b[i]-=mod;
	  }
	Solve(mid+1,r); 
}
int main()
{
	n=read();
	for(int i=1;i<n;++i)
	  a[i]=read();
	for(int i=1,N=n<<2;i<=N;i<<=1)
	  inv[i]=Pow(3,(mod-1)/i);
	b[0]=1,Solve();
	for(int i=0;i<n;++i)
	  printf("%d ",b[i]);
	return 0;
}
```

