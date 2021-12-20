# 백준 스타트 택시 19238

[스타트 택시](https://www.acmicpc.net/problem/19238)

## 문제

스타트링크가 "스타트 택시"라는 이름의 택시 사업을 시작했다. 스타트 택시는 특이하게도 손님을 도착지로 데려다줄 때마다 연료가 충전되고, 연료가 바닥나면 그 날의 업무가 끝난다.

택시 기사 최백준은 오늘 M명의 승객을 태우는 것이 목표이다. 백준이 활동할 영역은 N×N 크기의 격자로 나타낼 수 있고, 각 칸은 비어 있거나 벽이 놓여 있다. 택시가 빈칸에 있을 때, 상하좌우로 인접한 빈칸 중 하나로 이동할 수 있다. 알고리즘 경력이 많은 백준은 특정 위치로 이동할 때 항상 최단경로로만 이동한다.

M명의 승객은 빈칸 중 하나에 서 있으며, 다른 빈칸 중 하나로 이동하려고 한다. 여러 승객이 같이 탑승하는 경우는 없다. 따라서 백준은 한 승객을 태워 목적지로 이동시키는 일을 M번 반복해야 한다. 각 승객은 스스로 움직이지 않으며, 출발지에서만 택시에 탈 수 있고, 목적지에서만 택시에서 내릴 수 있다.

백준이 태울 승객을 고를 때는 현재 위치에서 최단거리가 가장 짧은 승객을 고른다. 그런 승객이 여러 명이면 그중 행 번호가 가장 작은 승객을, 그런 승객도 여러 명이면 그중 열 번호가 가장 작은 승객을 고른다. 택시와 승객이 같은 위치에 서 있으면 그 승객까지의 최단거리는 0이다. 연료는 한 칸 이동할 때마다 1만큼 소모된다. 한 승객을 목적지로 성공적으로 이동시키면, 그 승객을 태워 이동하면서 소모한 연료 양의 두 배가 충전된다. 이동하는 도중에 연료가 바닥나면 이동에 실패하고, 그 날의 업무가 끝난다. 승객을 목적지로 이동시킨 동시에 연료가 바닥나는 경우는 실패한 것으로 간주하지 않는다.

그림 생략

...

모든 승객을 성공적으로 데려다줄 수 있는지 알아내고, 데려다줄 수 있을 경우 최종적으로 남는 연료의 양을 출력하는 프로그램을 작성하시오.

## 입력

첫 줄에 N, M, 그리고 초기 연료의 양이 주어진다. (2 ≤ N ≤ 20, 1 ≤ M ≤ N2, 1 ≤ 초기 연료 ≤ 500,000) 연료는 무한히 많이 담을 수 있기 때문에, 초기 연료의 양을 넘어서 충전될 수도 있다.

다음 줄부터 N개의 줄에 걸쳐 백준이 활동할 영역의 지도가 주어진다. 0은 빈칸, 1은 벽을 나타낸다.

다음 줄에는 백준이 운전을 시작하는 칸의 행 번호와 열 번호가 주어진다. 행과 열 번호는 1 이상 N 이하의 자연수이고, 운전을 시작하는 칸은 빈칸이다.

그다음 줄부터 M개의 줄에 걸쳐 각 승객의 출발지의 행과 열 번호, 그리고 목적지의 행과 열 번호가 주어진다. 모든 출발지와 목적지는 빈칸이고, 모든 출발지는 서로 다르며, 각 손님의 출발지와 목적지는 다르다.

## 출력

모든 손님을 이동시키고 연료를 충전했을 때 남은 연료의 양을 출력한다. 단, 이동 도중에 연료가 바닥나서 다음 출발지나 목적지로 이동할 수 없으면 -1을 출력한다. 모든 손님을 이동시킬 수 없는 경우에도 -1을 출력한다.

## 전체 코드

```c++
#include<iostream>
#include<vector>
#include<queue>
#include<algorithm>
#include<cstring>

#define MAX 25
using namespace std;

const int dx[4] = {0,0,1,-1};
const int dy[4] = {1,-1,0,0};

struct Customer{
	int x;
	int y;
	int Destination_x;
	int Destination_y;
};

struct info
{
	int x;
	int y;
	int dist;
	int num;
};

int n,m,fuel;
int Taxi_x, Taxi_y;
int map[MAX][MAX];
bool visited[MAX][MAX];
Customer customer[MAX*MAX];

bool comp(info a, info b)
{
	if(a.dist == b.dist) // 거리가 같다면 
	{
		if(a.y == b.y) // 행의 번호가 가장 작은 
		{
			return a.x < b.x; // 행의 번호가 같다면, 열의 번호가 가장 작은 
		}
		return a.y < b.y; // 행의 번호가 작은 
	}
	return a.dist < b.dist; // 거리가 짧은 
}

bool destination(int y, int x, int d_y, int d_x, int num)
{
	memset(visited,false,sizeof(visited));
	queue<pair<pair<int,int>, pair<int,int> > > q;
	q.push({{y,x},{0,fuel}});
	visited[y][x] = true;
	
	while(!q.empty())
	{
		int y = q.front().first.first;
		int x = q.front().first.second;
		int dist = q.front().second.first;
		int left_fuel = q.front().second.second;
		q.pop();
		
		
		if(y == d_y && x == d_x)
		{
			fuel = fuel - dist;
			fuel = fuel + (dist * 2);
			Taxi_x = x;
			Taxi_y = y;
			return true;
		}
		
		if(left_fuel == 0) // 이동했을때 left_fuel이 도착지가 아니고, 다른 곳에서 0이면 continue 
			continue; // 이부분에서 바로 return false를 넣었더니 나머지 가능성 있는 것들을 다 배제 시켜서  답이 나오지 않음 

			
		for(int i = 0; i < 4; i++)
		{
			int nx = x + dx[i];
			int ny = y + dy[i];

			if(0 <= nx && nx < n && 0 <= ny && ny < n)
			{
				if(map[ny][nx] != -1 && !visited[ny][nx])
				{
					visited[ny][nx] = true;
					q.push({{ny,nx},{dist+1,left_fuel-1}});
				}
			}
		}
	}
	return false;
}


int shortest_customer()
{
	memset(visited, false, sizeof(visited));
	queue<pair<pair<int,int>, pair<int,int> > > q;
	q.push({{Taxi_y,Taxi_x},{0,fuel}});
	visited[Taxi_y][Taxi_x] = true; // 택시의 시작 위치 
	vector<info> v; 
	
	while(!q.empty())
	{
		int y = q.front().first.first;
		int x = q.front().first.second;
		int dist = q.front().second.first;
		int left_fuel = q.front().second.second;
		q.pop();
		
		if(map[y][x] >= 1 && visited[y][x]) // 승객있는 위치 
		{
			info I;
			I.y = y;
			I.x = x;
			I.dist = dist;
			I.num = map[y][x];
			v.push_back(I);
		}
			
		if(left_fuel == 0) // 이동 중에 left_feul이 없으면,  
			continue;
			
		for(int i = 0; i < 4; i++)
		{
			int nx = x + dx[i];
			int ny = y + dy[i];
			
			if(0 <= nx && nx < n && 0 <= ny && ny < n)
			{
				if(map[ny][nx] != -1 && !visited[ny][nx])
				{
					visited[ny][nx] = true;
					q.push({{ny,nx},{dist+1, left_fuel-1}});
				}
			}
		}
	}
	
	if(v.size() == 0) // 승객이 아무도 없으면 
		return -1;

	sort(v.begin(),v.end(),comp);
	
	map[v[0].y][v[0].x] = 0;
	fuel = fuel - v[0].dist;

	return v[0].num;
}

int main()
{
	cin >> n >> m >> fuel;
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < n; j++)
		{
			cin >> map[i][j];
			
			if(map[i][j] == 1)
				map[i][j] = -1;
		}
	}
	
	cin >> Taxi_y >> Taxi_x;
	
	Taxi_x--;
	Taxi_y--;
	
	for(int i = 1; i <= m; i++) // m명의 승객을 태우는 것이 목표이다. 
	{
		int y,x,d_y,d_x;
		
		cin >> y >> x >> d_y >> d_x;
		y--;
		x--;
		d_y--;
		d_x--;
		
		customer[i] = {x,y,d_x,d_y}; // i 번 ~  
		map[y][x] = i;
	}
	
	for(int i = 0; i < m; i++)
	{
		int num = shortest_customer(); // 현재 위치에서 최단거리가 가장 짧은 승객을 고른다. 
		if(num == -1)
		{
			cout << -1 << "\n";
			return 0;
		}
		
		bool move = destination(customer[num].y, customer[num].x, customer[num].Destination_y, customer[num].Destination_x, num);
		if(move == false)
		{
			cout << -1 << "\n";
			return 0;
		}
	}
	
	cout << fuel << "\n";
	
	return 0;
}

```

## 생각해야할 점

문제를 정확히 읽고 헛점이 있는지 제대로 파악을 하자.
