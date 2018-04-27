https://www.luogu.org/problemnew/show/P1396

“最大值最小”..一看就是二分啊..

然后用Kruskal跑出来了..

在开始选边之前，先把和s点、t点相连的最小边加入到生成树中：

  由于已经按照边权排序了，所以只要遍历两边边集，找到第一个与s、t点相连的边就好.
  
在跑Kruskal的过程中，不一定要加入(n - 1)条边

任务只是从s到t点

所以在每一次加边之前先看看s、t点是否相连（在同一分离集合），已经连通了就不必要再加边

```c++
#include<iostream>
#include<cstdio>
#include<algorithm>
#define For(i, l, r) for(int i = l; i <= r; i++)
using namespace std;

const int MAXN = 10000 + 1;
const int MAXM = MAXN << 1;

int n, m, s, t, ans;
int father[MAXN];

struct edge {
	int x, y, l;
}h[MAXM];

inline bool cmp(edge a, edge b) {
	return a.l < b.l;
}

int find(int x) {
	if(father[x] != x) father[x] = find(father[x]);
	return father[x];
}

inline void unionn(int x, int y) {
	father[find(x)] = father[find(y)];
}

void add_point(int p) {
	for(int i = 1; i <= m; i++) {
		if(h[i].x == p || h[i].y == p) {
			unionn(h[i].x, h[i].y);
			ans = max(ans, h[i].l);
			return;
		}
	}
}

int main() {
	scanf("%d%d%d%d", &n, &m, &s, &t);
	For(i, 1, n) father[i] = i; 
	For(i, 1, m) scanf("%d%d%d", &h[i].x, &h[i].y, &h[i].l);
	sort(h + 1, h + m + 1, cmp);
	add_point(s);
	add_point(t);
	for(int i = 1; i <= m; i++) {
		if(find(s) == find(t)) break;
		if(find(h[i].x) != find(h[i].y)) {
			unionn(h[i].x, h[i].y);
			ans = max(ans, h[i].l);
		}
	}
	printf("%d\n", ans);
	
	return 0;
}
```
