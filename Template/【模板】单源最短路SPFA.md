https://www.luogu.org/problemnew/show/P3371

SPFA的复杂度是个玄学，与点的进队次数有关

实际上是O(KE)，k为进队次数， E为边的数目

```c++
#include<iostream>
#include<cstdio>
using namespace std;

const int MAXM = 500000 + 1, MAXN = 10000 + 1;
const int INF = 0x7fffffff;

int n, m, s, fro, rear;
int edge_num, head[MAXM], dis[MAXN];
int que[MAXN * 10];
bool in_que[MAXN];
struct edge {
	int len, to, next;
}h[MAXM];

void Add(int from, int to, int len) {
	h[++edge_num] = (edge){len, to, head[from]};
	head[from] = edge_num;
}

void SPFA(int s) {
	for(int i = 1; i <= n; i++) dis[i] = INF;
	dis[s] = 0, in_que[s] = 1;
	que[fro = rear = 1] = s;
	while(fro <= rear) {
		int x = que[fro++];
		in_que[x] = 0;
		for(int i = head[x]; i; i = h[i].next) {
			int len = h[i].len, y = h[i].to;
			if(dis[x] + len < dis[y]) {
				dis[y] = dis[x] + len;
				if(!in_que[y]) in_que[y] = 1, que[++rear] = y;
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
	SPFA(s);
	for(int i = 1; i <= n; i++)
		printf("%d ", dis[i]);
	
	return 0;
}
```
