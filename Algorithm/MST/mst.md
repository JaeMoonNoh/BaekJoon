# 백준 최소스패닝트리 1197

## 문제

그래프가 주어졌을 때, 그 그래프의 최소 스패닝 트리를 구하는 프로그램을 작성하시오.

최소 스패닝 트리는, 주어진 그래프의 모든 정점들을 연결하는 부분 그래프 중에서 그 가중치의 합이 최소인 트리를 말한다.

## 입력

첫째 줄에 정점의 개수 V(1 ≤ V ≤ 10,000)와 간선의 개수 E(1 ≤ E ≤ 100,000)가 주어진다. 다음 E개의 줄에는 각 간선에 대한 정보를 나타내는 세 정수 A, B, C가 주어진다. 이는 A번 정점과 B번 정점이 가중치 C인 간선으로 연결되어 있다는 의미이다. C는 음수일 수도 있으며, 절댓값이 1,000,000을 넘지 않는다.

그래프의 정점은 1번부터 V번까지 번호가 매겨져 있고, 임의의 두 정점 사이에 경로가 있다. 최소 스패닝 트리의 가중치가 -2,147,483,648보다 크거나 같고, 2,147,483,647보다 작거나 같은 데이터만 입력으로 주어진다.

## 출력

첫째 줄에 최소 스패닝 트리의 가중치를 출력한다.

## 전체 코드


```c++

#include<iostream>
#include<vector>
#include<algorithm>

#define MAX 10000 + 1
using namespace std;

int parent[MAX]; // 부모 노드가 누구인지 알아내는 배열 
int setSize[MAX]; // 연결되어있는 값 

int findParent(int node)
{
	//node가 들어와서 자기의 부모를 찾는 과정
	
	if(parent[node] == node)
		return node;
		
	return parent[node] = findParent(parent[node]); // parent[node]의 node를 찾는것, parent[node] == node여야 한다. 
}

void merge(int node1, int node2)
{
	// parent[node]를 확인하고 각 노드를 합쳐주는 것
	
	node1 = findParent(node1);
	node2 = findParent(node2);
	
	if(node1 != node2)
	{
		if(setSize[node1] < setSize[node2]) // setSize크기의 변화 
			swap(node1, node2);
		
		parent[node2] = node1;
		setSize[node1] += setSize[node2];
		setSize[node2] = 0; 
	} 
} 

struct Edge{
	int u,v,weight;
	
	bool operator<(Edge const& e)
	{
		return weight < e.weight; // 간선 사이의 weight 비교, 오름차순으로 정렬 
	}
};

int main()
{
	int v,e;
	cin >> v >> e;
	
	vector<Edge> edge;
	
	for(int i = 0; i < e; i++)
	{
		int u,v,w;
		
		cin >> u >> v >> w;
		
		edge.push_back({u,v,w});
	}
	
	sort(edge.begin(), edge.end());
	
	for(int i = 0; i < v; i++)
	{
		parent[i] = i; // 초기에는 부모 그대로
		setSize[i] = 1; // 초기에는 node 하나씩 갖고있는 상태 
	}
	
	long long result = 0; // 범위는 long long 문제 한정
	
	for(int i = 0; i < edge.size(); i++)
	{
		Edge e = edge[i];
		
		if(findParent(e.u) != findParent(e.v))
		{
			result += e.weight;
			merge(e.u, e.v);
		}
	} 
	
	cout << result << "\n";
	return 0;
}

```
