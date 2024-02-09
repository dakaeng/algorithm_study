# Chapter 07 이진 탐색

# 1. 범위를 반씩 좁혀가는 탐색

### 순차 탐색

최악의 시간 복잡도 : $O(N)$

리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 차례대로 확인하는 방법

정렬되지 않은 리스트에서 데이터를 찾아야 할 때 사용

### 이진 탐색 : 반으로 쪼개면서 탐색하기

시간 복잡도 : $O(logN)$

배열 내부의 데이터가 정렬되어 있어야만 사용할 수 있는 알고리즘

찾으려는 데이터와 중간점 위치에 있는 데이터를 반복적으로 비교해서 원하는 데이터를 찾는 것

```python
array = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19]
target = 17

start = 0
end = len(array) - 1
while (start <= end) :
	mid = (start + end) // 2
	if array[mid] < target :
		start = mid
	elif array[mid] > target :
		end = mid
	else :
		print('target의 index :', mid)
		break
```

### 이진 탐색 트리

- 부모 노드보다 왼쪽 자식 노드가 작다.
- 부모 노드보다 오른쪽 자식 노드가 크다.

왼쪽 자식 노드 < 부모 노드 < 오른쪽 자식 노드

# 2. [실전 문제] 부품 찾기

```python
N = int(input())
nums = list(map(int, input().split()))
M = int(input())
check = list(map(int, input().split()))
nums.sort()

for c in check :
	start = 0
	end = N - 1
	result = 'no'
	while (start <= end) :
		mid = (start + end) // 2
		if nums[mid] < c :
			start = mid + 1  # start = mid 로 하면 루프가 안 끝남
		elif nums[mid] > c :
			end = mid - 1  # 얘도 마찬가지
		else :
			result = 'yes'
			break
	print(result, end = ' ')
```

# 3. [실전 문제] 떡볶이 떡 만들기

```python
N, M = map(int, input().split())
tteok = list(map(int, input().split()))

start = 0
end = max(tteok)  # start, end를 index가 아니라 떡의 길이로 보기
result = 0
while (start <= end) :
	mid = (start + end) // 2  # 절단기 높이를 mid로 보기
	total = 0
	for i in tteok :
		if i > mid :
			total += (i - mid)
	if total < M :
		end = mid - 1
	else :
		result = mid
		start = mid + 1
print(result)
```

범위 내에서 조건을 만족하는 가장 큰 값을 찾으라는 최적화 문제라면 이진 탐색으로 해결 가능