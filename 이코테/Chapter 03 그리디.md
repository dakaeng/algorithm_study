# Chapter 03 그리디

# 1. 당장 좋은 것만 선택하는 그리디

현재 상황에서 지금 당장 좋은 것만 고르는 방법 (매 순간 가장 좋아 보이는 것을 선택하며, 현재의 선택이 나중에 미칠 영향에 대해서는 고려하지 않음)

특정한 문제를 만났을 때 단순히 현재 상황에서 가장 좋아 보이는 것만을 선택해도 문제를 풀 수 있는지를 파악할 수 있어야 함

= 문제에서 '가장 큰 순서대로', '가장 작은 순서대로'와 같은 기준을 알게 모르게 제시해줌

### 예제 3-1. 거스름돈

'가장 큰 화폐 단위부터' 돈을 거슬러 주는 것

```python
n = 1260
coins = [500, 100, 50, 10]
count = 0

for c in coins :
	count += n // c
	n = n % c
print(count)
```

화폐의 종류가 $K$개라고 할 때, 시간 복잡도는 $O(K)$

### 그리디 알고리즘의 정당성

대부분의 문제는 그리디 알고리즘을 이용했을 때 '최적의 해'를 찾을 수 없을 가능성이 다분함

대부분의 그리디 알고리즘 문제에서는 문제 풀이를 위한 최소한의 아이디어를 떠올리고 이것이 정당한지 검토할 수 있어야 답을 도출할 수 있음

# 2. [실전 문제] 큰 수의 법칙

```python
N, M, K = map(int, input().split())
nums = list(map(int, input().split()))

nums.sort(reverse = True)
first = nums[0]
second = nums[1]
count = 0
answer = 0
n = M // (K+1)
answer += first * K * n
answer += second * (M - K*n)

print(answer)
```

# 3. [실전 문제] 숫자 카드 게임

```python
N, M = map(int, input().split())
answer = 0
for _ in range(N) :
	nums = list(map(int, input().split()))
	answer = max(answer, min(nums))
print(answer)
```

# 4. [실전 문제] 1이 될 때까지

```python
N, K = map(int, input().split())
count = 0
while True :
	if N % K == 0 :
		N //= K
		count += 1
	else :
		N -= 1
		count += 1
	if N == 1 :
		break
print(count)
```
