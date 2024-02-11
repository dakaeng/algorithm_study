# Chapter 04 구현

# 1. 아이디어를 코드로 바꾸는 구현

### 피지컬로 승부하기

- 완전 탐색 : 모든 경우의 수를 주저 없이 다 계산하는 해결 방법
- 시뮬레이션 : 문제에서 제시한 알고리즘을 한 단계씩 차례대로 직접 수행

### 예제 4-1. 상하좌우

```python
N = int(input())
move = list(input().split())

answer = [1, 1]
for m in move :
	if m == 'L' :
		if answer[1] != 1 :
			answer[1] -= 1
	elif m == 'R' :
		if answer[1] != N :
			answer[1] += 1
	elif m == 'U' :
		if answer[0] != 1 :
			answer[0] -= 1
	else :
		if answer[0] != N :
			answer[0] += 1
print(*answer)
```

### 예제 4-2. 시각

```python
N = int(input())

count = 0
for h in range(N+1) :
	for m in range(60) :
		for s in range(60) :
			time = str(h) + str(m) + str(s)
			if '3' in time :
				count += 1
print(count)
```

# 2. [실전 문제] 왕실의 나이트

```python
loc = input()
X = {'a':1, 'b':2, 'c':3, 'd':4, 'e':5, 'f':6, 'g':7, 'h':8}
xy = [X[loc[0]], int(loc[1])]
count = 8
if xy[0] > 6 or xy[0] < 3 :
	count -= 2
if xy[1] > 6 or xy[1] < 3 :
	count -= 2
if xy[0] == 1 or xy[0] == 8 :
	count -= 1
if xy[1] == 1 or xy[1] == 8 :
	count -= 1
print(count)
```

# 3. [실전 문제] 게임 개발

```python
N, M = map(int, input().split())
A, B, d = map(int, input().split())
maps = []
for _ in range(N) :
  maps.append(list(map(int, input().split())))

# 방향에 따른 이동
dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

check = [[0] * M for _ in range(N)]  # 현재 칸이 가본 적 있는 칸인지를 체크
check[A][B] = 1
count = 0  # 한 칸에서 확인한 방향 개수
answer = 1  # 답

while True :
  # 1번 매뉴얼
  d -= 1
  if d == -1 :
    d = 3

  # 2번 매뉴얼
  temp_A = A + dx[d]
  temp_B = B + dy[d]
    
  if (maps[temp_A][temp_B] == 0) and (check[temp_A][temp_B] == 0) :
    A = temp_A
    B = temp_B
    check[A][B] = 1
    answer += 1
    count = 0
    continue
  else :
    count += 1
 
  if count == 4 :  # 네 방향 모두 체크한 경우
    temp_A = A - dx[d]
    temp_B = B - dy[d]
    if maps[temp_A][temp_B] == 0 :
      A = temp_A
      B = temp_B
    else :
      break 
    count = 0
print(answer
```