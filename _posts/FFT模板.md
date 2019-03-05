---
title:		"FFT模板"

---

```c++
# include<iostream>
# include<cstring>
# include<cstdio>
# include<cmath>
# include<algorithm>
using namespace std;
const int MAX=5e6+5;
const double Pi=acos(-1.0);
struct Complex{
	double x,y;
	Complex(double X=0,double Y=0) {x=X,y=Y;}
}a[MAX],b[MAX];
int n,m,lim=1,L;
int R[MAX];
Complex operator+ (Complex x,Complex y) {return Complex(x.x+y.x,x.y+y.y);}
Complex operator- (Complex x,Complex y) {return Complex(x.x-y.x,x.y-y.y);}
Complex operator* (Complex x,Complex y) {return Complex(x.x*y.x-x.y*y.y,x.y*y.x+x.x*y.y);}
int read()
{
	int x(0);
	char ch=getchar();
	for(;!isdigit(ch);ch=getchar());
	for(;isdigit(ch);x=x*10+ch-48,ch=getchar());
	return x;
}
void FFT(Complex *A,int tt=1)
{
	for(int i=0;i<lim;++i)
	  if(i<R[i]) swap(A[i],A[R[i]]);
	for(int i=1;i<lim;i<<=1)
	  {
	  	Complex w1=Complex(cos(Pi/i),tt*sin(Pi/i));
	  	for(int l=i<<1,j=0;j<lim;j+=l)
	  	  {
	  	  	Complex w=Complex(1,0),x,y;
	  	  	for(int k=0;k<i;++k,w=w*w1)
	  	  	  x=A[j+k],y=w*A[i+j+k],A[j+k]=x+y,A[i+j+k]=x-y;
		  }
	  }
}
int main()
{
	n=read(),m=read();
	for(int i=0;i<=n;++i)
	  a[i].x=read();
	for(int i=0;i<=m;++i)
	  b[i].x=read();
	while(lim<=n+m) lim<<=1,++L;
	for(int i=0;i<lim;++i)
	  R[i]=(R[i>>1]>>1)|((i&1)<<L-1);
	FFT(a),FFT(b);
	for(int i=0;i<=lim;++i)
	  a[i]=a[i]*b[i];
	FFT(a,-1);
	for(int i=0;i<=n+m;++i)
	  printf("%d ",int(a[i].x/lim+0.5));
	return 0;
}
```



