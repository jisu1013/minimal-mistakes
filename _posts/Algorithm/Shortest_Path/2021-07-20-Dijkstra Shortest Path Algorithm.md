---
layout: posts
title: Dijkstra Shortest Path Algorithm
comments: true
tags : [Algorithm,Shortest Path]
author_profile: true
use_math: true
categories: [Algorithm]
---

가장 짧은 경로를 찾는 알고리즘이다. 길찾기 문제!


1. **다익스트라 최단 경로 알고리즘**

2. **플로이드 워셜**

3. 벨만 포드 알고리즘

그리디 알고리즘과 다이나믹 프로그래밍 알고리즘이 최단 경로 알고리즘에 그대로 적용된다.

## 다익스트라 최단 경로 알고리즘
그래프에서 여러 개의 노드가 있을 때, 특정 노드에서 출발하여 다른 노드로 가는 각각의 최단 경로를 구해주는 알고리즘이다.
'음의 간선'이 없을 때 정상적으로 동작한다. 다익스트라 최단 경로 알고리즘은 기본적으로 그리디 알고리즘으로 분류된다. 
매번 '가장 비용이 적은 노드'를 선택해서 임의의 과정을 반복하기 때문이다. 

알고리즘의 원리는 다음과 같다.

1) 출발 노드를 설정
2) 최단 거리 테이블을 초기화
3) 방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택
4) 해당 노드를 거쳐 다른 노드로 가는 비용을 계산하여 최단 거리 테이블을 갱신
5) 3번과 4번을 반복 

최단 경로를 구하는 과정에서 '각 노드에 대한 현재까지의 최단 거리'정보를 항상 1차원 리스트에 저장하여 리스트를 계속 갱신한다는 특징.
매번 현재 처리하고 있는 노드를 기준으로 주변 간선을 확인.방문하지 않은 노드 중에서 가장 최단 거리가 짧은 노드를 선택하는 과정을 반복한다. 
이렇게 선택된 노드는 최단 거리가 완전히 선택된 노드이므로, 더 이상 알고리즘을 반복해도 최단 거리가 줄어들지 않는다.

### 방법 1. 간단한 다익스트라 알고리즘
간단한 다익스트라 알고리즘은 $O(V^2)$의 시간 복잡도를 가진다.

여기서 V는 노드의 개수를 의미한다. 직관적이고 쉽게 이해할 수 있다.

처음에 각 노드에 대한 최단 거리를 담는 1차원 리스트를 선언한다. 

이후에 단계마다 '방문하지 않은 노드 중에서 최단 거리가 가장 짧은 노드를 선택'하기 위해서 매 단계마다 1차원 리스트의 모든 원소를 확인한다.

```python
import sys
input = sys.stdin.readline
INF = int(1e9)

n,m = map(int, input().split())
start = int(input())
graph = [[] for i in range(n+1)]
visited = [False] * (n+1)
distance = [INF] * (n+1)

for _ in range(m):
	a,b,c = map(int, input().split())
	graph[a].append((b,c))

def get_smallest_node():
	min_value = INF
	index = 0
	for i in range(1, n+1):
		if distance[i] < min_value and not visited[i]:
			min_value = distance[i]
			index = i
	return index
	
def dijkstra(start):
	distance[start] = 0
	visited[start] = True
	for j in graph[start]:
		distance[j[0]] = j[1]
	for i in range(n-1):
		now = get_smallest_node()
		visited[now] = True
		for j in graph[now]:
			cost = distance[now] + j[1]
			if cost < distance[j[0]]:
				distance[j[0]] = cost
dijkstra(start)

for i in range(1, n+1):
	if distance[i] == INF:
		print("INFINITY")
	else:
		print(distance[i])
```
### 방법 2. 개선된 다익스트라 알고리즘

최악의 경우에도 시간 복잡도 $O(ElogV)$를 보장하여 해결할 수 있다.

V는 노드의 개수이고, E는 간선의 개수를 의미.

개선된 다익스트라 알고리즘에서는 Heap 자료구조를 사용.

이 과정에서 로그 시간이 거린다.

### 힙 설명
우선순위 큐(Priority Queue)를 구현하기 위하여 사용하는 자료구조

우선순위 큐는 우선순위가 가장 높은 데이터를 가장 먼저 삭제한다는 점이 특징이다. 

파이썬에서는 우선순위 큐가 필요할 때  PriorityQueue혹은 heapq를 사용할 수 있다.

PriorityQueue보다는 일반적으로 heapq가 더 빠르게 동작하기 때문에 

**수행 시간이 제한된 상황에서는 heapq를 사용하는 것을 권장.**

우선순위 큐를 구현할 때는 내부적으로 Min Heap 혹은 Max Heap을 이용한다. 

파이썬 라이브러리에서는 기본적으로 최소 힙 구조를 이용하는데 

**다익스트라 최단 경로 알고리즘에서는 비용이 적은 노드를 우선하여 방문하므로** 

**최소 힙 구조를 기반으로 하는 파이썬의 우선순위 큐 라이브러리를 그대로 사용**

**최소 힙을 최대 힙처럼 사용**하기 위해서 일부러 우선순위에 해당하는 값에

음수 부호(-)를 붙여서 넣었다가, 

나중에 우선순위 큐에서 꺼낸 다음에 다시 음수 부호(-)를 붙여서 

원래의 값으로 돌리는 방식을 사용할 수 있다.

### 우선순위 큐를 구현하는 방법
1. 리스트를 이용

삽입시간: $O(1)$

삭제시간:$O(N)$

2. 힙

삽입시간:$O(logN)$

삭제시간:$O(logN)$

```python
import heapq
import sys
input = sys.stdin.readline
INF = int(1e9)
n,m = map(int, input().split())
start = int(input())
graph = [[] for i in range(n+1)]
distance = [INF] * (n+1)

for _ in range(m):
	a,b,c = map(int, input().split())
	graph[a].append((b,c))

def dijkstra(start):
	q = []
	heapq.heappush(q, (0,start))
	distance[start] = 0
	while q:
		dist, now = heapq.heappush(q)
		if distance[now] < dist:
			continue
		for i in graph[now]:
			cost = dist + i[1]
			if cost < distance[i[0]]:
				distance[i[0]] = cost
				heapq.heappush(q, (cost,i[0]))
dijkstra(start)

for i in range(1,n+1):
	if distance[i] == INF:
		print("INFINITY")
	else:
		print(distance[i])
```

