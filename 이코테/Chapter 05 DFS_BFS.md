# Chapter 05 DFS/BFS

# 1. 꼭 필요한 자료구조 기초

- 자료구조 : 데이터를 표현하고 관리하고 처리하기 위한 구조

### 스택(Stack)

- 선입후출(ex. 박스쌓기)
- 삽입(5) - 삽입(2) - 삽입(3) - 삽입(7) - 삭제() - 삽입(1) - 삽입(4) - 삭제()

```python
stack = []

stack.append(5)
stack.append(2)
stack.append(3)
stack.append(7)
stack.pop()
stack.append(1)
stack.append(4)
stack.pop()

print(stack)
```

### 큐

- 선입선출(ex. 대기 줄)
- 삽입(5) - 삽입(2) - 삽입(3) - 삽입(7) - 삭제() - 삽입(1) - 삽입(4) - 삭제()

```python
from collections import deque

queue = deque()

queue.append(5)
queue.append(2)
queue.append(3)
queue.append(7)
queue.popleft()
queue.append(1)
queue.append(4)
queue.popleft()

print(queue)
```

### 재귀 함수

- 자기 자신을 다시 호출하는 함수

```python
# 예제 : 종료 조건이 없으므로 무한히 반복
def recursive_function() :
  print('재귀 함수를 호출합니다.')
  recursive_function()

recursive_function()
```

```python
def recursive_function(i) :
  if i == 100 :
    return
  print(i, '번째 재귀 함수에서', i+1, '번째 재귀 함수를 호출합니다.')
  recursive_function(i+1)
  print(i, '번째 재귀 함수를 종료합니다.')

recursive_function(1)
```

- 재귀 함수는 내부적으로 스택 자료구조(선입후출)와 동일
    
    → 스택 자료구조 문제들은 대부분 재귀 함수를 이용해서 간편하게 구현 가능(ex. DFS)
    
- 팩토리얼 문제

```python
def factorial(n) :
  if n == 0 or n == 1 :
    return 1
  else :
    return n * factorial(n-1)

factorial(5)
```

# 2. 탐색 알고리즘 DFS/BFS

### DFS

- Depth-First Search. 깊이 우선 탐색
- 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘
- 그래프
    - 인접 행렬 방식 : 2차원 배열로 그래프의 연결 관계를 표현하는 방식
        - ‘2차원 리스트’로 구현 가능
    - 인접 리스트 방식 : 리스트로 그래프의 연결 관계를 표현하는 방식
        - 모든 노드에 연결된 노드에 대한 정보를 차례대로 연결하여 저장
        - ‘연결 리스트’를 이용해 구현 → 파이썬에서는 ‘2차원 리스트’로 구현 가능
    - 비교
        - 메모리 측면 : 인접 리스트 방식이 유리
        - 속도 측면 : 인접 행렬 방식이 유리
- 동작 방식
    1. 탐색 시작 노드를 **스택**에 삽입하고 방문 처리를 한다.
    2. 스택의 최상단 노드에 방문하지 않은 인접 노드가 있으면, 그 인접 노드를 스택에 넣고 방문 처리를 한다. 방문하지 않은 인접 노드가 없으면 스택에서 최상단 노드를 꺼낸다.
    3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복한다.
    
    ```python
    def dfs(graph, v, visited) :  # v : 현재 노드
      # 현재 노드 방문 처리
      visited[v] = True
      print(v, end = ' ')
      # 현재 노드와 연결된 노드 방문
      for i in graph[v] :
        if not visited[i] :
          dfs(graph, i, visited)
    
    graph = [
        [],
        [2, 3, 8],
        [1, 7],
        [1, 4, 5],
        [3, 5],
        [3, 4],
        [7],
        [2, 6, 8],
        [1, 7]
    ]
    
    visited = [False] * 9
    dfs(graph, 1, visited)
    ```
    

### BFS

- Breadth First Search. 너비 우선 탐색
- 가까운 노드부터 탐색하는 알고리즘
- 동작 방식
    1. 탐색 시작 노드를 **큐**에 삽입하고 방문 처리를 한다.
    2. 큐에서 노드를 꺼내 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입하고 방문 처리를 한다.
    3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복한다.
    
    ```python
    from collections import deque
    
    def bfs(graph, start, visited) :
      queue = deque([start])
      visited[start] = True
      # 큐가 빌 때까지 반복
      while queue :
        v = queue.popleft()
        print(v, end = ' ')
        for i in graph[v] :
          if not visited[i] :
            queue.append(i)
            visited[i] = True
    
    visited = [False] * 9
    bfs(graph, 1, visited)
    ```
    
    - 재귀 함수 이용 시 비효율적

# 3. [실전 문제] 음료수 얼려 먹기

- DFS

```python
N, M = map(int, input().split())
graph = []
for _ in range(N) :
  graph.append(list(map(int, input())))

def dfs(x, y) :
  if x < 0 or x >= N or y < 0 or y >= M :
    return False
  if graph[x][y] == 0 :
    graph[x][y] = 1  # 방문처리
    # 상하좌우 위치도 재귀적으로 호출
    dfs(x-1, y)
    dfs(x, y-1)
    dfs(x+1, y)
    dfs(x, y+1)
    return True
  return False

answer = 0
for i in range(N) :
  for j in range(M) :
    if dfs(i, j) == True :
      answer += 1
print(answer)
```

# 4. [실전 문제] 미로 탈출

- BFS

```python
from collections import deque

N, M = map(int, input().split())
maze = []
for _ in range(N) :
  maze.append(list(map(int, input())))

# (0, 0) -> (N-1, M-1)
# 상하좌우 이동
dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

def bfs(x, y) :
  queue = deque()
  queue.append([x, y])
  while queue :
    x, y = queue.popleft()
    for i in range(4) :
      nx = x + dx[i]
      ny = y + dy[i]
      if nx < 0 or nx >= N or ny < 0 or ny >= M :
        continue
      if maze[nx][ny] == 0 :
        continue
      if maze[nx][ny] == 1 :
        maze[nx][ny] = maze[x][y] + 1
        queue.append([nx, ny])
  return maze[N-1][M-1]

print(bfs(0, 0))
```