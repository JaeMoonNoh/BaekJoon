# 백준 합이 0인 네 정수 7453
[합이 0인 네 정수](https://www.acmicpc.net/problem/7453)

## 문제

정수로 이루어진 크기가 같은 배열 A, B, C, D가 있다.

A[a], B[b], C[c], D[d]의 합이 0인 (a, b, c, d) 쌍의 개수를 구하는 프로그램을 작성하시오.

## 입력
첫째 줄에 배열의 크기 n (1 ≤ n ≤ 4000)이 주어진다. 다음 n개 줄에는 A, B, C, D에 포함되는 정수가 공백으로 구분되어져서 주어진다. 배열에 들어있는 정수의 절댓값은 최대 2^28이다.

## 출력
합이 0이 되는 쌍의 개수를 출력한다.

## 전체 코드


```c++
#include<iostream>
#include<vector>
#include<algorithm>

#define MAX 4000
using namespace std;

long long arr[4][MAX];

int main()
{
	int n;
	cin >> n;
	
	for(int i = 0; i < n; i++)
		for(int j = 0; j < 4; j++)
			cin >> arr[j][i]; // 수직으로 내려오던 값을 가로로 바꾸어 주고
			
	vector<long long> v;
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < n; j++)
		{
			v.push_back(arr[2][i] + arr[3][j]); // 벡터에 해당하는 값을 전부 집어넣어준다. 갯수를 구하는것이므로 상관이 없다. 무슨 숫자인지 구하라고 하면 상관이 있다.
		}
	}
	
	sort(v.begin(), v.end()); // lower_bound, upper_bound를 해주기 위해서 sort를 해줘야 한다.
	
	long long result = 0;
	
	for(int i = 0; i < n; i++)
	{
		for(int j = 0; j < n; j++)
		{
			long long half = arr[0][i] + arr[1][j]; // 이제 위에서와 같이 모든 조합을 알아보는 과정
			
			long long start = lower_bound(v.begin(), v.end(), -half) - v.begin(); // -half이상되는 곳이 있는지 찾기
			long long end = upper_bound(v.begin(), v.end(), -half) - v.begin(); // -half를 초과하는 곳이 있는지 찾기
			
			//if(-half == v[start])
			result += (end - start); // 전체 빼주기, 그거에 대한 조합이니까
		}
	}
	
	cout << result << "\n";
	
	return 0;
}
```
