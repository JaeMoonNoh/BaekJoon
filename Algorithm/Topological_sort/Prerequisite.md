# 백준 선수과목(Prerequisite) 14567

## 문제
올해 Z대학 컴퓨터공학부에 새로 입학한 민욱이는 학부에 개설된 모든 전공과목을 듣고 졸업하려는 원대한 목표를 세웠다. 어떤 과목들은 선수과목이 있어 해당되는 모든 과목을 먼저 이수해야만 해당 과목을 이수할 수 있게 되어 있다. 공학인증을 포기할 수 없는 불쌍한 민욱이는 선수과목 조건을 반드시 지켜야만 한다. 민욱이는 선수과목 조건을 지킬 경우 각각의 전공과목을 언제 이수할 수 있는지 궁금해졌다. 계산을 편리하게 하기 위해 아래와 같이 조건을 간소화하여 계산하기로 하였다.

  1. 한 학기에 들을 수 있는 과목 수에는 제한이 없다.
  2. 모든 과목은 매 학기 항상 개설된다.
모든 과목에 대해 각 과목을 이수하려면 최소 몇 학기가 걸리는지 계산하는 프로그램을 작성하여라.

## 입력

첫 번째 줄에 과목의 수 N(1≤N≤1000)과 선수 조건의 수 M(0≤M≤500000)이 주어진다. 선수과목 조건은 M개의 줄에 걸쳐 한 줄에 정수 A B 형태로 주어진다. A번 과목이 B번 과목의 선수과목이다. A<B인 입력만 주어진다. (1≤A<B≤N)

## 출력

1번 과목부터 N번 과목까지 차례대로 최소 몇 학기에 이수할 수 있는지를 한 줄에 공백으로 구분하여 출력한다.

## 전체 코드


```c++
#include<iostream>
#include<vector>
#include<queue>

#define MAX 1001
using namespace std;

int n,m;
vector<int> adj[MAX]; // adjacent 인접 그래프
int subject[MAX]; // 이수할 과목들 index로 나타내기 
int result[MAX]; // 해당 인덱스 과목을 듣기 위해서 몇번 들어야 하나
queue<int> q; // 순서대로 들어야하기 때문에 queue를 사용해서 문제 해결

int main()
{
	cin >> n >> m;
	
	for(int i = 0; i < m; i++)
	{
		int from,to;
		cin >> from >> to;
		
		adj[from].push_back(to);
		subject[to]++;
	}
	
	for(int i = 0; i <= n; i++)
	{
		if(subject[i] == 0) // 선수 과목이 없고 시작점인 인덱스를 queue에 넣기 
			q.push(i);
			
		result[i] = 1;
	}
	
	while(!q.empty())
	{
		int now = q.front();
		q.pop();
		
		for(int i = 0; i < adj[now].size(); i++)
		{
			int next = adj[now][i]; // now : 현재 과목, next : 다음 과목
			subject[next]--; // 현재과목 다음과목을 들었음으로 지워준다.
			
			if(subject[next] == 0)
			{
				q.push(next); // 다음 과목이 0이 되면 이제 이것이 선수 과목이 된다. 
				result[next] = max(result[next], result[now] + 1); 
				// 다음 최대 값이 이전 선수과목을 마치고 + 1 보다 크면 바꿔주지 
			} 
		}
	}
	
	for(int i = 1; i <= n ;i++)
		cout << result[i] << " ";
	 
	return 0;
} 


```
