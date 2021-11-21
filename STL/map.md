# 맵(map)

기본적인 설명은 제외하고 코딩테스트에서 배웠던 것들 위주로 작성했습니다.

> key : string, value : vector

```c++

#include<unordered_map>

unordered_map<string, vector<int> > um_v; // unordered_map_vector_key

um_v["nohjaemoon"].push_back(11);
um_v["nohjaemoon"].push_back(9);
um_v["nohjaemoon"].push_back(4);
um_v["nohjaemoon"].push_back(8);
um_v["nohjaemoon"].push_back(10);
//들어간 값 key : nohjaemoon , value = {11,9,4,8,10}
```

> um_v의 value인 vector를 정렬하는 방법

```c++
#include<algorithm>

for(auto &c : um_v)
	ort(c.second.begin(),c.second.end()); // auto &c == um_v이고 c.second.begin() ~ c.second.end()
	

//sort( ~.begin(), ~.end()) 사용

```
> um_v의 정렬된 value값

```c++

for(int i = 0; i < um_v["nohjaemoon"].size(); i++)
{
	cout << um_v["nohjaemoon"][i] << endl; // [i]로 하나씩 접근
}

```


> key : string, value : queue


```c++

unordered_map<string, queue<int> > um_q;  // unordered_map_queue_key

um_q["developer"].push(5);
um_q["developer"].push(2);
um_q["developer"].push(3);
um_q["developer"].push(4);
um_q["developer"].push(1);
// key : developer , value = queue<int> {5,2,3,4,1}

```

> queue값인 value를 하나씩 뽑아내기 위함


```c++

while(!um_q["developer"].empty())
{
	int v = um_q["developer"].front();
	um_q["developer"].pop();
	
	//cout << v << endl;
} 

```

> key : string, value : priority_queue

```c++

unordered_map<string, priority_queue<int> > um_pq; // unordered_map_priority_queue_key

```

> priority_queue 값 빼내기

```c++

while(!um_pq["njm"].empty())
{	
	int z = um_pq["njm"].top();
	um_pq["njm"].pop();
		
	cout << z << endl;
}

```

### 전체 코드


```c++

#include<iostream>
#include<vector>
#include<map>
#include<unordered_map>
#include<queue>
#include<algorithm>

using namespace std;

unordered_map<string, vector<int> > um_v; // unordered_map_vector_key
unordered_map<string, queue<int> > um_q;  // unordered_map_queue_key
unordered_map<string, priority_queue<int> > um_pq;

int main()
{
	um_v["nohjaemoon"].push_back(11);
	um_v["nohjaemoon"].push_back(9);
	um_v["nohjaemoon"].push_back(4);
	um_v["nohjaemoon"].push_back(8);
	um_v["nohjaemoon"].push_back(10);
	
	um_q["developer"].push(5);
	um_q["developer"].push(2);
	um_q["developer"].push(3);
	um_q["developer"].push(4);
	um_q["developer"].push(1);
	
	
	um_pq["njm"].push(1);
	um_pq["njm"].push(3);
	
	um_pq["njm"].push(5);
	um_pq["njm"].push(2);
	
	um_pq["njm"].push(6);
	um_pq["njm"].push(8);
	
	for(auto &c : um_v)
	{
		sort(c.second.begin(),c.second.end());
	}
	
	for(int i = 0; i < um_v["nohjaemoon"].size(); i++)
	{
		cout << um_v["nohjaemoon"][i] << endl;
	}
	
	while(!um_q["developer"].empty())
	{
		int v = um_q["developer"].front();
		um_q["developer"].pop();
		
		cout << v << endl;
	} 
	
	if(um_q["developer"].size() == 0)
		cout << "empty!" << endl;
	
	while(!um_pq["njm"].empty())
	{
		int z = um_pq["njm"].top();
		um_pq["njm"].pop();
		
		cout << z << endl;
	}
	
	return 0;
}


```


### map iterator

> iterator 접근시 map에 key와 value가 존재

```c++

map<string,int> m;
map<string,int> :: iterator it;
	
m["a"] = 1;
m["b"] = 2;
m["c"] = 3;
m["d"] = 4;

for(it = m.begin(); it != m.end(); it++)
{
	cout << it->first << endl; // map 의 key 값 first로 표현	
	cout << it->second << endl; // map의 value 값 second로 표현이된다.
}


```

### 잡기술1

> map의 값을 vector에 한 번에 삽입하는 방식

```c++
map<string,int> m;
vector<pair<string,int> > vec(m.begin(),m.end());
	
for(int i = 0; i < vec.size(); i++)
	cout << vec[i].first << " " << vec[i].second << endl;
// 출력 결과 map에 저장되어 있는 상태로 나옴
// map은 값 저장시 자동으로 key값 기준으로 정렬이 되기 때문에 정렬된 상태로 출력

```
