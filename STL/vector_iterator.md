# vector iterator

코딩테스트에 나오는 방식 위주입니다.

> iterator 선언 방식

```c++

vector<int> :: iterator iter = vec.begin();
// iter 은 vec의 begin() 즉 맨 앞에 있는 값을 참조하고 있다.
// 주소값을 참조하고 있는 상태

```

> iterator 를 사용해 반복.

```c++

for(iter = vec.begin(); iter != vec.end(); iter++)
	cout << *iter << endl;
//iterator 반복자

```

> iterator를 사용해서 insert 기능

```c++

vector<int> :: iterator it = vec.begin();
vec.insert(it, 90);
//it 사용해서 insert 가능 (malloc 발생) 
	

for(it = vec.begin(); it != vec.end(); it++)
	cout << *it << endl; 
//출력 결과

```

> iterator를 이용해서 earse 기능

```c++

vector<int> :: iterator it2 = vec.begin();
vec.erase(it2); 
//it2 사용해서 erase 가능
	 
for(it2 = vec.begin(); it2 != vec.end(); it2++)
	cout << *it2 << endl;

```

> begin()은 맨 앞에 있는 값을 가리키지만, end()는 맨 마지막 값을 가리키는 것은 아니다.


### 전체 코드



```c++


/*

iterator에 대해서 

*/

#include<iostream>
#include<vector>
#include<set>

using namespace std;

vector<int> vec;

int main()
{
	vec.push_back(10);
	vec.push_back(20);
	vec.push_back(30);
	
	vector<int> :: iterator iter = vec.begin();
	
	for(iter = vec.begin(); iter != vec.end(); iter++)
		cout << *iter << endl;
	//iterator 반복자
	
	vector<int> :: iterator it = vec.begin();
	vec.insert(it, 90);
	//it 사용해서 insert 가능 (malloc 발생) 
	
	for(it = vec.begin(); it != vec.end(); it++)
		cout << *it << endl; 
	
	vector<int> :: iterator it2 = vec.begin();
	vec.erase(it2); 
	//it2 사용해서 erase 가능
	 
	for(it2 = vec.begin(); it2 != vec.end(); it2++)
		cout << *it2 << endl;
 	
	return 0;
} 


```
