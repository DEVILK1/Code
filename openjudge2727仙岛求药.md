http://noi.openjudge.cn/ch0205/2727/

调试了将近一个小时…
原来end是保留字啊..本地编译通过提交和在线IDE却CE了，就把end换成了destination...
然后发现bfs判断点有没有超出范围的时候两个都是打的n，是n*m的矩阵QwQ…

```c++
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;

const int MAXN = 20 + 5;
const int dx[] = {1, -1, 0, 0}, dy[] = {0, 0, 1, -1};

int rear, tail;
int m, n, pre[MAXN * MAXN], t;
bool flag, a[MAXN][MAXN], vis[MAXN][MAXN];
char ch;
struct rpg {
	int x, y;
}start, dest, que[MAXN * MAXN];

void init() {
	memset(a, 0, sizeof(a));
	memset(pre, 0, sizeof(pre));
	memset(que, 0, sizeof(que));
	memset(vis, 0, sizeof(vis));
	t = flag = 0;
}
void find(int x) {
	if(pre[x]) find(pre[x]);
	t++;
}

//void print() {
//	for(int i = 1; i <= m; i++) {
//		for(int j = 1; j <= n; j++) cout << vis[i][j] << " ";
//		puts("");
//	}
//	puts("");
//}

void bfs(int x, int y) {
	vis[x][y] = 1;
	rear = tail = 1;
	que[tail] = (rpg){x, y};
	while(rear <= tail) {
		if(flag) break;
		for(int i = 0; i < 4; i++) {
			if(flag) break;
			int xx = que[rear].x + dx[i],
				yy = que[rear].y + dy[i];
			if(xx >= 1 && yy >= 1 && xx <= m && yy <= n && !vis[xx][yy] && a[xx][yy] != 1) {
				que[++tail] = (rpg){xx, yy};
				pre[tail] = rear;
				vis[xx][yy] = 1;
				if(xx == dest.x && yy == dest.y) {
					find(tail);
					flag = 1;
					cout << t - 1 << endl;
				}
			}
		}
//		print();
		rear++;
	}
}
int main() {
	while(scanf("%d%d", &m, &n) == 2) {
		init();
		if(m == 0 && n == 0) break;
		for(int i = 1; i <= m; i++)
			for(int j = 1; j <= n; j++) {
				cin >> ch;
				if(ch == '@') start = (rpg){i, j};
				else if(ch == '*') dest = (rpg){i, j};
				else if(ch == '#') a[i][j] = 1;
			}
		bfs(start.x, start.y);
		if(!flag) puts("-1");
	}
}
```
