# 백준 로봇청소기 14503

[로봇청소기](https://www.acmicpc.net/problem/14503)

### 문제
로봇 청소기가 주어졌을 때, 청소하는 영역의 개수를 구하는 프로그램을 작성하시오.

로봇 청소기가 있는 장소는 N×M 크기의 직사각형으로 나타낼 수 있으며, 1×1크기의 정사각형 칸으로 나누어져 있다. 각각의 칸은 벽 또는 빈 칸이다. 청소기는 바라보는 방향이 있으며, 이 방향은 동, 서, 남, 북중 하나이다. 지도의 각 칸은 (r, c)로 나타낼 수 있고, r은 북쪽으로부터 떨어진 칸의 개수, c는 서쪽으로 부터 떨어진 칸의 개수이다.

로봇 청소기는 다음과 같이 작동한다.

1) 현재 위치를 청소한다.
2) 현재 위치에서 현재 방향을 기준으로 왼쪽 방향부터 차례대로 인접한 칸을 탐색한다.   
    a. 왼쪽 방향에 아직 청소하지 않은 공간이 존재한다면, 그 방향으로 회전한 다음 한 칸을 전진하고 1번부터 진행한다.   
    b. 왼쪽 방향에 청소할 공간이 없다면, 그 방향으로 회전하고 2번으로 돌아간다.   
    c. 네 방향 모두 청소가 이미 되어있거나 벽인 경우에는, 바라보는 방향을 유지한 채로 한 칸 후진을 하고 2번으로 돌아간다.   
    d. 네 방향 모두 청소가 이미 되어있거나 벽이면서, 뒤쪽 방향이 벽이라 후진도 할 수 없는 경우에는 작동을 멈춘다.   

로봇 청소기는 이미 청소되어있는 칸을 또 청소하지 않으며, 벽을 통과할 수 없다.

### 입력
첫째 줄에 세로 크기 N과 가로 크기 M이 주어진다. (3 ≤ N, M ≤ 50)

둘째 줄에 로봇 청소기가 있는 칸의 좌표 (r, c)와 바라보는 방향 d가 주어진다. d가 0인 경우에는 북쪽을, 1인 경우에는 동쪽을, 2인 경우에는 남쪽을, 3인 경우에는 서쪽을 바라보고 있는 것이다.

셋째 줄부터 N개의 줄에 장소의 상태가 북쪽부터 남쪽 순서대로, 각 줄은 서쪽부터 동쪽 순서대로 주어진다. 빈 칸은 0, 벽은 1로 주어진다. 지도의 첫 행, 마지막 행, 첫 열, 마지막 열에 있는 모든 칸은 벽이다.

로봇 청소기가 있는 칸의 상태는 항상 빈 칸이다.

### 출력

로봇 청소기가 청소하는 칸의 개수를 출력한다.

### 알고리즘

로봇이 작동하는 방식에 따라서 조건에 맞춰 구현하면 되는 문제입니다.
구현, 시뮬레이션

### 구현해야 할 조건

> 로봇 구조체

```c++
typedef struct{
	int y; // robot y
	int x; // robot x
	int dir; // robot direciton 바라보는 방향
	int clean_area; // robot 기준 4방향 청소됐는지 확인하기 위한 cnt 값
}Robot;

```

> 청소기는 바라보는 방향이 있으며, 이 방향은 동,서,남,북중 하나이다.

```c++
const int dx[4] = {0,1,0,-1};
const int dy[4] = {-1,0,1,0}; // up : 0, right : 1, down : 2, left : 3
```

> 현재 위치를 청소한다.

```c++
if(!office[y][x] && !visited[y][x]) // 현재 로봇의 위치가 0이고, 청소한 적이 없다면
{
	visited[y][x] = true;
	cnt_cleaned++;
} // 현재 위치 청소 
```

> 현재 위치에서 현재 방향을 기준으로 왼쪽 방향부터 차례대로 인접한 칸을 탐색한다.

