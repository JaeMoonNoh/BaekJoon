# 백준 N과 M(2) 15650

## 문제

자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

  * 1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열
  * 고른 수열은 오름차순이어야 한다.

## 입력

첫째 줄에 자연수 N과 M이 주어진다. (1 ≤ M ≤ N ≤ 8)

## 출력

한 줄에 하나씩 문제의 조건을 만족하는 수열을 출력한다. 중복되는 수열을 여러 번 출력하면 안되며, 각 수열은 공백으로 구분해서 출력해야 한다.

수열은 사전 순으로 증가하는 순서로 출력해야 한다.

## 전체 코드

```c++
//예제
4 2

1 2
1 3
1 4
2 3
2 4
3 4

```


```c++
#include<iostream>
#include<algorithm>
#include<vector>

#define MAX 8 + 1

using namespace std;

int n,m;
int visited[MAX];
vector<int> v;

void DFS(int cnt, int idx)
{
	if(cnt == m)
	{
		for(int i = 0; i < m; i++)
		{
			cout << v[i] << " ";
		}
		cout << "\n";
		
		return;
	}
	
	for(int i = idx + 1; i <= n; i++) // i = idx 이 부분이 중요한 부분, 기초적인 부분이다. 보통은 중복되어서 나오지 않게 하기 때문에
	{
		if(!visited[i])
		{
			visited[i] = true;
			v.push_back(i);
			
			DFS(cnt + 1, i); // idx : 중복되지 않게 1부터 시작해서 ~ n까지 중복 되지 않게 
			
			visited[i] = false;
			v.pop_back();
		}
	}
}

int main()
{
	
	cin >> n >> m;
	
	DFS(0,0);
	
	return 0;
} 
```
