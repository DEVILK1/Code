http://noi.openjudge.cn/ch0206/1768/

一道很有意思的dp题目，因为 “*矩阵的大小定义为矩阵中所有元素的和* ”，

所以用前缀和来处理每一行，就能在O(n)的时间内求出一个n行k列的矩阵大小（1 <= k <= n）

类似于最大子段和的题目，只不过把子段和变成了矩阵中每个元素的和.

DP中的贪心思想：元素有可能是负数，当一行为负时，便把这一行舍去.

```c++
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;

const int MAXN = 100 + 1;
const int INF = 0x3f3f3f3f;

int n, ans = -INF, sum;
int a[MAXN][MAXN], f[MAXN][MAXN];

int main() {
	scanf("%d", &n);
	for(int i = 1; i <= n; i++)
		for(int j = 1; j <= n; j++) {
			scanf("%d", &a[i][j]);
			a[i][j] += a[i][j - 1];
		}
	for(int i = 1; i <= n; i++) {
		for(int j = i; j <= n; j++) {
			sum = 0;
			for(int k = 1; k <= n; k++) {
				sum += a[k][j] - a[k][i - 1];
				ans = max(ans, sum);
				if(sum < 0) sum = 0;
			}
		}
	}
	cout << ans << endl;
}
```
