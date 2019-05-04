---
title:		"[PKUWC2018]随机游走（可读版）"
---

```c++
# include<iostream>
# include<cstring>
# include<cstdio>
# include<algorithm>
using namespace std;
const int MAX=1<<18,mod=998244353;
struct p{
	int x,y;
}c[MAX<<1];
int n,Q,s,num,N;
int A[18],B[18],a[MAX],h[18],du[18],D[18];
int read()
{
	int x(0);
	char ch=getchar();
	for(;!isdigit(ch);ch=getchar());
	for(;isdigit(ch);x=x*10+ch-48,ch=getchar());
	return x;
}
void add(int x,int y)
{
	++du[x],c[++num]=(p){h[x],y},h[x]=num;
	++du[y],c[++num]=(p){h[y],x},h[y]=num;
}
int Pow(int a,int b)
{
	int ans=1;
	for(;b;b>>=1,a=1ll*a*a%mod)
	  if(b&1) ans=1ll*ans*a%mod;
	return ans;
}
void dfs(int x,int fa,int s)
{
	if(s&(1<<x)) return void(A[x]=B[x]=0);
	int sumA=0,sumB=0;
	for(int i=h[x],y;i;i=c[i].x)
	  if((y=c[i].y)^fa) dfs(y,x,s),(sumA+=A[y])%=mod,(sumB+=B[y])%=mod;
	A[x]=Pow((du[x]-sumA+mod)%mod,mod-2),B[x]=1ll*(du[x]+sumB)%mod*A[x]%mod;
}
int GET_NUM(int x)
{
	int num=0;
	while(x) x&=x-1,++num;
	return num;
}
void FWT(int *A)
{
	for(int i=1;i<N;i<<=1)
	  for(int l=i<<1,j=0;j<N;j+=l)
	    for(int k=0;k<i;++k)
	      (A[i+j+k]+=A[j+k])%=mod;
}
int main()
{
	n=read(),N=1<<n,Q=read(),s=read()-1;
	for(int i=1;i<n;++i)
	  add(read()-1,read()-1);
	for(int i=0;i<N;++i)
	  dfs(s,-1,i),a[i]=(GET_NUM(i)&1?B[s]:(mod-B[s])%mod);
	FWT(a);
	for(int i=1,k,S;i<=Q;++i)
	  {
	  	k=read(),S=0;
	  	for(int j=1;j<=k;++j)
	  	  S|=(1<<read()-1);
	  	printf("%d\n",a[S]);
	  }
	return 0;
}
```

