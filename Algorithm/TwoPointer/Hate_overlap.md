# 백준 겹치는건 싫어 20922

[겹치는건 싫어](https://www.acmicpc.net/problem/20922)

## 문제
홍대병에 걸린 도현이는 겹치는 것을 매우 싫어한다. 특히 수열에서 같은 원소가 여러 개 들어 있는 수열을 싫어한다. 도현이를 위해 같은 원소가 $K$개 이하로 들어 있는 최장 연속 부분 수열의 길이를 구하려고 한다.

 $100\,000$ 이하의 양의 정수로 이루어진 길이가 $N$인 수열이 주어진다.  이 수열에서 같은 정수를 $K$개 이하로 포함한 최장 연속 부분 수열의 길이를 구하는 프로그램을 작성해보자.

## 입력
첫째 줄에 정수 $N$ ($1 \le N \le 200\,000$)과 $K$ ($1 \le K \le 100$)가 주어진다.

둘째 줄에는 ${a_1, a_2, ... a_n}$이 주어진다 ($1 \le a_i \le 100\,000$)

## 출력
조건을 만족하는 최장 연속 부분 수열의 길이를 출력한다.


## 전체 코드


```c++

#include<iostream>
#include<map>
#include<queue>
#include<vector>

#define MAX 200001
using namespace std;

int n,k;
int num;

int solution(int k, vector<int> hongdae)
{
	queue<int> q; // 전체의 길이와 그리고 이전 순차적인 값을 확인하기 위한 queue 자료구조 
	map<int,int> m; // 어떤 숫자가 몇번 들어왔는지 확인해주는 key,value 자료구조 
	int result = 0; // 결과 
	int from = 0; // 시작점은 항상 0번째 인덱스 부터 시작 
	
	for(int i = 0; i < hongdae.size(); i++)
	{
		if(m[hongdae[i]] == 0) //map 사용 
			m[hongdae[i]] = 1;
		else
			m[hongdae[i]] += 1;
			
		q.push(hongdae[i]);
		
		while(1)
		{
			if(m[hongdae[i]] > k) // m[hongdae[i]]에 값을 넣어서 해당하는 숫자의 cnt가 k값을 넘어 섰다면 
			{
				m[q.front()]--; // 순차적으로 들어왔던 숫자부터 차례로 빼주기 시작한다. 
				q.pop(); 
				from++; // 그에 시작점도 옮겨간다. 
			}
			else
				break;
		}
		
		result = max(result, i - from + 1); // 계속해서 최대값 업데이트 
	}
	return result;
}

int main()
{
	vector<int> hong;
	
	cin >> n >> k;
	
	for(int i = 0; i < n; i++)
	{
		cin >> num;
		hong.push_back(num);
	}
	
	cout << solution(k,hong);
	
	return 0;
}

```
