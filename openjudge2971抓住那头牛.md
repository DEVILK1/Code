http://noi.openjudge.cn/ch0205/solution/13269505/

一开始RE，发现判断要不要走这个点的时候不仅要x>=0，而且x要比数组的最大长度（数据范围）小，不然x*2一下子就炸了

Openjudge这道题AC显示的分数也是0分..WA了以后一直在改算法，以为是算法有问题，

发现当n==k的时候要特判（step=0），算法本身没有问题…QwQ，原来只是WA掉了一个点

```c++
#include<iostream>
#include<cstdio>
#include<cstdlib>
using namespace std;

const int MAXN = 100000 + 5;

int n, k, head, tail, step;
int que[MAXN], pre[MAXN];
bool flag, vis[MAXN];

void find(int x) {
  	//不使用递归防止步数太多而爆栈
	while(pre[x]) {
//		cout << que[x] << "<-";
		x = pre[x];
		step++;
	}
//	cout << que[x] << endl;
}
void bfs(int start) {
	head = tail = 1;
	que[tail] = start;
	vis[start] = 1;
	while(head <= tail) {
		for(int i = 1; i <= 3; i++) {
			int x;
			if(i == 1) x = que[head] - 1;
			else if(i == 2) x = que[head] + 1;
			else if(i == 3) x = que[head] * 2;
			if(x >= 0 && !vis[x] && x < MAXN) {
				vis[x] = 1;
				que[++tail] = x;
				pre[tail] = head;
				if(x == k) {
					find(tail);
					printf("%d\n", step);
					head = tail;
				}
			}
		}
		head++;
	}
}
int main() {
	scanf("%d%d", &n, &k);
	if(n == k) {
		puts("0");
		return 0;
	}
	bfs(n);
}
```
