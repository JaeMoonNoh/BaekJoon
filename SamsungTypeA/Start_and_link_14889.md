# 백준 스타트와 링크 14889
[스타트와 링크](https://www.acmicpc.net/problem/14889)

### 문제
오늘은 스타트링크에 다니는 사람들이 모여서 축구를 해보려고 한다. 축구는 평일 오후에 하고 의무 참석도 아니다. 축구를 하기 위해 모인 사람은 총 N명이고 신기하게도 N은 짝수이다. 이제 N/2명으로 이루어진 스타트 팀과 링크 팀으로 사람들을 나눠야 한다.

BOJ를 운영하는 회사 답게 사람에게 번호를 1부터 N까지로 배정했고, 아래와 같은 능력치를 조사했다. 능력치 Sij는 i번 사람과 j번 사람이 같은 팀에 속했을 때, 팀에 더해지는 능력치이다. 팀의 능력치는 팀에 속한 모든 쌍의 능력치 Sij의 합이다. Sij는 Sji와 다를 수도 있으며, i번 사람과 j번 사람이 같은 팀에 속했을 때, 팀에 더해지는 능력치는 Sij와 Sji이다.

N=4이고, S가 아래와 같은 경우를 살펴보자.

```c++


i\j	1	2	3	4   
1  	0	1	2	3   
2	4	0	5	6   
3	7	1	0	2   
4	3	4	5	0    

```

예를 들어, 1, 2번이 스타트 팀, 3, 4번이 링크 팀에 속한 경우에 두 팀의 능력치는 아래와 같다.

* 스타트 팀: S12 + S21 = 1 + 4 = 5
* 링크 팀: S34 + S43 = 2 + 5 = 7

1, 3번이 스타트 팀, 2, 4번이 링크 팀에 속하면, 두 팀의 능력치는 아래와 같다.

* 스타트 팀: S13 + S31 = 2 + 7 = 9
* 링크 팀: S24 + S42 = 6 + 4 = 10

축구를 재미있게 하기 위해서 스타트 팀의 능력치와 링크 팀의 능력치의 차이를 최소로 하려고 한다. 위의 예제와 같은 경우에는 1, 4번이 스타트 팀, 2, 3번 팀이 링크 팀에 속하면 스타트 팀의 능력치는 6, 링크 팀의 능력치는 6이 되어서 차이가 0이 되고 이 값이 최소이다.

### 입력

첫째 줄에 N(4 ≤ N ≤ 20, N은 짝수)이 주어진다. 둘째 줄부터 N개의 줄에 S가 주어진다. 각 줄은 N개의 수로 이루어져 있고, i번 줄의 j번째 수는 Sij 이다. Sii는 항상 0이고, 나머지 Sij는 1보다 크거나 같고, 100보다 작거나 같은 정수이다.

### 출력

첫째 줄에 스타트 팀과 링크 팀의 능력치의 차이의 최솟값을 출력한다.


### 알고리즘

(1,2) / (3,4) 와 같이 경우의 수를 구해야하기 때문에 BackTracking으로 모든 경우의 수를 구해야한다.
그래서 총 인원의 1/2만큼 인원을 구하고 backTracking을 시작한다.

### 구현해야할 조건

> 팀의 능력치는 팀에 속한 모든 쌍의 늘력치 S(i,j)의 합이다. S(i,j) + S(j,i)이 된다.

```c++

for(int i = 1; i <= n; i++)
{
	for(int j = 1; j <= n; j++)
	{
		if(i == j) // 중복 인원은 필요가 없고
			continue;
		if(visited[i] && visited[j])
			start += ability[i][j];
		if(!visited[i] && !visited[j])
			link += ability[i][j];
	}
}


```

> 스타트팀 : S(1,2) + S(2,1) , 링크 팀 : S(3,4) + S(4,3)

```c++

for(int i = 1; i <= n; i++)
{
	for(int j = 1; j <= n; j++)
	{
		if(i == j) // 중복 인원은 필요가 없고
			continue;
		if(visited[i] && visited[j])
			start += ability[i][j];
		if(!visited[i] && !visited[j])
			link += ability[i][j];
	}
}


```

> 팀이 중복되어서는 안된다.

```c++
void DFS(int cnt, int b) // int b는 중복되지 않은 값을 구할때
{
   if(cnt == n / 2) // 전체 인원중 1/2를 구했을때
   {
    .
    .
    .
   }
	for(int i = b; i <= n; i++) // int i = b
	{
		visited[i] = true;
		DFS(cnt + 1, i + 1);
		visited[i] = false;
	}
	
}


```


### 전체 코드

```c++

#include<iostream>
#include<algorithm>

#define MAX 21
#define INF 987654321
using namespace std;

int n; // company members
int ability[MAX][MAX];
bool visited[MAX];
int ans = INF;

void DFS(int cnt, int b)
{
	if(cnt == n / 2)
	{
		int start = 0;
		int link = 0;
		
		for(int i = 1; i <= n; i++)
		{
			for(int j = 1; j <= n; j++)
			{
				if(i == j)
					continue;
				if(visited[i] && visited[j])
					start += ability[i][j];
				if(!visited[i] && !visited[j])
					link += ability[i][j];
			}
		}
		
		//cout << start << " " << link << endl; 
		
		int temp = abs(start - link);
		
		if(temp < ans)
			ans = temp;
		return;
	}
	
	for(int i = b; i <= n; i++)
	{
		visited[i] = true;
		DFS(cnt + 1, i + 1);
		visited[i] = false;
	}
	
}

int main()
{
	cin >> n;
	
	if(n % 2 != 0)
		return 0;
		
	for(int i = 1; i <= n; i++)
	{
		for(int j = 1; j <= n; j++)
		{
			cin >> ability[i][j];
		}
	}
	
	DFS(0,1);
	
	cout << ans << "\n";
	
	return 0;
}

```

