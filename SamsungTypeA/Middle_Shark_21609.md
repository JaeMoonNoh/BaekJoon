# 백준 상어 중학교 21609
[상어 중학교](https://www.acmicpc.net/problem/21609)

## 문제
상어 중학교의 코딩 동아리에서 게임을 만들었다. 이 게임은 크기가 N×N인 격자에서 진행되고, 초기에 격자의 모든 칸에는 블록이 하나씩 들어있고, 블록은 검은색 블록, 무지개 블록, 일반 블록이 있다. 일반 블록은 M가지 색상이 있고, 색은 M이하의 자연수로 표현한다. 검은색 블록은 -1, 무지개 블록은 0으로 표현한다. (i, j)는 격자의 i번 행, j번 열을 의미하고, |r1 - r2| + |c1 - c2| = 1을 만족하는 두 칸 (r1, c1)과 (r2, c2)를 인접한 칸이라고 한다.

블록 그룹은 연결된 블록의 집합이다. 그룹에는 일반 블록이 적어도 하나 있어야 하며, 일반 블록의 색은 모두 같아야 한다. 검은색 블록은 포함되면 안 되고, 무지개 블록은 얼마나 들어있든 상관없다. 그룹에 속한 블록의 개수는 2보다 크거나 같아야 하며, 임의의 한 블록에서 그룹에 속한 인접한 칸으로 이동해서 그룹에 속한 다른 모든 칸으로 이동할 수 있어야 한다. 블록 그룹의 기준 블록은 무지개 블록이 아닌 블록 중에서 행의 번호가 가장 작은 블록, 그러한 블록이 여러개면 열의 번호가 가장 작은 블록이다.

오늘은 이 게임에 오토 플레이 기능을 만드려고 한다. 오토 플레이는 다음과 같은 과정이 블록 그룹이 존재하는 동안 계속해서 반복되어야 한다.

  1. 크기가 가장 큰 블록 그룹을 찾는다. 그러한 블록 그룹이 여러 개라면 포함된 무지개 블록의 수가 가장 많은 블록 그룹, 그러한 블록도 여러개라면 기준 블록의 행이 가장 큰 것을, 그 것도 여러개이면 열이 가장 큰 것을 찾는다.
  2. 1에서 찾은 블록 그룹의 모든 블록을 제거한다. 블록 그룹에 포함된 블록의 수를 B라고 했을 때, B2점을 획득한다.
  3. 격자에 중력이 작용한다.
  4. 격자가 90도 반시계 방향으로 회전한다.
  5. 다시 격자에 중력이 작용한다.

격자에 중력이 작용하면 검은색 블록을 제외한 모든 블록이 행의 번호가 큰 칸으로 이동한다. 이동은 다른 블록이나 격자의 경계를 만나기 전까지 계속 된다.

## 입력

첫째 줄에 격자 한 변의 크기 N, 색상의 개수 M이 주어진다.

둘째 줄부터 N개의 줄에 격자의 칸에 들어있는 블록의 정보가 1번 행부터 N번 행까지 순서대로 주어진다. 각 행에 대한 정보는 1열부터 N열까지 순서대로 주어진다. 입력으로 주어지는 칸의 정보는 -1, 0, M이하의 자연수로만 이루어져 있다.

## 출력

첫째 줄에 획득한 점수의 합을 출력한다.

## 자료구조 & 알고리즘

조건을 정확히 파악하고 문제를 풀어야하는데, 항상 조건을 나중에 대입하려고하니까 조금씩 헷갈리는 부분이 나오는 것 같다.
조금 더 집중하고 파악하자! 구현 시뮬레이션!

## 구현해야할 조건

> 격자의 i번 행, j번 열을 의미하고, 인접한 칸은 상하좌우이다.

```c++
const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0};
```


> 그룹에는 일반 블록이 적어도 하나 있어야 하며, 일반 블록의 색은 모두 같아야 한다.
  > 검은색 블록(-1)은 포함되면 안되고, 무지개 블록은 얼마나 들어있든 상관 없다.

```c++

		for(int i = 0; i < n; i++)
		{
			for(int j = 0; j < n; j++)
			{
				if(board[i][j] >= 1 && board[i][j] <= m)
				{
					BFS(i,j,board[i][j]); // y,x,block_color
				}
					
			}
		}

```


> 오토 플레이는 다음과 같은 과정이 블록 그룹(1개 초과)이 존재하는 동안 계속해서 반복

```c++

```


> 크기가 가장 큰 블록 그룹을 찾는다.
  > 그러한 블록이 여러 개라면 포함된 무지개 블록의 수가 가장 많은 블록 그룹, 그러한 블록도 여러개라면 기준 블록의 행이 가장 큰것을, 그것도 여러개이면 열이 가장 큰것을 찾는다.

```c++
void BFS(int y, int x, int block)
{
	v.clear();
	memset(visited,false,sizeof(visited));
	
	queue<pair<int,int> > q;
	
	v.push_back({y,x});
	q.push({y,x});
	visited[y][x] = true;
	
	int rainbow = 0;
	int cnt = 1;
	
	while(!q.empty())
	{
		int y = q.front().first;
		int x = q.front().second;
		int color = block;
		
		q.pop();
		
		for(int i = 0; i < 4; i++)
		{
			int nx = x + dx[i];
			int ny = y + dy[i];
			
			if(0 <= nx && nx < n && 0 <= ny && ny < n && !visited[ny][nx])
			{
				if(board[ny][nx] == 0 || board[ny][nx] == color)
				{
					if(board[ny][nx] == 0)
						rainbow++;
					
					cnt++;
					visited[ny][nx] = true;
					q.push({ny,nx});
					
					if(board[ny][nx] >= 1) // rainbow는 기준이 되지 않음 
						v.push_back({ny,nx}); // 기준은 노말 블록들이 가능하니까. 
				}
			}
		}
	}
	
	// 그룹에 속한 블록의 개수는 2보다 크거나 같아야 한다. 	
  // 블록 1개 초과!
	if(max_cnt == cnt && 2 <= cnt)
	{
		max_cnt = cnt;
		sort(v.begin(),v.end(),comp);
		block_group.push_back({{v[0].first,v[0].second},rainbow});
	}
	else if(max_cnt < cnt && 2 <= cnt)
	{
		block_group.clear();
		max_cnt = cnt;
		sort(v.begin(), v.end(), comp);

		block_group.push_back({{v[0].first,v[0].second},rainbow});
	}
}
```


> 가장 큰 블록 그룹을 모두 제거하고, 블록 그룹에 포함된 블록의 수를 B라고 했을때 B * B 점을 획득한다.

```c++
void Erase_BFS(int y, int x)
{
	memset(visited,false,sizeof(visited));
	
	queue<pair<int,int> > q;
	q.push({y,x});
	
	int color = board[y][x];
	int cnt = 0;
	
	while(!q.empty())
	{
		int y = q.front().first;
		int x = q.front().second;
		
		q.pop();
		
		for(int i = 0; i < 4; i++)
		{
			int nx = x + dx[i];
			int ny = y + dy[i];
			
			if(0 <= nx && nx < n && 0 <= ny && ny < n && !visited[ny][nx])
			{
				if(board[ny][nx] == color || board[ny][nx] == 0)
				{
					cnt++;
					visited[ny][nx] = true;
					board[ny][nx] = -2;
					q.push({ny,nx});
				}
			}
		}
	}
	
	ans += cnt*cnt;
}
```


> 격자에 중력이 작용한다.

```c++
void gravity(int board[MAX][MAX])
{
	for(int i = n-1; i >= 0; i--)
	{
		for(int j = 0; j < n; j++)
		{	
		
			if(board[i][j] == -2 || board[i][j] == -1)
				continue;
			else
			{	
				int temp = board[i][j];
				board[i][j] = -2;
				
				int y = i;
				int x = j;
				
				while(1)
				{
					int ny = y + dy[0];
					
					if(board[ny][x] == -2)
						y = ny;
					else
						break;
				}
				board[y][x] = temp;
			}
		}
	}
}
```


> 격자가 90도 반시계 방향으로 회전한다.

```c++
void rotate_ccw(int board[MAX][MAX])
{
	int tempBoard[MAX][MAX];
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < n; j++)
		{
			tempBoard[i][j] = board[i][j];
		}
	}
	
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < n; j++)
		{
			board[i][j] = tempBoard[j][n - 1 - i];
		}
	}
}
```

> 정방향 시계

```c++
for(int i = 0; i < n; i++)
{
  for(int j = 0; j < n; j++)
  {
    board[i][j] = tempBoard[n - 1 - i][j];
  }
}
```


> 다시 격자에 중력이 작용한다.

```c++
void gravity(int board[MAX][MAX])
{
	for(int i = n-1; i >= 0; i--)
	{
		for(int j = 0; j < n; j++)
		{	
		
			if(board[i][j] == -2 || board[i][j] == -1)
				continue;
			else
			{	
				int temp = board[i][j];
				board[i][j] = -2;
				
				int y = i;
				int x = j;
				
				while(1)
				{
					int ny = y + dy[0];
					
					if(board[ny][x] == -2)
						y = ny;
					else
						break;
				}
				board[y][x] = temp;
			}
		}
	}
}
```
> 기준 블록 잡는 조건

```c++
bool comp(pair<int,int> a, pair<int,int> b)
{
	//first : y, second : x
	
	if(a.first == b.first)
		return a.second < b.second;
	
	return a.first < b.first;
}
```

> 같은 크기가 여러개일때 잡는 조건

```c++
bool comp2(pair<pair<int,int>,int> a, pair<pair<int,int>,int> b)
{
	// first.first : y, first.second : x, second : rainbow
		
	if(a.second == b.second)
	{
		if(a.first.first == b.first.first)
		{
			return a.first.second > b.first.second;
		}
		return a.first.first > b.first.first;
	}
	return a.second > b.second;
}
```


> 반복

## 전체 코드

```c++
#include<iostream>
#include<vector>
#include<queue>
#include<cstring>
#include<algorithm>

#define MAX 21
using namespace std;

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0};

int n,m;
int ans = 0;
int max_cnt = 0;
int board[MAX][MAX];
bool visited[MAX][MAX];
vector<pair<int,int> > v;
vector<pair<pair<int,int>,int> > block_group;

bool comp2(pair<pair<int,int>,int> a, pair<pair<int,int>,int> b)
{
	// first.first : y, first.second : x, second : rainbow
		
	if(a.second == b.second)
	{
		if(a.first.first == b.first.first)
		{
			return a.first.second > b.first.second;
		}
		return a.first.first > b.first.first;
	}
	return a.second > b.second;
}

bool comp(pair<int,int> a, pair<int,int> b)
{
	//first : y, second : x
	
	if(a.first == b.first)
		return a.second < b.second;
	
	return a.first < b.first;
}

void BFS(int y, int x, int block)
{
	v.clear();
	memset(visited,false,sizeof(visited));
	
	queue<pair<int,int> > q;
	
	v.push_back({y,x});
	q.push({y,x});
	visited[y][x] = true;
	
	int rainbow = 0;
	int cnt = 1;
	
	while(!q.empty())
	{
		int y = q.front().first;
		int x = q.front().second;
		int color = block;
		
		q.pop();
		
		for(int i = 0; i < 4; i++)
		{
			int nx = x + dx[i];
			int ny = y + dy[i];
			
			if(0 <= nx && nx < n && 0 <= ny && ny < n && !visited[ny][nx])
			{
				if(board[ny][nx] == 0 || board[ny][nx] == color)
				{
					if(board[ny][nx] == 0)
						rainbow++;
					
					cnt++;
					visited[ny][nx] = true;
					q.push({ny,nx});
					
					if(board[ny][nx] >= 1) // rainbow는 기준이 되지 않음 
						v.push_back({ny,nx}); // 기준은 노말 블록들이 가능하니까. 
				}
			}
		}
	}
	
	// 그룹에 속한 블록의 개수는 2보다 크거나 같아야 한다. 	

	if(max_cnt == cnt && 2 <= cnt)
	{
		max_cnt = cnt;
		sort(v.begin(),v.end(),comp);
		block_group.push_back({{v[0].first,v[0].second},rainbow});
	}
	else if(max_cnt < cnt && 2 <= cnt)
	{
		block_group.clear();
		max_cnt = cnt;
		sort(v.begin(), v.end(), comp);

		block_group.push_back({{v[0].first,v[0].second},rainbow});
	}
}

void Erase_BFS(int y, int x)
{
	memset(visited,false,sizeof(visited));
	
	queue<pair<int,int> > q;
	q.push({y,x});
	
	int color = board[y][x];
	int cnt = 0;
	
	while(!q.empty())
	{
		int y = q.front().first;
		int x = q.front().second;
		
		q.pop();
		
		for(int i = 0; i < 4; i++)
		{
			int nx = x + dx[i];
			int ny = y + dy[i];
			
			if(0 <= nx && nx < n && 0 <= ny && ny < n && !visited[ny][nx])
			{
				if(board[ny][nx] == color || board[ny][nx] == 0)
				{
					cnt++;
					visited[ny][nx] = true;
					board[ny][nx] = -2;
					q.push({ny,nx});
				}
			}
		}
	}
	
	ans += cnt*cnt;
}

void gravity(int board[MAX][MAX])
{
	for(int i = n-1; i >= 0; i--)
	{
		for(int j = 0; j < n; j++)
		{	
		
			if(board[i][j] == -2 || board[i][j] == -1)
				continue;
			else
			{	
				int temp = board[i][j];
				board[i][j] = -2;
				
				int y = i;
				int x = j;
				
				while(1)
				{
					int ny = y + dy[0];
					
					if(board[ny][x] == -2)
						y = ny;
					else
						break;
				}
				board[y][x] = temp;
			}
		}
	}
}

void rotate_ccw(int board[MAX][MAX])
{
	int tempBoard[MAX][MAX];
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < n; j++)
		{
			tempBoard[i][j] = board[i][j];
		}
	}
	
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < n; j++)
		{
			board[i][j] = tempBoard[j][n - 1 - i];
		}
	}
}

int main()
{
	cin >> n >> m;
	
	for(int i = 0; i < n; i++)
		for(int j = 0; j < n; j++)
			cin >> board[i][j];

	while(1)
	{
		
		block_group.clear();
		max_cnt = 0;
		for(int i = 0; i < n; i++)
		{
			for(int j = 0; j < n; j++)
			{
				if(board[i][j] >= 1 && board[i][j] <= m)
				{
					//cout << i << " " << j << endl;
					BFS(i,j,board[i][j]);
				}
					
			}
		}
	
		if(block_group.size() >= 1)
			sort(block_group.begin(), block_group.end(), comp2);
		else
			break;

		Erase_BFS(block_group[0].first.first, block_group[0].first.second);
		gravity(board);
		rotate_ccw(board);
		gravity(board);
	}

	cout << ans << endl;
	
	return 0;
}

```


