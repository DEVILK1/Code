https://www.luogu.org/problemnew/show/P3385

多开一个cnt数组记录每个点被松弛的次数，

一个点i被松弛的时候，cnt[i]++

如果松弛的次数到了(n - 1)次，则有负环.

```c++
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;

const int MAXN = 2e5 + 10, MAXM = 3e5 + 10;
const int INF = 1e9;

int Q, n, m, fro, rear, que[MAXN * 10], dis[MAXN];
int edge_num, head[MAXM], cnt[MAXN];
bool in_que[MAXN];
struct edge {
	int next, to, len;
}h[MAXM];

void Add(int from, int to, int len) {
	h[++edge_num].next = head[from];
	h[edge_num].to = to;
	h[edge_num].len = len;
	head[from] = edge_num;
}

inline int read() {
	int x = 0, f = 1; char ch = getchar();
	while(ch < '0' || ch > '9') { if(ch == '-') f = -1; ch = getchar(); }
	while(ch >= '0' && ch <= '9')
		x = (x << 1) + (x << 3) + (ch ^ 48), ch = getchar();
	return x * f;
}

//有负环返回 false  
bool SPFA(int s) {
	memset(cnt, 0, sizeof(cnt));
	memset(in_que, 0, sizeof(in_que));
	memset(dis, 0, sizeof(dis));
	for(int i = 1; i <= n; i++) dis[i] = INF;
	que[fro = rear = 1] = s;
	cnt[s] = in_que[s] = 1, dis[s] = 0;
	while(fro <= rear) {
		int x = que[fro++];
		in_que[x] = 0;
		for(int i = head[x]; i; i = h[i].next) {
			int len = h[i].len, y = h[i].to;
			if(dis[y] > dis[x] + len) {
				cnt[y] = cnt[x] + 1;
				if(cnt[y] >= n) return false;
				dis[y] = dis[x] + len;
				if(!in_que[y]) in_que[y] = 1, que[++rear] = y;
			}
		}
	}
	return true;
}

int main() {
	Q = read();
	while(Q--) {
		edge_num = 0;
		memset(head, 0, sizeof(head));
		n = read(); m = read();
		for(int i = 1; i <= m; ++i) {
			int w, a, b;
			a = read(); b = read(); w = read();
			Add(a, b, w);
			if(w >= 0) Add(b, a, w);
		}
		
		if(SPFA(1)) puts("N0");
		else puts("YE5");
	}
	
	return 0;
}
```
