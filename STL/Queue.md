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
