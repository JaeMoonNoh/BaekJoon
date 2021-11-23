# 백준 감시 15683
[감시](https://www.acmicpc.net/problem/15683)

### 문제
스타트링크의 사무실은 1×1크기의 정사각형으로 나누어져 있는 N×M 크기의 직사각형으로 나타낼 수 있다. 사무실에는 총 K개의 CCTV가 설치되어져 있는데, CCTV는 5가지 종류가 있다. 각 CCTV가 감시할 수 있는 방법은 다음과 같다.

				
1번	2번	3번	4번	5번
1번 CCTV는 한 쪽 방향만 감시할 수 있다. 2번과 3번은 두 방향을 감시할 수 있는데, 2번은 감시하는 방향이 서로 반대방향이어야 하고, 3번은 직각 방향이어야 한다. 4번은 세 방향, 5번은 네 방향을 감시할 수 있다.

CCTV는 감시할 수 있는 방향에 있는 칸 전체를 감시할 수 있다. 사무실에는 벽이 있는데, CCTV는 벽을 통과할 수 없다. CCTV가 감시할 수 없는 영역은 사각지대라고 한다.

CCTV는 회전시킬 수 있는데, 회전은 항상 90도 방향으로 해야 하며, 감시하려고 하는 방향이 가로 또는 세로 방향이어야 한다.

```c++
0 0 0 0 0 0
0 0 0 0 0 0
0 0 1 0 6 0
0 0 0 0 0 0
```

지도에서 0은 빈 칸, 6은 벽, 1~5는 CCTV의 번호이다. 위의 예시에서 1번의 방향에 따라 감시할 수 있는 영역을 '#'로 나타내면 아래와 같다.

```c++
0 0 0 0 0 0
0 0 0 0 0 0
0 0 1 # 6 0
0 0 0 0 0 0
// right : →
```

```c++
0 0 0 0 0 0
0 0 0 0 0 0
# # 1 0 6 0
0 0 0 0 0 0

// left : ←	
```

```c++
0 0 # 0 0 0
0 0 # 0 0 0
0 0 1 0 6 0
0 0 0 0 0 0

// up : ↑		
```

```c++
0 0 0 0 0 0
0 0 0 0 0 0
0 0 1 0 6 0
0 0 # 0 0 0

// down : ↓
```

CCTV는 벽을 통과할 수 없기 때문에, 1번이 → 방향을 감시하고 있을 때는 6의 오른쪽에 있는 칸을 감시할 수 없다.

```c++
0 0 0 0 0 0
0 2 0 0 0 0
0 0 0 0 6 0
0 6 0 0 2 0
0 0 0 0 0 0
0 0 0 0 0 5
```

위의 예시에서 감시할 수 있는 방향을 알아보면 아래와 같다.
```c++
0 0 0 0 0 #
# 2 # # # #
0 0 0 0 6 #
0 6 # # 2 #
0 0 0 0 0 #
# # # # # 5

// 왼쪽 상단 2: ↔, 오른쪽 하단 2: ↔
```
```c++
0 0 0 0 0 #
# 2 # # # #
0 0 0 0 6 #
0 6 0 0 2 #
0 0 0 0 # #
# # # # # 5

// 왼쪽 상단 2: ↔, 오른쪽 하단 2: ↕
```
```c++
0 # 0 0 0 #
0 2 0 0 0 #
0 # 0 0 6 #
0 6 # # 2 #
0 0 0 0 0 #
# # # # # 5

// 왼쪽 상단 2: ↕, 오른쪽 하단 2: ↔
```
```c++
0 # 0 0 0 #
0 2 0 0 0 #
0 # 0 0 6 #
0 6 0 0 2 #
0 0 0 0 # #
# # # # # 5

// 왼쪽 상단 2: ↕, 오른쪽 하단 2: ↕
```
CCTV는 CCTV를 통과할 수 있다. 아래 예시를 보자.

```c++
0 0 2 0 3
0 6 0 0 0
0 0 6 6 0
0 0 0 0 0
```
위와 같은 경우에 2의 방향이 ↕ 3의 방향이 ←와 ↓인 경우 감시받는 영역은 다음과 같다.

```c++
# # 2 # 3
0 6 # 0 #
0 0 6 6 #
0 0 0 0 #
```

사무실의 크기와 상태, 그리고 CCTV의 정보가 주어졌을 때, CCTV의 방향을 적절히 정해서, 사각 지대의 최소 크기를 구하는 프로그램을 작성하시오.


### 입력

첫째 줄에 사무실의 세로 크기 N과 가로 크기 M이 주어진다. (1 ≤ N, M ≤ 8)

둘째 줄부터 N개의 줄에는 사무실 각 칸의 정보가 주어진다. 0은 빈 칸, 6은 벽, 1~5는 CCTV를 나타내고, 문제에서 설명한 CCTV의 종류이다. 

CCTV의 최대 개수는 8개를 넘지 않는다.

### 출력

첫째 줄에 사각 지대의 최소 크기를 출력한다.

### 알고리즘 자료구조

각 방향을 설정할 3차원 배열, 경우의 수에 따라서 바꿔야하는 카메라의 각도 backTracking 이다.


### 구현해야할 조건

> 1번 CCTV : right, 2번 CCTV : right, left , 3번 CCTV : up, right , 4번 CCTV : right, left, up , 5번 CCTV : 4 direciton

```c++

bool visited[MAX][MAX][4];
// 3차원 배열을 사용해서 90도씩 4방향을 체크 해주는 것입니다.

const int dx[4] = {1,0,-1,0};
const int dy[4] = {0,-1,0,1}; // right,up,left,down

// CCTV 2번을 위해서 right, up, left, down으로 해야한다. 궁금하면 right,left,down,up으로 설정해보면 된다.
	
switch(room[i][j]) // 잡기술2 
{
				case 1:
					
					visited[i][j][0] = true;
					// right
					
					break;
					
				
				case 2:
					
					visited[i][j][0] = true;
					visited[i][j][2] = true;
					//right, left
					break;
					
				case 3:
					
					visited[i][j][1] = true;
					visited[i][j][0] = true;
					//right, up
					break;
					
				case 4:
					
					visited[i][j][0] = true;
					visited[i][j][1] = true;
					visited[i][j][2] = true;
					//right, left, up
					break;
					
					
				
				case 5:
					
					visited[i][j][0] = true;
					visited[i][j][1] = true;
					visited[i][j][3] = true;
					visited[i][j][2] = true;
					//right, up, down, left;
					break;
}
```

> CCTV는 회전시킬 수 있는데, 회전은 항상 90도 방향으로 해야한다.

```c++

	if(cnt == cctv.size()) // cctv 갯수 
	{

		for(int i = 0; i < angle.size(); i++)
		{
			int y = cctv[i].first;
			int x = cctv[i].second;
			// CCTV 5번은 돌리지 않아도 되니까 다르게 코드를 작성가능하지만 그렇게 짜기엔 너무 길어짐, n이 적기 때문에 사용 가능 문제없음.
			for(int j = 0; j < 4; j++) // 4방향중 90도씩 한번씩 돌리기 위함.
			{
				if(visited[y][x][j]) // 바라보는 방향으로 설정된 곳 cctv에 따라서, 3차원 배열 사용
				{
					int ny = y + dy[(angle[i] + j) % 4]; 1칸씩 옮기면 
					int nx = x + dx[(angle[i] + j) % 4]; // angle 90도씩 돌렸는지 나머지를 체크하면 된다.
					
					while(1)
					{
						if(copyRoom[ny][nx] == 6)
							break;
						if(!(0 <= ny && ny < n && 0 <= nx && nx < m))
							break;
						
						if(copyRoom[ny][nx] == 0)
							copyRoom[ny][nx] = -1;
							
						ny += dy[(angle[i] + j) % 4];
						nx += dx[(angle[i] + j) % 4]; // dy,dx 이동 
					}
				}
			}
		}


for(int i = 0; i < 4; i++)
{
	angle.push_back(i);
	DFS(cnt + 1);
	angle.pop_back();
}

```


> CCTV의 방향을 적절히 정해서, 사각 지대의 최소 크기를 구하자

```c++

int chk(void)
{
	int ans = 0;
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < m; j++)
		{
			if(copyRoom[i][j] == 0)
				ans++;
		}
	}
	return ans;
}

```

### 전체 코드

```c++

#include<iostream>
#include<vector>
#include<algorithm>

#define MAX 8
#define INF 987654321

using namespace std;

const int dx[4] = {1,0,-1,0};
const int dy[4] = {0,-1,0,1}; // right,up,left,down

int n,m;
int result = INF;
int room[MAX][MAX], copyRoom[MAX][MAX];
bool visited[MAX][MAX][4];
vector<pair<int,int> > cctv;
vector<int> angle;

void copy(void)
{
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < m; j++)
		{
			copyRoom[i][j] = room[i][j];
		}
	}
	
}

int chk(void)
{
	int ans = 0;
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < m; j++)
		{
			if(copyRoom[i][j] == 0)
				ans++;
		}
	}
	return ans;
}

void DFS(int cnt)
{
	if(cnt == cctv.size()) // cctv 갯수 
	{

		for(int i = 0; i < angle.size(); i++)
		{
			int y = cctv[i].first;
			int x = cctv[i].second;
			
			for(int j = 0; j < 4; j++) // 4방향중 
			{
				if(visited[y][x][j]) // 바라보는 방향으로 설정된 곳 cctv에 따라서 
				{
					int ny = y + dy[(angle[i] + j) % 4];
					int nx = x + dx[(angle[i] + j) % 4]; // angle 90도씩 돌렸는지 
					
					while(1)
					{
						if(copyRoom[ny][nx] == 6)
							break;
						if(!(0 <= ny && ny < n && 0 <= nx && nx < m))
							break;
						
						if(copyRoom[ny][nx] == 0)
							copyRoom[ny][nx] = -1;
							
						ny += dy[(angle[i] + j) % 4];
						nx += dx[(angle[i] + j) % 4]; // dy,dx 이동 
					}
				}
			}
		}
		
		result = min(result, chk());
//		
//		cout << endl;
//		cout << result << endl;
//		cout << "=======================================" << endl;
//		
//		for(int i = 0; i < n; i++)
//		{
//			for(int j = 0; j < m; j++)
//			{
//				cout << copyRoom[i][j] << " ";
//			}
//			cout << endl;
//		}
//		
		copy();
		return;
		
	}
	
	for(int i = 0; i < 4; i++)
	{
		angle.push_back(i);
		DFS(cnt + 1);
		angle.pop_back();
	}
}

int main()
{
	cin >> n >> m;
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < m; j++)
		{
			cin >> room[i][j];
			
			if(1 <= room[i][j] && room[i][j] <= 5)
				cctv.push_back({i,j});
				
			switch(room[i][j]) // 잡기술2 
			{
				case 1:
					
					visited[i][j][0] = true;
					// right
					
					break;
					
				
				case 2:
					
					visited[i][j][0] = true;
					visited[i][j][2] = true;
					//right, left
					break;
					
				case 3:
					
					visited[i][j][1] = true;
					visited[i][j][0] = true;
					//right, up
					break;
					
				case 4:
					
					visited[i][j][0] = true;
					visited[i][j][1] = true;
					visited[i][j][2] = true;
					//right, left, up
					break;
					
					
				
				case 5:
					
					visited[i][j][0] = true;
					visited[i][j][1] = true;
					visited[i][j][3] = true;
					visited[i][j][2] = true;
					//right, up, down, left;
					break;
			}
			
		}
	}
	
	result = INF;
	copy();
	DFS(0);
	
	cout << result << "\n";
	
	return 0;
}

```

### 잡기술1

이렇게 항상 써왔는데, 90도를 꺾는다 뭐한다 하면 4방향의 순서도 중요하게 체크해야된다.
위에 90도 방향으로 꺾는 2번 CCTV로 right up left down 순서대로 적어야 통과가 가능하다.

```c++
const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0}; // down, up, right, left

```

### 잡기술 2

90도 회전한다고 쓰여져 있을때 대체 어떻게 90도를 돌릴수 있을까. 모든 값을 저장해야 할까 생각을했다.
물론 모든 값을 적어도 될것이다 왜냐면 카메라는 단 5대이고 n의 값도 작기 때문이다.
하지만 이렇게 푼다면 그냥 손가락으로 세는 것이 낫지 않을까. 다만 카메라도 최대 8개 이기 때문에 수많은 경우의 수를 세기에는 부담이 있다. 

각설하고 90도를 돌리면서 방문하는 방법은 3차원 배열을 이용해 해당 방향을 저장하고(right,up,down,left)
해당 방향으로 이동이 가능하다면 이동하면 된다. 

그리고 %4를 이용해 4방향을 체크해준다.

```c++

for(int j = 0; j < 4; j++) // 4방향중 right,left,up,down
{
	if(visited[y][x][j]) // 바라보는 방향으로 설정된 곳
	{
		int ny = y + dy[(angle[i] + j) % 4];
		int nx = x + dx[(angle[i] + j) % 4]; // angle 90도씩 돌렸는지 angle(1~4) 
		// 0(right) + 1 % 4 = 1 (up) .. 이런 방식		
		while(1)
		{
			if(copyRoom[ny][nx] == 6)
				break;
			if(!(0 <= ny && ny < n && 0 <= nx && nx < m))
				break;
						
			if(copyRoom[ny][nx] == 0)
				copyRoom[ny][nx] = -1;
							
			ny += dy[(angle[i] + j) % 4];
			nx += dx[(angle[i] + j) % 4]; // dy,dx 이동 
		}
	}
}


```


