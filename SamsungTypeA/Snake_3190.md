# 백준 뱀 3190

[뱀](https://www.acmicpc.net/problem/3190)

### 문제
'Dummy' 라는 도스게임이 있다. 이 게임에는 뱀이 나와서 기어다니는데, 사과를 먹으면 뱀 길이가 늘어난다. 뱀이 이리저리 기어다니다가 벽 또는 자기자신의 몸과 부딪히면 게임이 끝난다.

게임은 NxN 정사각 보드위에서 진행되고, 몇몇 칸에는 사과가 놓여져 있다. 보드의 상하좌우 끝에 벽이 있다. 게임이 시작할때 뱀은 맨위 맨좌측에 위치하고 뱀의 길이는 1 이다. 뱀은 처음에 오른쪽을 향한다.

뱀은 매 초마다 이동을 하는데 다음과 같은 규칙을 따른다.

먼저 뱀은 몸길이를 늘려 머리를 다음칸에 위치시킨다.
만약 이동한 칸에 사과가 있다면, 그 칸에 있던 사과가 없어지고 꼬리는 움직이지 않는다.
만약 이동한 칸에 사과가 없다면, 몸길이를 줄여서 꼬리가 위치한 칸을 비워준다. 즉, 몸길이는 변하지 않는다.
사과의 위치와 뱀의 이동경로가 주어질 때 이 게임이 몇 초에 끝나는지 계산하라.
### 입력
첫째 줄에 보드의 크기 N이 주어진다. (2 ≤ N ≤ 100) 다음 줄에 사과의 개수 K가 주어진다. (0 ≤ K ≤ 100)

다음 K개의 줄에는 사과의 위치가 주어지는데, 첫 번째 정수는 행, 두 번째 정수는 열 위치를 의미한다. 사과의 위치는 모두 다르며, 맨 위 맨 좌측 (1행 1열) 에는 사과가 없다.

다음 줄에는 뱀의 방향 변환 횟수 L 이 주어진다. (1 ≤ L ≤ 100)

다음 L개의 줄에는 뱀의 방향 변환 정보가 주어지는데,  정수 X와 문자 C로 이루어져 있으며. 게임 시작 시간으로부터 X초가 끝난 뒤에 왼쪽(C가 'L') 또는 오른쪽(C가 'D')로 90도 방향을 회전시킨다는 뜻이다. X는 10,000 이하의 양의 정수이며, 방향 전환 정보는 X가 증가하는 순으로 주어진다.

### 출력

첫째 줄에 게임이 몇 초에 끝나는지 출력한다.

### 알고리즘

뱀의 이동을 구현해야하는데 사용되는 자료구조는 **deque**를 사용했습니다. 뱀은 이동하면서 사과를 만날때 꼬리는 그대로 두고 머리만 이동하게 되며, 사과가 아닌 빈 공간을 이동한다면
머리와 꼬리까지 한번에 이동을 해야하기 때문에 한칸씩 이동을 해야됩니다. 맨뒤에 위치하고 있는 값을 제거하고 맨 앞에 이동하는 공간을 추가하면 되기 때문에 양방향 큐인 **deque**를 사용했습니다.


### 구현해야할 조건

항상 문제를 잘 읽어야한다. 처음 문제를 잘 못 이해하게 되면 코딩 내용을 자꾸 바꿔야 하기 때문에 입력값을 손으로 따라 써보는 방식으로 이해해보자.
> 뱀과 뱀의 진행 방향 변환 구조체

```c++
typedef struct{
	int y;
	int x;
	int dir;
}snake;

typedef struct{
	int second;
	char dir;
}orders;
```

> 뱀은 이리저리 기어다니다가 벽 또는 자기자신의 몸과 부딪히면 게임이 끝난다.


```c++
if(0 <= nx && nx < n && 0 <= ny && ny < n) // 벽을 부딪히지 않는 범위


else // 벽을 벗어난다는 말인 즉, 벽에 부딪힌다.
```


> 게임은 NxN 정사각 보드위에서 진행된다.

```c++
int n,k,L; // n : board size, k : apple count, l : snake direction
int board[MAX][MAX];
```

> 뱀은 처음에 오른쪽을 향한다.

```c++

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0}; // down : 0, up : 1, right : 2, left : 3

typedef struct{
	int y; //y
	int x; // x
	int dir; // direction right : 2 가 된다.
}snake;
```


> 뱀은 매 초마다 이동을 하는데 다음과 같은 규칙을 따른다.

> 먼저 뱀은 몸길이를 늘려 머리를 다음칸에 위치시킨다.

```c++

int nx = startX + dx[d]; // nextX
int ny = startY + dy[d]; // nextY 다음 위치

```


> 만약 이동한 칸에 사과가 있다면, 그 칸에 있던 사과가 없어지고 꼬리는 움직이지 않는다.

```c++

else if(board[ny][nx] == 1)
{
	dq.push_front({ny,nx});
	board[ny][nx] = 2; 
}

```

> 만약 이동한 칸에 사과가 없다면, 몸 길이를 줄여서 꼬리가 위치한 칸을 비워준다. 즉, 몸길이는 변하지 않는다.

```c++

if(board[ny][nx] == 0) // empty
{
	board[dq.back().first][dq.back().second] = 0;
	dq.pop_back();
	dq.push_front({ny,nx});
	board[ny][nx] = 2;
}

```


> L개의 줄에는 뱀의 방향 변환 정보가 주어진다. 정수 X와 문자 C로 이루어져 있다.

> 게임 시작 시간으로부터 X초가 끝난 뒤에 왼쪽(C가 'L') 또는 오른쪽(C가 'D')로 90도 방향을 회전 시킨다.

```c++

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0}; // down : 0, up : 1, right : 2, left : 3

int rotateDirection(int d, char c)
{
	 if (c == 'L') // left 일때 표현
    {
        if (d == 0) return 2; 
        else if (d == 1) return 3;
        else if (d == 2) return 1;
        else if (d == 3) return 0;
    }
    else if (c == 'D') // right 일때 표현
    {
        if (d == 0) return 3;
        else if (d == 1) return 2;
        else if (d == 2) return 0;
        else if (d == 3) return 1;
    }
}

```

```c++

for(int i = 0; i < L; i++)
{
	int seconds;
	char direction;
	cin >> seconds >> direction;
		
	order.push_back({seconds,direction});
}

```

```c++

if(index < order.size())
{
	if(count == order[index].second)
	{
		if(order[index].dir == 'L')
			d = rotateDirection(startD,order[index].dir); //L
		else
			d = rotateDirection(startD,order[index].dir); //D
		index++; // 다음 명령 수행
	}	
}

```

 ~이부분을 제대로 안 읽어서 문제가 발생했었다.~
 
 ### 전체 코드
 
```c++

#include<iostream>
#include<queue>
#include<deque>
#include<vector>
#include<cstring>

#define MAX 101
using namespace std;

typedef struct{
	int y;
	int x;
	int dir;
}snake;

typedef struct{
	int second;
	char dir;
}orders;

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0}; // down : 0, up : 1, right : 2, left : 3

int n,k,L; // n : board size, k : apple count, l : snake direction
int board[MAX][MAX];
deque<pair<int,int> > dq;
vector<orders> order;

int rotateDirection(int d, char c)
{
	 if (c == 'L')
    {
        if (d == 0) return 2;
        else if (d == 1) return 3;
        else if (d == 2) return 1;
        else if (d == 3) return 0;
    }
    else if (c == 'D')
    {
        if (d == 0) return 3;
        else if (d == 1) return 2;
        else if (d == 2) return 0;
        else if (d == 3) return 1;
    }
}

int main()
{
	memset(board,0,sizeof(board));
	
	cin >> n >> k;
	
	for(int i = 0; i < k; i++)
	{
		int y,x;
		cin >> y >> x;
		
		board[y-1][x-1] = 1; // apple location
	}
	
	cin >> L;
	
	int startX, startY, count = 0;
	int startD = 2; // right
	int index = 0;
	int result = 0;
	bool chk = true;
	
	dq.push_back({startY,startX});
	
	for(int i = 0; i < L; i++)
	{
		int seconds;
		char direction;
		cin >> seconds >> direction;
		
		order.push_back({seconds,direction});
	}
	
	int d = 2;
	
	while(1)
	{
		count++;
		int nx = startX + dx[d];
		int ny = startY + dy[d];
		
	//	cout << ny << " " << nx << endl;
			
		if(0 <= nx && nx < n && 0 <= ny && ny < n)
		{
			if(board[ny][nx] == 0) // empty
			{
				board[dq.back().first][dq.back().second] = 0;
				dq.pop_back();
				dq.push_front({ny,nx});
				board[ny][nx] = 2;
			}
			else if(board[ny][nx] == 1)
			{
				dq.push_front({ny,nx});
				board[ny][nx] = 2; 
			}
			else if(board[ny][nx] == 2)
			{
				break;
			}
		}
		else
			break;
		
		if(index < order.size())
		{
			if(count == order[index].second)
			{
				if(order[index].dir == 'L')
					d = rotateDirection(startD,order[index].dir);
				else
					d = rotateDirection(startD,order[index].dir);
				index++;
			}	
		}
		startD = d;
		startX = nx;
		startY = ny;
	}
	
//	for(int i = 0; i < n; i++)
//	{
//		for(int j = 0; j < n; j++)
//		{
//			cout << board[i][j] << " ";
//		}
//		cout << endl;
//	}
	
	
	cout << count << endl;
	return 0;
}


```
