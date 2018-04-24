https://www.luogu.org/problemnew/show/P1144


对于计数问题，先跑一边SPFA求出从点1到点i的最短路 (1 < i <= n)

然后记忆化搜索每一个点，

对于ans[i]表示点1到点i的不同最短路的条数

如果这个点的最短路和另一个点+1（无权图）相等，就更新ans的值

边界：ans[s] = 1 (s = 1)
```c++
#include<iostream>
#include<cstdio>
using namespace std;

const int MAXN = 1000000 + 1, MAXM = 2000000 + 1;
const int INF = 0x7f7f7f7f, MOD = 100003;

int n, m, fro, rear, ans[MAXN], dis[MAXN];
int edge_num, head[MAXM], que[MAXN * 10];
bool in_que[MAXN];
struct edge {
	int to, next;
}h[MAXM];

void Add(int from, int to) {
	h[++edge_num] = (edge){to, head[from]};
	head[from] = edge_num;
}
void SPFA(int s) {
	for(int i = 0; i <= n; i++) dis[i] = INF;
	dis[s] = 0, in_que[s] = 1;
	que[fro = rear = 1] = s;
	while(fro <= rear) {
		int x = que[fro++];
		in_que[x] = 0;
		for(int i = head[x]; i; i = h[i].next) {
			int y = h[i].to;
			if(dis[x] + 1 < dis[y]) {
				dis[y] = dis[x] + 1;
				if(!in_que[y]) in_que[y] = 1, que[++rear] = y;
			}
		}
	}
}
int dfs(int u) {
	if(ans[u]) return ans[u];
	for(int i = head[u]; i; i = h[i].next)
		if(dis[u] == dis[h[i].to] + 1) ans[u] = (ans[u] + dfs(h[i].to)) % MOD;
	return ans[u];
}
int main() {
	scanf("%d%d", &n, &m);
	for(int i = 1, x, y; i <= m; i++) {
		scanf("%d%d", &x, &y);
		Add(x, y);
		Add(y, x);
	}
	SPFA(1);
	ans[1] = 1;
	for(int i = 1; i <= n; i++)
		printf("%d\n", dfs(i));
		
	return 0;
}
```
