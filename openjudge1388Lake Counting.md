http://noi.openjudge.cn/ch0205/1388/

bfs求连通块，遍历一遍读入的二维数组，对于每个是'W'的点没有搜索到就对它进行bfs，连通块的个数+1

本来以为能一遍过呢，但有两个点RE了，原来是队列的数组开的小了，
对于每个点它都有可能进队，所以考虑到最坏的情况，应该开MAXN * MAXN的空间
```c++
#include<iostream>
#include<cstdio>
using namespace std;

const int dx[] = {1, -1, 0, 0, 1, -1, 1, -1};
const int dy[] = {0, 0, 1, -1, 1, -1, -1, 1};
const int MAXN = 100 + 5;

int n, m, head, tail, ans;
int sign[MAXN][MAXN], que_x[MAXN * MAXN], que_y[MAXN * MAXN];
bool a[MAXN][MAXN], vis[MAXN][MAXN];

void bfs(int x, int y) {
	head = tail = 1;
	que_x[tail] = x;
	que_y[tail] = y;
	while(head <= tail) {
		for(int i = 0; i < 8; i++) {
			int xx = que_x[head] + dx[i], yy = que_y[head] + dy[i];
			if(xx > 0 && yy > 0 && xx <= n && yy <= m && !vis[xx][yy] && a[xx][yy]) {
				vis[xx][yy] = 1;
				que_x[++tail] = xx;
				que_y[tail] = yy;
			}
		}
		head++;
	}
}
int main() {
	scanf("%d%d", &n, &m);
	for(int i = 1; i <= n; i++)
		for(int j = 1; j <= m; j++) {
			char ch;
			cin >> ch;
			if(ch == 'W') a[i][j] = 1;
		}
	for(int i = 1; i <= n; i++) {
		for(int j = 1; j <= m; j++) {
			if(a[i][j] && !sign[i][j]) {
				bfs(i, j);
				for(int k = 1; k <= tail; k++)
					sign[que_x[k]][que_y[k]] = tail;
				ans++;
			}
		}
	}
	printf("%d\n", ans);
}
```
