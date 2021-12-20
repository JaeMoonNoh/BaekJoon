# 백준 주사위굴리기 2 23288

[주사위 굴리기 2](https://www.acmicpc.net/problem/23288)

## 문제

크기가 N×M인 지도가 존재한다. 지도의 오른쪽은 동쪽, 위쪽은 북쪽이다. 지도의 좌표는 (r, c)로 나타내며, r는 북쪽으로부터 떨어진 칸의 개수, c는 서쪽으로부터 떨어진 칸의 개수이다. 가장 왼쪽 위에 있는 칸의 좌표는 (1, 1)이고, 가장 오른쪽 아래에 있는 칸의 좌표는 (N, M)이다. 이 지도의 위에 주사위가 하나 놓여져 있으며, 주사위의 각 면에는 1보다 크거나 같고, 6보다 작거나 같은 정수가 하나씩 있다. 주사위 한 면의 크기와 지도 한 칸의 크기는 같고, 주사위의 전개도는 아래와 같다.

```c++
  2
4 1 3
  5
  6
```
주사위는 지도 위에 윗 면이 1이고, 동쪽을 바라보는 방향이 3인 상태로 놓여져 있으며, 놓여져 있는 곳의 좌표는 (1, 1) 이다. 지도의 각 칸에도 정수가 하나씩 있다. 가장 처음에 주사위의 이동 방향은 동쪽이다. 주사위의 이동 한 번은 다음과 같은 방식으로 이루어진다.

  1. 주사위가 이동 방향으로 한 칸 굴러간다. 만약, 이동 방향에 칸이 없다면, 이동 방향을 반대로 한 다음 한 칸 굴러간다.
  2. 주사위가 도착한 칸 (x, y)에 대한 점수를 획득한다.
  3. 주사위의 아랫면에 있는 정수 A와 주사위가 있는 칸 (x, y)에 있는 정수 B를 비교해 이동 방향을 결정한다.
    * A > B인 경우 이동 방향을 90도 시계 방향으로 회전시킨다.
    * A < B인 경우 이동 방향을 90도 반시계 방향으로 회전시킨다.
    * A = B인 경우 이동 방향에 변화는 없다.
칸 (x, y)에 대한 점수는 다음과 같이 구할 수 있다. (x, y)에 있는 정수를 B라고 했을때, (x, y)에서 동서남북 방향으로 연속해서 이동할 수 있는 칸의 수 C를 모두 구한다. 이때 이동할 수 있는 칸에는 모두 정수 B가 있어야 한다. 여기서 점수는 B와 C를 곱한 값이다.

보드의 크기와 각 칸에 있는 정수, 주사위의 이동 횟수 K가 주어졌을때, 각 이동에서 획득하는 점수의 합을 구해보자.

이 문제의 예제 1부터 7은 같은 지도에서 이동하는 횟수만 증가시키는 방식으로 구성되어 있다. 예제 8은 같은 지도에서 이동하는 횟수를 매우 크게 만들었다.

## 입력

첫째 줄에 지도의 세로 크기 N, 가로 크기 M (2 ≤ N, M ≤ 20), 그리고 이동하는 횟수 K (1 ≤ K ≤ 1,000)가 주어진다. 

둘째 줄부터 N개의 줄에 지도에 쓰여 있는 수가 북쪽부터 남쪽으로, 각 줄은 서쪽부터 동쪽 순서대로 주어진다. 지도의 각 칸에 쓰여 있는 수는 10 미만의 자연수이다.

## 출력

첫째 줄에 각 이동에서 획득하는 점수의 합을 출력한다.

## 전체 코드


```c++
#include<iostream>
#include<cstring>
#include<queue>

#define endl "\n"
#define MAX 25
using namespace std;

const int dx[4] = {1,-1,0,0};
const int dy[4] = {0,0,1,-1};

int n,m,k,ans;
int board[MAX][MAX];
int dice[7] = {0,1,2,3,4,5,6};
bool visited[MAX][MAX];

int reverse(int d)
{
	if(d == 0)
		return 1;
	else if(d == 1)
		return 0;
	else if(d == 2)
		return 3;
	else
		return 2; // right, left, down, up
}

int changeDir(int dir, int res)
{
	if(res > 0)
	{
		if(dir == 0)
			return 2;
		else if(dir == 1)
			return 3;
		else if(dir == 2)
			return 1;
		else 
			return 0; // 시계 방향으로 돌리기 
	}
	else if(res < 0)
	{
		if(dir == 0)
			return 3;
		else if(dir == 1)
			return 2;
		else if(dir == 2)
			return 0;
		else 
			return 1; // 반시계 방향으로 돌리기 
	}
	else
		return dir; // 차이가 없으면 그대로 냅두기 
}

void getScore(int y, int x)
{
	memset(visited,false,sizeof(visited));
	
	int num = board[y][x];
	int cnt = 1; // 시작은 그자리에 있으니까. 
	
	queue<pair<int,int> > q;
	q.push({y,x});
	
	visited[y][x] = true;
	
	while(!q.empty())
	{
		int y = q.front().first;
		int x = q.front().second;
		q.pop();
		
		for(int i = 0; i < 4; i++)
		{
			int nx = x + dx[i];
			int ny = y + dy[i];
			
			if(nx >= 0 && nx < m && ny >= 0 && ny < n)
			{
				if(board[ny][nx] == num && !visited[ny][nx])
				{
					visited[ny][nx] = true;
					cnt++;
					q.push({ny,nx});
				}
			}
		}
	}
	ans += (num*cnt);
}

void diceState(int d)
{
	int d1 = dice[1];
	int d2 = dice[2];
	int d3 = dice[3];
	int d4 = dice[4];
	int d5 = dice[5];
	int d6 = dice[6];
	
	if(d == 0) // right 
	{
		dice[1] = d4;
		dice[4] = d6;
		dice[6] = d3;
		dice[3] = d1;
	}
	else if(d == 1) // left
	{
		dice[1] = d3;
		dice[3] = d6;
		dice[6] = d4;
		dice[4] = d1;
	}
	else if(d == 2) // down
	{
		dice[1] = d2;
		dice[2] = d6;
		dice[6] = d5;
		dice[5] = d1;
	}
	else // up
	{
		dice[1] = d5;
		dice[5] = d6;
		dice[6] = d2;
		dice[2] = d1;
	}
}

int main()
{
	cin >> n >> m >> k;
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < m; j++)
		{
			cin >> board[i][j];
		}
	}
	
	int x = 0;
	int y = 0;
	int dir = 0; // 오른쪽으로 이동 
	
	for(int i = 0; i < k; i++)
	{
		int nx = x + dx[dir];
		int ny = y + dy[dir];
		
		if(0 > nx || 0 > ny || nx >= m || ny >= n)
		{
			dir = reverse(dir);
			nx = x + dx[dir];
			ny = y + dy[dir];
		}
		getScore(ny, nx);
		diceState(dir);
		dir = changeDir(dir, dice[6] - board[ny][nx]);
		x = nx;
		y = ny;
	}
	
	cout << ans;
	
	return 0;
}
``
