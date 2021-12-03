# 백준 원판돌리기 17822
[원판돌리기](https://www.acmicpc.net/problem/17822)

### 문제

반지름이 1, 2, ..., N인 원판이 크기가 작아지는 순으로 바닥에 놓여있고, 원판의 중심은 모두 같다. 원판의 반지름이 i이면, 그 원판을 i번째 원판이라고 한다. 각각의 원판에는 M개의 정수가 적혀있고, i번째 원판에 적힌 j번째 수의 위치는 (i, j)로 표현한다. 수의 위치는 다음을 만족한다.

* (i, 1)은 (i, 2), (i, M)과 인접하다.
* (i, M)은 (i, M-1), (i, 1)과 인접하다.
* (i, j)는 (i, j-1), (i, j+1)과 인접하다. (2 ≤ j ≤ M-1)
* (1, j)는 (2, j)와 인접하다.
* (N, j)는 (N-1, j)와 인접하다.
* (i, j)는 (i-1, j), (i+1, j)와 인접하다. (2 ≤ i ≤ N-1)


1번 원판을 시계 방향으로 1칸 회전	2, 3번 원판을 반시계 방향으로 3칸 회전	1, 3번 원판을 시계 방향으로 2칸 회전
원판을 아래와 같은 방법으로 총 T번 회전시키려고 한다. 원판의 회전 방법은 미리 정해져 있고, i번째 회전할때 사용하는 변수는 xi, di, ki이다.

  * 번호가 xi의 배수인 원판을 di방향으로 ki칸 회전시킨다. di가 0인 경우는 시계 방향, 1인 경우는 반시계 방향이다.
  * 원판에 수가 남아 있으면, 인접하면서 수가 같은 것을 모두 찾는다.
    * 그러한 수가 있는 경우에는 원판에서 인접하면서 같은 수를 모두 지운다.
    * 없는 경우에는 원판에 적힌 수의 평균을 구하고, 평균보다 큰 수에서 1을 빼고, 작은 수에는 1을 더한다.


원판을 T번 회전시킨 후 원판에 적힌 수의 합을 구해보자.

### 입력

첫째 줄에 N, M, T이 주어진다.

둘째 줄부터 N개의 줄에 원판에 적힌 수가 주어진다. i번째 줄의 j번째 수는 (i, j)에 적힌 수를 의미한다.

다음 T개의 줄에 xi, di, ki가 주어진다.

### 출력

원판을 T번 회전시킨 후 원판에 적힌 수의 합을 출력한다.


### 알고리즘 & 자료구조

톱니바퀴랑 비슷한 문제이기 때문에 deque를 이용해서 회전을 했고, 인접한 칸은 이차원 배열에서 처리를 해주었습니다.
구현위주의 문제이고, 평균부분에서 조금 헷갈린것 제외하고는 무난한 문제였습니다.

### 구현해야할 조건

> 번호사 x의 배수인 원판은 d방향으로 k칸 회전시킨다.

```c++

    int multiple = o[i].multiple; // X의 배수
		int clock = o[i].clock; //  D 방향 0 == clock, 1 == counter clock
		int times = o[i].times; // 몇칸
				
		for(int j = 1; j <= n; j++) // deque 1번 부터
		{
			if(j % multiple == 0) // 배수
			{
				if(clock == 0) // 시계방향 
				{
					int counts = times % m; // m으로 나눠주면 그 나머지만 옮기면 된다. 굳이 전부 옮길 필요 없음
					for(int k = 0; k < counts; k++) // 시간 절약
					{
						int b = dq[j].back();
						dq[j].pop_back();
						dq[j].push_front(b);
					} 
					
				}
				else // 반시계 방향 (위 내용과 일치)
				{
					int counts = times % m;
					
					for(int k = 0; k < counts; k++)
					{
						int f = dq[j].front();
						dq[j].pop_front();
						dq[j].push_back(f);
					}
				}
			}
		}

```

> 인접한 수가 있고 그 수가 같으면 모두 지운다.


```c++


	bool all_chk = false;
	memset(visited,false,sizeof(visited));
	
  // 인접하며 같은 값은 지워주는 것이다.
	for(int i = 1; i <= n; i++)
	{
		for(int j = 0; j < m; j++)
		{
			int num = circle[i][j]; // 숫자 체크
			bool chk = false; // 인접한 숫자 발견했는지 체크
			//미리 없애주면 안댐 
			if(!num)
				continue; // 0이라면 인접해서 지워진 숫자이기 때문에 continue;
		
			for(int d = 0; d < 4; d++)
			{
				int nx = j + dx[d];
				int ny = i + dy[d]; // 4방향으로 된것
				
				if(0 <= nx && nx < m && 1 <= ny && ny <= n)
				{
					if(circle[ny][nx] == num)
					{
						visited[ny][nx] = true;
						all_chk = true; // 전체 체크도 체크를 해준다.
						chk = true;
					}
				}
			}
			
			if(j == 0) // 첫번째 자리라면, 마지막은 체크 안해줘도 된다. 어차피 돌린 결과에서 나온거라 처음만 체크하면 됨 
			{
				if(num == circle[i][m-1])
				{
					visited[i][j] = true;
					visited[i][m-1] = true;
				}		
			}
			
			if(chk) // 값이 같다면 
				visited[i][j] = true;	
		}
	}
	
	for(int i = 1; i <= n; i++)
		for(int j = 0; j < m; j++)
			if(visited[i][j])
				circle[i][j] = 0;


	if(!all_chk) // 현재 겹치는게 없음 
	{
		int sum = 0;
		int c = 0;
		double avg = 0.0;
		for(int i = 1; i <= n; i++)
		{
			for(int j = 0; j < m; j++)
			{
				if(circle[i][j] != 0)
					c++;
	
				sum += circle[i][j];
			}
		}
		
		avg = (double)sum / (double)c; // 이 부분에서 헤맸다. double은 double로 나눠줘야 하는 것 인지
		
		for(int i = 1; i <= n; i++)
		{
	
			for(int j = 0; j < m; j++)
			{
				if(!circle[i][j])
					continue;
					
				if(circle[i][j] > avg) // 평균보다 크다
				{
					circle[i][j] -= 1;
				}
				else if(circle[i][j] == avg) // 평균이랑 같으면 아무것도 하지 않음 
				else // 평균보다 작다
				{
					circle[i][j] += 1;
				}
			}
		}
	}
  

```

### 전체 문제


```c++

#include<iostream>
#include<vector>
#include<queue>
#include<cstring>
#include<algorithm>

#define MAX 51
using namespace std;

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0}; // down, up, right, left

typedef struct{
	int multiple;
	int clock;
	int times;
}Order;

int n,m,t,num;
int circle[MAX][MAX];
bool visited[MAX][MAX];
 
vector<Order> orders;
deque<int> dq[MAX];

void BFS(void)
{
	bool all_chk = false;
	memset(visited,false,sizeof(visited));
	
	for(int i = 1; i <= n; i++)
	{
		for(int j = 0; j < m; j++)
		{
			int num = circle[i][j];
			bool chk = false;
			//미리 없애주면 안댐 
			if(!num)
				continue;
		
			for(int d = 0; d < 4; d++)
			{
				int nx = j + dx[d];
				int ny = i + dy[d];
				
				if(0 <= nx && nx < m && 1 <= ny && ny <= n)
				{
					if(circle[ny][nx] == num)
					{
						visited[ny][nx] = true;
						all_chk = true;
						chk = true;
					}
				}
			}
			
			if(j == 0) // 첫번째 자리라면, 마지막은 체크 안해줘도 된다. 어차피 돌린 결과에서 나온거라 처음만 체크하면 됨 
			{
				if(num == circle[i][m-1])
				{
					visited[i][j] = true;
					visited[i][m-1] = true;
				}		
			}
			
			if(chk) // 값이 같다면 
				visited[i][j] = true;	
		}
	}
	
	for(int i = 1; i <= n; i++)
		for(int j = 0; j < m; j++)
			if(visited[i][j])
				circle[i][j] = 0;


	if(!all_chk) // 현재 겹치는게 없음 
	{
		int sum = 0;
		int c = 0;
		double avg = 0.0;
		for(int i = 1; i <= n; i++)
		{
			for(int j = 0; j < m; j++)
			{
				if(circle[i][j] != 0)
					c++;
	
				sum += circle[i][j];
			}
		}
		
		avg = (double)sum / (double)c; // 이 부분에서 헤맸다. 
		
		for(int i = 1; i <= n; i++)
		{
	
			for(int j = 0; j < m; j++)
			{
				if(!circle[i][j])
					continue;
					
				if(circle[i][j] > avg)
				{
					circle[i][j] -= 1;
				}
				else if(circle[i][j] == avg) // 평균이랑 같으면 아무것도 하지 않음 
				else
				{
					circle[i][j] += 1;
				}
			}
		}
	}

	for(int i = 1; i <= n; i++)
		for(int j = 0; j < m; j++)
			dq[i].push_back(circle[i][j]);

}

void solution(vector<Order> o)
{
	for(int i = 0; i < o.size(); i++)
	{
		int multiple = o[i].multiple;
		int clock = o[i].clock;
		int times = o[i].times;
				
		for(int j = 1; j <= n; j++)
		{
			if(j % multiple == 0)
			{
				if(clock == 0) // 시계방향 
				{
					int counts = times % m;
					for(int k = 0; k < counts; k++)
					{
						int b = dq[j].back();
						dq[j].pop_back();
						dq[j].push_front(b);
					} 
					
				}
				else // 반시계 방향 
				{
					int counts = times % m;
					
					for(int k = 0; k < counts; k++)
					{
						int f = dq[j].front();
						dq[j].pop_front();
						dq[j].push_back(f);
					}
				}
			}
		}
		
		for(int i = 1; i <= n; i++)
		{
			for(int j = 0; j < m; j++)
			{
				circle[i][j] = dq[i].front();
				dq[i].pop_front();
			}
		}
		
		BFS();
	}
}


int main()
{
	cin >> n >> m >> t;
	
	for(int i = 1; i <= n; i++)
	{
		for(int j = 0; j < m; j++)
		{
			cin >> num;
			dq[i].push_back(num);
		}
	}
	
	for(int i = 0; i < t; i++)
	{
		int x, d, k;
		cin >> x >> d >> k;
		
		orders.push_back({x,d,k});
	}
	
	solution(orders);
	
	int ans = 0;
	
	for(int i = 1; i <= n; i++)
	{
		for(int j = 0; j < m; j++)
		{
			ans += circle[i][j];
		}
	}
	
	cout << ans;
	
	return 0;
}


```


