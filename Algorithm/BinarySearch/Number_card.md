# 백준 숫자카드 10815

[숫자 카드](https://www.acmicpc.net/problem/10815)

## 문제
숫자 카드는 정수 하나가 적혀져 있는 카드이다. 상근이는 숫자 카드 N개를 가지고 있다. 정수 M개가 주어졌을 때, 이 수가 적혀있는 숫자 카드를 상근이가 가지고 있는지 아닌지를 구하는 프로그램을 작성하시오.

## 입력
첫째 줄에 상근이가 가지고 있는 숫자 카드의 개수 N(1 ≤ N ≤ 500,000)이 주어진다. 둘째 줄에는 숫자 카드에 적혀있는 정수가 주어진다. 숫자 카드에 적혀있는 수는 -10,000,000보다 크거나 같고, 10,000,000보다 작거나 같다. 두 숫자 카드에 같은 수가 적혀있는 경우는 없다.

셋째 줄에는 M(1 ≤ M ≤ 500,000)이 주어진다. 넷째 줄에는 상근이가 가지고 있는 숫자 카드인지 아닌지를 구해야 할 M개의 정수가 주어지며, 이 수는 공백으로 구분되어져 있다. 이 수도 -10,000,000보다 크거나 같고, 10,000,000보다 작거나 같다

## 출력
첫째 줄에 입력으로 주어진 M개의 수에 대해서, 각 수가 적힌 숫자 카드를 상근이가 가지고 있으면 1을, 아니면 0을 공백으로 구분해 출력한다.

## 전체 코드

```c++
#include<iostream>
#include<algorithm>
#include<vector>

#define MAX 500001
using namespace std;

int n,m;
long long arr1[MAX]; // 상근이가 가지고 있는 숫자카드 
long long arr2[MAX]; // M가지 숫자 카드
vector<int> v;

int main()
{
	ios_base::sync_with_stdio(0);
    cin.tie(0); // 해당 백준문제는 이 내용이 들어가야하는데 cin,cout의 속도를 높여주는 것이다.
	
	cin >> n;
	
	for(int i = 0; i < n; i++)
		cin >> arr1[i];
		
	sort(arr1, arr1 + n);
	
	cin >> m;
	
	for(int i = 0; i < m; i++)
	{
		int num;
		cin >> num; // 갖고있는 카드의 num이 주어졌을때
		
		int start = 0;
		int end = n - 1; // 처음인덱스 0과 마지막 인덱스 n-1 사이에 값이 
		bool chk = false;
		
		while(start <= end)
		{
			int mid = (start + end) / 2;
			
			if(arr1[mid] == num) // num과 같거나
			{
				chk = true;
				break;
			}
			else if(arr1[mid] < num) // num보다 작으면
			{
				start = mid + 1; // 탐색 위치를 올려주고
			}
			else // num보다 크면
			{
				end = mid - 1; // 탐색 위치를 내려준다.
			}
		}
		
		if(chk)
			cout << 1 << " ";
		else
			cout << 0 << " ";
	}
	return 0;
} 
```
