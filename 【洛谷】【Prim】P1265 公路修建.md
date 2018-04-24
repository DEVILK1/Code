https://www.luogu.org/problemnew/show/P1265

关于环形公路的判断？不存在的，直接打MST就好.

有一点需要注意：
读入了n个点，最多有$ \frac{n * (n - 1)}{2} $条边，用Kruskal的话会爆内存，而且在稠密图中也没有Prim跑得快

所以自然是用Prim，而且不存储每条边的长度，而是在运行过程中用到了再计算（这是Kruskal无法实现的）

```c++
#include<iostream>
#include<cstdio>
#include<cmath>
#include<algorithm>
#include<queue>
#include<vector>
using namespace std;

const int MAXN = 5000 + 1, MAXM = MAXN * (MAXN - 1) >> 1;
const int INF = 0x3f3f3f3f;

int n, m;
bool vis[MAXN];
double dis[MAXN], ans;

struct point {
    int x, y;
}a[MAXN];

int read();
inline double dist(point a, point b) {
    return sqrt((a.x - b.x) * 1.0 * (a.x - b.x) + (a.y - b.y) * 1.0 * (a.y - b.y));
}

int main() {
    n = read();
    for(int i = 1; i <= n; i++) {
        a[i].x = read();
        a[i].y = read();
    }
    int minn, k;
    for(int i = 1; i <= n; i++) dis[i] = dist(a[1], a[i]);
    vis[1] = 1;
    for(int i = 1; i < n; i++) {
        k = 0, minn = INF;
        for(int j = 1; j <= n; j++)
            if(!vis[j] && minn > dis[j]) minn = dis[j], k = j;
        vis[k] = 1;
//		cout << k << " " << i << " " << dis[k] << endl;
        ans += dis[k];
        for(int j = 1; j <= n; j++)
            if(!vis[j] && dis[j] > dist(a[j], a[k])) dis[j] = dist(a[j], a[k]);
    }
    printf("%0.2lf\n", ans);
}
int read() {
    int x = 0, f = 1; char ch = getchar();
    while(ch < '0' || ch > '9') {
        if(ch == '-') f = -1;
        ch = getchar();
    }
    for(;ch >= '0' && ch <= '9'; ch = getchar())
        x = (x << 3) + (x << 1) + (ch ^ 48);
    return x * f;
}
```
