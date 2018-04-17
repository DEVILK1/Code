```c++
#include<iostream>
#include<cstdio>
#include<algorithm>
using namespace std;

const int MAXN = 100000 + 1;
int n, len = 1, a[MAXN], b[MAXN], d[MAXN], map[MAXN];

int main() {
	scanf("%d", &n);
	for(int i = 1; i <= n; i++) scanf("%d", &a[i]);
	for(int i = 1; i <= n; i++) scanf("%d", &b[i]);
	
	for(int i = 1; i <= n; i++)
		map[a[i]] = i;
	d[1] = map[b[1]];
	for(int i = 2; i <= n; i++) {
		if(map[b[i]] > d[len]) {
			d[++len] = map[b[i]];
			continue;
		}
		int l = 1, r = len, j = 0;
		while(l <= r) {
			int mid = (l + r) >> 1;
			if(d[mid] < map[b[i]]) l = mid + 1;
			else {
				r = mid - 1;
				j = mid;
			}
		}
		d[j] = map[b[i]];
	}
	printf("%d\n", len);
	
	return 0;
}
```
