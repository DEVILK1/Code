一开始枚举了n * m个点中的每一个点作为终点进行bfs，当然超时了，时间复杂度是正解的n * m倍.

以下是错误代码：
```c++
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;
#define For(i, l, r) for(int i = l; i <= r; i++)

const int MAXN = 500 + 1, MAXM = 400 + 1;
const int dx[] = {2, 2, -2, -2, 1, -1, 1, -1};
const int dy[] = {1, -1, 1, -1, 2, 2, -2, -2};

int n, m, head, tail, step;
int pre[MAXN * MAXM];
bool flag, vis[MAXN][MAXM];
struct rpg {
	int x, y;
}start, que[MAXN * MAXM];

int read() {
	int x = 0, flag = 1;
	char ch = getchar();
	if(ch < 0) flag = false;
	while(ch < '0' || ch >'9') ch = getchar();
	for(; ch >= '0' && ch <= '9'; ch = getchar())
		x = x * 10 + ch - '0';
	return flag ? x : -x;
}
void init() {
	memset(vis, 0, sizeof(vis));
	memset(que, 0, sizeof(que));
	memset(pre, 0, sizeof(pre));
	step = flag = 0;
}

void find(int x) {
	while(pre[x]) {
		x = pre[x];
		step++;
	}
}
void bfs(int x, int y) {
	if(x == start.x && y == start.y) {
		flag = true;
		printf("%-5d", 0);
		return;
	}
	head = tail = 1;
	que[tail] = (rpg){start.x, start.y};
	vis[start.x][start.y] = 1;
	while(head <= tail) {
		for(int i = 0; i < 8; i++) {
			int xx = que[head].x + dx[i],
				yy = que[head].y + dy[i];
			if(xx > 0 && xx <= n && yy > 0 && yy <= m && !vis[xx][yy]) {
				vis[xx][yy] = 1;
				que[++tail] = (rpg){xx, yy};
				pre[tail] = head;
			}
			if(xx == x && yy == y) {
				flag = true;
				find(tail);
				printf("%-5d", step);
				if(y == m) puts("");
				head = tail;
				break;
			}
		}
		head++;
	}
}
int main() {
	n = read();
	m = read();
	start.x = read();
	start.y = read();
	
	for(int i = 1; i <= n; i++)
		for(int j = 1; j <= m; j++) {
			init();
			bfs(i, j);//对每一个点作为终点都进行了bfs，但bfs一遍就能遍历这个矩阵
			if(!flag) {
				printf("%-5d", -1);
				if(j == m) puts("");
			}
		}
}
```

只进行一遍bfs，每一个点都能搜索到（不判断终点，一直搜到最后），把每一个点所需要的步数存入数组，最后查询的时间复杂度变为O(1)

以下为正解：

```c++
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;
#define For(i, l, r) for(int i = l; i <= r; i++)

const int MAXN = 500 + 1, MAXM = 400 + 1;
const int dx[] = {2, 2, -2, -2, 1, -1, 1, -1};
const int dy[] = {1, -1, 1, -1, 2, 2, -2, -2};

int n, m, head, tail, step;
int pre[MAXN * MAXM], a[MAXN][MAXM];
bool vis[MAXN][MAXM];
struct rpg {
	int x, y;
}start, que[MAXN * MAXM];

int read() {
	int x = 0, flag = 1;
	char ch = getchar();
	if(ch < 0) flag = false;
	while(ch < '0' || ch >'9') ch = getchar();
	for(; ch >= '0' && ch <= '9'; ch = getchar())
		x = x * 10 + ch - '0';
	return flag ? x : -x;
}

void find(int x) {
	while(pre[x]) {
		x = pre[x];
		step++;
	}
}
void bfs() {
	head = tail = 1;
	que[tail] = (rpg){start.x, start.y};
	vis[start.x][start.y] = 1;
	while(head <= tail) {
		for(int i = 0; i < 8; i++) {
			int xx = que[head].x + dx[i],
				yy = que[head].y + dy[i];
			if(xx > 0 && xx <= n && yy > 0 && yy <= m && !vis[xx][yy]) {
				vis[xx][yy] = 1;
				que[++tail] = (rpg){xx, yy};
				pre[tail] = head;
				find(tail);
				a[xx][yy] = step;
				step = 0;//每一步进行完之后要对step清零，但是不能清空pre
			}
		}
		head++;
	}
}
int main() {
	memset(a, -1, sizeof(a));
	n = read();
	m = read();
	start.x = read();
	start.y = read();
	a[start.x][start.y] = 0;
	
	bfs();
	for(int i = 1; i <= n; i++) {
		for(int j = 1; j <= m; j++) printf("%-5d", a[i][j]);
		puts("");
	}
}
```
