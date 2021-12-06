# 백준 청소년 상어 19236
[청소년 상어](https://www.acmicpc.net/problem/19236)

### 문제
아기 상어가 성장해 청소년 상어가 되었다.

4×4크기의 공간이 있고, 크기가 1×1인 정사각형 칸으로 나누어져 있다. 공간의 각 칸은 (x, y)와 같이 표현하며, x는 행의 번호, y는 열의 번호이다. 한 칸에는 물고기가 한 마리 존재한다. 각 물고기는 번호와 방향을 가지고 있다. 번호는 1보다 크거나 같고, 16보다 작거나 같은 자연수이며, 두 물고기가 같은 번호를 갖는 경우는 없다. 방향은 8가지 방향(상하좌우, 대각선) 중 하나이다.

오늘은 청소년 상어가 이 공간에 들어가 물고기를 먹으려고 한다. 청소년 상어는 (0, 0)에 있는 물고기를 먹고, (0, 0)에 들어가게 된다. 상어의 방향은 (0, 0)에 있던 물고기의 방향과 같다. 이후 물고기가 이동한다.

물고기는 번호가 작은 물고기부터 순서대로 이동한다. 물고기는 한 칸을 이동할 수 있고, 이동할 수 있는 칸은 빈 칸과 다른 물고기가 있는 칸, 이동할 수 없는 칸은 상어가 있거나, 공간의 경계를 넘는 칸이다. 각 물고기는 방향이 이동할 수 있는 칸을 향할 때까지 방향을 45도 반시계 회전시킨다. 만약, 이동할 수 있는 칸이 없으면 이동을 하지 않는다. 그 외의 경우에는 그 칸으로 이동을 한다. 물고기가 다른 물고기가 있는 칸으로 이동할 때는 서로의 위치를 바꾸는 방식으로 이동한다.

물고기의 이동이 모두 끝나면 상어가 이동한다. 상어는 방향에 있는 칸으로 이동할 수 있는데, 한 번에 여러 개의 칸을 이동할 수 있다. 상어가 물고기가 있는 칸으로 이동했다면, 그 칸에 있는 물고기를 먹고, 그 물고기의 방향을 가지게 된다. 이동하는 중에 지나가는 칸에 있는 물고기는 먹지 않는다. 물고기가 없는 칸으로는 이동할 수 없다. 상어가 이동할 수 있는 칸이 없으면 공간에서 벗어나 집으로 간다. 상어가 이동한 후에는 다시 물고기가 이동하며, 이후 이 과정이 계속해서 반복된다.

[그림생략]

<그림 1>

<그림 1>은 청소년 상어가 공간에 들어가기 전 초기 상태이다. 상어가 공간에 들어가면 (0, 0)에 있는 7번 물고기를 먹고, 상어의 방향은 ↘이 된다. <그림 2>는 상어가 들어간 직후의 상태를 나타낸다.


<그림 2>

이제 물고기가 이동해야 한다. 1번 물고기의 방향은 ↗이다. ↗ 방향에는 칸이 있고, 15번 물고기가 들어있다. 물고기가 있는 칸으로 이동할 때는 그 칸에 있는 물고기와 위치를 서로 바꿔야 한다. 따라서, 1번 물고기가 이동을 마치면 <그림 3>과 같아진다.


<그림 3>

2번 물고기의 방향은 ←인데, 그 방향에는 상어가 있으니 이동할 수 없다. 방향을 45도 반시계 회전을 하면 ↙가 되고, 이 칸에는 3번 물고기가 있다. 물고기가 있는 칸이니 서로 위치를 바꾸고, <그림 4>와 같아지게 된다.


<그림 4>

3번 물고기의 방향은 ↑이고, 존재하지 않는 칸이다. 45도 반시계 회전을 한 방향 ↖도 존재하지 않으니, 다시 회전을 한다. ← 방향에는 상어가 있으니 또 회전을 해야 한다. ↙ 방향에는 2번 물고기가 있으니 서로의 위치를 교환하면 된다. 이런 식으로 모든 물고기가 이동하면 <그림 5>와 같아진다.


<그림 5>

물고기가 모두 이동했으니 이제 상어가 이동할 순서이다. 상어의 방향은 ↘이고, 이동할 수 있는 칸은 12번 물고기가 있는 칸, 15번 물고기가 있는 칸, 8번 물고기가 있는 칸 중에 하나이다. 만약, 8번 물고기가 있는 칸으로 이동하면, <그림 6>과 같아지게 된다.


<그림 6>

상어가 먹을 수 있는 물고기 번호의 합의 최댓값을 구해보자.

### 입력

첫째 줄부터 4개의 줄에 각 칸의 들어있는 물고기의 정보가 1번 행부터 순서대로 주어진다. 물고기의 정보는 두 정수 ai, bi로 이루어져 있고, ai는 물고기의 번호, bi는 방향을 의미한다. 방향 bi는 8보다 작거나 같은 자연수를 의미하고, 1부터 순서대로 ↑, ↖, ←, ↙, ↓, ↘, →, ↗ 를 의미한다.

### 출력

상어가 먹을 수 있는 물고기 번호의 합의 최댓값을 출력한다.

### 자료구조 & 알고리즘

보통 최댓값을 구하는 문제는 DFS이고 글을 읽어보면 상어가 이동할수 있는 것은 한칸 두칸 또는 세칸이다.
이동하는 것에 따른 값을 구하는 것이기 때문에 DFS로 따져보면 된다. 그리고 처음에는 sort를 생각했으나 배열에 저장하여 순서대로 이동만 시켜주면 된다.


### 구현해야할 조건

> 상어와 물고기의 구조체

```c++
typedef struct{
	int y;
	int x;
	int fish_num;
	int dir;
	bool Live;
}Fish;

typedef struct{
	int y;
	int x;
	int dir;
	int sum = 0;
}Shark;
```

> 방향이 이동할 수 있는 칸을 향할 때까지 방향을 45도 반시계 회전시킨다.

```c++

const int dy[9] = {-1,-1,0,1,1,1,0,-1};
const int dx[9] = {0,-1,-1,-1,0,1,1,1}; // 

		if(!chk) // 방향을 못잡았을때 
		{
			int ccw_dir = dir + 1;
			ccw_dir = ccw_dir % 8;
			
			int nx = x + dx[ccw_dir];
			int ny = y + dy[ccw_dir];
			
			while(ccw_dir != dir)
			{
				if(0 <= nx && nx < 4 && 0 <= ny && ny < 4)
				{
					if(board[ny][nx] == 0)
					{
						fishes[i].x = nx;
						fishes[i].y = ny;
						board[ny][nx] = i;
						board[y][x] = 0;
						fishes[i].dir = ccw_dir; // 반시계 방향 이동 가능 
						break;
					}
					else if(board[ny][nx] != -1)
					{
						fishes[i].x = nx;
						fishes[i].y = ny;
						fishes[board[ny][nx]].x = x;
						fishes[board[ny][nx]].y = y; // fishes 바꾸기, 위치만 바꾸면 된다 
						
						int temp = board[y][x];
						board[y][x] = board[ny][nx];
						board[ny][nx] = temp; // board 바꾸기 
						fishes[i].dir = ccw_dir;
						break;
					}
				}
				ccw_dir++;
				ccw_dir = ccw_dir % 8; // 계속해서 반시계방향으로 돌림 
				
				nx = x + dx[ccw_dir];
				ny = y + dy[ccw_dir];
			}
		}
```



> 물고기가 다른 물고기가 있는 칸으로 이동할 때는 서로의 위치를 바꾸는 방식으로 이동한다.

```c++
	for(int i = 1; i <= 16; i++) // 물고기 순서~
	{
		if(!fishes[i].Live)
			continue;
			
		int y = fishes[i].y;
		int x = fishes[i].x;
		int dir = fishes[i].dir;
		
		int nx = x + dx[dir];
		int ny = y + dy[dir];
		bool chk = false;
		
		if(0 <= nx && nx < 4 && 0 <= ny && ny < 4)
		{
			if(board[ny][nx] == 0) // 비어있는 곳 
			{
				chk = true;
				fishes[i].y = ny;
				fishes[i].x = nx;
				board[ny][nx] = i;
				board[y][x] = 0;
			}
			else if(board[ny][nx] != -1) // 상어가 있는 곳이 아니라면, 물고기가 있는 곳 
			{
				chk = true;
				
				fishes[i].x = nx;
				fishes[i].y = ny;
				fishes[board[ny][nx]].x = x;
				fishes[board[ny][nx]].y = y;
				
				int temp = board[y][x];
				board[y][x] = board[ny][nx];
				board[ny][nx] = temp; // board와 fishes 바꿔주기, 위치만 바꿔주면 된다.
			}
		}
```

> 상어는 방향에 있는 칸으로 이동할 수 있는데, 물고기 칸으로 이동하면 물고기를 먹고, 방향을 얻는다.

```c++
	for(int i = 1; i <= 3; i++) // 상어는 방향 어디든 갈 수 있다. DFS를 시작 
	{
		int nx = x + dx[dir] * i;
		int ny = y + dy[dir] * i;
		
		if(0 <= nx && nx < 4 && 0 <= ny && ny < 4)
		{
			if(board[ny][nx] == 0)
				continue;
			
			int fish_score = board[ny][nx];
			int sharkDir = fishes[board[ny][nx]].dir;
			
			board[y][x] = 0; // board 비워주고 
			board[ny][nx] = -1; // 상어위치 만들어 주고 
			fishes[fish_score].Live = false;
			
			DFS(ny,nx,sharkDir, sum + fish_score);
			
			board[y][x] = -1; // 상어의 위치 돌려주고 
			board[ny][nx] = fish_score; // board 값 돌려주고 
			fishes[fish_score].Live = true;
		}
	}
```


### 전체코드


```c++
#include<iostream>
#include<vector>
#include<queue>
#include<algorithm>

#define MAX 17
using namespace std;

typedef struct{
	int y;
	int x;
	int fish_num;
	int dir;
	bool Live;
}Fish;

typedef struct{
	int y;
	int x;
	int dir;
	int sum = 0;
}Shark;

const int dy[9] = {-1,-1,0,1,1,1,0,-1};
const int dx[9] = {0,-1,-1,-1,0,1,1,1};

int answer = 0;
int board[MAX][MAX];
vector<Fish> fish[MAX];
Fish fishes[MAX]; // vector와 array의 차이 
queue<Shark> shark;

void DFS(int y, int x, int dir, int sum)
{
	answer = max(answer, sum);
	
	int copyBoard[4][4];
	Fish copyFishes[MAX];
	
	for(int i = 0; i < 4; i++)
		for(int j = 0; j < 4; j++)
			copyBoard[i][j] = board[i][j];
	
	for(int i = 1; i <= 16; i++)
		copyFishes[i] = fishes[i]; // 현재 값을 저장해놓고 
		
	for(int i = 1; i <= 16; i++)
	{
		if(!fishes[i].Live)
			continue;
			
		int y = fishes[i].y;
		int x = fishes[i].x;
		int dir = fishes[i].dir;
		
		int nx = x + dx[dir];
		int ny = y + dy[dir];
		bool chk = false;
		
		if(0 <= nx && nx < 4 && 0 <= ny && ny < 4)
		{
			if(board[ny][nx] == 0) // 비어있는 곳 
			{
				chk = true;
				fishes[i].y = ny;
				fishes[i].x = nx;
				board[ny][nx] = i;
				board[y][x] = 0;
			}
			else if(board[ny][nx] != -1) // 상어가 있는 곳이 아니라면, 물고기가 있는 곳 
			{
				chk = true;
				
				fishes[i].x = nx;
				fishes[i].y = ny;
				fishes[board[ny][nx]].x = x;
				fishes[board[ny][nx]].y = y;
				
				int temp = board[y][x];
				board[y][x] = board[ny][nx];
				board[ny][nx] = temp;
			}
		}
		
		if(!chk) // 방향을 못잡았을때 
		{
			int ccw_dir = dir + 1;
			ccw_dir = ccw_dir % 8;
			
			int nx = x + dx[ccw_dir];
			int ny = y + dy[ccw_dir];
			
			while(ccw_dir != dir)
			{
				if(0 <= nx && nx < 4 && 0 <= ny && ny < 4)
				{
					if(board[ny][nx] == 0)
					{
						fishes[i].x = nx;
						fishes[i].y = ny;
						board[ny][nx] = i;
						board[y][x] = 0;
						fishes[i].dir = ccw_dir; // 반시계 방향 이동 가능 
						break;
					}
					else if(board[ny][nx] != -1)
					{
						fishes[i].x = nx;
						fishes[i].y = ny;
						fishes[board[ny][nx]].x = x;
						fishes[board[ny][nx]].y = y; // fishes 바꾸기, 위치만 바꾸면 된다 
						
						int temp = board[y][x];
						board[y][x] = board[ny][nx];
						board[ny][nx] = temp; // board 바꾸기 
						fishes[i].dir = ccw_dir;
						break;
					}
				}
				ccw_dir++;
				ccw_dir = ccw_dir % 8; // 계속해서 반시계방향으로 돌림 
				
				nx = x + dx[ccw_dir];
				ny = y + dy[ccw_dir];
			}
		}
		
	}
	
	for(int i = 1; i <= 3; i++) // 상어는 방향 어디든 갈 수 있다. DFS를 시작 
	{
		int nx = x + dx[dir] * i;
		int ny = y + dy[dir] * i;
		
		if(0 <= nx && nx < 4 && 0 <= ny && ny < 4)
		{
			if(board[ny][nx] == 0)
				continue;
			
			int fish_score = board[ny][nx];
			int sharkDir = fishes[board[ny][nx]].dir;
			
			board[y][x] = 0; // board 비워주고 
			board[ny][nx] = -1; // 상어위치 만들어 주고 
			fishes[fish_score].Live = false;
			
			DFS(ny,nx,sharkDir, sum + fish_score);
			
			board[y][x] = -1; // 상어의 위치 돌려주고 
			board[ny][nx] = fish_score; // board 값 돌려주고 
			fishes[fish_score].Live = true;
		}
	}
	
	for(int i = 0; i < 4; i++)
		for(int j = 0; j < 4; j++)
			board[i][j] = copyBoard[i][j];
	
	for(int i = 1; i <= 16; i++)
		fishes[i] = copyFishes[i]; // 상어가 이동한게 없으면 변하기 전을 돌려놔야 이전 DFS를 돌릴수 있다. 
}


int main()
{
	for(int i = 0; i < 4; i++)
	{
		for(int j = 0; j < 4; j++)
		{
			int fish_num, dir;
			cin >> fish_num >> dir;
			
			board[i][j] = fish_num; 
			fishes[fish_num] = {i,j,fish_num,dir-1,true};
		}
	}
	
	int shark_sum = board[0][0];
	int shark_dir = fishes[board[0][0]].dir;
	fishes[board[0][0]].Live = false;
	board[0][0] = -1;
	
	DFS(0,0,shark_dir,shark_sum);
	
	cout << answer;
	
	return 0;
}
```


