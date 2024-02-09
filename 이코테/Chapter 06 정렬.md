# Chapter 06 정렬

# 1. 기준에 따라 데이터를 정렬

### 선택 정렬

평균 시간 복잡도 : $O(N^{2})$

데이터가 무작위로 여러 개 있을 때, 이 중에서 가장 작은 데이터를 선택해 맨 앞에 있는 데이터와 바꾸고, 그다음 작은 데이터를 선택해 앞에서 두 번째 데이터와 바꾸는 과정을 반복하는 것

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

for i in range(len(array)) :
	min_idx = i  # min_idx : 최솟값의 인덱스
	for j in range(i+1, len(array)) :  # 최솟값의 인덱스를 찾는 과정
		if array[min_idx] > array[j] :
			min_idx = j
	temp = array[i]  # 현재 값(i번째 값)과 최솟값의 위치 바꾸기
	array[i] = array[min_idx]
	array[min_idx] = temp
	# array[i], array[min_idx] = array[min_idx], array[i]
print(array)
```

### 삽입 정렬

평균 시간 복잡도 : $O(N^{2})$

데이터가 거의 정렬되어 있는 상태에서 입력이 주어지는 문제라면 효율적!

데이터를 하나씩 확인하며, 각 데이터를 적절한 위치에 삽입하는 것

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

for i in range(len(array)) :
	for j in range(i, 0, -1) :
		if array[j] < array[j-1] :
			array[j], array[j-1] = array[j-1], array[j]
		else :
			break
print(array)
```

### 퀵 정렬

평균 시간 복잡도 : $O(Nlog{N})$

데이터 개수가 많을수록 선택 정렬, 삽입 정렬에 비해 빠르게 동작

데이터의 특성을 파악하기 어려운 경우에는 퀵 정렬 사용이 유리

기준을 설정한 다음, 기준값보다 큰 수와 작은 수를 교환한 후 리스트를 반으로 나누는 방식으로 동작

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

def quick_sort(array) :
	if len(array) <= 1 :
		return array

	pivot = array[0]
	tail = array[1:]

	left_side = [x for x in tail if x <= pivot]
	right_side = [x for x in tail if x > pivot]

	return quick_sort(left_side) + [pivot] + quick_sort(right_side)

print(quick_sort(array))
```

### 계수 정렬

평균 시간 복잡도 : $O(N+K)$

특정한 조건이 부합할 때만 사용할 수 있지만 매우 빠른 정렬 알고리즘(데이터 크기가 제한되어 있을 때)

별도의 리스트를 선언하고, 그 안에 정렬에 대한 정보를 담는다

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 9, 1, 4, 8, 0, 5, 2]

count = [0] * (max(array) + 1)
for i in range(len(array)) :
	count[array[i]] += 1

for i in range(len(count)) :
	for j in range(count[i]) :
		print(i, end = ' ')
```

### 파이썬 라이브러리

### sorted()

병합 정렬 기반. 최악의 경우에도 시간 복잡도 $O(NlogN)$을 보장함

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]
result = sorted(array)
print(result)
```

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]
array.sort()
print(array)
```

### 문제 풀이 팁!!

문제에서 별도의 요구가 없을 경우, 단순히 정렬해야 하는 상황이면 **기본 정렬 라이브러리** 사용!!

데이터의 **범위가 한정**되어 있으며, 더 **빠르게** 동작해야 할 때는 계수 정렬 사용

# 2. [실전 문제] 위에서 아래로

```python
N = int(input())
nums = []
for _ in range(N) :
	nums.append(int(input()))
result = sorted(nums, reverse = True)
for i in result :
	print(i, end = ' ')
```

# 3. [실전 문제] 성적이 낮은 순서로 학생 출력하기

```python
N = int(input())
student = []
for _ in range(N) :
	A, B = input().split()
	student.append((A, int(B)))
result = sorted(student, key = lambda x: x[1])
for i in result :
	print(i[0], end = ' ')
```

# 4. [실전 문제] 두 배열의 원소 교체

```python
N, K = map(int, input().split())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

A.sort()
B.sort(reverse = True)
for i in range(K) :
	if A[i] < B[i] :
		A[i], B[i] = B[i], A[i]
	else :
		break
print(sum(A))
```