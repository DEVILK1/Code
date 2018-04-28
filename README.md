http://noi.openjudge.cn/ch0113/11/



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
