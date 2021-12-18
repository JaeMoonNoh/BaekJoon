# 백준 마법사 상어와 복제 23290

## 문제
마법사 상어는 파이어볼, 토네이도, 파이어스톰, 물복사버그, 비바라기, 블리자드 마법을 할 수 있다. 오늘은 기존에 배운 물복사버그 마법의 상위 마법인 복제를 배웠고, 4 × 4 크기의 격자에서 연습하려고 한다. (r, c)는 격자의 r행 c열을 의미한다. 격자의 가장 왼쪽 윗 칸은 (1, 1)이고, 가장 오른쪽 아랫 칸은 (4, 4)이다.

격자에는 물고기 M마리가 있다. 각 물고기는 격자의 칸 하나에 들어가 있으며, 이동 방향을 가지고 있다. 이동 방향은 8가지 방향(상하좌우, 대각선) 중 하나이다. 마법사 상어도 연습을 위해 격자에 들어가있다. 상어도 격자의 한 칸에 들어가있다. 둘 이상의 물고기가 같은 칸에 있을 수도 있으며, 마법사 상어와 물고기가 같은 칸에 있을 수도 있다.

상어의 마법 연습 한 번은 다음과 같은 작업이 순차적으로 이루어진다.

  1. 상어가 모든 물고기에게 복제 마법을 시전한다. 복제 마법은 시간이 조금 걸리기 때문에, 아래 5번에서 물고기가 복제되어 칸에 나타난다.
  2. 모든 물고기가 한 칸 이동한다. 상어가 있는 칸, 물고기의 냄새가 있는 칸, 격자의 범위를 벗어나는 칸으로는 이동할 수 없다. 각 물고기는 자신이 가지고 있는 이동 방향이 이동할 수 있는 칸을 향할 때까지 방향을 45도 반시계 회전시킨다. 만약, 이동할 수 있는 칸이 없으면 이동을 하지 않는다. 그 외의 경우에는 그 칸으로 이동을 한다. 물고기의 냄새는 아래 3에서 설명한다.
  3. 상어가 연속해서 3칸 이동한다. 상어는 현재 칸에서 상하좌우로 인접한 칸으로 이동할 수 있다. 연속해서 이동하는 칸 중에 격자의 범위를 벗어나는 칸이 있으면, 그 방법은 불가능한 이동 방법이다. 연속해서 이동하는 중에 상어가 물고기가 있는 같은 칸으로 이동하게 된다면, 그 칸에 있는 모든 물고기는 격자에서 제외되며, 제외되는 모든 물고기는 물고기 냄새를 남긴다. 가능한 이동 방법 중에서 제외되는 물고기의 수가 가장 많은 방법으로 이동하며, 그러한 방법이 여러가지인 경우 사전 순으로 가장 앞서는 방법을 이용한다. 사전 순에 대한 문제의 하단 노트에 있다.
  4. 두 번 전 연습에서 생긴 물고기의 냄새가 격자에서 사라진다.
  5. 1에서 사용한 복제 마법이 완료된다. 모든 복제된 물고기는 1에서의 위치와 방향을 그대로 갖게 된다.

격자에 있는 물고기의 위치, 방향 정보와 상어의 위치, 그리고 연습 횟수 S가 주어진다. S번 연습을 모두 마쳤을때, 격자에 있는 물고기의 수를 구해보자.

## 입력

첫째 줄에 물고기의 수 M, 상어가 마법을 연습한 횟수 S가 주어진다. 둘째 줄부터 M개의 줄에는 물고기의 정보 fx, fy, d가 주어진다. (fx, fy)는 물고기의 위치를 의미하고, d는 방향을 의미한다. 방향은 8 이하의 자연수로 표현하고, 1부터 순서대로 ←, ↖, ↑, ↗, →, ↘, ↓, ↙ 이다. 마지막 줄에는 sx, sy가 주어지며, 상어가 (sx, sy)에 있음을 의미한다.

격자 위에 있는 물고기의 수가 항상 1,000,000 이하인 입력만 주어진다.

## 출력

S번의 연습을 마친 후 격자에 있는 물고기의 수를 출력한다.

## 노트