```c++

int rotate_left(int d)
{
	if(d == 0) return 3; // up -> left
	else if(d == 1) return 0; // right -> up
	else if(d == 2) return 1; // down -> right
	else return 2; // left -> down
} // 현재 바라보는 방향에서 왼쪽으로 돌면서 탐색하는 기준.

```

> 왼쪽 방향에 아직 청소하지 않은 공간이 존재, 그 방향으로 회전한 다음 한 칸을 전진하고 1번부터 진행한다.

```c++

int nd = rotate_left(dir); // 왼쪽 방향 청소자리 
		
if(!office[y + dy[nd]][x + dx[nd]] && !visited[y+dy[nd]][x+dx[nd]]) 
{
	q.push({y+dy[nd],x+dx[nd],nd,0});
}
//한칸 전진을 하게 되는데 바라보는 방향에 청소를 아직 하지 않았고 방문한적이 없고, 벽이 아니라면
```
> 바라보는 방향에서 왼쪽 구역이 청소가 이미 되어있거나, 방문한 적이 있다.

```c++

else
	q.push({y,x,nd,clean_chk+1}); // clean_chk++를 하나씩 올려주게 됩니다.


```

> 네 방향 모두 청소가 이미 되어있거나, 벽인 경우 바라보는 방향을 유지한 채로 한 칸 후진을 하고 2번으로 돌아간다.

```c++

//Robot struct에서 clean_chk값이 4가 되었다면 해당 제자리에서 
//동,서,남,북 4방향을 전부 봤기 때문에 값이 4가 되었다는 결론이 된다.
if(clean_chk == 4)
{
	if(office[y-dy[dir]][x-dx[dir]] == 1) // 후진을 하게 될때, 벽이 있다면 청소는 종료
		break;
	else
		q.push({y-dy[dir],x-dx[dir],dir,0}); // 벽이 없다면 다시 시작
}


```


### 전체 코드


```c++

#include<iostream>
#include<vector>
#include<queue>
#include<algorithm>

#define MAX 51
using namespace std;

typedef struct{
	int y;
	int x;
	int dir;
	int clean_area;
}Robot;

const int dx[4] = {0,1,0,-1};
const int dy[4] = {-1,0,1,0}; // up, right, down, left

int n,m;
int r,c,d;
int office[MAX][MAX];
bool visited[MAX][MAX]; // 청소시킬 칸 

int rotate_left(int d)
{
	if(d == 0) return 3;
	else if(d == 1) return 0;
	else if(d == 2) return 1;
	else return 2;
}

int main()
{
	cin >> n >> m;
	cin >> r >> c >> d;
	
	queue<Robot> q;
	q.push({r,c,d,0});
	
	for(int i = 0; i < n; i++)
		for(int j = 0; j < m; j++)
			cin >> office[i][j];
	
	int cnt_cleaned = 0;
	
	while(!q.empty())
	{
		int y = q.front().y;
		int x = q.front().x;
		int dir = q.front().dir;
		int clean_chk = q.front().clean_area;
		q.pop();
		
		//cout << y << " " << x << " " << dir << " " << clean_chk << endl;
		
		if(clean_chk == 4)
		{
			if(office[y-dy[dir]][x-dx[dir]] == 1)
				break;
			else
				q.push({y-dy[dir],x-dx[dir],dir,0});
		}
		
		if(!office[y][x] && !visited[y][x])
		{
			visited[y][x] = true;
			cnt_cleaned++;
		} // 현재 위치 청소 
		
		int nd = rotate_left(dir); // 왼쪽 방향 청소자리 
		
		if(!office[y + dy[nd]][x + dx[nd]] && !visited[y+dy[nd]][x+dx[nd]])
		{
			q.push({y+dy[nd],x+dx[nd],nd,0});
		}
		else
		{
			q.push({y,x,nd,clean_chk+1});
		}
	}
	
	cout << cnt_cleaned << endl;
	
	return 0;
}

```
