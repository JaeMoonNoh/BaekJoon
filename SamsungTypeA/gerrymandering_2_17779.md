# 백준 게리맨더링 2 17779
[게리맨더링2](https://www.acmicpc.net/problem/17779)

### 문제
재현시의 시장 구재현은 지난 몇 년간 게리맨더링을 통해서 자신의 당에게 유리하게 선거구를 획정했다. 견제할 권력이 없어진 구재현은 권력을 매우 부당하게 행사했고, 심지어는 시의 이름도 재현시로 변경했다. 이번 선거에서는 최대한 공평하게 선거구를 획정하려고 한다.

재현시는 크기가 N×N인 격자로 나타낼 수 있다. 격자의 각 칸은 구역을 의미하고, r행 c열에 있는 구역은 (r, c)로 나타낼 수 있다. 구역을 다섯 개의 선거구로 나눠야 하고, 각 구역은 다섯 선거구 중 하나에 포함되어야 한다. 선거구는 구역을 적어도 하나 포함해야 하고, 한 선거구에 포함되어 있는 구역은 모두 연결되어 있어야 한다. 구역 A에서 인접한 구역을 통해서 구역 B로 갈 수 있을 때, 두 구역은 연결되어 있다고 한다. 중간에 통하는 인접한 구역은 0개 이상이어야 하고, 모두 같은 선거구에 포함된 구역이어야 한다.

선거구를 나누는 방법은 다음과 같다.

1. 기준점 (x, y)와 경계의 길이 d1, d2를 정한다. (d1, d2 ≥ 1, 1 ≤ x < x+d1+d2 ≤ N, 1 ≤ y-d1 < y < y+d2 ≤ N)

2. 다음 칸은 경계선이다.
  1. (x, y), (x+1, y-1), ..., (x+d1, y-d1)
  2. (x, y), (x+1, y+1), ..., (x+d2, y+d2)
  3. (x+d1, y-d1), (x+d1+1, y-d1+1), ... (x+d1+d2, y-d1+d2)
  4. (x+d2, y+d2), (x+d2+1, y+d2-1), ..., (x+d2+d1, y+d2-d1)

3. 경계선과 경계선의 안에 포함되어있는 곳은 5번 선거구이다.
4. 5번 선거구에 포함되지 않은 구역 (r, c)의 선거구 번호는 다음 기준을 따른다.
  * 1번 선거구: 1 ≤ r < x+d1, 1 ≤ c ≤ y
  * 2번 선거구: 1 ≤ r ≤ x+d2, y < c ≤ N
  * 3번 선거구: x+d1 ≤ r ≤ N, 1 ≤ c < y-d1+d2
  * 4번 선거구: x+d2 < r ≤ N, y-d1+d2 ≤ c ≤ N

구역 (r, c)의 인구는 A[r][c]이고, 선거구의 인구는 선거구에 포함된 구역의 인구를 모두 합한 값이다. 선거구를 나누는 방법 중에서, 인구가 가장 많은 선거구와 가장 적은 선거구의 인구 차이의 최솟값을 구해보자.

### 입력

첫째 줄에 재현시의 크기 N이 주어진다.

둘째 줄부터 N개의 줄에 N개의 정수가 주어진다. r행 c열의 정수는 A[r][c]를 의미한다.

### 출력

첫째 줄에 인구가 가장 많은 선거구와 가장 적은 선거구의 인구 차이의 최솟값을 출력한다.

### 자료구조 & 알고리즘

BackTracking으로 중복된 수열까지 포함해서 문제를 풀려고 했으나, 시간적인 문제가 걸렸고
범위를 설정해 세부적으로 조건문에 추가를 해줘야한다는 생각이 들었으나, 구현은 실패했다.
다른 블로그의 내용을 토대로 이해는 했지만, 추후에 다시 검토해봐야 겠다. 생각보다 너무 오래 걸린 문제이다.


### 전체 코드


```c++

#include<iostream>
#include<cstring>
#include<algorithm>

#define MAX 20
#define INF 987654321
using namespace std;

typedef struct{
	int y;
	int x;
}location;

int N;
int result = INF;
int area[MAX][MAX];
int copyArea[MAX][MAX];

location loc[4];

int Min(int A, int B) { if (A < B) return A; return B; }

bool MadeLine(int x, int y, int d1, int d2)
{
	if (x + d1 >= N || y - d1 < 0) return false;
    if (x + d2 >= N || y + d2 >= N) return false;
    if (x + d1 + d2 >= N || y - d1 + d2 >= N) return false;
    if (x + d2 + d1 >= N || y + d2 - d1 < 0) return false;
 
    return true;
}

void DFS(int x, int y, int d1, int d2)
{
	
	for (int i = 0; i < N; i++)
    {
        for (int j = 0; j < N; j++)
        {
            copyArea[i][j] = 5;
        }
    }
 
   int SubArea = 0;
    for (int i = 0; i < loc[1].x; i++)
    {
        if (i >= loc[0].x) SubArea++;
        for (int j = 0; j <= loc[0].y - SubArea; j++)
        {
            copyArea[i][j] = 1;
        }
    }
 
  int PlusArea = 0;
    for (int i = 0; i <= loc[2].x; i++)
    {
        if (i > loc[0].x) PlusArea++;
        for (int j = loc[0].y + 1 + PlusArea; j < N; j++)
        {
            copyArea[i][j] = 2;
        }
    } 
 
    SubArea = 0;
    for (int i = N - 1; i >= loc[1].x; i--)
    {
        if (i < loc[3].x) SubArea++;
        for (int j = 0; j < loc[3].y - SubArea; j++)
        {
            copyArea[i][j] = 3;
        }
    }
 
    PlusArea = 0;
    for (int i = N - 1; i > loc[2].x; i--)
    {
        if (i <= loc[3].x) PlusArea++;
        for (int j = loc[3].y + PlusArea; j < N; j++)
        {
            copyArea[i][j] = 4;
        }
    }
    
    int people[6] = {0,0,0,0,0,0};
 	
 	for(int i = 0; i < N; i++)
 	{
 		for(int j = 0; j < N; j++)
 		{
 			people[copyArea[i][j]] = people[copyArea[i][j]] + area[i][j];
		 }
	}
	
	sort(people, people + 6);
	int differ = people[5] - people[1];
	result = Min(differ, result);
}

int main()
{
	cin >> N;
	
	for(int i = 0; i < N; i++)
		for(int j = 0; j < N; j++)
			cin >> area[i][j];

	
    for (int i = 0; i < N; i++)
    {
        for (int j = 1; j < N; j++)
        {
            for (int D1 = 1; D1 <= j; D1++)            // D1의 범위는 1 부터 현재 y좌표 까지 
            {
                for (int D2 = 1 ; D2 < N - j; D2++)    // D2의 범위는 현재 y좌표부터 맵의 가로길이 끝까지 남은 칸수
                {
                    if (MadeLine(i, j, D1, D2) == true)        // 위의 범위만으로 무조건 선거구를 그릴 수 없기 때문에 선거구를 그릴 수 있는지 체크하는 함수.
                    {
                        loc[0].x = i; loc[0].y = j;
                        loc[1].x = i + D1; loc[1].y = j - D1;
                        loc[2].x = i + D2; loc[2].y = j + D2;
                        loc[3].x = i + D1 + D2; loc[3].y = j - D1 + D2;
                        DFS(i, j, D1, D2);
                    }
                }
            }
        }
    }
	cout << result;
	
	return 0;
}


```
