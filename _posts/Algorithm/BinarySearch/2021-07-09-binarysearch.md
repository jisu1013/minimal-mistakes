---
layout: posts
title: Binary Search
comments: true
author_profile: true
use_math: true
categories: [Algorithm]
---

리스트 내에서 데이터를 매우 빠르게 탐색하는 이진 탐색 알고리즘
## Sequential Search (순차 탐색)
가장 기본 탐색 방법
리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 차례대로 확인하는 방법
정렬되지 않는 리스트에서 데이터를 찾아야 할 때 사용
리스트 내에 데이터가 아무리 많아도 시간만 충분하다면 항상 원하는 원소(데이터)를 찾을 수 있다는 장점
리스트에 특정 값의 원소가 있는지 체크할 때도 순차 탐색으로 원소 확인
 리스트 자료형에서 특정한 값을 가지는 원소의 개수를 세는 count( ) method를 이용할 때도 내부에서는 순차 탐색이 수행
 
### Code
```python
def sequential_search(n, target, array):
	for i in range(n):
		if array[i] == target:
			return i + 1
print("생성할 원소 개수를 입력한 다음 한 칸 띄고 찾을 문자열을 입력")
input_data = input().split()
n = int(input_data[0])
target = input_data[1]
print("앞서 적은 원소 개수만큼 문자열을 입력. 구분은 띄워쓰기 한 칸으로")
array = input().split()

print(sequential_search(n,target,array))
```
### 시간 복잡도
최악의 경우 시간 복잡도는 $O(N)$이다.

## Binary Search
배열 내부의 데이터가 정렬되어 있어야만 사용할 수 있는 알고리즘.
정렬되어 있는 경우에는 빠르게 데이터를 찾을 수 있다는 특징과 탐색 범위를 절반씩 좁혀 가며 데이터를 탐색하는 특징이 있다.
이진 탐색은 위치를 나타내는 변수 3개를 사용하는데 탐색하고자 하는 범위의 **시작점, 끝점, 중간점**이다.
**찾으려는 데이터와 Middle 위치에 있는 데이터를 반복적으로 비교**해서 원하는 데이터를 찾는게 이진 탐색 과정이다.

### 시간 복잡도
원소의 개수가 절반씩 줄어든다는 점에서 시간 복잡도가 $O(logN)$이다.

### Code
1. 재귀함수로 구현하는 방법

```python
def binary_search(array, target, start, end):
	if start > end:
		return None
	mid = (start+end) // 2
	if array[mid] == target:
		return mid
	elif array[mid] > target:
		return binary_search(array, target, start, mid-1)
	else:
		return binary_search(array, target, mid+1, end)

n, target = list(map(int, input().split()))
array = list(map(int, input().split()))

result = binary_search(array, target, 0, n-1)
if result == None:
	print("원소가 존재하지 않습니다")
else:
	print(result+1)
```

2. 반복문으로 구현하는 방법

```python
def binary_search(array, target, start, end):
	while start <= end:
		mid = (start+end) // 2
		if array[mid] == target:
			return mid
		elif array[mid] > target:
			end = mid - 1
		else:
			start = mid + 1
	return None
n, target = list(map(int, input().split()))
array = list(map(int, input().split()))
result = binary_search(array, target, 0, n-1)
if result == None:
	print("원소가 존재하지 않습니다")
else:
	print(result + 1)
```

## Binary Search Tree
왼쪽 자식 노드 < 부모 노드 < 오른쪽 자식 노드

## 특징
가급적 암기하자!!
탐색 범위가 큰 상황(2,000 만을 넘어가는 상황)에서는 이진 탐색으로 접근하자!!
데이터의 개수나 값이 1,000만 단위 이상으로 넘어가거나 탐색 범위의 크기가 1,000억 이상일 경우엔 
이진 탐색과 같이 $O(logN)$의 속도를 내야 하는 알고리즘을 떠올려야 문제를 풀 수 있다.

### 빠르게 입력 받기 위한 팁
입력 데이터의 개수가 많은 문제에 input( ) 함수를 사용하면 동작 속도가 느려서 시간 초과로 오답 판정을 받을 수 있다. 
이처럼 입력 데이터가 많은 문제는 sys library의 readline( ) 함수를 이용하면 시간 초과를 피할 수 있다.

```python
import sys
# 하나의 문자열 데이터 입력 받기
input_data = sys.stdin.readline().rstrip()
print(input_data)
```
