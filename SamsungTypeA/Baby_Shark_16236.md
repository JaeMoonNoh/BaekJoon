# 백준 아기 상어 16236
[아기 상어](https://www.acmicpc.net/problem/16236)

### 문제

N×N 크기의 공간에 물고기 M마리와 아기 상어 1마리가 있다. 공간은 1×1 크기의 정사각형 칸으로 나누어져 있다. 한 칸에는 물고기가 최대 1마리 존재한다.

아기 상어와 물고기는 모두 크기를 가지고 있고, 이 크기는 자연수이다. 가장 처음에 아기 상어의 크기는 2이고, 아기 상어는 1초에 상하좌우로 인접한 한 칸씩 이동한다.

아기 상어는 자신의 크기보다 큰 물고기가 있는 칸은 지나갈 수 없고, 나머지 칸은 모두 지나갈 수 있다. 아기 상어는 자신의 크기보다 작은 물고기만 먹을 수 있다. 따라서, 크기가 같은 물고기는 먹을 수 없지만, 그 물고기가 있는 칸은 지나갈 수 있다.

아기 상어가 어디로 이동할지 결정하는 방법은 아래와 같다.

* 더 이상 먹을 수 있는 물고기가 공간에 없다면 아기 상어는 엄마 상어에게 도움을 요청한다.
* 먹을 수 있는 물고기가 1마리라면, 그 물고기를 먹으러 간다.
* 먹을 수 있는 물고기가 1마리보다 많다면, 거리가 가장 가까운 물고기를 먹으러 간다.
    * 거리는 아기 상어가 있는 칸에서 물고기가 있는 칸으로 이동할 때, 지나야하는 칸의 개수의 최솟값이다.
    * 거리가 가까운 물고기가 많다면, 가장 위에 있는 물고기, 그러한 물고기가 여러마리라면, 가장 왼쪽에 있는 물고기를 먹는다.
    * 아기 상어의 이동은 1초 걸리고, 물고기를 먹는데 걸리는 시간은 없다고 가정한다. 즉, 아기 상어가 먹을 수 있는 물고기가 있는 칸으로 이동했다면, 이동과 동시에 물고기를 먹는다. 물고기를 먹으면, 그 칸은 빈 칸이 된다.

아기 상어는 자신의 크기와 같은 수의 물고기를 먹을 때 마다 크기가 1 증가한다. 예를 들어, 크기가 2인 아기 상어는 물고기를 2마리 먹으면 크기가 3이 된다.

공간의 상태가 주어졌을 때, 아기 상어가 몇 초 동안 엄마 상어에게 도움을 요청하지 않고 물고기를 잡아먹을 수 있는지 구하는 프로그램을 작성하시오.

### 입력

첫째 줄에 공간의 크기 N(2 ≤ N ≤ 20)이 주어진다.

둘째 줄부터 N개의 줄에 공간의 상태가 주어진다. 공간의 상태는 0, 1, 2, 3, 4, 5, 6, 9로 이루어져 있고, 아래와 같은 의미를 가진다.

* 0: 빈 칸
* 1, 2, 3, 4, 5, 6: 칸에 있는 물고기의 크기
* 9: 아기 상어의 위치

아기 상어는 공간에 한 마리 있다.

### 출력

첫째 줄에 아기 상어가 엄마 상어에게 도움을 요청하지 않고 물고기를 잡아먹을 수 있는 시간을 출력한다.

### 알고리즘 & 자료구조

문제에서 주어지는 조건들을 읽고 정확히 구현해야하는 문제이다. 다양하게 풀수 있지만, 기본을 놓치기 시작하면 굉장히 시간이 오래걸리 수가 있다.
본인이 무척 오래 걸렸다. 처음 시작 위치를 visited[y][x]를 해줬다고 생각을 했으나, 하지 않아서 50분은 더 쳐다 봤던거 같다.
BFS를 통해서 상어의 움직임을 파악하고, 상어의 조건에 맞춰 sort를 해주어야 문제를 풀수 있다.


### 구현해야할 조건

> 아기 상어와 물고기는 모두 크기를 가지고 있고, 이 크기는 자연수이다. 가장 처음에 아기 상어의 크기는 2이다.

```c++
typedef struct{
	int y;
	int x;
	int dist; // 물고기와 상어사이의 거리를 표현하는 변수
}Fish; // 물고기 struct

typedef struct{
	int y;
	int x;
	int size; // 아기 상어의 사이즈
	int eat_fish; // 성장전 물고기를 얼마나 먹었는지 표현하는 변수
	int time; // 움직인 시간
}Shark; // 아기상어 struct

Shark s; // 전역 변수로 구조체 변수를 써놓는다. 전체 과정에서 계속해서 상어의 위치는 사용해야하기 때문에

```

>  아기 상어는 1초에 상하좌우로 인접한 한 칸씩 이동한다.

```c++
const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0}; // down, up, right, left
```

> 아기 상어는 자신의 크기보다 큰 물고기가 있는 칸은 지나갈 수 없다.

```c++

int aquarium[MAX][MAX]; // 물고기와 상어의 위치를 표현하는 값

if(s.size < aquarium[ny][nx])
// 지나갈 수 없는 조건

```


> 나머지 칸은 모두 지나갈 수 있다.


```c++
if(!aquarium[ny][nx])
{
	visited[ny][nx] = true;
	//shark
	q.push({{ny,nx},d+1});
	}
	else if(aquarium[ny][nx] < s.size)
	{
		visited[ny][nx] = true;
							
		Fish tempF;
		tempF.x = nx;
		tempF.y = ny;
		tempF.dist = d+1;
							
		q.push({{ny,nx},d+1});
		fish.push_back(tempF);
    // 아기 상어는 자신의 크기보다 작은 물고기만 먹을 수 있다.
	}
	else if(aquarium[ny][nx] == s.size)
	{
		visited[ny][nx] = true;
		q.push({{ny,nx},d+1});
	}//크기가 같은 물고기는 먹을 수 없지만, 그 물고기가 있는 칸은 지나갈 수 있다.
}
```


> 더 이상 먹을 수 있는 물고기가 공간에 없다면 아기 상어는 엄마 상어에게 도움을 요청한다.

```c++
	if(fish.size() == 0)
		{
			cout << time << "\n"; // 엄마한테 요청하면서 시간 말하기
			break;
		}
```

> 먹을 수 있는 물고기가 1마리면, 그 고기를 먹으러 간다.

```c++
	else if(fish.size() == 1)
		{
			time += fish[0].dist;
			aquarium[fish[0].y][fish[0].x] = 9;
			aquarium[s.y][s.x] = 0;
			s.eat_fish++;
			s.y = fish[0].y;
			s.x = fish[0].x;
			if(s.eat_fish == s.size) // 먹었는데 이제까지 먹은 수가 사이즈랑 같다면 몸값 올려주기
			{
				s.eat_fish = 0;
				s.size++;
			}
		}
```

> 먹을 수 있는 물고기가 1마리보다 많다면, 거리가 가장 가까운 물고기를 먹으러 간다.

```c++
bool comp(Fish &a, Fish &b)
{
	if(a.dist == b.dist) // 거리가 같으면
	{
		if(a.y == b.y) // 가장 위에도 같다면
		{
			return a.x < b.x; // 가장 왼쪽에 있는 물고기
		}
		return a.y < b.y;
	}
	return a.dist < b.dist;
	
}


sort(fish.begin(),fish.end(),comp); // comp를 통해 sort

```

### 전체 코드

```c++
#include<iostream>
#include<cstring>
#include<algorithm>
#include<queue>
#include<vector>

#define MAX 22
using namespace std;
typedef struct{
	int y;
	int x;
	int dist;
}Fish;

typedef struct{
	int y;
	int x;
	int size;
	int eat_fish;
	int time;
}Shark;

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0};

bool comp(Fish &a, Fish &b)
{
	if(a.dist == b.dist)
	{
		if(a.y == b.y)
		{
			return a.x < b.x;
		}
		return a.y < b.y;
	}
	return a.dist < b.dist;
	
}	

int n;
int time = 0;
int aquarium[MAX][MAX];
int move_copy[MAX][MAX];
bool visited[MAX][MAX]; 

Shark s;
queue<Shark> shark;
queue<pair<pair<int,int>,int> > q;

int main()
{
	cin >> n;
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < n; j++)
		{
			cin >> aquarium[i][j];
			
			if(aquarium[i][j] == 9)
			{
				s.x = j;
				s.y = i;
				s.size = 2;
				s.eat_fish = 0;
				s.time = 0;
				q.push({{i,j},0});
			}
		}
	}
	
	
	while(1)
	{
		memset(visited,false,sizeof(visited));
		vector<Fish> fish;
		
		// moving shark
		while(!q.empty())
		{
			int y = q.front().first.first;
			int x = q.front().first.second;
			int d = q.front().second;
			q.pop();
			
			visited[y][x] = true; // 가장 오래걸렸던 이유
			
			for(int i = 0; i < 4; i++)
			{
				int nx = x + dx[i];
				int ny = y + dy[i];
				if(0 <= nx && nx < n && 0 <= ny && ny < n) // 해당 범위 
				{
					if(!visited[ny][nx])
					{
						if(!aquarium[ny][nx])
						{
							visited[ny][nx] = true;
							//shark
							q.push({{ny,nx},d+1});
						}
						else if(aquarium[ny][nx] < s.size)
						{
							visited[ny][nx] = true;
							
							Fish tempF;
							tempF.x = nx;
							tempF.y = ny;
							tempF.dist = d+1;
							
							q.push({{ny,nx},d+1});
							fish.push_back(tempF);
						}
						else if(aquarium[ny][nx] == s.size)
						{
							visited[ny][nx] = true;
							q.push({{ny,nx},d+1});
						}
					}
				}
			}
		}
		
		
		if(fish.size() == 0)
		{
			cout << time << "\n";
			break;
		}
		else if(fish.size() == 1)
		{
			time += fish[0].dist;
			aquarium[fish[0].y][fish[0].x] = 9;
			aquarium[s.y][s.x] = 0;
			s.eat_fish++;
			s.y = fish[0].y;
			s.x = fish[0].x;
			if(s.eat_fish == s.size)
			{
				s.eat_fish = 0;
				s.size++;
			}
		}
		else
		{
			sort(fish.begin(),fish.end(),comp);
			time += fish[0].dist;
			aquarium[fish[0].y][fish[0].x] = 9;
			aquarium[s.y][s.x] = 0;
			s.eat_fish++;
			s.y = fish[0].y;
			s.x = fish[0].x;
			if(s.eat_fish == s.size)
			{
				s.eat_fish = 0;
				s.size++;
			}
		}
		
		q.push({{s.y,s.x},0});
		fish.clear();
	}
	
	return 0;
}


```

### 잡기술 1


처음 아기 상어가 이동하는 곳에 visited를 설정하지 않으면 되지 않는다.
본인은 한자리에 물고기와 상어가 같이 있으면 안된다고 해서 필요없을 줄 알았는데, 꼭 필요한 조건이였다.
visited[y][x] = true; 가 빠져버리면 아예 틀렸다고 나오게 된다. 처음 이동후에 아기상어가 처음 위치한 장소가 visited가 설정이 안되어있어 생기는 문제라고 파악이 된다.



