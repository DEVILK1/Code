https://www.luogu.org/problemnew/show/P1991#sub

此题的要点有以下几处：

1.给出的是点的坐标，如何转化成每条边的长度

2.S个不用管的哨所如何选择

3.怎样求通讯的最小距离

solve:

1.n个点存 **n  * (n - 1)/2**  条边（因为是无向边），每条边存储的信息有：这条边连接的两个点以及这条边的长度

长度利用勾股定理计算 **√(x1 - x2) ^ 2 + (y1 - y2) ^ 2**

2.在加入生成树时只加入 (n - s) 个点，由于已经将边按照长度排序，保证了通讯距离最小

3.其实就是 MST 中的最大边权

```c++
#include<iostream>
#include<cstdio>
#include<cmath>
#include<algorithm>
using namespace std;
#define re register

const int MAXN = 500 + 1, MAXM = MAXN * (MAXN - 1) >> 1;

int n, s;
struct point { int x, y; }a[MAXN];
int m;
struct edge {
	int x, y;
	double l;
}h[MAXM];

int read() {
	int x = 0;
	bool flag = false;
	char ch = getchar();
	if(ch == '-') flag = true;
	while(ch < '0' || ch > '9') ch = getchar();
	for(; ch >= '0' && ch <= '9'; ch = getchar()) x = (x << 3) + (x << 1) + ch - '0';
	return flag ? -x : x;
}

inline double max(double a, double b) { return a > b ? a : b; }
inline bool cmp(edge a, edge b) { return a.l < b.l; }
inline double dis(point a, point b) { return sqrt((a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y) * 1.0); }

int father[MAXN];
int find(int x) {
	if(father[x] != x) father[x] = find(father[x]);
	return father[x];
}
inline void unionn(int x, int y) {
	father[find(x)] = father[find(y)];
}
int main() {
	s = read();
	n = read();
	for(re int i = 1; i <= n; i++) {
		a[i].x = read();
		a[i].y = read();
		father[i] = i;
	}
	for(re int i = 1; i <= n; i++) {
		for(re int j = 1; j < i; j++) {
				h[++m].x = i;
				h[m].y = j;
				h[m].l = dis(a[i], a[j]);
		}
	}
	sort(h + 1, h + m + 1, cmp);
	n -= s;
	double ans = 0.0;
	
	for(re int i = 1, k = n; k; i++) {
		if(find(h[i].x) != find(h[i].y)) {
			k--;
			unionn(h[i].x, h[i].y);
			ans = max(ans, h[i].l);
		}
	}
	printf("%0.2lf\n", ans);
}
```
