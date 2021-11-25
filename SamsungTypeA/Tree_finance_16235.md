# 백준 나무 재테크 16235
[나무 재테크](https://www.acmicpc.net/problem/16235)


### 문제

부동산 투자로 억대의 돈을 번 상도는 최근 N×N 크기의 땅을 구매했다. 상도는 손쉬운 땅 관리를 위해 땅을 1×1 크기의 칸으로 나누어 놓았다. 각각의 칸은 (r, c)로 나타내며, r은 가장 위에서부터 떨어진 칸의 개수, c는 가장 왼쪽으로부터 떨어진 칸의 개수이다. r과 c는 1부터 시작한다.

상도는 전자통신공학과 출신답게 땅의 양분을 조사하는 로봇 S2D2를 만들었다. S2D2는 1×1 크기의 칸에 들어있는 양분을 조사해 상도에게 전송하고, 모든 칸에 대해서 조사를 한다. 가장 처음에 양분은 모든 칸에 5만큼 들어있다.

매일 매일 넓은 땅을 보면서 뿌듯한 하루를 보내고 있던 어느 날 이런 생각이 들었다.

> 나무 재테크를 하자!

나무 재테크란 작은 묘목을 구매해 어느정도 키운 후 팔아서 수익을 얻는 재테크이다. 상도는 나무 재테크로 더 큰 돈을 벌기 위해 M개의 나무를 구매해 땅에 심었다. 같은 1×1 크기의 칸에 여러 개의 나무가 심어져 있을 수도 있다.

이 나무는 사계절을 보내며, 아래와 같은 과정을 반복한다.

봄에는 나무가 자신의 나이만큼 양분을 먹고, 나이가 1 증가한다. 각각의 나무는 나무가 있는 1×1 크기의 칸에 있는 양분만 먹을 수 있다. 하나의 칸에 여러 개의 나무가 있다면, 나이가 어린 나무부터 양분을 먹는다. 만약, 땅에 양분이 부족해 자신의 나이만큼 양분을 먹을 수 없는 나무는 양분을 먹지 못하고 즉시 죽는다.

여름에는 봄에 죽은 나무가 양분으로 변하게 된다. 각각의 죽은 나무마다 나이를 2로 나눈 값이 나무가 있던 칸에 양분으로 추가된다. 소수점 아래는 버린다.

가을에는 나무가 번식한다. 번식하는 나무는 나이가 5의 배수이어야 하며, 인접한 8개의 칸에 나이가 1인 나무가 생긴다. 어떤 칸 (r, c)와 인접한 칸은 (r-1, c-1), (r-1, c), (r-1, c+1), (r, c-1), (r, c+1), (r+1, c-1), (r+1, c), (r+1, c+1) 이다. 상도의 땅을 벗어나는 칸에는 나무가 생기지 않는다.

겨울에는 S2D2가 땅을 돌아다니면서 땅에 양분을 추가한다. 각 칸에 추가되는 양분의 양은 A[r][c]이고, 입력으로 주어진다.

K년이 지난 후 상도의 땅에 살아있는 나무의 개수를 구하는 프로그램을 작성하시오.

### 입력

첫째 줄에 N, M, K가 주어진다.

둘째 줄부터 N개의 줄에 A배열의 값이 주어진다. r번째 줄의 c번째 값은 A[r][c]이다.

다음 M개의 줄에는 상도가 심은 나무의 정보를 나타내는 세 정수 x, y, z가 주어진다. 처음 두 개의 정수는 나무의 위치 (x, y)를 의미하고, 마지막 정수는 그 나무의 나이를 의미한다.

### 출력

첫째 줄에 K년이 지난 후 살아남은 나무의 수를 출력한다.

### 제한

* 1 ≤ N ≤ 10
* 1 ≤ M ≤ N2
* 1 ≤ K ≤ 1,000
* 1 ≤ A[r][c] ≤ 100
* 1 ≤ 입력으로 주어지는 나무의 나이 ≤ 10
* 입력으로 주어지는 나무의 위치는 모두 서로 다름

### 알고리즘 & 자료구조

한 구역에 (i,j)에 나무가 여러개가 들어갈수 있기 때문에, vector를 사용해서 문제를 해결했습니다.
문제를 푸는 중에 시간초과,segmentation fault, Out of bounds 등이 발생했었고, Out of bounds는 나무의 범위를 정확히 지정했어야했던 문제이고.
시간초과는 struct로 sort를 진행했는데 상당히 오랜시간이 걸렸다. 각 벡터마다 돌려서 구조체에 따른 sort를 진행하다 보니 오래 걸렸던것 같습니다.
이유는 찾아봐서 다시 올릴 계획입니다. ~또는 우선순위 큐를 사용해서 heap이기 때문에 logN이 사용되지만...~

전체적으로 그놈이 SpringAndSummer에서 많이 헷갈린 문제이다.


### 구현해야할 조건

> 모든 칸에 처음 5만큼의 양분이 들어가 있다.

```c++
	for(int i = 1; i <= N; i++)
	{
		for(int j = 1; j <= N; j++)
		{
			land[i][j] = 5;
			cin >> A[i][j]; 
		}
	} // land : 토지 양분(초기시작), A : 매년 겨울마다 추가되는 양분 
	
	for(int i = 0; i < m; i++)
	{
		int x,y,z;
		cin >> x >> y >> z;

		trees[x][y].push_back(z);
	} // vector<int> trees[MAX][MAX]로 만들어 해당 위치에 나무가 동적으로 저장이 된다.
```

> [Spring] 나무가 자신의 나이만큼 양분을 먹고, 나이가 1 증가한다.

```c++
for(int k = 0; k < trees[i][j].size(); k++)
{
		//나무가 저장되어있는 애들
		if(land[i][j] >= trees[i][j][k]) // 해당 양분이 trees의 나이 이상 있다면
		{
			land[i][j] -= trees[i][j][k]; // 양분을 빼먹고
			Temp.push_back(trees[i][j][k] + 1); // 나이를 추가한다. Temp vector에 저장
		}
    else
    {
       Die_Tree_Energy = Die_Tree_Energy + (trees[i][j][k] / 2); // 그대로 여름을 진행해도 된다.
    } // 어차피 나이가 양분보다 많으면 바로 즉사이기 때문에 Spring에 섞여도 된다.
}
```

> [Spring] 하나 칸에 여러개의 나무가 있다면 나이가 어린 나무부터 먹는다.

```c++

sort(trees[i][j].begin(),trees[i][j].end()); // sort함수를 이용해 나이가 어린 나무부터 먹게 한다.
			
```

> [Spring] 자기 나이만큼의 양분이 없으면 즉사한다.

```c++
    else
    {
       Die_Tree_Energy = Die_Tree_Energy + (trees[i][j][k] / 2); // 그대로 여름을 진행해도 된다.
    } // 어차피 나이가 양분보다 많으면 바로 즉사이기 때문에 Spring에 섞여도 된다.
```

> [Summer] 죽은 나무는 양분이 된다. 나이 / 2 가 양분에 추가 된다.

```c++
trees[i][j].clear();
for (int k = 0; k < Temp.size(); k++)
  trees[i][j].push_back(Temp[k]); // 죽은 나무를 제외한
            
land[i][j] = land[i][j] + Die_Tree_Energy; // 이제껏 죽었던 나무의 값을 전부 더한다.
```

> [Fall] 번식을 하는 나무는 나이가 5의 배수이며, 인접한 8칸에 나이가 1인 나무가 생긴다.

```c++
for (int k = 0; k < trees[i][j].size(); k++)
{
  if (trees[i][j][k] % 5 == 0)
  {
   for (int a = 0; a < 8; a++)
   {
    int nx = i + dx[a];
    int ny = j + dy[a];
 
    if (nx >= 1 && ny >= 1 && nx <= N && ny <= N)
      trees[nx][ny].push_back(1); // 나이 한살짜리 애들 추가
    }
  }
}
```

> [Winter] A[r][c]이 양분에 추가 된다. 입력으로 주어진다.

```c++
for (int i = 1; i <= N; i++)
{
    for (int j = 1; j <= N; j++)
    {
        land[i][j] = land[i][j] + A[i][j];
    }
}
```

### 전체 코드

```c++
#include<iostream>
#include<vector>
#include<algorithm>

#define MAX 11
using namespace std;


//bool comp(tree &a, tree &b)
//{
//	return a.Age < b.Age;
//}

const int dx[] = { -1, -1, -1, 0, 0, 1, 1, 1 };
const int dy[] = { -1, 0, 1, -1, 1, -1, 0, 1 };

int N,m,k; // n : land size, m : trees, k : years
int land[MAX][MAX];
int A[MAX][MAX];
vector<int> trees[MAX][MAX];

void SpringAndSummer()
{
    for (int i = 1; i <= N; i++)
    {
        for (int j = 1; j <= N; j++)
        {
            if (trees[i][j].size() == 0) continue;
            
            int Die_Tree_Energy = 0;
            vector<int> Temp;
 
            sort(trees[i][j].begin(), trees[i][j].end());
            for (int k = 0; k < trees[i][j].size(); k++)
            {
                if (land[i][j] >= trees[i][j][k])
                {
                    land[i][j] = land[i][j] - trees[i][j][k];
                    Temp.push_back(trees[i][j][k] + 1);
                }
                else
                {
                    Die_Tree_Energy = Die_Tree_Energy + (trees[i][j][k] / 2);
                }
            }
 
            trees[i][j].clear();
            for (int k = 0; k < Temp.size(); k++)
                trees[i][j].push_back(Temp[k]);
            
            land[i][j] = land[i][j] + Die_Tree_Energy;
        }
    }    
}
 
void Fall()
{
    for (int i = 1; i <= N; i++)
    {
        for (int j = 1; j <= N; j++)
        {
            if (trees[i][j].size() == 0) continue;
 
            for (int k = 0; k < trees[i][j].size(); k++)
            {
                
 
                if (trees[i][j][k] % 5 == 0)
                {
                    for (int a = 0; a < 8; a++)
                    {
                        int nx = i + dx[a];
                        int ny = j + dy[a];
 
                        if (nx >= 1 && ny >= 1 && nx <= N && ny <= N)
                        {
                            trees[nx][ny].push_back(1);
                        }
                    }
                }
            }
        }
    }
}
 
void Winter()
{
    for (int i = 1; i <= N; i++)
    {
        for (int j = 1; j <= N; j++)
        {
            land[i][j] = land[i][j] + A[i][j];
        }
    }
}

int main()
{
	
	ios::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
	cin >> N >> m >> k;
	
	for(int i = 1; i <= N; i++)
	{
		for(int j = 1; j <= N; j++)
		{
			land[i][j] = 5;
			cin >> A[i][j]; 
		}
	} // land : 토지 양분(초기시작), A : 매년 겨울마다 추가되는 양분 
	
	for(int i = 0; i < m; i++)
	{
		int x,y,z;
		cin >> x >> y >> z;

		trees[x][y].push_back(z);
	}
	
	while(k--)
	{
		SpringAndSummer();
		Fall();
		Winter();
	}
	
	int treeCount = 0;
	
	for(int i = 1; i <= N; i++)
		for(int j = 1; j <= N; j++)
			treeCount += trees[i][j].size();
	
	cout << treeCount;
	
	return 0;
}

```

