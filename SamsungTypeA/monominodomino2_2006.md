# 백준 모노미노도미노 2 20061

[모노미노도미노 2](https://www.acmicpc.net/problem/20061)

## 문제

실제 문제에 들어가서 그림 보면서 이해하는게 빠르다.

## 입력

첫째 줄에 블록을 놓은 횟수 N(1 ≤ N ≤ 10,000)이 주어진다.

둘째 줄부터 N개의 줄에 블록을 놓은 정보가 한 줄에 하나씩 순서대로 주어지며, t x y와 같은 형태이다.

  * t = 1: 크기가 1×1인 블록을 (x, y)에 놓은 경우
  * t = 2: 크기가 1×2인 블록을 (x, y), (x, y+1)에 놓은 경우
  * t = 3: 크기가 2×1인 블록을 (x, y), (x+1, y)에 놓은 경우
블록이 차지하는 칸이 빨간색 칸의 경계를 넘어가는 경우는 입력으로 주어지지 않는다.

## 출력

첫째 줄에 블록을 모두 놓았을 때 얻은 점수를 출력한다.

둘째 줄에는 파란색 보드와 초록색 보드에서 타일이 들어있는 칸의 개수를 출력한다.

## 전체 코드

```c++
#include<iostream>
#include<algorithm>

#define Block 1
#define Empty 0
#define Green 0
#define Blue 1
#define Full 4
using namespace std;

int n,type,y,x,ans;
int area[2][6][4];

void drop(int type, int x, int color)
{
	int insert;
	
	if(type == 1 || type == 3)
	{
		for(insert = 0; insert < 6; insert++)
		{
			if(area[color][insert][x] != Empty)
				break;
		}
		insert--;
		area[color][insert][x] = Block;
		if(type == 3) // 3이면 그 위에 하나 올리는 반대로 생각하면 된다. 반대로 
			area[color][insert-1][x] = Block;
	}
	else
	{
		for(insert = 0; insert < 6; insert++)
		{
			if(area[color][insert][x] != Empty || area[color][insert][x+1] != Empty)
				break; // 옆으로 늘어지기 때문에, 둘중 하나라도 비어있지 않으면 멈춘다. 
		}
		
		insert--;
		area[color][insert][x] = Block;
		area[color][insert][x+1] = Block;
	}	
}

void remove(int color, int y)
{
	for(int r = y; r >= 0; r--)
	{
		if(r == 0)
		{
			for(int c = 0; c < 4; c++)
			{
				area[color][r][c] = Empty;
			}
			return;
		}
		for(int c = 0; c < 4; c++)
		{
			area[color][r][c] = area[color][r-1][c];
		}
	} // 딱히 움직이거나 중복이거나 그런게 없으면, 배열의 차원을 높여 
}

void StackBlock(int color)
{
	for(int r = 2; r < 6; r++)
	{
		int cnt = 0;
		for(int c = 0; c < 4; c++)
		{
			if(area[color][r][c] == Block)
				cnt++;
		}
		if(cnt == Full)
		{
			remove(color, r);
			ans++;
		}
	}
}

void checkLightArea(int color) 
{
    for (int r = 0; r <= 1; ++r) 
	{
        for (int c = 0; c < 4; ++c) 
		{
            if (area[color][r][c] == Block) 
			{
                remove(color, 5);
                break;
            }
        }
    }
}

int main()
{
	cin >> n;
	
	for(int i = 1; i <= n; i++)
	{
		cin >> type >> y >> x;
		drop(type,x,Green);
		if(type == 1) drop(1, 4 - y - 1, Blue);
		else if(type == 2) drop(3, 4 - y - 1, Blue); // 2가 3으로 바뀐다. 
		else drop(2, 4 - (y+1) - 1, Blue);// 2로 바뀌기 때문에, 한칸 더 밀어서 2를 만들어야 된다. 
		
		StackBlock(Green);
		StackBlock(Blue);
		
		checkLightArea(Green);
		checkLightArea(Blue);
	}
	
	cout << ans << "\n";
	
	int blockCnt = 0;
	
	for(int color = Green; color <= Blue; color++)
	{
		for(int r = 2; r < 6; r++)
		{
			for(int c = 0; c < 4; c++)
			{
				if(area[color][r][c] == Block)
					blockCnt++;
			}
		}
	}
	
	cout << blockCnt << "\n";
	
	return 0;
}

```
