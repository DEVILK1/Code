http://noi.openjudge.cn/ch0205/2753/

```c++
#include<iostream>
#include<cstdio>
#include<cstdlib>
using namespace std;

const int MAXN = 40 + 1;
const int dx[] = {1, -1, 0, 0};
const int dy[] = {0, 0, 1, -1};

int n, m, head, tail, step;
int a[MAXN * 2][MAXN], que_x[MAXN * MAXN * 2], que_y[MAXN * MAXN * 2], pre[MAXN * MAXN * 2];
bool vis[MAXN * 2][MAXN];

void find(int x) {
	if(pre[x]) find(pre[x]);
	step++;
}
void print() {
	for(int i = 1; i <= n; i++) {
		for(int j = 1; j <= m; j++) cout << vis[i][j] << " ";
		cout << endl;
	}
	cout << endl;
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
			if(xx > 0 && xx <= n && yy > 0 && yy <= m && !vis[xx][yy] && !a[xx][yy]) {
				vis[xx][yy] = 1;
				que_x[++tail] = xx;
				que_y[tail] = yy;
				pre[tail] = head;
			}
			if(xx == n && yy == m) {
				find(tail);
				printf("%d\n", step);
				exit(0);
			}
		}
//		print();
		head++;
	}
}
int main() {
	scanf("%d%d", &n, &m);
	for(int i = 1; i <= n; i++)
		for(int j = 1; j <= m; j++) {
			char ch;
			cin >> ch;
			if(ch == '#') a[i][j] = 1;
		}
	bfs(1, 1);
//	find(tail);
//	printf("%d\n", step - 1);
}
```
