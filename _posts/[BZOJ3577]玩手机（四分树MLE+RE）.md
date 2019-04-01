---
title:		"[BZOJ3577]玩手机（四分树MLE+RE）"
---

```c++
# include<iostream>
# include<cstring>
# include<cstdio>
# include<algorithm>
# define txl (k<<2)-2
# define txr (k<<2)-1
# define tyl (k<<2)
# define tyr (k<<2)+1
# define midx (xl+xr>>1)
# define midy (yl+yr>>1)
using namespace std;
const int MAX=2e5+5,inf=1e8,N=2.4e6+5;
struct q{
	int x,y,dis;
}c[N];
int tot,n,m,A,B,num=1,cnt,T,mx;
int d[MAX],h[MAX],H[MAX],qu[MAX],vec[MAX],val[MAX];
int S[61][61];
bool bfs()
{
	memset(d,0,sizeof(d));
	int hl=1,tl=d[0]=1;
	while(hl<=tl)
	{
		int tt=qu[hl++];
		for(int i=h[tt];i;i=c[i].x)
		  if(!d[c[i].y]&&c[i].dis)
		  {
		  	d[c[i].y]=d[tt]+1,qu[++tl]=c[i].y;
		  	if(c[i].y==T) return 1;
		  }
	}
	return 0;
}
int dfs(int x=0,int dix=inf)
{
	if(x==T||!dix) return dix;
	int sum=0;
	for(int &i=H[x],dis;i;i=c[i].x)
	  if(d[c[i].y]==d[x]+1&&c[i].dis)
	  {
	  	dis=dfs(c[i].y,min(dix,c[i].dis));
	  	if(dis)
	  	{
	  		sum+=dis,dix-=dis,c[i].dis-=dis,c[i^1].dis+=dis;
	  		if(!dix) break;
		}
	  }
	if(!sum) d[x]=-2;
	return sum;
}
int Dinic()
{
	int tot=0,D;
	while(bfs())
	{
		memcpy(H,h,sizeof(h));
		while(D=dfs()) tot+=D;
	}
	return tot;
}
void add(int x,int y,int dis)
{
	c[++num]=(q){h[x],y,dis},h[x]=num;
	c[++num]=(q){h[y],x,0},h[y]=num;
}
int read()
{
	int x(0);
	char ch=getchar();
	for(;!isdigit(ch);ch=getchar());
	for(;isdigit(ch);x=x*10+ch-48,ch=getchar());
	return x;
}
void build(int xl=1,int xr=n,int yl=1,int yr=m,int k=1,int fk=0)
{
	if(xl>xr||yl>yr) return;
	mx=max(mx,k);
	if(fk) add(fk,k,inf);
	if(xl==xr&&yl==yr) return;
	build(xl,midx,yl,midy,txl,k),build(midx+1,xr,yl,midy,txr,k),build(xl,midx,midy+1,yr,tyl,k),build(midx+1,xr,midy+1,yr,tyr,k);
}
void _build(int xl=1,int xr=n,int yl=1,int yr=m,int k=1,int fk=0)
{
	if(xl>xr||yl>yr) return;
	if(fk) add(k+mx,fk+mx,inf);
	if(xl==xr&&yl==yr) return void(add(k,k+mx,S[xl][yl]));
	_build(xl,midx,yl,midy,txl,k),_build(midx+1,xr,yl,midy,txr,k),_build(xl,midx,midy+1,yr,tyl,k),_build(midx+1,xr,midy+1,yr,tyr,k);
}
void change(int xl,int xr,int yl,int yr,int k,int xL,int xR,int yL,int yR,int d)
{
	if(xr<xL||xR<xl||yr<yL||yR<yl) return;
	if(xl>=xL&&xr<=xR&&yl>=yL&&yr<=yR) return void(add(d,k,inf));
	change(xl,midx,yl,midy,txl,xL,xR,yL,yR,d),change(midx+1,xr,yl,midy,txr,xL,xR,yL,yR,d),change(xl,midx,midy+1,yr,tyl,xL,xR,yL,yR,d),change(midx+1,xr,midy+1,yr,tyr,xL,xR,yL,yR,d);
}
void _change(int xl,int xr,int yl,int yr,int k,int xL,int xR,int yL,int yR,int d)
{
	if(xr<xL||xR<xl||yr<yL||yR<yl) return;
	if(xl>=xL&&xr<=xR&&yl>=yL&&yr<=yR) return void(add(k+mx,d,inf));
	_change(xl,midx,yl,midy,txl,xL,xR,yL,yR,d),_change(midx+1,xr,yl,midy,txr,xL,xR,yL,yR,d),_change(xl,midx,midy+1,yr,tyl,xL,xR,yL,yR,d),_change(midx+1,xr,midy+1,yr,tyr,xL,xR,yL,yR,d);
}
int main()
{
	n=read(),m=read(),A=read(),B=read();
	for(int i=1;i<=n;++i)
	  for(int j=1;j<=m;++j)
	    S[i][j]=read();
	build(),_build(),tot=T=(mx<<1)+1;
	for(int i=1,xl,xr,yl,yr,x;i<=A;++i)
	  x=read(),xl=read(),yl=read(),xr=read(),yr=read(),change(1,n,1,m,1,xl,xr,yl,yr,++tot),add(0,tot,x);
	for(int i=2,xl,xr,yl,yr,x;i<=B;++i)
	  x=read(),xl=read(),yl=read(),xr=read(),yr=read(),_change(1,n,1,m,1,xl,xr,yl,yr,++tot),add(tot,T,x);
	return printf("%d",Dinic()),0;
}
```

