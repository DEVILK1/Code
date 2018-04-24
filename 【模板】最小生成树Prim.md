https://www.luogu.org/problemnew/show/P3366

堆优化的Prim，就和dijkstra差不多...

```c++
//MST
//Prim
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<queue>
#include<vector>
using namespace std;

const int MAXN = 5000 + 1, MAXM = 200000 * 2 + 1;
const int INF = 0x7fffffff;

int n, m, ans, dis[MAXN];
int edge_num, head[MAXN];
bool vis[MAXN];
struct edge {
	int len, to, next;
}h[MAXM];

struct tHeap {
	int point, dist;
	bool operator > (const tHeap &o) const {
		return dist > o.dist;
	}
};
priority_queue<tHeap, vector<tHeap>, greater<tHeap> > q;

void Add(int from, int to, int len) {
	h[++edge_num].next = head[from];
	h[edge_num].to = to;
	h[edge_num].len = len;
	head[from] = edge_num;
}
int main() {
	scanf("%d%d", &n, &m);
	for(int i = 1; i <= m; i++) {
		int x, y, l;
		scanf("%d%d%d", &x, &y, &l);
		Add(x, y, l);
		Add(y, x, l);
	}
	
	for(int i = 1; i <= n; i++) dis[i] = INF;
	vis[1] = 1, dis[1] = 0;
	for(int i = head[1]; i; i = h[i].next) {
		q.push((tHeap){h[i].to, h[i].len});
		dis[h[i].to] = h[i].len;
	}
	int cnt = 0;
	while(!q.empty() && cnt < n) {
		tHeap ext = q.top();
		q.pop();
		if(vis[ext.point]) continue;
		cnt++;
		ans += ext.dist;
		vis[ext.point] = true;
		for(int i = head[ext.point]; i; i = h[i].next) {
			if(dis[h[i].to] > h[i].len) {
				dis[h[i].to] = h[i].len;
				q.push((tHeap){h[i].to, h[i].len});
			}
		}
	}
	if(cnt != n) printf("%d\n", ans);
	else puts("orz");
	
	return 0;
}
```
