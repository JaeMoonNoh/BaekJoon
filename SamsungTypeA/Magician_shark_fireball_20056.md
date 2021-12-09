# 백준 마법사 상어와 파이어 볼 20056
[마법사 상어와 파이어볼](https://www.acmicpc.net/problem/20056)

## 문제

어른 상어가 마법사가 되었고, 파이어볼을 배웠다.

마법사 상어가 크기가 N×N인 격자에 파이어볼 M개를 발사했다. 가장 처음에 파이어볼은 각자 위치에서 이동을 대기하고 있다. i번 파이어볼의 위치는 (ri, ci), 질량은 mi이고, 방향은 di, 속력은 si이다. 위치 (r, c)는 r행 c열을 의미한다.

격자의 행과 열은 1번부터 N번까지 번호가 매겨져 있고, 1번 행은 N번과 연결되어 있고, 1번 열은 N번 열과 연결되어 있다.

파이어볼의 방향은 어떤 칸과 인접한 8개의 칸의 방향을 의미하며, 정수로는 다음과 같다.

```c++
7	0	1
6	 	2
5	4	3
```

마법사 상어가 모든 파이어볼에게 이동을 명령하면 다음이 일들이 일어난다.

  1. 모든 파이어볼이 자신의 방향 di로 속력 si칸 만큼 이동한다.
    * 이동하는 중에는 같은 칸에 여러 개의 파이어볼이 있을 수도 있다.
  2. 이동이 모두 끝난 뒤, 2개 이상의 파이어볼이 있는 칸에서는 다음과 같은 일이 일어난다.
    1. 같은 칸에 있는 파이어볼은 모두 하나로 합쳐진다.
    2. 파이어볼은 4개의 파이어볼로 나누어진다.
    3. 나누어진 파이어볼의 질량, 속력, 방향은 다음과 같다.
      1. 질량은 ⌊(합쳐진 파이어볼 질량의 합)/5⌋이다.
      2. 속력은 ⌊(합쳐진 파이어볼 속력의 합)/(합쳐진 파이어볼의 개수)⌋이다.
      3. 합쳐지는 파이어볼의 방향이 모두 홀수이거나 모두 짝수이면, 방향은 0, 2, 4, 6이 되고, 그렇지 않으면 1, 3, 5, 7이 된다.
      4. 질량이 0인 파이어볼은 소멸되어 없어진다.

마법사 상어가 이동을 K번 명령한 후, 남아있는 파이어볼 질량의 합을 구해보자.

## 입력

첫째 줄에 N, M, K가 주어진다.

둘째 줄부터 M개의 줄에 파이어볼의 정보가 한 줄에 하나씩 주어진다. 파이어볼의 정보는 다섯 정수 ri, ci, mi, si, di로 이루어져 있다.

서로 다른 두 파이어볼의 위치가 같은 경우는 입력으로 주어지지 않는다.

## 출력

마법사 상어가 이동을 K번 명령한 후, 남아있는 파이어볼 질량의 합을 출력한다.

## 자료구조 & 알고리즘

단순한 구현입니다. 처음에 vector를 사용해서 문제를 풀었던 거와, queue를 사용해 문제를 풀었던 것중

시간의 차이가 조금 있는데, 어떤게 조금더 구현하기 쉬운지 조금 더 생각을 해야 할것 같다.

## 구현해야할 조건

> 파이어볼의 방향은 어떤 칸과 인접한 8개의 칸의 방향을 의미한다.

```c++
const int dx[9] = {0,1,1,1,0,-1,-1,-1};
const int dy[9] = {-1,-1,0,1,1,1,0,-1}; // 0 ~ 7
```



> 모든 파이버볼이 자신의 방향 di로 속력 si 칸만큼 이동한다.
  * 이동하는 중에는 같은 칸에 여러 개의 파이어볼이 있을 수도 있다.

```c++
	while(!sharks.empty())
	{
		int y = sharks.front().y;
		int x = sharks.front().x;
		int massive = sharks.front().massive;
		int dir = sharks.front().dir;
		int speed = sharks.front().speed;
		sharks.pop();
		
		int moduler = speed % n; // 1번 행은 N번과 연결되어 있고, 1번 열은 N번 열과 연결되어 있다. 모든 값을 계산할 필요가 없다.

		int nx = x + dx[dir] * moduler;
		int ny = y + dy[dir] * moduler;
		
		if(nx < 1) nx += n;
		if(ny < 1) ny += n;
		if(nx > n) nx -= n;
		if(ny > n) ny -= n;
		
		Shark temp;
		
		temp.y = ny; // 이동된 애들 다시 queue에 넣기
		temp.x = nx;
		temp.dir = dir;
		temp.massive = massive;
		temp.speed = speed;
		
		q.push(temp); // first.first = y, first.second = x, second = number
	}
```


> 이동이 모두 끝난 뒤, 2개 이상의 파이어볼이 있는 칸에서는 다음과 같은 일이 일어난다.

  * 같은 칸에 있는 파이어볼은 모두 하나로 합쳐진다.
  * 파이어볼은 4개의 파이어볼로 나누어진다.

```c++


        const int edir[4] = {0,2,4,6};
        const int odir[4] = {1,3,5,7};

        if(even && !odd) // 짝수만 나오거나
				{
					for(int c = 0; c < 4; c++)
					{
						Shark temp;
						temp.y = i;
						temp.x = j;
						temp.speed = sum_speed / board[i][j].size();
						temp.massive = sum_massive / 5;
						
						if(temp.massive == 0)
							continue;
						
						temp.dir = edir[c];
						
						sharks.push(temp);
					}
				}
				else if(!even && odd) // 홀수만 나오거나
				{
					for(int c = 0; c < 4; c++)
					{
						Shark temp;
						temp.y = i;
						temp.x = j;
						temp.speed = sum_speed / board[i][j].size();
						temp.massive = sum_massive / 5;
						
						if(temp.massive == 0)
							continue;
						
						temp.dir = edir[c];
						
						sharks.push(temp);
					}
				}
				else if(even && odd) // 짝수, 
				{
					for(int c = 0; c < 4; c++)
					{
						Shark temp;
						temp.y = i;
						temp.x = j;
						temp.speed = sum_speed / board[i][j].size();
						temp.massive = sum_massive / 5;
						
						if(temp.massive == 0)
							continue;
						
						temp.dir = odir[c];
						
						sharks.push(temp);
					}
				}
```




  * 나누어진 파이어볼의 질량, 속력, 방향은 다음과 같다

    * 질량 = [합쳐진 파이어볼 질량의 합 / 5]이다.
    * 속력 = [합쳐진 파이어볼 속력의 합 / 합쳐진 파이어볼의 개수] 이다.
    * 합쳐지는 파이어볼의 방향이 모두 홀수이거나, 모두 짝수이면, 방향은 0,2,4,6이 되고, 그렇지 않으면 1,3,5,7이 된다.
    * 질량이 0인 파이어볼은 소멸되어 없어진다.

```c++

      if(board[i][j].size() > 1)
			{
				int sum_massive = 0;
				int sum_speed = 0;
				bool even = false;
				bool odd = false;
				
				for(int k = 0; k < board[i][j].size(); k++)
				{
					sum_massive += board[i][j][k].massive;
					sum_speed += board[i][j][k].speed;
					
					if(board[i][j][k].dir % 2 == 0)
						even = true;
					else
						odd = true;
				}
.
.
.
.
```


## 전체코드


```c++
#include<iostream>
#include<queue>
#include<cstring> 
#include<algorithm>

#define MAX 51
using namespace std;

typedef struct{
	int y;
	int x;
	int massive;
	int dir;
	int speed;
}Shark;

const int dx[9] = {0,1,1,1,0,-1,-1,-1};
const int dy[9] = {-1,-1,0,1,1,1,0,-1}; // 0 ~ 7

const int edir[4] = {0,2,4,6};
const int odir[4] = {1,3,5,7};

int n,m,k;
int r,c,ma,s,d;
vector<Shark> board[MAX][MAX];

queue<Shark> sharks;

void shark_move()
{
	memset(board,0,sizeof(board)); // board 초기화 
	queue<Shark> q;
	
	while(!sharks.empty())
	{
		int y = sharks.front().y;
		int x = sharks.front().x;
		int massive = sharks.front().massive;
		int dir = sharks.front().dir;
		int speed = sharks.front().speed;
		sharks.pop();
		
		int moduler = speed % n;

		int nx = x + dx[dir] * moduler;
		int ny = y + dy[dir] * moduler;
		
		if(nx < 1) nx += n;
		if(ny < 1) ny += n;
		if(nx > n) nx -= n;
		if(ny > n) ny -= n;
		
		Shark temp;
		
		temp.y = ny;
		temp.x = nx;
		temp.dir = dir;
		temp.massive = massive;
		temp.speed = speed;
		
		q.push(temp); // first.first = y, first.second = x, second = number
	}
	
	// 이동이 모두 끝난 뒤, 2개 이상의 파이볼에 있는 칸에서는 다음과 같은 일이 벌어진다. 
	
	while(!q.empty())
	{
		int y = q.front().y;
		int x = q.front().x;
		
		board[y][x].push_back(q.front());
				
		q.pop();
	}
	
	for(int i = 1; i <= n; i++)
	{
		for(int j = 1; j <= n; j++)
		{
			if(board[i][j].size() > 1)
			{
				int sum_massive = 0;
				int sum_speed = 0;
				bool even = false;
				bool odd = false;
				
				for(int k = 0; k < board[i][j].size(); k++)
				{
					sum_massive += board[i][j][k].massive;
					sum_speed += board[i][j][k].speed;
					
					if(board[i][j][k].dir % 2 == 0)
						even = true;
					else
						odd = true;
				}
				
				
				if(even && !odd)
				{
					for(int c = 0; c < 4; c++)
					{
						Shark temp;
						temp.y = i;
						temp.x = j;
						temp.speed = sum_speed / board[i][j].size();
						temp.massive = sum_massive / 5;
						
						if(temp.massive == 0)
							continue;
						
						temp.dir = edir[c];
						
						sharks.push(temp);
					}
				}
				else if(!even && odd)
				{
					for(int c = 0; c < 4; c++)
					{
						Shark temp;
						temp.y = i;
						temp.x = j;
						temp.speed = sum_speed / board[i][j].size();
						temp.massive = sum_massive / 5;
						
						if(temp.massive == 0)
							continue;
						
						temp.dir = edir[c];
						
						sharks.push(temp);
					}
				}
				else if(even && odd)
				{
					for(int c = 0; c < 4; c++)
					{
						Shark temp;
						temp.y = i;
						temp.x = j;
						temp.speed = sum_speed / board[i][j].size();
						temp.massive = sum_massive / 5;
						
						if(temp.massive == 0)
							continue;
						
						temp.dir = odir[c];
						
						sharks.push(temp);
					}
				}
				
			}
			else if(board[i][j].size() == 1)
			{
				Shark temp;
				temp.y = i;
				temp.x = j;
				temp.massive = board[i][j][0].massive;
				temp.speed = board[i][j][0].speed;
				temp.dir = board[i][j][0].dir;
				
				sharks.push(temp);
			}
		}

	}
	
	
}

int main()
{
	
	cin >> n >> m >> k;
	
	for(int i = 0; i < m; i++)
	{
		cin >> r >> c >> ma >> s >> d;
		
		Shark temp;
		temp.y = r;
		temp.x = c;
		temp.massive = ma;
		temp.speed = s;
		temp.dir = d;
		
		sharks.push(temp);
	}
	
	while(k--)
	{
		shark_move();
	}
	
	int ans = 0;
	
	while(!sharks.empty())
	{
		ans += sharks.front().massive;
		sharks.pop();
	}
	
	cout << ans;
	
	
	return 0;
}
```















