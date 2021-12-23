# 백준 N과 M(1) 15649

## 문제

자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

  * 1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열

## 입력
첫째 줄에 자연수 N과 M이 주어진다. (1 ≤ M ≤ N ≤ 8)

## 출력
한 줄에 하나씩 문제의 조건을 만족하는 수열을 출력한다. 중복되는 수열을 여러 번 출력하면 안되며, 각 수열은 공백으로 구분해서 출력해야 한다.

수열은 사전 순으로 증가하는 순서로 출력해야 한다.

## 전체 코드

```c++

#include<iostream>
#include<algorithm>
#include<vector>

#define MAX 8 + 1
using namespace std;

int n,m;
bool visited[MAX];
vector<int> v;

/*
4 2

1 2
1 3
1 4
2 1
2 3
2 4
3 1
3 2
3 4
4 1
4 2
4 3

*/

void DFS(int cnt)
{
	if(cnt == m) // M 크기만큼의 값을 갖으면
	{
		for(int i = 0; i < m; i++) 
		{
			cout << v[i] << " ";
		}
		cout << "\n";
		return; // return을 항상 써야한다. 
	} 
	
	for(int i = 1; i <= n; i++)
	{
		if(!visited[i]) // 중복되지 않기 위해서 사용, 그리고 1~n까지의 숫자
		{
			visited[i] = true;//1
			v.push_back(i);//3
			
			DFS(cnt+1); // 숫자가 하나 들어갔기 때문에 cnt+1을 해준다. 
			
			v.pop_back();//4
			visited[i] = false;//2
		} 
	}
}
int main()
{
	cin >> n >> m;
	
	DFS(0);
	
	return 0;
}

```
