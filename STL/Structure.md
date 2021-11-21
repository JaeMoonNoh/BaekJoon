# Structure(구조체)

기초적인 것은 작성하지 않고 코딩테스트에서 쓰였던 형식위주로 작성했습니다.

> string, vector, priority_queue struct

```c++

typedef struct{
	string name;
	vector<int> score1;
	priority_queue<int> score2;
}Noh;

```

> name 출력

```c++

Noh n;

cout << n.name; 

```

> struct 내 vector 출력

```c++

for(int i = 0; i < n.score1.size(); i++)
{
	cout << n.score1[i] << " ";
} 

```

> struct 내 priority_queue 출력

```c++

while(!n.score2.empty())
{
	int z = n.score2.top();
	n.score2.pop();
		
	cout << z << " ";
}

```

> struct 내 vector 정렬


```c++

sort(n.score1.begin(),n.score1.end());

```

