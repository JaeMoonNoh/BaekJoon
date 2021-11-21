# 큐(queue) / 우선순위 큐(Priority_queue)

기본적인 설명은 제외하고 코딩 테스트를 보면서 배운 여러가지 내용입니다.

> 비교연산자 만들어서 사용하기

```c++

#include<queue>

struct compare{
	bool operator()(pair<int,int> a, pair<int,int> b)
	{
		if(a.first == b.first)
			return a.second > b.second;
			
		return a.first > b.first;
	}
};

//a.first == b.first의 값이 같다면 a.second > b.second로 정렬을 한다.

```

> 우선순위 큐(pair<int,int>)

```c++

priority_queue<pair<int,int>, vector<pair<int,int> >,  compare> pq;
//위 compare 비교연산자 사용

```

> 우선순위 큐(오름차순)

```c++

priority_queue<int, vector<int>, greater<int> > pq; // ASC 

pq.push(); // pq에 삽입
pq.top(); // pq가장 앞에 있는 값
pq.pop(); // pq 값 빼기
```

> 우선순위 큐(내림차순)

```c++

priority_queue<int, vector<int>, less<int> > pq; // DESC

```

> 큐

```c++

queue<pair<int,int> > q;

q.push(); // q에 삽입
q.pop(); // q값 빼기
```

### 전체 코드


```c++

/*
queue -> 1. queue 2. deque 3. priority_queue

*/

#include<iostream>
#include<queue> // include
#include<map>

using namespace std;

struct compare{
	bool operator()(pair<int,int> a, pair<int,int> b)
	{
		if(a.first == b.first)
			return a.second > b.second;
			
		return a.first > b.first;
	}
};

queue<pair<int,int> > q;
deque<int> dq; // 앞뒤로 넣을수 있다는것 
priority_queue<pair<int,int>, vector<pair<int,int> >,  compare> pq;
priority_queue<int, vector<int>, greater<int> > d; // ASC 
priority_queue<int, vector<int>, less<int> > d1; // DESC

int main()
{
	q.push({1,1});
	q.push({2,2});
	q.push({3,3});
	
	pq.push({10,5});
	pq.push({3,2});
	pq.push({5,3});
	
	while(!q.empty())
	{
		int n = q.front().first;
		int m = q.front().first;
		q.pop();
		
		cout << n << " " << m << endl;
	}
	
	while(!pq.empty())
	{
		int n = pq.top().first;
		int m = pq.top().second;
		pq.pop();
		
		cout << n << " " << m << endl;
	}
	
	return 0;
}


```
