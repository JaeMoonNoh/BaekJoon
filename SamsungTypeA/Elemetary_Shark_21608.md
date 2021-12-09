# 백준 상어 초등학교 21608 
[상어 초등학교](https://www.acmicpc.net/problem/21608)

## 문제
상어 초등학교에는 교실이 하나 있고, 교실은 N×N 크기의 격자로 나타낼 수 있다. 학교에 다니는 학생의 수는 N2명이다. 오늘은 모든 학생의 자리를 정하는 날이다. 학생은 1번부터 N2번까지 번호가 매겨져 있고, (r, c)는 r행 c열을 의미한다. 교실의 가장 왼쪽 윗 칸은 (1, 1)이고, 가장 오른쪽 아랫 칸은 (N, N)이다.

선생님은 학생의 순서를 정했고, 각 학생이 좋아하는 학생 4명도 모두 조사했다. 이제 다음과 같은 규칙을 이용해 정해진 순서대로 학생의 자리를 정하려고 한다. 한 칸에는 학생 한 명의 자리만 있을 수 있고, |r1 - r2| + |c1 - c2| = 1을 만족하는 두 칸이 (r1, c1)과 (r2, c2)를 인접하다고 한다.

  1. 비어있는 칸 중에서 좋아하는 학생이 인접한 칸에 가장 많은 칸으로 자리를 정한다.
  2. 1을 만족하는 칸이 여러 개이면, 인접한 칸 중에서 비어있는 칸이 가장 많은 칸으로 자리를 정한다.
  3. 2를 만족하는 칸도 여러 개인 경우에는 행의 번호가 가장 작은 칸으로, 그러한 칸도 여러 개이면 열의 번호가 가장 작은 칸으로 자리를 정한다.

예를 들어, N = 3이고, 학생 N2명의 순서와 각 학생이 좋아하는 학생이 다음과 같은 경우를 생각해보자.

학생의 번호	좋아하는 학생의 번호

4	            2, 5, 1, 7

3	            1, 9, 4, 5

9	            8, 1, 2, 3

8	            1, 9, 3, 4

7	            2, 3, 4, 8

1	            9, 2, 5, 7

6	            5, 2, 3, 4

5	            1, 9, 2, 8

2	            9, 3, 1, 4


가장 먼저, 4번 학생의 자리를 정해야 한다. 현재 교실의 모든 칸은 빈 칸이다. 2번 조건에 의해 인접한 칸 중에서 비어있는 칸이 가장 많은 칸인 (2, 2)이 4번 학생의 자리가 된다.
```c++	 	 
 	4	 
```
 	 	 
다음 학생은 3번이다. 1번 조건을 만족하는 칸은 (1, 2), (2, 1), (2, 3), (3, 2) 이다. 이 칸은 모두 비어있는 인접한 칸이 2개이다. 따라서, 3번 조건에 의해 (1, 2)가 3번 학생의 자리가 된다.
```c++
 	3	 
 	4	 
```
 	 	 
다음은 9번 학생이다. 9번 학생이 좋아하는 학생의 번호는 8, 1, 2, 3이고, 이 중에 3은 자리에 앉아있다. 좋아하는 학생이 가장 많이 인접한 칸은 (1, 1), (1, 3)이다. 두 칸 모두 비어있는 인접한 칸이 1개이고, 행의 번호도 1이다. 따라서, 3번 조건에 의해 (1, 1)이 9번 학생의 자리가 된다.
```c++
9	3	 
 	4	 
```
 	 	 
이번에 자리를 정할 학생은 8번 학생이다. (2, 1)이 8번 학생이 좋아하는 학생과 가장 많이 인접한 칸이기 때문에, 여기가 그 학생의 자리이다.
```c++
9	3	 
8	4	 
```
 	 	 
7번 학생의 자리를 정해보자. 1번 조건을 만족하는 칸은 (1, 3), (2, 3), (3, 1), (3, 2)로 총 4개가 있고, 비어있는 칸과 가장 많이 인접한 칸은 (2, 3), (3, 2)이다. 행의 번호가 작은 (2, 3)이 7번 학생의 자리가 된다.
```c++
9	3	 
8	4	7
```
 	 	 
이런식으로 학생의 자리를 모두 정하면 다음과 같다.
```c++
9	3	2
8	4	7
5	6	1
```
이제 학생의 만족도를 구해야 한다. 학생의 만족도는 자리 배치가 모두 끝난 후에 구할 수 있다. 학생의 만족도를 구하려면 그 학생과 인접한 칸에 앉은 좋아하는 학생의 수를 구해야 한다. 그 값이 0이면 학생의 만족도는 0, 1이면 1, 2이면 10, 3이면 100, 4이면 1000이다.

학생의 만족도의 총 합을 구해보자.

## 입력
첫째 줄에 N이 주어진다. 둘째 줄부터 N2개의 줄에 학생의 번호와 그 학생이 좋아하는 학생 4명의 번호가 한 줄에 하나씩 선생님이 자리를 정할 순서대로 주어진다.

학생의 번호는 중복되지 않으며, 어떤 학생이 좋아하는 학생 4명은 모두 다른 학생으로 이루어져 있다. 입력으로 주어지는 학생의 번호, 좋아하는 학생의 번호는 N2보다 작거나 같은 자연수이다. 어떤 학생이 자기 자신을 좋아하는 경우는 없다.

## 출력

첫째 줄에 학생의 만족도의 총 합을 출력한다.

## 자료구조 & 알고리즘

단순 구현, 신경쓸게 좀 있다.

## 구현해야할 조건

> 비어있는 칸 중에서 좋아하는 학생이 인접한 칸에 가장 많은 칸으로 자리를 정한다.


```c++
	while(!arrangement.empty()) // queue에 있는 학생들 순서대로 세우기
	{
		int a = arrangement.front();
		arrangement.pop();
		
		if(empty)
		{
			visited[1][1] = true; //행의 번호가 작고, 열의 번호가 가장 작은 칸이라면 1,1이다.
			classes[1][1] = a;
			empty = false;
		}
		else
		{
			check.clear();
			
			for(int i = 0; i < n; i++)
			{
				for(int j = 0; j < n; j++)
				{
					if(visited[i][j])
						continue;
						
					int y = i;
					int x = j;
					int cnt = 0;
					int emptyCnt = 0;
					
					for(int d = 0; d < 4; d++)
					{
						int nx = x + dx[d];
						int ny = y + dy[d];
						
						if(nx < 0 || ny < 0 || nx >= n || ny >= n)
						{
							continue;
						}	
						if(classes[ny][nx] == 0)
						{
							emptyCnt++;
						}
						else
						{
							for(int k = 0; k < friends[a].size(); k++)
							{
								if(classes[ny][nx] == friends[a][k])
								{
									cnt++;
								}
							}
						}
					}
										
					check.push_back({{y,x},{cnt,emptyCnt}});
				}
			}
			
			sort(check.begin(),check.end(),comp);

			visited[check[0].first.first][check[0].first.second] = true;
			classes[check[0].first.first][check[0].first.second] = a;	
		}
	}
```


> 2를 만족하는 칸도 여러 개인 경우에는 행의 번호가 가장 작은 칸으로, 그러한 칸도 여러 개이면 열의 번호가 가장 작은 칸으로 자리를 정한다.


```c++
bool comp(pair<pair<int,int>,pair<int,int> > a, pair<pair<int,int>,pair<int, int> > b)
{	
	if(a.second.first == b.second.first)
	{
		if(a.second.second == b.second.second)
		{
			if(a.first.first == b.first.first)
			{
				return a.first.second < b.first.second;
			}
			return a.first.first < b.first.first;
		}
		return a.second.second > b.second.second;
	}
	return a.second.first > b.second.first;
}

// 전체의 조건 compare

```

## 전체 코드


```c++

#include<iostream>
#include<vector>
#include<queue>
#include<algorithm> 
#include<cstring>

#define MAX 25
using namespace std;

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0}; 
//down, up, right, left

int n,student,bf;
bool visited[MAX][MAX];
vector<int> friends[MAX*MAX];
vector<pair<pair<int,int>,pair<int,int> > > check;
queue<int> arrangement;

bool comp(pair<pair<int,int>,pair<int,int> > a, pair<pair<int,int>,pair<int, int> > b)
{	
	if(a.second.first == b.second.first)
	{
		if(a.second.second == b.second.second)
		{
			if(a.first.first == b.first.first)
			{
				return a.first.second < b.first.second;
			}
			return a.first.first < b.first.first;
		}
		return a.second.second > b.second.second;
	}
	return a.second.first > b.second.first;
}

int main()
{
	cin >> n;
	int classes[n][n];
	memset(visited,false,sizeof(visited));
	memset(classes,0,sizeof(classes));
	
	for(int i = 0; i < n*n; i++)
	{
		cin >> student;
		arrangement.push(student);
		for(int j = 0; j < 4; j++)
		{
			cin >> bf;
			friends[student].push_back(bf);
		}
	}
	
	bool empty = true;
	
	while(!arrangement.empty())
	{
		int a = arrangement.front();
		arrangement.pop();
		
		if(empty)
		{
			visited[1][1] = true; 
			classes[1][1] = a;
			empty = false;
		}
		else
		{
			check.clear();
			
			for(int i = 0; i < n; i++)
			{
				for(int j = 0; j < n; j++)
				{
					if(visited[i][j])
						continue;
						
					int y = i;
					int x = j;
					int cnt = 0;
					int emptyCnt = 0;
					
					for(int d = 0; d < 4; d++)
					{
						int nx = x + dx[d];
						int ny = y + dy[d];
						
						if(nx < 0 || ny < 0 || nx >= n || ny >= n)
						{
							continue;
						}	
						if(classes[ny][nx] == 0)
						{
							emptyCnt++;
						}
						else
						{
							for(int k = 0; k < friends[a].size(); k++)
							{
								if(classes[ny][nx] == friends[a][k])
								{
									cnt++;
								}
							}
						}
					}
										
					check.push_back({{y,x},{cnt,emptyCnt}});
				}
			}
			
			sort(check.begin(),check.end(),comp);

			visited[check[0].first.first][check[0].first.second] = true;
			classes[check[0].first.first][check[0].first.second] = a;	
		}
	}


	int sum = 0;
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j <n; j++)
		{
			int y = i;
			int x = j;
			int cnt = 0;
			for(int d = 0; d < 4; d++)
			{
				int nx = x + dx[d];
				int ny = y + dy[d];
				
				if(nx < 0 || ny < 0 || nx >= n || ny >= n)
				{
					continue;
				}
				for(int k = 0; k < friends[classes[y][x]].size(); k++)
				{
					if(classes[ny][nx] == friends[classes[y][x]][k])
						cnt++;
				}
				
			}
			
			if(cnt == 0)
				sum += 0;
			else if(cnt == 1)
				sum += 1;
			else if(cnt == 2)
				sum += 10;
			else if(cnt == 3)
				sum += 100;
			else
				sum += 1000;
		}
	} 
	
	cout << sum;
		
	return 0;
}

```
