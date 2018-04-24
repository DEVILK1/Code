https://www.luogu.org/problemnew/show/P3366

最小生成树的Kruskal算法，巧妙地运用了并查集

在稀疏图（边比较少）中跑的比较快...因为是枚举的边

```c++
// MST
// Kruskal
#include<iostream>
#include<cstdio>
#include<algorithm>
using namespace std;
#define For(i, l, r) for(register int i = l; i <= r; i++)

const int MAXN = 5000 + 1, MAXM = 200000 + 1;
int n, m;
struct edge {
	int x, y, l;
}h[MAXM];

bool cmp(edge a, edge b) { return a.l < b.l; } 
int father[MAXN];
int find(int x) {
	if(father[x] != x) father[x] = find(father[x]);
	return father[x];
}
void unionn(int x, int y) {
	x = find(x), y = find(y);
	father[x] = y;
}

int main() {
	cin >> n >> m;
	For(i, 1, n) father[i] = i;
	For(i, 1, m) cin >> h[i].x >> h[i].y >> h[i].l;
	sort(h + 1, h + m + 1, cmp);
	int k = 0, ans = 0;
	For(i, 1, m) {
		if(find(h[i].x) != find(h[i].y)) {
			unionn(h[i].x, h[i].y);
			k++;
			ans += h[i].l;
		}
		if(k == n - 1) break;
	}
	if(k != n - 1) puts("orz");
	else cout << ans << endl;
	
	return 0;
}
```
