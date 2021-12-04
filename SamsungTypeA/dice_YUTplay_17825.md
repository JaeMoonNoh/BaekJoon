# 백준 주사위 윷놀이 17825 
[주사위 윷놀이](https://www.acmicpc.net/problem/17825)

### 문제

주사위 윷놀이는 다음과 같은 게임판에서 하는 게임이다.

[그림생략]

* 처음에는 시작 칸에 말 4개가 있다.
* 말은 게임판에 그려진 화살표의 방향대로만 이동할 수 있다. 말이 파란색 칸에서 이동을 시작하면 파란색 화살표를 타야 하고, 이동하는 도중이거나 파란색이 아닌 칸에서 이동을 시작하면 빨간색 화살표를 타야 한다. 말이 도착 칸으로 이동하면 주사위에 나온 수와 관계 없이 이동을 마친다.
* 게임은 10개의 턴으로 이루어진다. 매 턴마다 1부터 5까지 한 면에 하나씩 적혀있는 5면체 주사위를 굴리고, 도착 칸에 있지 않은 말을 하나 골라 주사위에 나온 수만큼 이동시킨다.
* 말이 이동을 마치는 칸에 다른 말이 있으면 그 말은 고를 수 없다. 단, 이동을 마치는 칸이 도착 칸이면 고를 수 있다.
* 말이 이동을 마칠 때마다 칸에 적혀있는 수가 점수에 추가된다.
주사위에서 나올 수 10개를 미리 알고 있을 때, 얻을 수 있는 점수의 최댓값을 구해보자.

### 입력

첫째 줄에 주사위에서 나올 수 10개가 순서대로 주어진다.

### 출력

얻을 수 있는 점수의 최댓값을 출력한다.

### 구현해야할 조건

* 윷놀이 판을 구현 (map 초기화/설정)

* 해당 노드에 다음 이동할 노드 번호를 저장 (map 배열)

* 방향 전환이 되는 노드를 따로 관리 (turn 배열)

* 각각의 노드의 점수를 관리 (score 배열)

* 해당 노드에 말이 있는지 없는지 확인 배열 (check 배열)


출처
[안산학생](https://haejun0317.tistory.com/163) 

스스로 다시 풀어봐야 할 것 같다.

### 전체코드


```c++
#include<iostream>

using namespace std;

int dice[10];
int horse[4];

int map[35];
int turn[35];
bool check[35];
int score[35];

int ans = 0;

void DFS(int cnt, int sum)
{
	if(cnt == 10)
	{
		if(sum > ans)
			ans = sum;
			
		return ;
	}
	
	for(int i = 0; i < 4; i++)
	{
		int pre = horse[i];
		int now = pre;
		int move = dice[cnt];
		
		if(turn[now] > 0)
		{
			now = turn[now];
			move -= 1;
		}
		
		while(move--)
		{
			now = map[now];
		}
		
		if(now != 21 && check[now] == true)
			continue;
		
		check[pre] = false;
		check[now] = true;
		horse[i] = now;
		
		DFS(cnt + 1, sum + score[now]);
		
		horse[i] = pre;
		check[pre] = true;
		check[now] = false;
	}
}

int main()
{
	for(int i = 0; i < 21; i++)
	{
		map[i] = i + 1;
	}
	
	map[21] = 21;
	
	for(int i = 22; i < 27; i++)
	{
		map[i] = i + 1;
	}
	
	map[28] = 29; map[29] = 30; map[30] = 25;
	map[31] = 32; map[32] = 25; map[27] = 20;
	
	turn[5] = 22; turn[10] = 31; turn[15] = 28;
    turn[25] = 26;
 
    for (int i = 0; i < 21; i++) 
	{
		score[i] = i * 2;		
	}
	
    score[22] = 13; score[23] = 16; score[24] = 19;
    score[31] = 22; score[32] = 24; score[28] = 28;
    score[29] = 27; score[30] = 26; score[25] = 25;
    score[26] = 30; score[27] = 35;
	
	for(int i = 0; i < 10; i++)
		cin >> dice[i];
		
	DFS(0,0);
	
	cout << ans << "\n";
	
	return 0;
}
```
