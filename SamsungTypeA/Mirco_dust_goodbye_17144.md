# 백준 미세먼지 안녕! 17144
[미세먼지 안녕!](https://www.acmicpc.net/problem/17144)

### 문제

[그림생략]

미세먼지를 제거하기 위해 구사과는 공기청정기를 설치하려고 한다. 공기청정기의 성능을 테스트하기 위해 구사과는 집을 크기가 R×C인 격자판으로 나타냈고, 1×1 크기의 칸으로 나눴다. 구사과는 뛰어난 코딩 실력을 이용해 각 칸 (r, c)에 있는 미세먼지의 양을 실시간으로 모니터링하는 시스템을 개발했다. (r, c)는 r행 c열을 의미한다.

공기청정기는 항상 1번 열에 설치되어 있고, 크기는 두 행을 차지한다. 공기청정기가 설치되어 있지 않은 칸에는 미세먼지가 있고, (r, c)에 있는 미세먼지의 양은 Ar,c이다.

1초 동안 아래 적힌 일이 순서대로 일어난다.

1. 미세먼지가 확산된다. 확산은 미세먼지가 있는 모든 칸에서 동시에 일어난다.
    * (r, c)에 있는 미세먼지는 인접한 네 방향으로 확산된다.
    * 인접한 방향에 공기청정기가 있거나, 칸이 없으면 그 방향으로는 확산이 일어나지 않는다.
    * 확산되는 양은 Ar,c/5이고 소수점은 버린다.
    * (r, c)에 남은 미세먼지의 양은 Ar,c - (Ar,c/5)×(확산된 방향의 개수) 이다.
2. 공기청정기가 작동한다.
    * 공기청정기에서는 바람이 나온다.
    * 위쪽 공기청정기의 바람은 반시계방향으로 순환하고, 아래쪽 공기청정기의 바람은 시계방향으로 순환한다.
    * 바람이 불면 미세먼지가 바람의 방향대로 모두 한 칸씩 이동한다.
    * 공기청정기에서 부는 바람은 미세먼지가 없는 바람이고, 공기청정기로 들어간 미세먼지는 모두 정화된다.

[위에 링크타고 가면 문제 그림이 있습니다]

다음은 확산의 예시이다.

왼쪽과 오른쪽에 칸이 없기 때문에, 두 방향으로만 확산이 일어났다.

인접한 네 방향으로 모두 확산이 일어난다.

공기청정기가 있는 칸으로는 확산이 일어나지 않는다.

공기청정기의 바람은 다음과 같은 방향으로 순환한다.

방의 정보가 주어졌을 때, T초가 지난 후 구사과의 방에 남아있는 미세먼지의 양을 구해보자.

### 입력

첫째 줄에 R, C, T (6 ≤ R, C ≤ 50, 1 ≤ T ≤ 1,000) 가 주어진다.

둘째 줄부터 R개의 줄에 Ar,c (-1 ≤ Ar,c ≤ 1,000)가 주어진다. 공기청정기가 설치된 곳은 Ar,c가 -1이고, 나머지 값은 미세먼지의 양이다. -1은 2번 위아래로 붙어져 있고, 가장 윗 행, 아랫 행과 두 칸이상 떨어져 있다.

### 출력

첫째 줄에 T초가 지난 후 구사과 방에 남아있는 미세먼지의 양을 출력한다.

### 자료구조 & 알고리즘

문제에 주어진 조건에 맞춰 구현하면 되는 문제이다. 구현 / 시뮬레이션
조건을 확실하게 파악하고 제대로 한번에 구현을 해야한다. 자칫 30분이면 풀 수 있는 문제를 2시간을 붙잡고 있을 수 있다.

### 구현해야할 조건

> 미세먼지가 확신된다. 확산은 미세먼지가 있는 모든 칸에서 동시에 일어난다.


```c++

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0}; // down, up, right, left

  for(int i = 0; i < R; i++)
		{
			for(int j = 0; j < C; j++)
			{
				house2[i][j] = house[i][j]; // house2 배열을 하나 더 열어서 값을 따로 저장을 해둔다.
				
				if(house[i][j] >= 1)
					q.push({i,j});
			}
		}
		
		while(!q.empty())
		{
			int y = q.front().first;
			int x = q.front().second;
			q.pop();
			
			int temp = house2[y][x] / 5; // house2는 고정되어있는 값이기 때문에 변화는 house에서만 해준다. 확산되는 양은 A(r,c)/5 이고 소수점은 버린다.
			
			for(int i = 0; i < 4; i++) // (r,c)에 있는 미세먼지는 인접한 네 방향으로 확산된다.
			{
				int nx = x + dx[i];
				int ny = y + dy[i];
				
				if(0 <= nx && nx < C && 0 <= ny && ny < R) // 칸이 없으면 그 방향으로는 확산이 일어나지 않는다.
				{
					if(house2[ny][nx] >= 0) // 인접한 방향에 공기청정기가 있거나 공기 청정기는 -1로 표현되어있다.
					{
						house[ny][nx] += temp;
						house[y][x] -= temp; // (r,c)에 남은 미세먼지의 양은 A(r,c) - A(r,c/5)*확산된 방향의 개수이다. 고정되어있는 값인 house에 쓴다.
					}
				}
			}
		}

```

> 공기 청정기 위쪽은 반시계방향으로 순환한다.

```c++
int cw[4] = {2,0,3,1}; // 시계방향 
int ccw[4] = {2,1,3,0}; // 반시계방 

  for(int i = 0; i < 4; i++)
		{
			while(1)
			{
				int ny = y + dy[ccw[i]];
				int nx = x + dx[ccw[i]]; // 바람이 불면 미세먼지가 바람의 방향대로 모두 한 칸씩 이동한다.
				
				if(ny == cleaned_Y1 && nx == cleaned_X1) // 공기청정기에 도착하면 while 탈출
					break;
				if(!(0 <= nx && nx < C && 0 <= ny && ny < R)) // 범위를 벗어나면 while을 탈출
					break;
				
				house[ny][nx] = house2[y][x]; // 이제까지 house2에 저장되어 있는 애들을 house에 다시 입력 위에서 다시 고정값으로 사용해야하기 때문에
				y = ny;
				x = nx;
        // 복사된 방법이 싫다면, 그대로 돌려주어도 되는데 생각을 반대로 해야한다.
			}
		}
		

```

> 공기 청정기 아래쪽은 시계방향으로 순환한다.

```c++
int cw[4] = {2,0,3,1}; // 시계방향 
int ccw[4] = {2,1,3,0}; // 반시계방 


  for(int i = 0; i < 4; i++)
		{
			while(1)
			{
				int ny = y + dy[cw[i]];
				int nx = x + dx[cw[i]]; // ccw, cw의 이동하는 순서를 보면 된다.
				
				if(ny == cleaned_Y2 && nx == cleaned_X2)
					break;
				if(!(0 <= nx && nx < C && 0 <= ny && ny < R))
					break;
				
				house[ny][nx] = house2[y][x];
				y = ny;
				x = nx;
			}
		}
		//위의 설명과 같다
```

### 전체 코드


```c++

#include<iostream>
#include<queue>
#include<cstring> 

#define MAX 51
using namespace std;

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0};

int R,C,T;
int house[MAX][MAX];
int house2[MAX][MAX];

int cw[4] = {2,0,3,1}; // 시계방향 
int ccw[4] = {2,1,3,0}; // 반시계방 

int main()
{
	cin >> R >> C >> T;
	
	int cleaned_Y1 = -1;
	int cleaned_X1 = -1; // first
	
	int cleaned_Y2 = -1;
	int cleaned_X2 = -1; // second;
	
	for(int i = 0; i < R; i++)
	{
		for(int j = 0; j < C; j++)
		{
			cin >> house[i][j];
			
			if(house[i][j] == -1)
			{
				if(cleaned_Y1 == -1)
				{
					cleaned_Y1 = i;
					cleaned_X1 = j;	
				}
				else
				{
					cleaned_Y2 = i;
					cleaned_X2 = j;
				}
			}
		}
	}
	
	while(T--)
	{
		memset(house2, 0, sizeof(house2));
		queue<pair<int,int> > q;
		
		for(int i = 0; i < R; i++)
		{
			for(int j = 0; j < C; j++)
			{
				house2[i][j] = house[i][j];
				
				if(house[i][j] >= 1)
					q.push({i,j});
			}
		}
		
		while(!q.empty())
		{
			int y = q.front().first;
			int x = q.front().second;
			q.pop();
			
			int temp = house2[y][x] / 5;
			
			for(int i = 0; i < 4; i++)
			{
				int nx = x + dx[i];
				int ny = y + dy[i];
				
				if(0 <= nx && nx < C && 0 <= ny && ny < R)
				{
					if(house2[ny][nx] >= 0)
					{
						house[ny][nx] += temp;
						house[y][x] -= temp;
					}
				}
			}
		}
		
		for(int i = 0; i < R; i++)
			for(int j = 0; j < C; j++)
				house2[i][j] = house[i][j];
		
		int y = cleaned_Y1;
		int x = cleaned_X1 + 1;
		house[y][x] = 0;
		
		for(int i = 0; i < 4; i++)
		{
			while(1)
			{
				int ny = y + dy[ccw[i]];
				int nx = x + dx[ccw[i]];
				
				if(ny == cleaned_Y1 && nx == cleaned_X1)
					break;
				if(!(0 <= nx && nx < C && 0 <= ny && ny < R))
					break;
				
				house[ny][nx] = house2[y][x];
				y = ny;
				x = nx;
			}
		}
		
		y = cleaned_Y2;
		x = cleaned_X2 + 1;
		house[y][x] = 0;
		
		for(int i = 0; i < 4; i++)
		{
			while(1)
			{
				int ny = y + dy[cw[i]];
				int nx = x + dx[cw[i]];
				
				if(ny == cleaned_Y2 && nx == cleaned_X2)
					break;
				if(!(0 <= nx && nx < C && 0 <= ny && ny < R))
					break;
				
				house[ny][nx] = house2[y][x];
				y = ny;
				x = nx;
			}
		}	
	}
	
	int ans = 0;
	
	for(int i = 0; i < R; i++)
	{
		for(int j = 0; j < C; j++)
		{
			if(house[i][j] >= 1)
				ans += house[i][j];
		}
	}
	
	cout << ans << "\n";
	
	return 0;
}


```

### 잡기술 1

house를 하나 더 만들어서 메모리를 사용해서 복사하는 방법이 있다. house2는 고정되어있는 값으로 바꾸고, house에 바뀐 값들을 넣는 것이다.
헷갈릴 수도 있으니 항상 염두해 두고 문제를 해결해야한다. 미세먼지 이동시에도 항상 염두해 두어야 한다.


