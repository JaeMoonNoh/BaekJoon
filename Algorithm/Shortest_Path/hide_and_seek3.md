# 백준 숨바꼭질3 13549

## 문제

수빈이는 동생과 숨바꼭질을 하고 있다. 수빈이는 현재 점 N(0 ≤ N ≤ 100,000)에 있고, 동생은 점 K(0 ≤ K ≤ 100,000)에 있다. 수빈이는 걷거나 순간이동을 할 수 있다. 만약, 수빈이의 위치가 X일 때 걷는다면 1초 후에 X-1 또는 X+1로 이동하게 된다. 순간이동을 하는 경우에는 0초 후에 2*X의 위치로 이동하게 된다.

수빈이와 동생의 위치가 주어졌을 때, 수빈이가 동생을 찾을 수 있는 가장 빠른 시간이 몇 초 후인지 구하는 프로그램을 작성하시오.

## 입력

첫 번째 줄에 수빈이가 있는 위치 N과 동생이 있는 위치 K가 주어진다. N과 K는 정수이다.

## 출력

수빈이가 동생을 찾는 가장 빠른 시간을 출력한다.

## 전체 코드

순간이동, 한칸이동, 한칸 뒤로 이동, 그래프로 생각해서 해당 위치에 visisted이 되었다거나, 혹은 MAX 이내 0 이상일때 옮겨주고 pq에 넣어준다. 이동시간에 따라서 pq이기 때문에
return만 넣어주면 된다.

```c++
#include<iostream>
#include<vector>
#include<queue>

#define MAX 100000 + 1

using namespace std;

bool visited[MAX];

int minSecond(int n, int k)
{
	priority_queue<pair<int,int>, vector<pair<int,int> >, greater<pair<int,int> > > pq;
	
	pq.push({0,n});
	visited[n] = true;
	
	while(!pq.empty())
	{
		int time = pq.top().first;
		int location = pq.top().second;
		pq.pop();
		
		if(location == k)
			return time;
		
		if(location * 2 < MAX && !visited[location*2])
		{
			pq.push({time, location * 2});
			visited[location * 2] = true;
		}
		if(location + 1 < MAX && !visited[location + 1])
		{
			visited[location + 1] = true;
			pq.push({time+1, location + 1});
		}
		if(location - 1 >= 0 && !visited[location - 1])
		{
			visited[location - 1] = true;
			pq.push({time+1, location - 1});
		}
	}
}

int main()
{
	int n,k;
	cin >> n >> k;
	
	cout << minSecond(n,k) << "\n";
	
	return 0;
}
```


