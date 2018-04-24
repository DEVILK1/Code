https://www.luogu.org/problemnew/show/P3371

堆优化用的stl里面的优先队列，

就是把每次遍历数组寻找最小值的过程用小根堆代替，

时间复杂度由O(n^2)变成了O(n log_2 n)

```c++
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<queue>
using namespace std;

const int MAXM = 500000 + 1, MAXN = 10000 + 1;
const int INF = 0x7fffffff;

int n, m, s, dis[MAXN];
int edge_num, head[MAXM];
bool vis[MAXN];
struct edge {
	int len, to, next;
}h[MAXM];

struct tHeap {
	int point, len;
	bool operator > (const tHeap &o) const {
		return len > o.len;
	}
};

priority_queue<tHeap, vector<tHeap>, greater<tHeap> > q;

void Add(int from, int to, int len) {
	h[++edge_num].next = head[from];
	h[edge_num].len = len;
	h[edge_num].to = to;
	head[from] = edge_num;
}
void Dijkstra(int s) {
	for(int i = 0; i <= MAXN; i++) dis[i] = INF;
	dis[s] = 0;
	q.push((tHeap){s, dis[s]});
//有两种写法，都可以
//	for(int i = 1; i <= n; i++) {
//		if(q.empty()) break;
//		while(vis[q.top().point]) q.pop();
//		int k = q.top().point;
//		q.pop();
//		vis[k] = 1;
//		for(int j = head[k]; j; j = h[j].next) {
//			if(!vis[h[j].to] && dis[k] + h[j].len < dis[h[j].to]) {
//				dis[h[j].to] = dis[k] + h[j].len;
//				q.push((tHeap){h[j].to, dis[k] + h[j].len});
//			}
//		}
//	}
	while(!q.empty()) {
		int k = q.top().point;
		q.pop();
		if(vis[k]) continue;
		vis[k] = 1;
		for(int i = head[k]; i; i = h[i].next) {
			if(!vis[h[i].to] && dis[h[i].to] > dis[k] + h[i].len) {
				dis[h[i].to] = dis[k] + h[i].len;
				q.push((tHeap){h[i].to, dis[k] + h[i].len});
			}
		}
	}
}
int main() {
	scanf("%d%d%d", &n, &m, &s);
	for(int i = 1, f, g, w; i <= m; i++) {
		scanf("%d%d%d", &f, &g, &w);
		Add(f, g, w);
	}
	Dijkstra(s);
	for(int i = 1; i <= n; i++)
		printf("%d ", dis[i]);
		
	return 0;
}
```
