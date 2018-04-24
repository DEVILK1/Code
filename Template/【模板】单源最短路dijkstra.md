https://www.luogu.org/problemnew/show/P3371

时间复杂度O(n^2)
```c++
#include<iostream>
#include<cstdio>
using namespace std;

const int MAXM = 500000 + 1, MAXN = 10000 + 1;
const int INF = 0x7fffffff;

int n, m, s, dis[MAXN];
int edge_num, head[MAXM];
bool vis[MAXN];
struct edge {
	int len, to, next;
}e[MAXM];

//还是用邻接表存图比较好...
void Add(int from, int to, int len) {
	e[++edge_num].next = head[from];
	e[edge_num].len = len;
	e[edge_num].to = to;
	head[from] = edge_num;
}
void Dijkstra(int s) {
	for(int i = 0; i <= MAXN; i++) dis[i] = INF;
	dis[s] = 0;
	for(int i = 1; i <= n; i++) {
		int k = -1, minn = INF;
		for(int j = 1; j <= n; j++)
			if(!vis[j] && minn > dis[j])
				minn = dis[j], k = j;
		if(k == -1) break;
		vis[k] = 1;
		for(int j = head[k]; j; j = e[j].next) {
			if(!vis[e[j].to] && dis[k] + e[j].len < dis[e[j].to])
				dis[e[j].to] = dis[k] + e[j].len;
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
