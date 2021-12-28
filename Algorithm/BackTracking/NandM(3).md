# 백준 N과 M(3) 15651

## 입력

```c++
4 2

1 1
1 2
1 3
1 4
2 1
2 2
2 3
2 4
3 1
3 2
3 3
3 4
4 1
4 2
4 3
4 4
```

## 전체 코드

중복이 되면서, 한칸씩 이동

```c++
#include<iostream>
#include<algorithm>
#include<vector>

#define MAX 8 + 1
using namespace std;

int N,M; // N : number, M : size
int visited[MAX];
vector<int> v;
void DFS(int cnt)
{
	if(cnt == M)
	{
		for(int i = 0 ; i < M; i++)
		{
			cout << v[i] << " ";
		}
		cout << "\n";
		return;
	}
	
	for(int i = 1; i <= N; i++)
	{
		v.push_back(i);
		DFS(cnt + 1);
		v.pop_back();
	}
	
}

int main()
{
	cin >> N >> M;
	
	DFS(0);

	return 0;
}
```
