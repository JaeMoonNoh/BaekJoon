# 백준 낚시왕 17143
[낚시왕](https://www.acmicpc.net/problem/17143)

### 문제

낚시왕이 상어 낚시를 하는 곳은 크기가 R×C인 격자판으로 나타낼 수 있다. 격자판의 각 칸은 (r, c)로 나타낼 수 있다. r은 행, c는 열이고, (R, C)는 아래 그림에서 가장 오른쪽 아래에 있는 칸이다. 칸에는 상어가 최대 한 마리 들어있을 수 있다. 상어는 크기와 속도를 가지고 있다.

낚시왕은 처음에 1번 열의 한 칸 왼쪽에 있다. 다음은 1초 동안 일어나는 일이며, 아래 적힌 순서대로 일어난다. 낚시왕은 가장 오른쪽 열의 오른쪽 칸에 이동하면 이동을 멈춘다.

낚시왕이 오른쪽으로 한 칸 이동한다.
낚시왕이 있는 열에 있는 상어 중에서 땅과 제일 가까운 상어를 잡는다. 상어를 잡으면 격자판에서 잡은 상어가 사라진다.
상어가 이동한다.
상어는 입력으로 주어진 속도로 이동하고, 속도의 단위는 칸/초이다. 상어가 이동하려고 하는 칸이 격자판의 경계를 넘는 경우에는 방향을 반대로 바꿔서 속력을 유지한채로 이동한다.

왼쪽 그림의 상태에서 1초가 지나면 오른쪽 상태가 된다. 상어가 보고 있는 방향이 속도의 방향, 왼쪽 아래에 적힌 정수는 속력이다. 왼쪽 위에 상어를 구분하기 위해 문자를 적었다.

상어가 이동을 마친 후에 한 칸에 상어가 두 마리 이상 있을 수 있다. 이때는 크기가 가장 큰 상어가 나머지 상어를 모두 잡아먹는다.

낚시왕이 상어 낚시를 하는 격자판의 상태가 주어졌을 때, 낚시왕이 잡은 상어 크기의 합을 구해보자.

### 출력

첫째 줄에 격자판의 크기 R, C와 상어의 수 M이 주어진다. (2 ≤ R, C ≤ 100, 0 ≤ M ≤ R×C)

둘째 줄부터 M개의 줄에 상어의 정보가 주어진다. 상어의 정보는 다섯 정수 r, c, s, d, z (1 ≤ r ≤ R, 1 ≤ c ≤ C, 0 ≤ s ≤ 1000, 1 ≤ d ≤ 4, 1 ≤ z ≤ 10000) 로 이루어져 있다. (r, c)는 상어의 위치, s는 속력, d는 이동 방향, z는 크기이다. d가 1인 경우는 위, 2인 경우는 아래, 3인 경우는 오른쪽, 4인 경우는 왼쪽을 의미한다.

두 상어가 같은 크기를 갖는 경우는 없고, 하나의 칸에 둘 이상의 상어가 있는 경우는 없다.

### 입력

낚시왕이 잡은 상어 크기의 합을 출력한다.

### 알고리즘 & 자료구조

구현, 시뮬레이션이다. 하지만 계속되는 반복이 이어지는 상어의 움직임을 줄일 필요가 있다.
상어의 움직임을 줄이는것이 핵심이다.

### 구현해야할 조건


> 낚시왕 한칸 이동해 열에 있는 상어 중에서 땅과 제일 가까운 상어를 잡는다. 상어를 잡으면 격자판에서 잡은 상어가 사라진다.

```c++

// first : speed, second.first : dir, second.second : size

void fisher_Catching(int location)
{
	for(int i = 1; i <= R; i++)
	{
		if(aquarium[i][location].size() == 1) // location : x의 위치이다.
		{
			ans += aquarium[i][location][0].second.second; // size에 해당함
			aquarium[i][location].clear();
			break;
		}
	}
	
	//cout << ans << endl;
}

```

> 방향 바꾸는 방법

```c++

const int dx[5] = {0,0,0,1,-1};
const int dy[5] = {0,-1,1,0,0}; // up : 1, down : 2, right : 3, left : 4

int rotate_direction(int d)
{
	if(d == 1) return 2;
	else if(d == 2)	return 1;
	else if(d == 3)	return 4;
	else return 3;
}

```

> 상어가 이동한다.

```c++

void move_Shark()
{
	
	while(!q.empty())
	{
		int y = q.front().first.first;
		int x = q.front().first.second;
		int speed = q.front().second.first;
		int dir = q.front().second.second.first;
		int size = q.front().second.second.second;
		
		q.pop();

		//cout << y << " " << x <<  " " << speed << " " << dir << " " << size << endl;
		
		if(dir == 1 || dir == 2)
		{
			int r = (R-1) * 2; // 위,아래일때 바꾸는 방법
			if(speed >= r)
				speed = speed % r; // 굳이 모든 시간을 돌릴 필요는 없다.
			
			for(int i = 0; i < speed; i++)
			{
				int ny = y + dy[dir];
				if(!(1 <= ny && ny <= R)) // down, up 범위 
				{
					dir = rotate_direction(dir);
					ny = y + dy[dir];
				}
				y = ny;
			}
		}
		else
		{
			int r = (C - 1) * 2; // 이 문제의 시간 줄이기 핵심, 계속해서 시간초과가 나와서 도움을 받았다.
			if(speed >= r)
				speed = speed % r;

			// 상어는 칸/초로 이동하고, 이동하려고 하는 칸이 격자판의 경계를 넘는 경우에는 방향을 반대로 바꿔서 속력을 유지한채로 이동한다.
			for(int i = 0; i < speed; i++)
			{
				int nx = x + dx[dir];
				if(!(1 <= nx && nx <= C)) // right, left 범위 
				{
					dir = rotate_direction(dir); // 경계를 넘어서는 순간 방향을 바꾼다.
					nx = x + dx[dir];
				}
				x = nx;
			}
		}
		
    //상어가 이동을 마친 후에 한 칸에 상어가 두마리 이상 있을 수 있다. 이때는 크기가 가장 큰 상어가 나머지 상어를 모두 잡아 먹는다.
		if(aquarium[y][x].size()) // 해당 칸에 상어가 있을때
		{
			if(size > aquarium[y][x][0].second.second) // 사이즈가 지금 이동한 상어가 더 크다면, queue에 넣어줄때 상어를 다 삭제해야함.
			{
				aquarium[y][x].clear();
				aquarium[y][x].push_back({speed,{dir,size}});
			}
		}
		else // 해당사항이 없을 때
		{		
			aquarium[y][x].push_back({speed,{dir,size}});
		}
			
	}
}

```

### 전체 코드


```c++

#include<iostream>
#include<vector>
#include<queue>

#define MAX 101
using namespace std;

const int dx[5] = {0,0,0,1,-1};
const int dy[5] = {0,-1,1,0,0}; // up : 1, down : 2, right : 3, left : 4

int R,C,M; // y, x, shark counts
int ans = 0;

vector<pair<int,pair<int,int> > > aquarium[MAX][MAX];
queue<pair<pair<int,int>, pair<int,pair<int,int> > > > q; 

int rotate_direction(int d)
{
	if(d == 1) return 2;
	else if(d == 2)	return 1;
	else if(d == 3)	return 4;
	else return 3;
}

void fisher_Catching(int location)
{
	for(int i = 1; i <= R; i++)
	{
		if(aquarium[i][location].size() == 1)
		{
			ans += aquarium[i][location][0].second.second;
			aquarium[i][location].clear();
			break;
		}
	}
	
	//cout << ans << endl;
}

void move_Shark()
{
	
	while(!q.empty())
	{
		int y = q.front().first.first;
		int x = q.front().first.second;
		int speed = q.front().second.first;
		int dir = q.front().second.second.first;
		int size = q.front().second.second.second;
		
		q.pop();
	
		if(dir == 1 || dir == 2)
		{
			int r = (R-1) * 2;
			if(speed >= r)
				speed = speed % r;
			
			for(int i = 0; i < speed; i++)
			{
				int ny = y + dy[dir];
				if(!(1 <= ny && ny <= R)) // down, up 범위 
				{
					dir = rotate_direction(dir);
					ny = y + dy[dir];
				}
				y = ny;
			}
		}
		else
		{
			int r = (C - 1) * 2;
			if(speed >= r)
				speed = speed % r;
			
			for(int i = 0; i < speed; i++)
			{
				int nx = x + dx[dir];
				if(!(1 <= nx && nx <= C)) // right, left 범위 
				{
					dir = rotate_direction(dir);
					nx = x + dx[dir];
				}
				x = nx;
			}
		}
		
		if(aquarium[y][x].size())
		{
			if(size > aquarium[y][x][0].second.second)
			{
				aquarium[y][x].clear();
				aquarium[y][x].push_back({speed,{dir,size}});
			}
		}
		else
		{	
			
			aquarium[y][x].push_back({speed,{dir,size}});
		}
			
	}
}

int main()
{
	cin >> R >> C >> M;
	
	for(int i = 0; i < M; i++)
	{
		int r,c,s,d,z; // r : y, c : x, s : speed, d : direction, z : size		
		cin >> r >> c >> s >> d >> z;
		
		aquarium[r][c].push_back({s,{d,z}}); // first : speed, second.first : dir, second.second : size
	}
	
	int location = 1;
	
	for(int i = 1; i <= C; i++)
	{
		fisher_Catching(location);
		//shark moving
		for(int i = 1; i <= R; i++)
		{
			for(int j = 1; j <= C; j++)
			{
				if(aquarium[i][j].size())
				{
					q.push({{i,j},{aquarium[i][j][0]}}); // 구조체 넣기 
					aquarium[i][j].clear();
				}
			}
		}
		move_Shark();
		
		location++;
	}
			
	cout << ans;
	
	return 0;
}


```

### 잡기술

해당 상어의 움직임이 많다고 판단되었을 때, 그 움직임을 줄일수 있는 방법을 꼭 생각해내야 한다.
해당 문제에서는 (R-1)*2는 제자리로 돌아오기 때문에 이 값으로 나머지를 구하면 최소한의 움직임으로 구할 수 있다.
계속해서 움직이기 때문에 시간적인 낭비가 크다.

그리고 q에 vector에 해당하는 값을 넣을 때 q.push({{i,j},{aquarium[i][j][0]}}); // 구조체 넣기
vector에 들어가는 값과 q에 들어가는 값이 같다면 이와 같이 넣어줄 수 있다.

구현은 했으나 시간초과, outofbounds 그리고 결국엔 시간초과가 나와 상어의 이동에 문제가 있다는 판단이 나와.
다른 블로그에 나와있는 식을 인용했다. 감사합니다.

