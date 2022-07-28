
# 「POJ3662」Telephone Lines题解

## 题目描述

笨笨长大了，成为了电话线布置师。

由于地震使得某市的电话线全部损坏，笨笨是负责接到震中市的负责人。

该市周围分布着N(1≤N≤1000)根据1…n顺序编号的废弃的电话线杆，任意两根线杆之间没有电话线连接，一共有p(1≤p≤10000)对电话杆可以拉电话线。其他的由于地震使得无法连接。 第i对电线杆的两个端点分别是ai,bi，它们的距离为li(1≤li≤1000000)。数据中每对(ai,bi)只出现一次。

编号为1的电话杆已经接入了全国的电话网络，整个市的电话线全都连到了编号N的电话线杆上。也就是说，笨笨的任务仅仅是找一条将1号和N号电线杆连起来的路径，其余的电话杆并不一定要连入电话网络。

电信公司决定支援灾区免费为此市连接k对由笨笨指定的电话线杆，对于此外的那些电话线，需要为它们付费，总费用决定于其中最长的电话线的长度(每根电话线仅连接一对电话线杆)。如果需要连接的电话线杆不超过k对，那么支出为0. 请你计算一下，将电话线引导震中市最少需要在电话线上花多少钱？

## 输入输出格式

#### 输入格式

输入文件的第一行包含三个数字n,p,k;

第二行到第p+1行，每行分别都为三个整数ai,bi,li。

#### 输出格式

一个整数，表示该项工程的最小支出，如果不可能完成则输出`-1`.

## 输入输出样例

#### 输入样例 #1

```
5 7 1
1 2 5
3 1 4
2 4 8
3 2 3
5 2 9
3 4 7
4 5 6
```

#### 输出样例 #1

```
4
```

### 数据范围

20 % ： $n≤10$,$p≤20$

40 %: n≤100

100%：$n≤10^4$,$p≤10000$;

## 解题思路

~~震惊！本蒟蒻第一眼注意到的竟是输入样例而不是题目描述？~~一看，哦！这不是一个简简单单的Dijkstra最短路嘛，~~（虽然这题是在BFS题单里的）~~，然后就开始打模板，打到一半，冷静下来后，往数据范围一看，~~哎呀~~，TLE欢迎你，于是乎就开始想方设法优化，打了接近一个小时吧，打出个二分，一交，嗯，~~针不戳~~，过了，然后去看网上daolao们的题解，发现也是二分，~~但比我快了不止亿点点~~

## 实现细节

### 1.注意在Dijkstra内部初始化，以及在判断最短路时，累加的不是两点之间的距离，而是电线杆的对数。

### 2.记得返回值，~~我就因为这个卡了好久~~

### 3.双向图，双向图，双向图！！！！！！！！~~（重要的事情说三遍）~~

## code

```c++
#include <bits/stdc++.h>
using namespace std;
long long n,P,k;
struct edge {
	int to;
	int ne;
	int v;
} p[1000001];
long long fir[100001],cnt=0,dis[100001],vis[100001],ans=0x3f,tot[100000];
void add(int a,int b,int c) {
	p[++cnt].to=b;
	p[cnt].ne=fir[a];
	fir[a]=cnt;
	p[cnt].v=c;
}
priority_queue<pair <int,int> >q;
int di(int s) {
	memset(dis,0x3f,sizeof(dis));
	memset(vis,0,sizeof(vis));
	dis[1]=0;
	q.push(make_pair(0,1));
	while(q.size()) {
		int u=q.top().second;
		q.pop();
		if(vis[u])continue;
		vis[u]=1;
		for(long long i=fir[u]; i; i=p[i].ne) {
			long long t=p[i].to,v=p[i].v;
			int ad;
			if(v<=s) {
				ad=0;
			} else {
				ad=1;
			}
			if(dis[t]>dis[u]+ad) {
				dis[t]=dis[u]+ad;
				q.push(make_pair(-dis[t],t));
			}
		}
	}
	return dis[n];
}
bool check(long long mid) {
	if(di(mid)<=k) {
		return 1;
	} else {
		return 0;
	}
}
int a,b,l;
int main() {
	cin>>n>>P>>k;
	for(int i=1; i<=P; i++) {
		scanf("%d%d%d",&a,&b,&l);
		add(a,b,l);
		add(b,a,l);
	}
	long long l=0,r=1000002,mid;
	while(l<r) {
		mid=(l+r)>>1;
		if(check(mid)) {
			r=mid;
		} else {
			l=mid+1;
		}
	}
	if(r<0) {
		cout<<0;
	} else
		cout<<l;
	return 0;
}

```