상어의 이동 방법 중 사전 순으로 가장 앞서는 방법을 찾으려면 먼저, 방향을 정수로 변환해야 한다. 상은 1, 좌는 2, 하는 3, 우는 4로 변환한다. 변환을 모두 마쳤으면, 수를 이어 붙여 정수로 하나 만든다. 두 방법 A와 B가 있고, 각각을 정수로 변환한 값을 a와 b라고 하자. a < b를 만족하면 A가 B보다 사전 순으로 앞선 것이다.

예를 들어, [상, 하, 좌]를 정수로 변환하면 132가 되고, [하, 우, 하]를 변환하면 343이 된다. 132 < 343이기 때문에, [상, 하, 좌]가 [하, 우, 하]보다 사전 순으로 앞선다.

총 43 = 64가지 방법을 사전 순으로 나열해보면 [상, 상, 상], [상, 상, 좌], [상, 상, 하], [상, 상, 우], [상, 좌, 상], [상, 좌, 좌], [상, 좌, 하], [상, 좌, 우], [상, 하, 상], ..., [우, 하, 하], [우, 하, 우], [우, 우, 상], [우, 우, 좌], [우, 우, 하], [우, 우, 우] 이다.

## 전체 코드

나중에 다시 설명

```c++
#include<iostream>
#include<queue> 
#include<vector>
#include<algorithm>

using namespace std;

const int dx[9] = {0,-1,-1,0,1,1,1,0,-1};
const int dy[9] = {0,0,-1,-1,-1,0,1,1,1}; // direction 정보 

const int shark_x[5] = {0,0,-1,0,1};
const int shark_y[5] = {0,-1,0,1,0}; // shark_move

int m,s;
vector<int> map[5][5]; // 물고기 여러마리가 이동이 가능하기 때문에 vector로 map을 만듦 
queue<pair<pair<int,int>,int> > copy_fish; // 처음 위치 물고기의 위치를 만듦 
vector<pair<int,int> > shark; // 상어의 위치는 움직이기전, 후로 바뀜 

vector<int> direction; // DFS 방향 정해주기 
vector<pair<string,int> > sorted_shark; // 방향에 맞춰서, 정렬된 상어 방향 vector 

int shark_location[5][5]; // 상어의 위치 
int smell[5][5]; // 냄새가 존재하는 곳 

bool comp(pair<string,int> a, pair<string,int> b)
{
	if(a.second == b.second)
	{
		return a.first < b.first;
	}
	return a.second > b.second;
} // sorted_shark 해주는 방법 사전형식 

void move_fish(void)
{
	queue<pair<pair<int,int>, int> > fish;
	
	for(int i = 1; i <= 4; i++)
	{
		for(int j = 1; j <= 4; j++)
		{
			if(1 <= map[i][j].size()) // fish가 있으면 
			{
				for(int s = 0; s < map[i][j].size(); s++)
				{
					fish.push({{i,j},map[i][j][s]}); // fish 다 집어넣고 
					copy_fish.push({{i,j},map[i][j][s]}); // 겹친 물고기도 전부 넣는 거니까. 
				}
			}
			map[i][j].clear(); // map에서 fish & copy_fish를 받았으면 지워주고, 새롭게 받아야한다. 
		}
	}
	
	while(!fish.empty())
	{
		int y = fish.front().first.first;
		int x = fish.front().first.second;
		int dir = fish.front().second;
		fish.pop();
		
		int now_dir = dir;
		
		while(1)
		{	
			int ny = y + dy[dir];
			int nx = x + dx[dir];
					
			if(smell[ny][nx] || shark_location[ny][nx] || !(1 <= ny && ny <= 4 && 1 <= nx && nx <= 4))
			{
				dir -= 1;
				
				if(dir == 0)
					dir = 8;
			}
			else
			{
				map[ny][nx].push_back(dir);
				break;
			}
			
			if(now_dir == dir)
			{
				map[y][x].push_back(now_dir);
				break;
			}
		}	// 반시계 방향으로 돌리는 알고리즘	
	}
		
}


void DFS(int cnt)
{
	if(cnt == 3) // 방향, 잡을 물고기 수 
	{
		int temp_fish[5][5];
		string str = "";
		
		for(int i = 0; i < direction.size(); i++)
		{
			str += to_string(direction[i]);
		}
		
		for(int i = 1; i <= 4; i++)
		{
			for(int j = 1; j <= 4; j++)
			{
				temp_fish[i][j] = map[i][j].size(); // temp_fish에 map의 한 블록 크기(물고기의 개수) 입력 
			}
		}
		
		bool chk = true;
		int sum = 0;
		
		for(int i = 0; i < shark.size(); i++)
		{
			int y = shark[i].first;
			int x = shark[i].second;

			for(int i = 0; i < direction.size(); i++)
			{
				int ny = y + shark_y[direction[i]];
				int nx = x + shark_x[direction[i]];
								
				if(1 <= nx && nx <= 4 && 1 <= ny && ny <= 4)
				{
					 sum += temp_fish[ny][nx];
					 y += shark_y[direction[i]];
					 x += shark_x[direction[i]];
					 temp_fish[ny][nx] = 0;
				}
				else
				{
					chk = false; // 격자를 넘어가게 되면 이 방향은 옳지 않음 
					break;
				}
			}
		}
		
		if(chk)
		{
			sorted_shark.push_back({str,sum}); // 격자 내부에 있는 애들로 해결 
		}
		
		return;
	}
	
	for(int i = 1; i < 5; i++)
	{
		direction.push_back(i);
		DFS(cnt+1); // 111 ~ 444 BackTracking
		direction.pop_back();
	}
}

void move_shark(void)
{
	sort(sorted_shark.begin(), sorted_shark.end(), comp); // comp 형식에 맞춰 정렬 
	
	for(int i = 1; i <= 4; i++)
	{
		for(int j = 1; j <= 4; j++)
		{
			if(smell[i][j])
				smell[i][j]++; // 냄새를 지우기 위한 작업 
		}
	} 
	
	int dir = stoi(sorted_shark[0].first); // string to int 
	vector<int> d;
	
	int first = dir/100;
	d.push_back(first);
	dir %= 100; // first
	
	int second = dir/10;
	d.push_back(second);
	dir %= 10; // second
	
	d.push_back(dir); // third
		
	shark_location[shark[0].first][shark[0].second] = 0; // 상어 현위치 
	
	int y = shark[0].first;
	int x = shark[0].second;
	
	for(int i = 0; i < d.size(); i++)
	{
		int ny = y + shark_y[d[i]];
		int nx = x + shark_x[d[i]];
		
		if(map[ny][nx].size() >= 1)
			smell[ny][nx] = 1; // 새로운 위치여도 업데이트는 계속해서 되니까, smell을 업데이트 해줍니다. 
		
		map[ny][nx].clear(); // 물고기 지워주기 
		
		y += shark_y[d[i]];
		x += shark_x[d[i]]; // 현 위치 계속해서 이동 
	}
	
	shark_location[y][x] = 1; // 상어의 이동 위치 
	sorted_shark.clear(); // sorted_shark clear 
}

void fade_smell()
{
	for(int i = 1; i <= 4; i++)
	{
		for(int j = 1; j <= 4; j++)
		{
			if(smell[i][j] == 3)// 두번 전 마법은 사라지게 되는데 
			{
				smell[i][j] = 0;
			}
		}
	}
}

void copy_complete()
{
	
	while(!copy_fish.empty())
	{
		int y = copy_fish.front().first.first;
		int x = copy_fish.front().first.second;
		int d = copy_fish.front().second;
		copy_fish.pop();
				
		map[y][x].push_back(d); // 복사마법 하기 
	}
}


int main()
{
	cin >> m >> s;
	
	for(int i = 0; i < m; i++)
	{
		int y,x,d;
		cin >> y >> x >> d;
		map[y][x].push_back(d);
	}
	
	int y,x;
	cin >> y >> x;
	shark_location[y][x] = 1;
	
	while(s--)
	{
		for(int i = 1; i <= 4; i++)
		{
			for(int j = 1; j <= 4; j++)
			{
				if(shark_location[i][j])
					shark.push_back({i,j});
			}
		} // shark의 위치 넣어주고 
		
		move_fish();
		DFS(0);
		move_shark();
		fade_smell(); // smell[y][x]가 3이면 삭제 
		copy_complete();
		
		shark.pop_back();// shark의 위치 빼주고 
	}
	
	int ans = 0;
	
	for(int i = 1; i <= 4; i++)
	{
		for(int j = 1; j <= 4; j++)
		{
			if(map[i][j].size() >= 1)
			{
				ans += map[i][j].size();
			}
		}
	}
	
	cout << ans;
	
	return 0;
}
```
