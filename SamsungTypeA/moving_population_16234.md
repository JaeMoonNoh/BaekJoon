# 백준 인구이동 16234
[인구이동](https://www.acmicpc.net/problem/16234)


### 문제

N×N크기의 땅이 있고, 땅은 1×1개의 칸으로 나누어져 있다. 각각의 땅에는 나라가 하나씩 존재하며, r행 c열에 있는 나라에는 A[r][c]명이 살고 있다. 인접한 나라 사이에는 국경선이 존재한다. 모든 나라는 1×1 크기이기 때문에, 모든 국경선은 정사각형 형태이다.

오늘부터 인구 이동이 시작되는 날이다.

인구 이동은 하루 동안 다음과 같이 진행되고, 더 이상 아래 방법에 의해 인구 이동이 없을 때까지 지속된다.

* 국경선을 공유하는 두 나라의 인구 차이가 L명 이상, R명 이하라면, 두 나라가 공유하는 국경선을 오늘 하루 동안 연다.
* 위의 조건에 의해 열어야하는 국경선이 모두 열렸다면, 인구 이동을 시작한다.
* 국경선이 열려있어 인접한 칸만을 이용해 이동할 수 있으면, 그 나라를 오늘 하루 동안은 연합이라고 한다.
* 연합을 이루고 있는 각 칸의 인구수는 (연합의 인구수) / (연합을 이루고 있는 칸의 개수)가 된다. 편의상 소수점은 버린다.
* 연합을 해체하고, 모든 국경선을 닫는다.

각 나라의 인구수가 주어졌을 때, 인구 이동이 며칠 동안 발생하는지 구하는 프로그램을 작성하시오.

### 입력

첫째 줄에 N, L, R이 주어진다. (1 ≤ N ≤ 50, 1 ≤ L ≤ R ≤ 100)

둘째 줄부터 N개의 줄에 각 나라의 인구수가 주어진다. r행 c열에 주어지는 정수는 A[r][c]의 값이다. (0 ≤ A[r][c] ≤ 100)

인구 이동이 발생하는 일수가 2,000번 보다 작거나 같은 입력만 주어진다.


### 출력

인구 이동이 며칠 동안 발생하는지 첫째 줄에 출력한다.

### 알고리즘 & 자료구조

국가의 인구가 이동할 수 있는 조건은 L <= x <= R이고, 그리고 범위 안에 존재하면 된다.
국가에서 이동할수 있는 값을 (0,0) ~ (N,N) 까지 차례대로 구해주면 된다. 그리고 각 그리드 개수마다 국가를 +1 씩 올려주면 된다.

DFS를 사용한 완탐으로 풀면 된다. N의 값이 50이하이기 때문에 시간복잡도상 초과가 나지 않는다.

### 구현해야할 조건

> 국경선을 공유하는 두 나라의 인구 차이가 L명 이상, R명 이하라면, 두 나라가 공유하는 국경선을 오늘 하루 동안 연다.

```c++
for(int i = 0; i < n; i++)
		{
			for(int j = 0; j < n; j++)
			{
				if(visited[i][j])
					continue;
				
				num = 1; // 나라의 개수(처음 시작이기 때문에 1로 시작)
				visited[i][j] = true; // 방문을 했다는 것 표시
				population = country[i][j]; // 인구수도 처음 시작하는 나라의 인구수로 시작을 한다.
				c.clear();
				c.push_back({i,j}); // vector<pair<int,int> > c 인데 나라의 위치가 i,j로 표기되기 때문에
				
				DFS(i,j); // DFS로 이동 시작을 한다.
				.
        .
        .
			}
```

> 위의 조건에 의해 열어야하는 국경선이 모두 열렸다면, 인구 이동을 시작한다.

```c++
void DFS(int y, int x)
{
	int temp = country[y][x];
	visited[y][x] = true;
	
	for(int i = 0; i < 4; i++)
	{
		int nx = x + dx[i];
		int ny = y + dy[i];

		if(0 <= nx && nx < n && 0 <= ny && ny < n)
		{
			int diff = abs(temp - country[ny][nx]); // 절댓값 

			if(l <= diff && diff <= r && !visited[ny][nx]) // 국가간의 차이가 L <= x <= R이라면, 그리고 방문하지 않은 국가라면
			{
				c.push_back({ny,nx}); // 국경을 연 나라를 vector에 push_back 해주면 된다.
				population += country[ny][nx]; // 나라의 인구를 더하고
				num++; // 국경을 연 나라의 개수를 세고
				DFS(ny,nx); // 다시 돌린다.
			} 
		}
	}
}
```

> 국경선이 열려있어 인접한 칸만을 이용해 이동할 수 있으면, 그 나라를 오늘 하루 동안은 연합이라고 한다.

```c++

```

> 연합을 이루고 있는 각 칸의 인구수는(연합의 인구수)/(연합을 이루고 있는 칸의 개수)가 된다. 편의상 소수점은 버린다.

```c++

if(num >= 2) // 국경을 연 나라가 두 나라 이상이면 인구 이동이 가능하다.
{
	chk = true; // 이동을 했으므로 체크!
	for(int k = 0; k < c.size(); k++)
	{	
		country[c[k].first][c[k].second] = population / num;
	}
}

```

> 연합을 해제하고 모든 국경선을 닫는다.

```c++
 // (i,j)에서 시작한 DFS가 종료가 된다.
```

> 각 나라의 인구수가 주어졌을 때, 인구 이동이 며칠 동안 발생하는지 구하는 프로그램을 만들어라.

```c++
	while(1)
	{
		memset(visited,false,sizeof(visited)); // 계속해서 돌려야하기 때문에 memset을 이용해 초기화 시킨다.
		bool chk = false;
	  .
    .
    .
    // 해당하는 조건이 있다면 L <= X <= R 있다면 계속해서 완탐을 해주어야 한다.
	}
```

### 전체 코드


```c++

#include<iostream>
#include<algorithm>
#include<cstring> 
#include<vector>

#define MAX 51
using namespace std;

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0};

int n,l,r; // country size, l <= x <= r
int country[MAX][MAX];
bool visited[MAX][MAX];
int population,num;
vector<pair<int,int> > c;

void DFS(int y, int x)
{
	int temp = country[y][x];
	visited[y][x] = true;
	
	for(int i = 0; i < 4; i++)
	{
		int nx = x + dx[i];
		int ny = y + dy[i];

		if(0 <= nx && nx < n && 0 <= ny && ny < n)
		{
			int diff = abs(temp - country[ny][nx]); // 절댓값 

			if(l <= diff && diff <= r && !visited[ny][nx])
			{
				c.push_back({ny,nx});
				population += country[ny][nx];
				num++;
				DFS(ny,nx);
			} 
		}
	}
}

int main()
{
	cin >> n >> l >> r;
	
	for(int i = 0; i < n; i++)
		for(int j = 0; j < n; j++)
			cin >> country[i][j];
		
	int day = 0;
		
	while(1)
	{
		memset(visited,false,sizeof(visited));
		bool chk = false;
		
		for(int i = 0; i < n; i++)
		{
			for(int j = 0; j < n; j++)
			{
				if(visited[i][j])
					continue;
				
				num = 1;
				visited[i][j] = true;
				population = country[i][j];
				c.clear();
				c.push_back({i,j});
				
				DFS(i,j);
				
				if(num >= 2)
				{
					chk = true;
					for(int k = 0; k < c.size(); k++)
					{	
						country[c[k].first][c[k].second] = population / num;
					}
				}
			}
		}
		
		if(chk)
			day++;	
		else
			break;
	}
	
	cout << day << "\n";
	
//	
//	for(int i = 0; i < n; i++)
//	{
//		for(int j = 0; j < n; j++)
//		{
//			cout << country[i][j] << " ";
//		}
//		cout << endl;
//	}	
	
	return 0;
}

```

### 잡기술1

DFS를 사용한다고 생각이 들면, 거의 대부분 DFS를 한 위치에서 몇번 돌리는가에 대한 문제가 다분히 많이 나온다.
문제를 상세히 읽으면서 DFS의 냄새를 맡으면 조건을 제대로 파악을 하자.
