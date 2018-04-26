https://www.luogu.org/problemnew/show/P1379

想练一练双向bfs就来做这道题了..结果普通的bfs就过了...

将 3 * 3 的棋盘看做一个长度为9的string，对这个字符串进行bfs

判重问题：

string对bool或者int的映射比较慢（虽然不知道为什么但就是慢），

可以先对字符串hash一遍再使用map<int, int>或者map<int, bool>进行离散化，效率会大大提升

```c++
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<queue>
#include<map>
#define re register
using namespace std;

const string goal = "123804765";
const int MAXN = 100 + 1;

int fro, rear, ans = 1e8;
string str;
map<int, int> h;

struct rpg {
	string s;//当前搜索到的字符串
	int step, locat;//花费的步数，locat表示当前‘0’在string中的位置
};

queue<rpg> q;
int find(string s, char ch) {
	int lens = s.size();
	for(re int i = 0; i < lens; i++)
		if(s[i] == ch) return i;
	return -1;
}
int Hash(const char *str) {
	static const int seed = 2333;
	static const int mod = 123456789;
	int ret = 0, l = strlen(str);
	for(re int i = 0; i < l; i++)
		ret = 1LL * ret * seed % mod + str[i];
	return ret % mod;
}
void bfs() {
	q.push((rpg){str, 0, find(str, '0')});
	while(!q.empty()) {
		string a = q.front().s, b = a;
		int t = q.front().step, lo = q.front().locat;
		q.pop();
		if(a == goal) {
			ans = min(ans, t);
			continue;
		}
		if(h[Hash(a.c_str())]) continue;//利用 哈希 + map 判重
		h[Hash(a.c_str())] = 1;
		if(lo < 6) {//向上走
			swap(a[lo], a[lo + 3]);
			q.push((rpg){a, t + 1, lo + 3});
			a = b;
		}
		if(lo >= 3) {//向下走
			swap(a[lo], a[lo - 3]);
			q.push((rpg){a, t + 1, lo - 3});
			a = b;
		}
		if(lo != 0 && lo != 3 && lo != 6) {//向右走
			swap(a[lo], a[lo - 1]);
			q.push((rpg){a, t + 1, lo - 1});
			a = b;
		}
		if(lo != 2 && lo != 5 && lo != 8) {//向左走
			swap(a[lo], a[lo + 1]);
			q.push((rpg){a, t + 1, lo + 1});
		}
	}
}
int main() {
	cin >> str;
	bfs();
	cout << ans << endl;
}
```
