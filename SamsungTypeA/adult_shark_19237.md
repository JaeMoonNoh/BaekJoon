# 백준 어른 상어 19237
[어른상어](https://www.acmicpc.net/problem/19237)

## 문제

청소년 상어는 더욱 자라 어른 상어가 되었다. 상어가 사는 공간에 더 이상 물고기는 오지 않고 다른 상어들만이 남아있다. 상어에는 1 이상 M 이하의 자연수 번호가 붙어 있고, 모든 번호는 서로 다르다. 상어들은 영역을 사수하기 위해 다른 상어들을 쫓아내려고 하는데, 1의 번호를 가진 어른 상어는 가장 강력해서 나머지 모두를 쫓아낼 수 있다.

N×N 크기의 격자 중 M개의 칸에 상어가 한 마리씩 들어 있다. 맨 처음에는 모든 상어가 자신의 위치에 자신의 냄새를 뿌린다. 그 후 1초마다 모든 상어가 동시에 상하좌우로 인접한 칸 중 하나로 이동하고, 자신의 냄새를 그 칸에 뿌린다. 냄새는 상어가 k번 이동하고 나면 사라진다.

각 상어가 이동 방향을 결정할 때는, 먼저 인접한 칸 중 아무 냄새가 없는 칸의 방향으로 잡는다. 그런 칸이 없으면 자신의 냄새가 있는 칸의 방향으로 잡는다. 이때 가능한 칸이 여러 개일 수 있는데, 그 경우에는 특정한 우선순위를 따른다. 우선순위는 상어마다 다를 수 있고, 같은 상어라도 현재 상어가 보고 있는 방향에 따라 또 다를 수 있다. 상어가 맨 처음에 보고 있는 방향은 입력으로 주어지고, 그 후에는 방금 이동한 방향이 보고 있는 방향이 된다.

모든 상어가 이동한 후 한 칸에 여러 마리의 상어가 남아 있으면, 가장 작은 번호를 가진 상어를 제외하고 모두 격자 밖으로 쫓겨난다.


## 입력

첫 줄에는 N, M, k가 주어진다. (2 ≤ N ≤ 20, 2 ≤ M ≤ N2, 1 ≤ k ≤ 1,000)

그 다음 줄부터 N개의 줄에 걸쳐 격자의 모습이 주어진다. 0은 빈칸이고, 0이 아닌 수 x는 x번 상어가 들어있는 칸을 의미한다.

그 다음 줄에는 각 상어의 방향이 차례대로 주어진다. 1, 2, 3, 4는 각각 위, 아래, 왼쪽, 오른쪽을 의미한다.

그 다음 줄부터 각 상어의 방향 우선순위가 상어 당 4줄씩 차례대로 주어진다. 각 줄은 4개의 수로 이루어져 있다. 하나의 상어를 나타내는 네 줄 중 첫 번째 줄은 해당 상어가 위를 향할 때의 방향 우선순위, 두 번째 줄은 아래를 향할 때의 우선순위, 세 번째 줄은 왼쪽을 향할 때의 우선순위, 네 번째 줄은 오른쪽을 향할 때의 우선순위이다. 각 우선순위에는 1부터 4까지의 자연수가 한 번씩 나타난다. 가장 먼저 나오는 방향이 최우선이다. 예를 들어, 우선순위가 1 3 2 4라면, 방향의 순서는 위, 왼쪽, 아래, 오른쪽이다.

맨 처음에는 각 상어마다 인접한 빈 칸이 존재한다. 따라서 처음부터 이동을 못 하는 경우는 없다.

## 출력

1번 상어만 격자에 남게 되기까지 걸리는 시간을 출력한다. 단, 1,000초가 넘어도 다른 상어가 격자에 남아 있으면 -1을 출력한다.

## 알고리즘 & 자료구조

구현하라는 조건들만 자세히 구현하면 되는 문제이다. 하지만 문제의 의미를 초반에 정확히 이해를 하지 못해 한동안 헷갈렸다. 방향을 정해준건지, 그 방향의 우선순위를 따지는건지
헷갈려서 예제를 이해하고 문제를 해결했다.

### 구현해야할 조건

> 상어에는 1 이상 M 이하의 자연수 번호가 붙어 있고~... 상어에 대한 구조체

```c++
typedef struct{
	int y; 
	int x;
	int dir;
	bool Live; // 상어가 겹치면 죽거나, 살거나
	int pri[5][5]; // 각 방향의 우선순위가 주어짐
}Shark;

```

> 상어가 이동 방향을 결정할 때

  * 인접한 칸 중 아무 냄새가 없는 칸의 방향으로 잡는다.
  
```c++

for(int i = 1; i <= m; i++)
		{
			if(sharks[i].Live == false)
				continue;
				
			int y = sharks[i].y;
			int x = sharks[i].x;
			int dir = sharks[i].dir;
			
			
			for(int d = 1; d <= 4; d++)
			{
				int next_dir = sharks[i].pri[dir][d];
				int ny = y + dy[next_dir];
				int nx = x + dx[next_dir];
	
				if(!board[ny][nx] && 0 <= nx && nx < n && 0 <= ny && ny < n)
				{
					sharks[i].y = ny;
					sharks[i].x = nx;
					sharks[i].dir = next_dir;
					
					q.push({{ny,nx},i});
					break;
				}
				else 
					continue;
			}

```

  * 그런 칸이 없으면 자신의 냄새가 있는 칸의 방향으로 잡는다.

```c++

			if(x == sharks[i].x && y == sharks[i].y) // 빈칸이 없었다면
			{
				for(int d = 1; d <= 4; d++) // 4방향중
				{
					int next_dir = sharks[i].pri[dir][d]; // 우선순위를 파악해서
					int nx = x + dx[next_dir];
					int ny = y + dy[next_dir];
					
					if(0 <= nx && nx < n && 0 <= ny && ny < n && board[ny][nx] == i)
					{ // 자신의 냄새가 있는 쪽으로 이동을 해주자
						sharks[i].y = ny;
						sharks[i].x = nx;
						sharks[i].dir = next_dir;
						q.push({{ny,nx},i});
						break;
					}
					else
						continue;
				}
			}

```

  * 이때, 가능한 칸이 여러 개일 수 있는데, 그 경우에는 특정한 우선 순위를 따른다.

```c++

	for(int s = 1; s <= m; s++) // 상어마다
	{
		for(int i = 1; i <= 4; i++) // 4 방향
		{	
			for(int j = 1; j <= 4; j++) // 4 방향
			{
				cin >> pri;
				sharks[s].pri[i][j] = pri;
			}	
		}	
	}
```


> 모든 상어가 이동한 후 한 칸에 여러 마리의 상어가 남아 있으면, 가장 작은 번호를 가진 상어를 제외하고 모두 겪자 밖으로 쫓아 낸다.

```c++
		while(!q.empty()) // 상어의 움직임을 여기서 전부 처리를 
		{
			int y = q.front().first.first;
			int x = q.front().first.second;
			int num = q.front().second;
			q.pop();
			
			if(smell[y][x] == k)  
				sharks[num].Live = false;
			else
			{
				board[y][x] = num;
				smell[y][x] = k;
			}
				 
		} // 이 부분을 구현하는데 , 미리 위에서 처리를 했다가 다시 queue를 사용했다.

```
> 상어의 냄새는 이동마다 1씩 떨어진다.

```c++

		for(int i = 0; i < n; i++)
		{
			for(int j = 0; j < n; j++)
			{
				if(smell[i][j] > 0)
				{
					smell[i][j]--;
					if(smell[i][j] == 0)
					{
						board[i][j] = 0;
					}
				}	
			}
		}

```




### 전체 코드

```c++
#include<iostream>
#include<vector>
#include<cstring>
#include<algorithm>
#include<queue>

#define MAX 21
using namespace std;

typedef struct{
	int y;
	int x;
	int dir;
	bool Live;
	int pri[5][5];
}Shark;

const int dx[5] = {0,0,0,-1,1};
const int dy[5] = {0,-1,1,0,0}; // 1 : up, 2 : down, 3 : left, 4 : right

int n,m,k;
int board[MAX][MAX];
int smell[MAX][MAX];
int priority_dir[5][5];
bool visited[MAX][MAX];

Shark sharks[401];

queue<pair<pair<int,int>,int> > q;

int shark_move(void)
{
	memset(visited,false,sizeof(visited));
	int time = 0;
	
	while(1)
	{	
		int cnt = 0;
		
		for(int s_count = 2; s_count <= m; s_count++)
		{
			if(sharks[s_count].Live == false)
				cnt++;
		}
		
		if(cnt == m - 1)
			return time;
		
		
		if(time >= 1000)
			return -1;
			
		for(int i = 1; i <= m; i++)
		{
			if(sharks[i].Live == false)
				continue;
				
			int y = sharks[i].y;
			int x = sharks[i].x;
			int dir = sharks[i].dir;
			
			
			for(int d = 1; d <= 4; d++)
			{
				int next_dir = sharks[i].pri[dir][d];
				int ny = y + dy[next_dir];
				int nx = x + dx[next_dir];
	
				if(!board[ny][nx] && 0 <= nx && nx < n && 0 <= ny && ny < n)
				{
					sharks[i].y = ny;
					sharks[i].x = nx;
					sharks[i].dir = next_dir;
					
					q.push({{ny,nx},i});
					break;
				}
				else 
					continue;
			}
			
			if(x == sharks[i].x && y == sharks[i].y)
			{
				for(int d = 1; d <= 4; d++)
				{
					int next_dir = sharks[i].pri[dir][d];
					int nx = x + dx[next_dir];
					int ny = y + dy[next_dir];
					
					if(0 <= nx && nx < n && 0 <= ny && ny < n && board[ny][nx] == i)
					{
						sharks[i].y = ny;
						sharks[i].x = nx;
						sharks[i].dir = next_dir;
						q.push({{ny,nx},i});
						break;
					}
					else
						continue;
				}
			}
		}	
		
		for(int i = 0; i < n; i++)
		{
			for(int j = 0; j < n; j++)
			{
				if(smell[i][j] > 0)
				{
					smell[i][j]--;
					if(smell[i][j] == 0)
					{
						board[i][j] = 0;
					}
				}	
			}
		}
		
		while(!q.empty()) // Shark move smell
		{
			int y = q.front().first.first;
			int x = q.front().first.second;
			int num = q.front().second;
			q.pop();
			
			if(smell[y][x] == k)  
				sharks[num].Live = false;
			else
			{
				board[y][x] = num;
				smell[y][x] = k;
			}
				 
		}
		
		time++;
	}
}

int main()
{
	cin >> n >> m >> k;
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < n; j++)
		{
			cin >> board[i][j];
			
			if(board[i][j])
			{
				sharks[board[i][j]].y = i;
				sharks[board[i][j]].x = j;
				sharks[board[i][j]].Live = true;
				smell[i][j] = k;
			}
		}
	}
	
	int dir;
	int pri;
	
	for(int i = 1; i <= m; i++)
	{
		cin >> dir;
		sharks[i].dir = dir;
	}
		
	for(int s = 1; s <= m; s++)
	{
		for(int i = 1; i <= 4; i++)
		{	
			for(int j = 1; j <= 4; j++)
			{
				cin >> pri;
				sharks[s].pri[i][j] = pri;
			}	
		}	
	}
	
	cout << shark_move();
	
	return 0;
}
```

### 잡기술1

문제의 흐름을 정확히 이해해야한다. 하나하나 어떻게 이동하는지 체크를하고 문제를 해결하는 습관을 기르자.
왜냐하면 흐름을 이해해야 구현을 할 수 있기 때문이다. 초반에 구현할때 쓸데없는 vector<pair<int,int> > smell로 구현해서 문제가 있었다.

