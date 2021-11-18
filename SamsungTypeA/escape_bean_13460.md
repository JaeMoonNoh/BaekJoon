# 백준 13460 구슬 탈출 2
[구슬 탈출 2](https://www.acmicpc.net/problem/13460)

### 문제
스타트링크에서 판매하는 어린이용 장난감 중에서 가장 인기가 많은 제품은 구슬 탈출이다. 구슬 탈출은 직사각형 보드에 빨간 구슬과 파란 구슬을 하나씩 넣은 다음, 빨간 구슬을 구멍을 통해 빼내는 게임이다.

보드의 세로 크기는 N, 가로 크기는 M이고, 편의상 1×1크기의 칸으로 나누어져 있다. 가장 바깥 행과 열은 모두 막혀져 있고, 보드에는 구멍이 하나 있다. 빨간 구슬과 파란 구슬의 크기는 보드에서 1×1크기의 칸을 가득 채우는 사이즈이고, 각각 하나씩 들어가 있다. 게임의 목표는 빨간 구슬을 구멍을 통해서 빼내는 것이다. 이때, 파란 구슬이 구멍에 들어가면 안 된다.

이때, 구슬을 손으로 건드릴 수는 없고, 중력을 이용해서 이리 저리 굴려야 한다. 왼쪽으로 기울이기, 오른쪽으로 기울이기, 위쪽으로 기울이기, 아래쪽으로 기울이기와 같은 네 가지 동작이 가능하다.

각각의 동작에서 공은 동시에 움직인다. 빨간 구슬이 구멍에 빠지면 성공이지만, 파란 구슬이 구멍에 빠지면 실패이다. 빨간 구슬과 파란 구슬이 동시에 구멍에 빠져도 실패이다. 빨간 구슬과 파란 구슬은 동시에 같은 칸에 있을 수 없다. 또, 빨간 구슬과 파란 구슬의 크기는 한 칸을 모두 차지한다. 기울이는 동작을 그만하는 것은 더 이상 구슬이 움직이지 않을 때 까지이다.

보드의 상태가 주어졌을 때, 최소 몇 번 만에 빨간 구슬을 구멍을 통해 빼낼 수 있는지 구하는 프로그램을 작성하시오.

### 입력
첫 번째 줄에는 보드의 세로, 가로 크기를 의미하는 두 정수 N, M (3 ≤ N, M ≤ 10)이 주어진다. 다음 N개의 줄에 보드의 모양을 나타내는 길이 M의 문자열이 주어진다. 이 문자열은 '.', '#', 'O', 'R', 'B' 로 이루어져 있다. '.'은 빈 칸을 의미하고, '#'은 공이 이동할 수 없는 장애물 또는 벽을 의미하며, 'O'는 구멍의 위치를 의미한다. 'R'은 빨간 구슬의 위치, 'B'는 파란 구슬의 위치이다.

입력되는 모든 보드의 가장자리에는 모두 '#'이 있다. 구멍의 개수는 한 개 이며, 빨간 구슬과 파란 구슬은 항상 1개가 주어진다.

### 출력
최소 몇 번 만에 빨간 구슬을 구멍을 통해 빼낼 수 있는지 출력한다. 만약, 10번 이하로 움직여서 빨간 구슬을 구멍을 통해 빼낼 수 없으면 -1을 출력한다.

### 구현해야하는 내용들
 
> 게임의 목적   

빨간 구슬을 구멍을 통해 빼내는 게임이다.  
  
> 입력 범위

보드의 세로 크기는 N, 가로 크기는 M이다.
```c++
  vector<string> board;

	for(int i = 0; i < n; i++)
	{
		string str;
		cin >> str;
		
		board.push_back(str);
	}
```

> 구슬을 손으로 건드릴 수는 없고, 중력을 이용해서 이리 저리 굴려야 한다.

중력이라는 말은 한 번 게임기를 기울이면 벽이 나올때 까지 끝까지 밀린다는 말이다.   
그렇기에 '#'을 안만나거나 'O'이 아니라면 계속해서 이동한다.
```c++

void bean_move(int &x, int &y,int &c, int dx, int dy)
{
	while(board[y + dy][x + dx] != '#' && board[y][x] != 'O')
	{
		x += dx;
		y += dy;
		c += 1;
	}
}
```

> 4가지 동작이 가능하다(상,하,좌,우)

이동 방향은 4가지 코드 상으로 down, up, right, left이다.
```c++
const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0}; // down, up, right, left
```
> 공은 동시에 움직인다. 빨간공, 파란공 동시에 움직인다.

편하게 표현하기 위해서 구조체에 전부 집어넣는다.
```c++
typedef struct{
	int rx;
	int ry;
	int bx;
	int by;
	int cnt; 
}Bean;
```

> 파란 구슬이 구멍에 빠지면 실패이다. -> 실패 조건

nby,nbx는 next blue y, x를 줄인 말이다. 중력을 이용한 움직임 이후에 파란공의 위치가 구멍이라면 해당 행동은 그만두고, 다른 방향으로 이동시켜야 한다.

```c++
if(board[nby][nbx] == 'O')
  continue;
```

> 빨간 구슬, 파란 구슬이 동시에 구멍에 빠져도 실패이다. -> 실패 조건

위와 같은 조건이므로 continue에 걸리게 된다.

> 빨간 구슬과 파란 구슬은 동시에 같은 칸에 있을 수 없다. -> 예외 상황

위 bean_move 함수에서 매번 '#'을 만나거나 'O'을 만나게 되면 멈추는 조건아래 c += 1이 있는데
움직이는 횟수가 몇번인지 체크하기 위한 값이다. 이유는 만약 벽에 부딪혀 멈추게 되었는데 빨간공이 파란 공보다 이동 횟수가 적다면 파란공이 빨간공보다 늦게 도착한 것이고, 반대의 상황이라면 빨간공이 파란공보다 늦게 도착한 것이다. 

```c++
if(nrx == nbx && nry == nby)
{
  if(rc > bc)
  {
    nrx -= dx[i];
		nry -= dy[i];
	}
	else
	{
		nbx -= dx[i];
		nby -= dy[i];
	}
}
```

> 기울이는 동작을 그만하는 것은 더 이상 구슬이 움직이지 않을 때 까지이다. -> 예외 상황

4방향으로 움직였을 경우 공이 그대로라면 -1을 출력한다. 하지만 이번 문제에서는 구현하지는 않았다.

> 최소 몇 번 만에 빨간 구슬을 구멍을 통해 빼낼 수 있는지 구하는 프로그램을 작성하시오. -> 최소 몇 번이 가능한지 결과 값

해당 구조체에 cnt값이 몇번 이동한지 체크해주는 값이다.

```c++
typedef struct{
	int rx;
	int ry;
	int bx;
	int by;
	int cnt; 
}Bean;
```


### 전체 코드
```c++
#include<iostream>
#include<algorithm>
#include<queue>
#include<vector>
#include<string>

using namespace std;

typedef struct{
	int rx;
	int ry;
	int bx;
	int by;
	int cnt; 
}Bean;

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0}; // down, up, right, left

int n,m; // n : row, m : column
vector<string> board;
queue<Bean> q;

void bean_move(int &x, int &y,int &c, int dx, int dy)
{
	while(board[y + dy][x + dx] != '#' && board[y][x] != 'O')
	{
		x += dx;
		y += dy;
		c += 1;
	}
}

int main()
{
	cin >> n >> m;
	
	for(int i = 0; i < n; i++)
	{
		string str;
		cin >> str;
		
		board.push_back(str);
	}
	
	Bean B;
	//red, blue 합친 애들 
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < m; j++)
		{
			if(board[i][j] == 'R')
			{
				B.rx = j;
				B.ry = i;		
			}
			else if(board[i][j] == 'B')
			{
				B.bx = j;
				B.by = i;
			}
		}
	}
	B.cnt = 0;
	
	q.push(B);
	
	while(!q.empty())
	{
		int rx = q.front().rx;
		int ry = q.front().ry;
		int bx = q.front().bx;
		int by = q.front().by;
		int cnt = q.front().cnt;		
		q.pop();
		
		if(cnt >= 10)
			break;
		
		for(int i = 0; i < 4; i++)
		{
			Bean b; 
			int nrx = rx;
			int nry = ry;
			int nbx = bx;
			int nby = by;
			int rc = 0;
			int bc = 0;
			int ncnt = cnt+1;
			
			bean_move(nrx,nry,rc,dx[i],dy[i]);
			bean_move(nbx,nby,bc,dx[i],dy[i]);
			
			if(board[nby][nbx] == 'O')
				continue;
			if(board[nry][nrx] == 'O')
			{
				cout << ncnt;
				return 0;
			}
			
			if(nrx == nbx && nry == nby)
			{
				if(rc > bc)
				{
					nrx -= dx[i];
					nry -= dy[i];
				}
				else
				{
					nbx -= dx[i];
					nby -= dy[i];
				}
			}
			
			b.rx = nrx;
			b.ry = nry;
			b.bx = nbx;
			b.by = nby;
			b.cnt = ncnt;
			
			q.push(b);
		}
	}
	cout << -1 << "\n";
	
	return 0;
}
```
