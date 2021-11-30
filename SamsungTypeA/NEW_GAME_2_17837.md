# 백준 새로운 게임 2 17837

[새로운 게임2](https://www.acmicpc.net/problem/17837)

### 문제

재현이는 주변을 살펴보던 중 체스판과 말을 이용해서 새로운 게임을 만들기로 했다. 새로운 게임은 크기가 N×N인 체스판에서 진행되고, 사용하는 말의 개수는 K개이다. 말은 원판모양이고, 하나의 말 위에 다른 말을 올릴 수 있다. 체스판의 각 칸은 흰색, 빨간색, 파란색 중 하나로 색칠되어있다.

게임은 체스판 위에 말 K개를 놓고 시작한다. 말은 1번부터 K번까지 번호가 매겨져 있고, 이동 방향도 미리 정해져 있다. 이동 방향은 위, 아래, 왼쪽, 오른쪽 4가지 중 하나이다.

턴 한 번은 1번 말부터 K번 말까지 순서대로 이동시키는 것이다. 한 말이 이동할 때 위에 올려져 있는 말도 함께 이동한다. 말의 이동 방향에 있는 칸에 따라서 말의 이동이 다르며 아래와 같다. 턴이 진행되던 중에 말이 4개 이상 쌓이는 순간 게임이 종료된다.

A번 말이 이동하려는 칸이
흰색인 경우에는 그 칸으로 이동한다. 이동하려는 칸에 말이 이미 있는 경우에는 가장 위에 A번 말을 올려놓는다.
A번 말의 위에 다른 말이 있는 경우에는 A번 말과 위에 있는 모든 말이 이동한다.
예를 들어, A, B, C로 쌓여있고, 이동하려는 칸에 D, E가 있는 경우에는 A번 말이 이동한 후에는 D, E, A, B, C가 된다.
빨간색인 경우에는 이동한 후에 A번 말과 그 위에 있는 모든 말의 쌓여있는 순서를 반대로 바꾼다.
A, B, C가 이동하고, 이동하려는 칸에 말이 없는 경우에는 C, B, A가 된다.
A, D, F, G가 이동하고, 이동하려는 칸에 말이 E, C, B로 있는 경우에는 E, C, B, G, F, D, A가 된다.
파란색인 경우에는 A번 말의 이동 방향을 반대로 하고 한 칸 이동한다. 방향을 반대로 바꾼 후에 이동하려는 칸이 파란색인 경우에는 이동하지 않고 가만히 있는다.
체스판을 벗어나는 경우에는 파란색과 같은 경우이다.

체스판의 크기와 말의 위치, 이동 방향이 모두 주어졌을 때, 게임이 종료되는 턴의 번호를 구해보자.

### 입력

첫째 줄에 체스판의 크기 N, 말의 개수 K가 주어진다. 둘째 줄부터 N개의 줄에 체스판의 정보가 주어진다. 체스판의 정보는 정수로 이루어져 있고, 각 정수는 칸의 색을 의미한다. 0은 흰색, 1은 빨간색, 2는 파란색이다.

다음 K개의 줄에 말의 정보가 1번 말부터 순서대로 주어진다. 말의 정보는 세 개의 정수로 이루어져 있고, 순서대로 행, 열의 번호, 이동 방향이다. 행과 열의 번호는 1부터 시작하고, 이동 방향은 4보다 작거나 같은 자연수이고 1부터 순서대로 →, ←, ↑, ↓의 의미를 갖는다.

같은 칸에 말이 두 개 이상 있는 경우는 입력으로 주어지지 않는다.


### 출력

게임이 종료되는 턴의 번호를 출력한다. 그 값이 1,000보다 크거나 절대로 게임이 종료되지 않는 경우에는 -1을 출력한다.

### 알고리즘 & 자료구조

구현이 어려웠던 문제이다. 구현은 전체적으로 했지만, 모듈화를 어떻게 시켜야하는지 제대로 몰라서
고민이 많았던 문제이다. 그래서 다른 분이 풀었던 해설을 보고 모듈화를 시켰는데, 내가 처음 풀이했던 우선순위 큐로 풀이를 했는데 오류가 크게 나서

이유를 모르는게 분통이 터질 지경이다. 끄아아아아아악

### 구현해야할 조건

> 하나의 말 위에 다른 말을 올릴 수 있다.

```c++
vector<int> board[MAX][MAX]; // 이차원 배열 벡터로 만들어서 말이 쌓이는 것으로 만든다.
```

> 이동 방향은 위, 아래, 왼쪽, 오른쪽 4가지 중 하나이다.

```c++
const int dx[] = { 0, 0, 0, -1, 1 };
const int dy[] = { 0, 1, -1, 0, 0 }; // 1 : right, 2 : left, 3 : up, 4 : down
```

> 한 말이 이동할때 위에 올려져 있는 말도 함께 이동한다.

```c++

//흰색 블록에 들어섰을때

 if (Ms == 0)
    {
        for (int i = Pos; i < MAP_State[x][y].size(); i++)
        {
            MAP_State[nx][ny].push_back(MAP_State[x][y][i]);
            int Idx = MAP_State[x][y][i];
            Chess[Idx].x = nx;
            Chess[Idx].y = ny;
        }
        int Delete_Num = Find_Delete_Num(x, y, Chess_Num);
        for (int i = 0; i < Delete_Num; i++) MAP_State[x][y].pop_back();
    }
    
    // 계속해서 구현을 할때, 한개인지 여러개인지 체크를 해주었으나 조건문 for를 이용하면 한번에 해결이된다. 제발 생각 하자!!
```



> 턴이 진행되던 중에 말이 4개 이상 쌓이는 순간 게임이 종료된다.

```c++
bool Check_State()
{
    for (int i = 0; i < K; i++)
    {
        int x = Chess[i].x;
        int y = Chess[i].y;
        if (MAP_State[x][y].size() >= 4) return true;
    }
    return false;
}

if (Check_State() == true)
{
  Flag = true;
  break;
}
```



> 흰색 칸인 경우에는 그 칸으로 이동한다. 이동하려는 칸에 말이 이미 있는 경우에는 가장 위에 이동되는 말을 올려 놓는다.

```c++
 if (Ms == 0)
    {
        for (int i = Pos; i < MAP_State[x][y].size(); i++) // 모든말이 이동한다.
        {
            MAP_State[nx][ny].push_back(MAP_State[x][y][i]);
            int Idx = MAP_State[x][y][i];
            Chess[Idx].x = nx;
            Chess[Idx].y = ny;
        }
        int Delete_Num = Find_Delete_Num(x, y, Chess_Num);
        for (int i = 0; i < Delete_Num; i++) MAP_State[x][y].pop_back(); // 어느시점부터 삭제가 되는지, 1~k번째로 이동을 하니까 1번이 이동하면 1번 위의 값들은 다같이 이동해서 그 위는 pop_back()해야함
    }
```


> 빨간색인 경우에는 이동한 후에 이동되는 말과 그 위에 있는 모든 말의 쌓여있는 순서를 반대로 바꾼다.

```c++
    else if (Ms == 1)
    {
        for (int i = MAP_State[x][y].size() - 1; i >= Pos; i--) // 맨뒤에 있는 값부터 꺼내고, pos은 그 마지막 위치이다.
        {
            MAP_State[nx][ny].push_back(MAP_State[x][y][i]); // 이동하려는 자리에 있는 값은 바뀌지 않으니 상관없음
            int Idx = MAP_State[x][y][i];
            Chess[Idx].x = nx;
            Chess[Idx].y = ny;
        }
        int Delete_Num = Find_Delete_Num(x, y, Chess_Num);
        for (int i = 0; i < Delete_Num; i++) MAP_State[x][y].pop_back();
    }
```


> 파란색인 경우에는 A번 말의 이동 방향을 반대로 하고 한 칸 이동한다. 방향을 반대로 바꾼 후에 이동하려는 칸이 파란색인 경우에는 이동하지 않고 가만히 있는다.

```c++

// 가장 헷갈렸던 부분인데 파란색칸을 밟으면 방향이 바뀌고 그 다음 전진인데 자꾸 파란칸이라면 그자리에서 바꾸고 이동을 해서 꽤나 고생했다.
// 잘 읽어보면 이동 칸이 블루이면 방향을 바꾼뒤 한칸 이동하는 칸이 경계 안이고 파란칸이 아니라면 그대로 인데, 자꾸 제자리에서 한칸 이동을 하려고 해서 오래 거렸다.
    else if (Ms == 2)
    {
        int Dir = Reverse_Dir(Chess_Num);
        Chess[Chess_Num].dir = Dir;
        int nnx = x + dx[Dir];
        int nny = y + dy[Dir];
        
        if (nnx >= 0 && nny >= 0 && nnx < N && nny < N)
        {
            if (MAP[nnx][nny] != 2) MoveChess(x, y, nnx, nny, Chess_Num, Pos, MAP[nnx][nny]);
        }
    }
    
    //체스판의 경계를 넘어가는 것도 같은 파란칸이라고 친다.
```

### 전체 코드
###### 다른 블로그의 도움을 받았다. 나중에 다시 내가 풀어버린다.

```c++
#include<iostream>
#include<vector>
 
#define endl "\n"
#define MAX 12
#define CHESS_MAX 10
using namespace std;
 
struct CHESS
{
    int x;
    int y;
    int dir;
};
 
int N, K;
int MAP[MAX][MAX];
vector<int> MAP_State[MAX][MAX];
CHESS Chess[CHESS_MAX];
 
const int dx[] = { 0, 0, 0, -1, 1 };
const int dy[] = { 0, 1, -1, 0, 0 };

int Find_Delete_Num(int x, int y, int Chess_Num)
{
    /* 해당 말을 옮긴 후, 기존 좌표에서 몇 번 삭제를 해야 하는지 찾는 함수. */
    int Cnt = 0;
    for (int i = MAP_State[x][y].size() - 1; i >= 0; i--)
    {
        if (MAP_State[x][y][i] == Chess_Num) break;
        Cnt++;
    }
    return Cnt + 1;
}
 
int Reverse_Dir(int Num)
{
    int Dir = Chess[Num].dir;
    if (Dir == 1) return 2;
    else if (Dir == 2) return 1;
    else if (Dir == 3) return 4;
    else if (Dir == 4) return 3;
}
 
void MoveChess(int x, int y, int nx, int ny, int Chess_Num, int Pos, int Ms)
{
    if (Ms == 0)
    {
        for (int i = Pos; i < MAP_State[x][y].size(); i++)
        {
            MAP_State[nx][ny].push_back(MAP_State[x][y][i]);
            int Idx = MAP_State[x][y][i];
            Chess[Idx].x = nx;
            Chess[Idx].y = ny;
        }
        int Delete_Num = Find_Delete_Num(x, y, Chess_Num);
        for (int i = 0; i < Delete_Num; i++) MAP_State[x][y].pop_back();
    }
    else if (Ms == 1)
    {
        for (int i = MAP_State[x][y].size() - 1; i >= Pos; i--)
        {
            MAP_State[nx][ny].push_back(MAP_State[x][y][i]);
            int Idx = MAP_State[x][y][i];
            Chess[Idx].x = nx;
            Chess[Idx].y = ny;
        }
        int Delete_Num = Find_Delete_Num(x, y, Chess_Num);
        for (int i = 0; i < Delete_Num; i++) MAP_State[x][y].pop_back();
    }
    else if (Ms == 2)
    {
        int Dir = Reverse_Dir(Chess_Num);
        Chess[Chess_Num].dir = Dir;
        int nnx = x + dx[Dir];
        int nny = y + dy[Dir];
        
        if (nnx >= 0 && nny >= 0 && nnx < N && nny < N)
        {
            if (MAP[nnx][nny] != 2) MoveChess(x, y, nnx, nny, Chess_Num, Pos, MAP[nnx][nny]);
        }
    }
}
 
int Find_Position(int x, int y, int Idx)
{
    /* 해당 말이 몇 번째에 위치하고 있는지 찾아서 return 하는 함수. */
    for (int i = 0; i < MAP_State[x][y].size(); i++)
    {
        if (MAP_State[x][y][i] == Idx) return i;
    }
}
 
bool Check_State()
{
    for (int i = 0; i < K; i++)
    {
        int x = Chess[i].x;
        int y = Chess[i].y;
        if (MAP_State[x][y].size() >= 4) return true;
    }
    return false;
}


int main()
{
	cin >> N >> K;
    for (int i = 0; i < N; i++)
    {
        for (int j = 0; j < N; j++)
        {
            cin >> MAP[i][j];
        }
    }
    for (int i = 0; i < K; i++)
    {
        int x, y, d; cin >> x >> y >> d;
        x--; y--;
        Chess[i] = { x, y, d };
        MAP_State[x][y].push_back(i);
    }
    
	bool Flag = false;
    int Time = 0;
    while (1)
    {
        if (Time > 1000) break;
 
        for (int i = 0; i < K; i++)
        {
            int x = Chess[i].x;
            int y = Chess[i].y;
            int dir = Chess[i].dir;
 
            int nx = x + dx[dir];
            int ny = y + dy[dir];
 
            int Pos = Find_Position(x, y, i);
            if (nx >= 0 && ny >= 0 && nx < N && ny < N) MoveChess(x, y, nx, ny, i, Pos, MAP[nx][ny]);            
            else MoveChess(x, y, nx, ny, i, Pos, 2);
 
            if (Check_State() == true)
            {
                Flag = true;
                break;
            }
        }
        if (Flag == true) break;
        Time++;
    }
 
    if (Flag == true) cout << Time + 1 << endl;
    else cout << -1 << endl;
    
    return 0;
}
```


