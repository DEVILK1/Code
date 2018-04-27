https://www.luogu.org/problemnew/show/P2212

注意此题两点间的距离是欧几里得距离的平方，不用开根号.

关于小于c的边不能用，就排序之后遍历一遍边集就好.
```c++
#include<iostream>
#include<cstdio>
#include<algorithm>
#define For(i, l, r) for(int i = l; i <= r; i++)
using namespace std;

const int MAXN = 2000 + 1;
const int MAXM = MAXN * (MAXN - 1) >> 1; 

int n, m, c, s = 1;
int father[MAXN];
int ans;

struct point {
	int x, y;
}a[MAXN];
struct edge {
	int x, y;
	double l;
}h[MAXM];

inline int dist(point a, point b) {
	return (a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y);
}
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
int main() {
	scanf("%d%d", &n, &c);
	For(i, 1, n) {
		scanf("%d%d", &a[i].x, &a[i].y);
		father[i] = i;
	}
	for(int i = 1; i <= n; i++)
		for(int j = i + 1; j <= n; j++) {
			h[++m] = (edge){i, j, dist(a[i], a[j])};
		}
	sort(h + 1, h + m + 1, cmp);
	For(i, 1, m) if(h[i].l < c) s++;
	int k = 0;
	for(int i = s; i <= m; i++) {
		if(find(h[i].x) != find(h[i].y)) {
			unionn(h[i].x, h[i].y);
			ans += h[i].l;
			k++;
		}
	}
	if(k == n - 1) cout << ans << endl;
	else puts("-1");
	
	return 0;
}
```
