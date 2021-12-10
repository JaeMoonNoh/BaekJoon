# 백준 마법사 상어와 파이어스톰 20058
[마법사 상어와 파이어스톰](https://www.acmicpc.net/problem/20058)

## 문제

마법사 상어는 파이어볼과 토네이도를 조합해 파이어스톰을 시전할 수 있다. 오늘은 파이어스톰을 크기가 2N × 2N인 격자로 나누어진 얼음판에서 연습하려고 한다. 위치 (r, c)는 격자의 r행 c열을 의미하고, A[r][c]는 (r, c)에 있는 얼음의 양을 의미한다. A[r][c]가 0인 경우 얼음이 없는 것이다.

파이어스톰을 시전하려면 시전할 때마다 단계 L을 결정해야 한다. 파이어스톰은 먼저 격자를 2L × 2L 크기의 부분 격자로 나눈다. 그 후, 모든 부분 격자를 시계 방향으로 90도 회전시킨다. 이후 얼음이 있는 칸 3개 또는 그 이상과 인접해있지 않은 칸은 얼음의 양이 1 줄어든다. (r, c)와 인접한 칸은 (r-1, c), (r+1, c), (r, c-1), (r, c+1)이다. 아래 그림의 칸에 적힌 정수는 칸을 구분하기 위해 적은 정수이다.

마법사 상어는 파이어스톰을 총 Q번 시전하려고 한다. 모든 파이어스톰을 시전한 후, 다음 2가지를 구해보자.

  1. 남아있는 얼음 A[r][c]의 합
  2. 남아있는 얼음 중 가장 큰 덩어리가 차지하는 칸의 개수

얼음이 있는 칸이 얼음이 있는 칸과 인접해 있으면, 두 칸을 연결되어 있다고 한다. 덩어리는 연결된 칸의 집합

## 입력

첫째 줄에 N과 Q가 주어진다. 둘째 줄부터 2N개의 줄에는 격자의 각 칸에 있는 얼음의 양이 주어진다. r번째 줄에서 c번째 주어지는 정수는 A[r][c] 이다.

마지막 줄에는 마법사 상어가 시전한 단계 L1, L2, ..., LQ가 순서대로 주어진다.

## 출력

첫째 줄에 남아있는 얼음 A[r][c]의 합을 출력하고, 둘째 줄에 가장 큰 덩어리가 차지하는 칸의 개수를 출력한다. 단, 덩어리가 없으면 0을 출력한다.

## 자료구조 & 알고리즘

구현, 시뮬레이션!!

## 구현해야할 조건

> 2의N승 * 2의N승 격자로 나누어진 얼음판에서 연습을 한다.

```c++
	N = 1 << N; // 비트마스크를 사용해서 이동시킨다 Shift
	
	for(int i = 0; i < N; i++)
	{
		for(int j = 0; j < N; j++)
		{
			cin >> board[i][j];
		}
	}
```


> 파이어스톰을 시전하려면 시전할 때마다 단계 L을 결정해야 한다.
> 2의L승 * 2의L승 크기의 부분 격자로 나눠, 그 부분을 시계 방향으로 90도 회전 시킨다.

```c++
void rotate(int x, int y, int len)
{
//시계방향으로 이동시켜준다.
	for(int i = 0; i < len; i++)
	{
		for(int j = 0; j < len; j++)
		{
			temp[j][len - 1 - i] = board[y + i][x + j];
		}
	}
	
	for(int i = 0; i < len; i++)
	{
		for(int j = 0; j < len; j++)
		{
			board[y+i][x+j] = temp[i][j];
		}
	}
}

void rotate()
{
	int len = 1 << L; // L 이동시켜서 시계방향으로 이동
	
	for(int y = 0; y < N; y += len)
	{
		for(int x = 0; x < N; x += len)
		{
			rotate(y,x,len);
		}
	}
}
```

> 얼음이 있는칸 3개 또는 그 이상과 인접해있지 않은 칸은 얼음의 양이 1 줄어든다.

```c++
void meltIce()
{
	memset(target,false,sizeof(target));
	
	for(int y = 0; y < N; y++)
	{
		for(int x = 0; x < N; x++)
		{
			int cnt = 0;
			
			for(int i = 0; i < 4; i++)
			{
				int nx = x + dx[i];
				int ny = y + dy[i];
				
				if(nx < 0 || ny < 0 || nx >= N || ny >= N)
					continue;
				if(board[ny][nx] <= 0)
					continue;
				
				cnt++;
			}
			if(cnt < 3)
				target[y][x] = 1;
		}
	}
	
	for(int y = 0; y < N; y++)
	{
		for(int x = 0; x < N; x++)
		{
			if(target[x][y])
				board[x][y]--;
		}
	}
}
```

> 남아있는 얼음의 합

```c++
int sumIce()
{
	int res = 0;
	
	for(int i = 0; i < N; i++)
	{
		for(int j = 0; j < N; j++)
		{
			if(board[i][j] <= 0)
				continue;
			
			res += board[i][j];
		}
	}
	
	return res;
}
```

> 남아있는 얼음 중 가장 큰 덩어리가 차지하는 칸의 개수

```c++
int BFS(int y, int x)
{
	int res = 1;
	q.push({y,x});
	visited[y][x] = 1;
	
	while(!q.empty())
	{
		int x = q.front().second;
		int y = q.front().first;
		
		q.pop();
		
		for(int i = 0; i < 4; i++)
		{
			int nx = x + dx[i];
			int ny = y + dy[i];
			
			if(nx < 0 || ny < 0 || nx >= N || ny >= N)
				continue;
			if(visited[ny][nx] || board[ny][nx] <= 0)
				continue;
			
			res++;
			visited[ny][nx] = 1;
			q.push({ny,nx});
		}
	}
	return res;
}

int findLargePiece()
{
	int largePiece = 0;
	
	for(int i = 0; i < N; i++)
	{
		for(int j = 0; j < N; j++)
		{
			if(visited[i][j] || board[i][j] <= 0)
				continue;
			largePiece = max(largePiece, BFS(i,j));
		}
	}
	return largePiece;
}

```

## 전체 코드


```c++
#include<iostream>
#include<queue>
#include<cstring>
#include<cmath>

#define MAX 75
using namespace std;

const int dx[4] = {0,0,-1,1};
const int dy[4] = {-1,1,0,0};

int N,Q,L;
int board[MAX][MAX];
bool visited[MAX][MAX];
bool target[MAX][MAX];
int temp[MAX][MAX];

queue<pair<int,int> > q;

void rotate(int x, int y, int len)
{
	for(int i = 0; i < len; i++)
	{
		for(int j = 0; j < len; j++)
		{
			temp[j][len - 1 - i] = board[y + i][x + j];
		}
	}
	
	for(int i = 0; i < len; i++)
	{
		for(int j = 0; j < len; j++)
		{
			board[y+i][x+j] = temp[i][j];
		}
	}
}

void rotate()
{
	int len = 1 << L;
	
	for(int y = 0; y < N; y += len)
	{
		for(int x = 0; x < N; x += len)
		{
			rotate(y,x,len);
		}
	}
}

void meltIce()
{
	memset(target,false,sizeof(target));
	
	for(int y = 0; y < N; y++)
	{
		for(int x = 0; x < N; x++)
		{
			int cnt = 0;
			
			for(int i = 0; i < 4; i++)
			{
				int nx = x + dx[i];
				int ny = y + dy[i];
				
				if(nx < 0 || ny < 0 || nx >= N || ny >= N)
					continue;
				if(board[ny][nx] <= 0)
					continue;
				
				cnt++;
			}
			if(cnt < 3)
				target[y][x] = 1;
		}
	}
	
	for(int y = 0; y < N; y++)
	{
		for(int x = 0; x < N; x++)
		{
			if(target[x][y])
				board[x][y]--;
		}
	}
}

int sumIce()
{
	int res = 0;
	
	for(int i = 0; i < N; i++)
	{
		for(int j = 0; j < N; j++)
		{
			if(board[i][j] <= 0)
				continue;
			
			res += board[i][j];
		}
	}
	
	return res;
}

int BFS(int y, int x)
{
	int res = 1;
	q.push({y,x});
	visited[y][x] = 1;
	
	while(!q.empty())
	{
		int x = q.front().second;
		int y = q.front().first;
		
		q.pop();
		
		for(int i = 0; i < 4; i++)
		{
			int nx = x + dx[i];
			int ny = y + dy[i];
			
			if(nx < 0 || ny < 0 || nx >= N || ny >= N)
				continue;
			if(visited[ny][nx] || board[ny][nx] <= 0)
				continue;
			
			res++;
			visited[ny][nx] = 1;
			q.push({ny,nx});
		}
	}
	return res;
}

int findLargePiece()
{
	int largePiece = 0;
	
	for(int i = 0; i < N; i++)
	{
		for(int j = 0; j < N; j++)
		{
			if(visited[i][j] || board[i][j] <= 0)
				continue;
			largePiece = max(largePiece, BFS(i,j));
		}
	}
	return largePiece;
}


int main()
{
	cin >> N >> Q;
	
	N = 1 << N;
	
	for(int i = 0; i < N; i++)
	{
		for(int j = 0; j < N; j++)
		{
			cin >> board[i][j];
		}
	}
	
	for(int i = 0; i < Q; i++)
	{
		cin >> L;
		
		rotate();
		meltIce();
	}
	
	cout << sumIce() << "\n";
	cout << findLargePiece() << "\n";
}

```

