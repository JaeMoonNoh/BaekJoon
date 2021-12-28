# 백준 부분합 1806 
[부분합](https://www.acmicpc.net/problem/1806)

## 문제

10,000 이하의 자연수로 이루어진 길이 N짜리 수열이 주어진다. 이 수열에서 연속된 수들의 부분합 중에 그 합이 S 이상이 되는 것 중, 가장 짧은 것의 길이를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N (10 ≤ N < 100,000)과 S (0 < S ≤ 100,000,000)가 주어진다. 둘째 줄에는 수열이 주어진다. 수열의 각 원소는 공백으로 구분되어져 있으며, 10,000이하의 자연수이다.

## 출력

첫째 줄에 구하고자 하는 최소의 길이를 출력한다. 만일 그러한 합을 만드는 것이 불가능하다면 0을 출력하면 된다.

## 전체 코드

```c++
#include<iostream>
#include<algorithm>

#define MAX 100000
#define INF 987654321

using namespace std;

int n,s;
int arr[MAX];
vector<int> nums;

int main()
{
	cin >> n >> s;
	
	for(int i = 0; i < n; i++)
	{
		int num;
		cin >> num;
		nums.push_back(num);
	}
	
	int low = 0;
	int high = 0;
	int sum = nums[0];
	int result = INF;
	
	while(low <= high && high < n)
	{
		if(sum < s)
			sum += nums[++high];
			
		else if(sum == s)
		{
			result == min(result, (high - low) + 1);
			sum += nums[++high]; // 값이 같으면 계속해서 늘려주는 것 
		}
		else if(sum > s)
		{
			result = min(result, (high - low) + 1);
			sum -= nums[low++]; // 처음 값 빼주고, low 후위 연산 
		}
	}
	
	if(result == INF)
		cout << 0 << "\n";
	else
		cout << result << "\n";
	
	return 0;
}
```
