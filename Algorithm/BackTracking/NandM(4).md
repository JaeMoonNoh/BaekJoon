## 예시

```c++
4 2

1 1
1 2
1 3
1 4
2 2
2 3
2 4
3 3
3 4
4 4
```



## 전체 코드

```c++
#include<iostream>
#include<algorithm>
#include<vector>

#define MAX 8 + 1
using namespace std;

int N,M; // N : number, M : size
int visited[MAX];
vector<int> v;
void DFS(int cnt,int idx)
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
		if(i < idx) // 중복은 되지만 이전값을 포함하지는 않는다. idx를 0부터 시작해서 1을 포함한 후, 2부터는 1을 포함하지 않는다.
			continue;
		v.push_back(i);
		DFS(cnt + 1,i);
		v.pop_back();
	}
	
}

int main()
{
	cin >> N >> M;
	
	DFS(0,0);

	return 0;
}
```
