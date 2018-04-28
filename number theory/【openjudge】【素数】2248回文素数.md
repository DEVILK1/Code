http://noi.openjudge.cn/ch0113/11/

比较有效的做法是用dfs生成所有n位的回文数，再判断分别判断它们是否为素数.

生成的回文数用字符串存比较方便，判素的时候再用sprintf将字符串转换成数字.

1.关于回文数的生成：
* 从字符串的第0位一直到第(n/2 + 1)位生成，第(n - i - 1)位和第i位的数字相同
* 也就是从0~9个数字中选取(n/2 + 2)个数字的不同排列.

2.关于判素：
* 1e9的范围太大，数组存不下，自然不能用筛法，只能一个一个判断
* 一开始用miller-rabin写炸了..然后发现普通版本的判素刚好过.

3.关于去重
* 由于[0, 1e9]的范围内即是素数也是回文数的数并不多，但是数据的范围很大，所以就要用到map来离散化

```c++
#include<iostream>
#include<cstdio>
#include<ctime>
#include<algorithm>
#include<cstdlib>
#include<cmath>
#include<map>
#define random(a, b) a + rand() % (b - a + 1)
using namespace std;

map<int, int> ans, used;
int n, tot;
char s[21];

bool prime(int x) {
	if(x < 2) return false;
	if(x == 2) return true;
	int l = sqrt(x) + 1;
	for(int i = 2; i <= l; i++)
		if(x % i == 0) return false;
	return true;
}
void dfs(int t, int n) {
	if(t == (n >> 1) + 1) {
		if(s[0] == '0' && n != 1) return;
		int a;
		sscanf(s, "%d", &a);
		if(prime(a) && !used[a]) {
			ans[++tot] = a;
			used[a] = 1;
		}
		return;
	}
	for(int i = 0; i < 10; i++) {
		s[t] = s[n - t - 1] = i + '0';
		dfs(t + 1, n);
		s[t] = s[n - t - 1] = 0;
	}
}
int main() {
	srand(time(0));
	scanf("%d", &n);
	dfs(0, n);
	printf("%d\n", tot);
	for(int i = 1; i <= tot; i++)
		printf("%d ", ans[i]);
}
```
