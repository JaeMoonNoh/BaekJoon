# 백준 테트로미노 14500
[테트로미노](https://www.acmicpc.net/problem/14500)

### 문제
폴리오미노란 크기가 1×1인 정사각형을 여러 개 이어서 붙인 도형이며, 다음과 같은 조건을 만족해야 한다.

정사각형은 서로 겹치면 안 된다.
도형은 모두 연결되어 있어야 한다.
정사각형의 변끼리 연결되어 있어야 한다. 즉, 꼭짓점과 꼭짓점만 맞닿아 있으면 안 된다.
정사각형 4개를 이어 붙인 폴리오미노는 테트로미노라고 하며, 다음과 같은 5가지가 있다.

아름이는 크기가 N×M인 종이 위에 테트로미노 하나를 놓으려고 한다. 종이는 1×1 크기의 칸으로 나누어져 있으며, 각각의 칸에는 정수가 하나 쓰여 있다.

테트로미노 하나를 적절히 놓아서 테트로미노가 놓인 칸에 쓰여 있는 수들의 합을 최대로 하는 프로그램을 작성하시오.

테트로미노는 반드시 한 정사각형이 정확히 하나의 칸을 포함하도록 놓아야 하며, 회전이나 대칭을 시켜도 된다.

### 입력
첫째 줄에 종이의 세로 크기 N과 가로 크기 M이 주어진다. (4 ≤ N, M ≤ 500)

둘째 줄부터 N개의 줄에 종이에 쓰여 있는 수가 주어진다. i번째 줄의 j번째 수는 위에서부터 i번째 칸, 왼쪽에서부터 j번째 칸에 쓰여 있는 수이다. 입력으로 주어지는 수는 1,000을 넘지 않는 자연수이다.

### 출력

첫째 줄에 테트로미노가 놓인 칸에 쓰인 수들의 합의 최댓값을 출력한다.

### 알고리즘

DFS, BackTracking 사각형 4가지가 우우우, 좌좌좌, 좌상상 등 변끼리 연결되어 있으므로 구현 가능, 그리고 ~fuxk~ 모양은 따로 구현

### 구현해야할 조건

> 도형은 모두 연결되어 있어야 한다.

```c++

//DFS, BackTracking을 생각하게 되는 내용

```

> 정사각형의 변끼리 연결되어 있어야 한다. 즉, 꼭짓점과 꼭짓점만 맞닿아 있으면 안된다.

```c++

//DFS, BackTracking을 생각하게 되는 내용

```

> 테트로미노 하나를 적절히 놓아서 테트로미노가 놓인 칸에 쓰여 있는 수들의 합을 최대로 하자

```c++

int DFS(int cnt, int y, int x)
{
	if(cnt == 4)
		return board[y][x];
		
	int sum = 0;
	
	for(int i = 0; i < 4; i++)
	{
		int nx = x + dx[i];
		int ny = y + dy[i];
		
		if(0 <= nx && nx < m && 0 <= ny && ny < n && !visited[ny][nx])
		{
			visited[ny][nx] = true;
			sum = max(sum, board[y][x] + DFS(cnt+1,ny,nx));
			visited[ny][nx] = false;
		}
	}
	return sum;
}

```

> 가운데 손가락 모양 구현

가운데가 있기 때문에 첫번째 마지막 행은 위 혹은 아래에 가운데가 올라와 있고, 첫번째 마지막 열은 좌측 혹은 우측에만 가운데가 존재한다.

```c++

int middleFinger(int y, int x)
{
	int result = 0;
	
	if(y >= 1 && x >= 1 && x < m - 1)
		result = max(result, board[y][x-1] + board[y][x] + board[y-1][x] + board[y][x+1]);
	if(y >= 1 && y < n - 1 && x < m - 1)
		result = max(result, board[y-1][x] + board[y][x] + board[y][x+1] + board[y+1][x]);	
	if(y >= 0 && y < n - 1 && x < m - 1)
		result = max(result, board[y][x-1] + board[y][x] + board[y+1][x] + board[y][x+1]);
	if(y >= 1 && y < n - 1 && x >= 1)
		result = max(result, board[y-1][x] + board[y][x] + board[y][x-1] + board[y+1][x]);
		
	return result;
}

```

### 전체 코드


```c++

#include<iostream>
#include<algorithm>

#define MAX 501

using namespace std;

int n,m;
int board[MAX][MAX];
bool visited[MAX][MAX];

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0};

int DFS(int cnt, int y, int x)
{
	if(cnt == 4)
		return board[y][x];
		
	int sum = 0;
	
	for(int i = 0; i < 4; i++)
	{
		int nx = x + dx[i];
		int ny = y + dy[i];
		
		if(0 <= nx && nx < m && 0 <= ny && ny < n && !visited[ny][nx])
		{
			visited[ny][nx] = true;
			sum = max(sum, board[y][x] + DFS(cnt+1,ny,nx));
			visited[ny][nx] = false;
		}
	}
	return sum;
}

int middleFinger(int y, int x)
{
	int result = 0;
	
	if(y >= 1 && x >= 1 && x < m - 1)
		result = max(result, board[y][x-1] + board[y][x] + board[y-1][x] + board[y][x+1]);
	if(y >= 1 && y < n - 1 && x < m - 1)
		result = max(result, board[y-1][x] + board[y][x] + board[y][x+1] + board[y+1][x]);	
	if(y >= 0 && y < n - 1 && x < m - 1)
		result = max(result, board[y][x-1] + board[y][x] + board[y+1][x] + board[y][x+1]);
	if(y >= 1 && y < n - 1 && x >= 1)
		result = max(result, board[y-1][x] + board[y][x] + board[y][x-1] + board[y+1][x]);
		
	return result;
}

int main()
{
	cin >> n >> m;
	
	for(int i = 0; i < n; i++)
		for(int j = 0; j < m; j++)
			cin >> board[i][j];
			
	int result = 0;
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < m; j++)
		{
			visited[i][j] = true;
			result = max(result, DFS(1,i,j));
			result = max(result, middleFinger(i,j));
			visited[i][j] = false;
		}
	}
	
	cout << result << "\n";
	
	return 0;
}

```
