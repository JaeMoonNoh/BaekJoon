## 전체 코드


```c++

#include<iostream>
#include<vector>
#include<queue>
#include<map>
#include<cstring>

#define MAX 21
using namespace std;

const int dx[5] = {0,1,-1,0,0};
const int dy[5] = {0,0,0,-1,1}; // 0, right, left, up, down

map<int,vector<int> > m;

int r,c,k,w;
int room[MAX][MAX];
int temperature[MAX][MAX];
bool wall[MAX][MAX];
bool visited[MAX][MAX];

vector<pair<int,int> > check_room;
vector<pair<pair<int,int>, int > > heater;
vector<int> wall_chk[MAX][MAX];
	
int rotate_dir(int d)
{
	if(d == 3) return 4; // down
	else if(d == 1) return 2; // left
}

vector<int> turn_direction(int d)
{
	if(d == 3 || d == 4)
	{
		vector<int> v = {1,0,2}; // 제자리, 좌,우 
		return v;
	}
	else
	{
		vector<int> v = {3,0,4}; // 제자리 상,하 
		return v;
	}
}

void heat(void)
{
	for(int i = 0; i < heater.size(); i++)
	{
		memset(visited,false,sizeof(visited));
		
		int y = heater[i].first.first;
		int x = heater[i].first.second;
		int dir = heater[i].second;
				
		int ny = y + dy[dir];
		int nx = x + dx[dir];
		
		temperature[ny][nx] += 5;
		visited[ny][nx] = true;
		
		queue<pair<pair<int,int>, pair<int,int> > > q;
		q.push({{ny,nx},{5,dir}}); // 
		
		while(!q.empty())
		{
			int qy = q.front().first.first;
			int qx = q.front().first.second;
			int temp = q.front().second.first;
			int direction = q.front().second.second;
			q.pop();
			
			if(temp == 0)
				continue;
				
			vector<int> d = turn_direction(direction);
			
			for(int i = 0; i < d.size(); i++)
			{
				int nny = qy + dy[d[i]];
				int nnx = qx + dx[d[i]]; // 제자리 좌, 우 
				
				if(0 > nnx || 0 > nny || r < nny || c < nnx)
					continue;
				
				if(wall[nny][nnx])
				{
					bool chk = false;
					
					for(int k = 0; k < wall_chk[nny][nnx].size(); k++)
					{
						int dir = wall_chk[nny][nnx][k];
						if(dir == direction)
						{
							chk = true;
						}
					}

					if(chk) // up(0)
					{
						continue; 
					}
					else
					{
						if(visited[nny+dy[direction]][nnx+dx[direction]])
							continue;
						visited[nny+dy[direction]][nnx+dx[direction]] = true;
						temperature[nny+dy[direction]][nnx+dx[direction]] += temp - 1;
						q.push({{nny+dy[direction],nnx+dx[direction]},{temp-1,direction}});
					}
				}	
				else
				{
					if(visited[nny+dy[direction]][nnx+dx[direction]])
						continue;
					visited[nny+dy[direction]][nnx+dx[direction]] = true;
					temperature[nny+dy[direction]][nnx+dx[direction]] += temp - 1;
					q.push({{nny+dy[direction],nnx+dx[direction]},{temp-1,direction}});		
				}
			}
		}
	}
	
//	cout << "===================temperature==========================" << endl;
//	cout << endl;
//	for(int i = 1; i <= r; i++)
//	{
//		for(int j = 1; j <= c; j++)
//		{
//			cout << temperature[i][j] << " ";
//		}
//		cout << endl;
//	}
//	cout << endl;
//	cout << endl;
}

void distribute(void)
{
	
	int temp_temp[MAX][MAX];
	memset(temp_temp,0,sizeof(temp_temp));
	
	for(int i = 1; i <= r; i++)
	{
		for(int j = 1; j <= c; j++)
		{
			if(temperature[i][j]) // 값이 존재하는 애들만 
			{
				int temp = temperature[i][j]; // temp  값 

				for(int d = 1; d < 5; d++)
				{
					int nx = j + dx[d]; 
					int ny = i + dy[d]; // 4방향 이동 
			
					if(0 >= nx || 0 >= ny || r < ny || c < nx)
						continue; // 범위를 벗어나면 continue 
					

					if(wall[i][j]) // 현재 위치가 wall 이면 
					{
	
						bool chk = false;
						
						for(int k = 0; k < wall_chk[i][j].size(); k++)
						{
							int dir = wall_chk[i][j][k];
							
							if(dir == d)
							{
								chk = true;
							}
						}
						
						if(chk)
						{
							continue;
						}
						else
						{
							if(temperature[ny][nx] < temp)
							{
								temp_temp[ny][nx] += (temp - temperature[ny][nx])/4;
								temp_temp[i][j] -= (temp - temperature[ny][nx])/4;
							}
						}
					}
					else // 현재 위치가 !wall이면 범위 안에 있으므로 그냥 쳐주면 된다. 
					{
						if(temperature[ny][nx] < temp)
						{
							temp_temp[ny][nx] += (temp - temperature[ny][nx])/4;
							temp_temp[i][j] -= (temp - temperature[ny][nx])/4;
						}
					}
				}
			}
		}
	}
	
	for(int i = 1; i <= r; i++)
	{
		for(int j = 1; j <= c; j++)
		{
			temperature[i][j] += temp_temp[i][j];
		}
	}
	
//	cout << "===============spread=================" << endl;
//	cout << endl;
//	
//	for(int i = 1; i <= r; i++)
//	{
//		for(int j = 1; j <= c; j++)
//		{
//			cout << temperature[i][j] << " ";
//		}
//		cout << endl;
//	}
//	cout << endl;
	
	
}

void erase_outside()
{
	for(int i = 1; i <= r; i++)
	{
		for(int j = 1; j <= c; j++)
		{
			if(i == 1 || i == r)
			{
				if(temperature[i][j] == 0)
					continue;
				temperature[i][j] -= 1;
			}
			else if(j == 1 || j == c)
			{
				if(temperature[i][j] == 0)
					continue;
				temperature[i][j] -= 1;
			}
		}
	}
	
//	cout << "====================erase======================" << endl;
//	
//	cout << endl;
//	cout << endl;
//	
//	for(int i = 1; i <= r; i++)
//	{
//		for(int j = 1; j <= c; j++)
//		{
//			cout << temperature[i][j] << " ";
//		}
//		cout << endl;
//	}
//	
	
}

int main()
{
	
	memset(temperature,0,sizeof(temperature));

	cin >> r >> c >> k; // r행, c열을 의미한다. 
	
	for(int i = 1; i <= r; i++)
	{
		for(int j = 1; j <= c; j++)
		{
			cin >> room[i][j];
			
			if(room[i][j] == 5) // 온도 체크해야할 위치 
			{
				check_room.push_back({i,j});
			}
			else if(room[i][j] >= 1  && room[i][j] <= 4) // 1 ~ 4 사이 온풍기
			{
				heater.push_back({{i,j},room[i][j]}); // first.first = y, first.second = x, second = heater dir
			} 
		}
	}
	
	cin >> w;
	

	for(int i = 0; i < w; i++)
	{
		int y,x,t;
		cin >> y >> x >> t;
		 
		if(t == 1) // right
		{
			wall[y][x] = true;
			wall[y][x+1] = true;
			wall_chk[y][x].push_back(t);
			wall_chk[y][x+1].push_back(rotate_dir(t));
		}
		else // up
		{
			wall[y][x] = true;
			wall[y-1][x] = true;
			wall_chk[y][x].push_back(3);
			wall_chk[y-1][x].push_back(rotate_dir(3));
		}
	} 
	
	int cnt = 0;
	
	while(1)
	{
		int chk = 0;
		cnt++;
		
		if(cnt == 101)
			break;
		//cout <<"=======================cnt" <<  cnt << "=======================" << endl;
		heat(); // 바람이 한번 나오고 
		distribute(); // 온도가 조절이 되고 
		erase_outside(); //  바깥 온도 1이상 온도 1씩 감소 
		
		for(int i = 0; i < check_room.size(); i++)
		{
			int y = check_room[i].first; 
			int x = check_room[i].second; // 행렬 검사 
			
			if(temperature[y][x] >= k)
				chk++; 
			else
				chk--;
		}
		
		if(chk == check_room.size())
			break;
	}
	
	cout << cnt;
	
	return 0;
}


```

## 다시 풀어야된다.
