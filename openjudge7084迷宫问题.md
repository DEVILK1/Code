http://noi.openjudge.cn/ch0205/7084/

关于路径输出的问题，如果从tail一直往回推的话输出的路径是倒着的，

所以开一个out的二维数组记录需要输出的点的坐标，

用out[0][0]记录所存的点的数目.

还有一点需要注意的是，题目中的坐标是从(0, 0)开始，第一个点用(1, 1)存的话最后输出时值要-1.
```c++
#include<iostream>
#include<cstdio>
using namespace std;

const int N = 5;
const int MAXN = 5 + 1;
const int dx[] = {1, -1, 0, 0}, dy[] = {0, 0, 1, -1};

int head, tail;
int a[MAXN][MAXN], que_x[MAXN * MAXN], que_y[MAXN * MAXN], pre[MAXN * MAXN];
bool vis[MAXN][MAXN];

int out[MAXN * MAXN][2];
void find(int x) {
	out[++out[0][0]][0] = que_x[x] - 1;
	out[out[0][0]][1] = que_y[x] - 1;
	while(pre[x]) {
		out[++out[0][0]][0] = que_x[pre[x]] - 1;
		out[out[0][0]][1] = que_y[pre[x]] - 1;
		x = pre[x];
	}
	for(int i = out[0][0]; i >= 1; i--)
		printf("(%d, %d)\n", out[i][0], out[i][1]);
}
void bfs(int x, int y) {
	head = tail = 1;
	vis[x][y] = 1;
	que_x[tail] = x;
	que_y[tail] = y;
	while(head <= tail) {
		for(int i = 0; i < 4; i++) {
			int xx = que_x[head] + dx[i],
				yy = que_y[head] + dy[i];
			if(xx > 0 && xx < 6 && yy >0 && yy < 6 && !vis[xx][yy] && !a[xx][yy]) {
				vis[xx][yy] = 1;
				que_x[++tail] = xx;
				que_y[tail] = yy;
				pre[tail] = head;
			}
			if(xx == 5 && yy == 5) {
				find(tail);
				head = tail;
				break;
			}
		}
		head++;
	}
}
int main() {
	for(int i = 1; i <= N; i++)
		for(int j = 1; j <= N; j++) scanf("%d", &a[i][j]);
	bfs(1, 1);
}
```
