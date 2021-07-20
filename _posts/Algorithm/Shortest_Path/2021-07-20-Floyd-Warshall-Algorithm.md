---
layout: posts
title: Dijkstra Shortest Path Algorithm
comments: true
tags : [Algorithm,Shortest Path]
author_profile: true
use_math: true
categories: [Algorithm]
---

모든 지점에서 다른 모든 지접까지의 최단 경로를 모두 구해야 하는 경우에 사용할 수 있는 알고리즘

단계마다 거쳐 가는 노드를 기준으로 알고리즘을 수행. 하지만 매번 방문하지 않은 노드 중에서 최단 거리를 찾을 필요가 없다는 점이 다르다.

플로이드 워셜 알고리즘의 시간 복잡도는 $O(N^3)$ 이다.

2차원 리스트에 '최단 거리' 정보를 저장한다는 특징

다익스트라 알고리즘은 그리디 알고리즘인데, 

플로이드 워셜 알고리즘은 다이나믹 프로그래밍이다.

현재 확인하고 있는 노드를 제외하고, N-1개의 노드 중에서 서로 다른 노드 (A, B) 쌍을 선택한다.

이후에 A->1번 노드->B 로 가는 비용을 확인한 뒤에 최단 거리를 갱신하다. 

다시 말해 $_{N-1}P_2$개의 쌍을 단계마다 반복해서 확인한다.

이때 $O(_{N-1}P_2)$는 $O(N^2)$이라고 볼 수 있기 때문에 전체 시간 복잡도는 $O(N^3)$이다. 

구체적인 점화식은 다음과 같다.

$D_{ab}=min(D_{ab}, D_{ak}+D_{kb})$

A에서 B로 가는 최소 비용과 A에서 K를 거쳐 B로 가는 비용을 비교하여 더 작은 값으로 갱신하겠다는 것이다. 

```python
INF = int(1e9)
n = int(input())
m = int(input())
graph = [[INF] * (n+1) for _ in range(n+1)]
for a in range(1, n+1):
	for b in range(1, n+1):
		if a == b:
			graph[a][b] = 0
for _ in range(m):
	a,b,c = map(int, input().split())
	graph[a][b] = c
for k in range(1, n+1):
	for a in range(1, n+1):
		for b in range(1, n+1):
			graph[a][b] = min(graph[a][b], graph[a][k]+graph[k][b])
for a in range(1, n+1):
	for b in range(1, n+1):
		if graph[a][b] == INF:
			print("INFINITY",end=" ")
		else:
			print(graph[a][b],end=" ")
	print()			
```
