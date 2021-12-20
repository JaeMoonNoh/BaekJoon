# 백준 컨베이어 벨트 위의 로봇 20055

## 문제

길이가 N인 컨베이어 벨트가 있고, 길이가 2N인 벨트가 이 컨베이어 벨트를 위아래로 감싸며 돌고 있다. 벨트는 길이 1 간격으로 2N개의 칸으로 나뉘어져 있으며, 각 칸에는 아래 그림과 같이 1부터 2N까지의 번호가 매겨져 있다.



벨트가 한 칸 회전하면 1번부터 2N-1번까지의 칸은 다음 번호의 칸이 있는 위치로 이동하고, 2N번 칸은 1번 칸의 위치로 이동한다. i번 칸의 내구도는 Ai이다. 위의 그림에서 1번 칸이 있는 위치를 "올리는 위치", N번 칸이 있는 위치를 "내리는 위치"라고 한다.

컨베이어 벨트에 박스 모양 로봇을 하나씩 올리려고 한다. 로봇은 올리는 위치에만 올릴 수 있다. 언제든지 로봇이 내리는 위치에 도달하면 그 즉시 내린다. 로봇은 컨베이어 벨트 위에서 스스로 이동할 수 있다. 로봇을 올리는 위치에 올리거나 로봇이 어떤 칸으로 이동하면 그 칸의 내구도는 즉시 1만큼 감소한다.

컨베이어 벨트를 이용해 로봇들을 건너편으로 옮기려고 한다. 로봇을 옮기는 과정에서는 아래와 같은 일이 순서대로 일어난다.

벨트가 각 칸 위에 있는 로봇과 함께 한 칸 회전한다.
가장 먼저 벨트에 올라간 로봇부터, 벨트가 회전하는 방향으로 한 칸 이동할 수 있다면 이동한다. 만약 이동할 수 없다면 가만히 있는다.
로봇이 이동하기 위해서는 이동하려는 칸에 로봇이 없으며, 그 칸의 내구도가 1 이상 남아 있어야 한다.
올리는 위치에 있는 칸의 내구도가 0이 아니면 올리는 위치에 로봇을 올린다.
내구도가 0인 칸의 개수가 K개 이상이라면 과정을 종료한다. 그렇지 않다면 1번으로 돌아간다.
종료되었을 때 몇 번째 단계가 진행 중이었는지 구해보자. 가장 처음 수행되는 단계는 1번째 단계이다.
## 입력

첫째 줄에 N, K가 주어진다. 둘째 줄에는 A1, A2, ..., A2N이 주어진다.

## 출력

몇 번째 단계가 진행 중일때 종료되었는지 출력한다.

## 전체문제


```c++
#include<iostream>
#include<vector>
#include<deque>

using namespace std;

deque<int> durablity;
deque<bool> conv;

int n,k,step;

int check()
{
    int count = 0;
    for (int i = 0; i < 2 * n; i++) {
        if (durablity[i] == 0)
            count++;
    }
    return count;
}

int main()
{
	step = 1;
	
	cin >> n >> k;
	
	for(int i = 0; i < 2 * n; i++)
	{
		int in;
		cin >> in;
		
		durablity.push_back(in);
		conv.push_back(false);
	}
	
	while(1)
	{
		conv.push_front(conv.back());
		conv.pop_back();
		
		durablity.push_front(durablity.back());
		durablity.pop_back();
		conv[n-1] = false;
		
		for(int i = n - 2; i >= 0; i--)
		{
			if((!conv[i+1]) && (durablity[i+1] > 0) && (conv[i]))
			{
				conv[i] = false;
				conv[i+1] = true;
				durablity[i+1]--;
			}
		}
		conv[n-1] = false;
		
		if(!conv[0] && durablity[0] > 0)
		{
			conv[0] = true;
			durablity[0]--;
		}
		
		int cnt = check();
		
		if(cnt >= k)
		{
			cout << step <<"\n";
			return 0;
		}
		step++;
		
	}
	
	return 0;
}
```