# Chapter 08 다이나믹 프로그래밍

# 1. 다이나믹 프로그래밍

### 중복되는 연산을 줄이자

- 피보나치 함수
    - 점화식 → 재귀 함수로 구현
    - $a_{n+2} = a_{n+1} + a_{n}$
    
    ```python
    def fibo(x) :
      if x == 1 or x == 2 :
        return 1
      else :
        return fibo(x-1) + fibo(x-2)
    
    fibo(4)
    ```
    
    - $n$이 커질수록 수행 시간이 기하급수적으로 늘어나는 문제 (시간 복잡도 : $O(2^{N})$)
    - 다이나믹 프로그래밍 사용 시 효율적으로 해결 가능
- 다이나믹 프로그래밍을 사용할 수 있는 조건
    1. 큰 문제를 작은 문제로 나눌 수 있다.
    2. 작은 문제에서 구한 정답은 그것을 포함하는 큰 문제에서도 동일하다.
- 메모이제이션(Memoization) 기법 : Top-Down 방식
    - 한 번 구한 결과를 메모리 공간에 메모해두고, 같은 식을 다시 호출하면 메모한 결과를 그대로 가져오는 기법
    
    ```python
    # 위의 피보나치 코드에 메모이제이션 적용
    # Top-Down 방식
    d = [0] * 100  # fibo[x]를 d[x]에 저장해놓기
    def fibo(x) :
      if x == 1 or x == 2 :
        return 1
      if d[x] != 0 :
        return d[x]
      d[x] = fibo(x-1) + fibo(x-2)
      return d[x]
    
    fibo(99)
    ```
    
    - 다이나믹 프로그래밍 적용 시 피보나치 수열 알고리즘의 시간 복잡도 : $O(N)$
- 반복문 사용 : Bottom-Up 방식
    - 전형적인 다이나믹 프로그래밍의 형태

```python
# Bottom-Up 방식
d = [0] * 100

d[1] = 1
d[2] = 1
n = 99
for i in range(3, n+1) :
  d[i] = d[i-1] + d[i-2]

print(d[n])
```

- (중요) 다이나믹 프로그래밍 유형임을 파악하기 ❗
    - 완전 탐색 알고리즘으로 접근했을 때 시간이 매우 오래 걸리면, 다이나믹 프로그래밍을 적용할 수 있는지 확인해보기
        - 해결하고자 하는 **부분 문제들의 중복 여부** 확인

# 2. [실전 문제] 1로 만들기

- 2부터 X까지 연산 횟수를 answer에 저장
    
    ```python
    X = int(input())
    answer = [0] * (X+1)
    for i in range(2, X+1) :
      answer[i] = answer[i-1] + 1
      if i % 5 == 0 :
        answer[i] = min(answer[i], answer[i//5] + 1)
      if i % 3 == 0 :
        answer[i] = min(answer[i], answer[i//3] + 1)  # ex. 3 : 1을 먼저 빼면 연산 2번 필요. 3으로 먼저 나누면 연산 1번 필요
      if i % 2 == 0 :
        answer[i] = min(answer[i], answer[i//2] + 1)
    print(answer[X])
    ```
    

# 3. [실전 문제] 개미 전사

- 점화식 생각하기!!
    
    ```python
    N = int(input())
    K = list(map(int, input().split()))
    
    answer = [0] * N
    answer[0] = K[0]
    answer[1] = max(K[0], K[1])
    for i in range(2, N) :
      answer[i] = max(answer[i-2] + K[i], answer[i-1])
    print(answer[N-1])
    ```
    

# 4. [실전 문제] 바닥 공사

- 점화식 : $a_{i} = a_{i-1} + a_{i-2} \times 2$
    
    ```python
    N = int(input())
    
    answer = [0] * (N+1)
    answer[1] = 1
    answer[2] = 3
    for i in range(3, N+1) :
      answer[i] = (answer[i-1] + answer[i-2]*2) % 796796
    
    print(answer[N])
    ```
    
    - 문제 보고 점화식을 세우는게 가장 중요!!

# 5. [실전 문제] 효율적인 화폐 구성

- 점화식
    - $a_{i}$ : 금액 $i$를 만들 수 있는 최소한의 화폐 개수. $k$ : 화폐의 단위
    - $a_{i-k}$를 만드는 방법이 존재하는 경우 : $a_{i} = min(a_{i}, a_{i-k} + 1)$
    - $a_{i-k}$를 만드는 방법이 존재하지 않는 경우 : $a_{i} = 10,001$
    
    ```python
    N, M = map(int, input().split())
    coins = []
    for _ in range(N) :
      coins.append(int(input()))
    
    answer = [10001] * (M+1)
    answer[0] = 0
    
    for i in range(N) :
      for j in range(coins[i], M+1) :
        if answer[j - coins[i]] != 10001 :
          answer[j] = min(answer[j], answer[j - coins[i]] + 1)
    
    if answer[M] == 10001 :
      print(-1)
    else :
      print(answer[M])
    ```