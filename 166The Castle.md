http://noi.openjudge.cn/ch0205/166/

第一次一遍过的bfs，from**IOI1994**(划重点，不过好像以前的竞赛题都很水)

其实就是bfs求连通块的数目和最大的连通块的“块数”

这道题目最关键的是如何处理 *'wall'* 的问题
对于1 2 4 8，分别表示的是west north east south，

读入m*n的矩阵中每个数是每面墙所表示的数的和，如何拆分这个数？

枚举的话无疑会增大时间复杂度，试着找一找规律：

1 = 2^0（2的0次方）

2 = 2^1

4 = 2^2

8 = 2^3

将数x分别与1、2、4、8进行按位与，结果非0数x的拆分中便有1（、2、4、8）

BIN(1) = 0001

BIN(2) = 0010

BIN(2 + 1) = BIN(3) = 0011

BIN(4) = 0100

BIN(4 + 1) = BIN(5) = 0101

BIN(4 + 2) = BIN(6) = 0110

BIN(4 + 2 + 1) = BIN(7) = 0111

这样规律就很明显了，也就是说我们可以用四个位运算来判断块(i, j)的四面有没有墙

建墙的时候开一个三维数组，最后一维有四个数分别表示四个方向的墙，用刚刚找过的规律建墙就好了

核心代码：
```c++
scanf("%d", &b[i][j]);
a[i][j][0] = (b[i][j]&1);//build the walls
a[i][j][1] = (b[i][j]&2);
a[i][j][2] = (b[i][j]&4);
a[i][j][3] = (b[i][j]&8);
 ```

最后只需要在bfs中判断能不能走的时候加一些小小的改动就好啦
```c++
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;

const int MAXN = 50 + 5;
const int dx[] = {1, -1, 0, 0};
const int dy[] = {0, 0, 1, -1};
//0:down 1:up 2:right 3:left

int n, m, head, tail;
int sign[MAXN][MAXN], b[MAXN][MAXN];
int que_x[MAXN * MAXN], que_y[MAXN * MAXN];
bool a[MAXN][MAXN][4], vis[MAXN][MAXN];
//0:west 1:north 2:east 3:south

void bfs(int x, int y) {
	head = tail = 1;
	vis[x][y] = 1;
	que_x[tail] = x;
	que_y[tail] = y;
	while(head <= tail) {
		for(int i = 0; i < 4; i++) {
			bool flag = false;
			int xx = que_x[head] + dx[i],
				yy = que_y[head] + dy[i];
			if(xx > 0 && xx <= m && yy > 0 && yy <= n && !vis[xx][yy]) {
				if(i == 0) if(!a[xx][yy][1]) flag = 1;
				if(i == 1) if(!a[xx][yy][3]) flag = 1;
				if(i == 2) if(!a[xx][yy][0]) flag = 1;
				if(i == 3) if(!a[xx][yy][2]) flag = 1;

				if(flag) {
					vis[xx][yy] = 1;
					que_x[++tail] = xx;
					que_y[tail] = yy;
				}
			}
		}
		head++;
	}
}
int num, maxn;
int main() {
	scanf("%d%d", &m, &n);
	for(int i = 1; i <= m; i++) {
		for(int j = 1, x; j <= n; j++) {
			scanf("%d", &b[i][j]);
			a[i][j][0] = (b[i][j]&1);//build the walls
			a[i][j][1] = (b[i][j]&2);
			a[i][j][2] = (b[i][j]&4);
			a[i][j][3] = (b[i][j]&8);
		}
	}
	for(int i = 1; i <= m; i++) {
		for(int j = 1; j <= n; j++) {
			if(!sign[i][j]) {
				bfs(i, j);
				for(int k = 1; k <= tail; k++)
					sign[que_x[k]][que_y[k]] = tail;
				num++;
				maxn = max(maxn, tail);
			}
		}
	}
	printf("%d\n%d\n", num, maxn);
}
```
附自己写的一组测试数据：
```c++
//3 4
//11 6 3 6
//7 13 1 12
//9 10 12 15
```
