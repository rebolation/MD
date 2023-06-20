---
date: 2021.10.01
title: 파이썬 자료구조
tag: 
---


# 파이썬 자료구조



### 스택/큐

스택은 등산 배낭에 짐을 넣는 것과 같습니다. 가장 마지막으로 넣은 짐을 가장 먼저 꺼낼 수 있습니다. 가장 처음에 넣었던 짐은 다른 모든 짐을 꺼낸 뒤에야 꺼낼 수 있습니다. 이 성질을 LIFO(Last In First Out)라고 부릅니다. 큐는 스택과 반대로 FIFO(First In First Out) 성질을 갖습니다. 파이썬에서는 리스트의 append(), pop(), pop(0) 을 활용하여 스택과 큐를 운영할 수 있습니다.

```python
stack = []
stack.append(1); print(stack) # [1]
stack.append(2); print(stack) # [1, 2]
stack.append(3); print(stack) # [1, 2, 3]
p = stack.pop(); print(stack, p) # [1, 2] 3 ... pop last: O(1)

queue = []
queue.append(1); print(queue) # [1]
queue.append(2); print(queue) # [1, 2]
queue.append(3); print(queue) # [1, 2, 3]
p = queue.pop(0); print(p, queue) # 1 [2, 3] ... pop intermediate: O(n) - deque 권장
```

하지만 리스트를 사용한 큐 구현에서 pop(0) 메소드는 O(n)이라는 비교적 낮은 성능을 갖기 때문에 collections 모듈의 deque(double-ended queue) 클래스를 사용하여 성능을 향상시킵니다. deque의 좌우 끝단 처리 메소드는 O(1)이라는 높은 성능을 갖습니다. maxlen 옵셔널 파라미터로 최대 용량을 설정할 수 있습니다.

```python
from collections import deque
queue = deque(maxlen=3)
queue.append(1); print(queue) # deque([1])
queue.append(2); print(queue) # deque([1, 2])
queue.append(3); print(queue) # deque([1, 2, 3]) ... append: O(1)
queue.appendleft(0); print(queue) # deque([0, 1, 2]) ... appendleft: O(1)
p = queue.pop(); print(p, queue) # 2 deque([0, 1]) ... pop: O(1)
p = queue.popleft(); print(p, queue) # 0 deque([1]) ... popleft: O(1) 
```



### 최소힙

힙은 모든 부모 노드가 자식보다 작거나 같은 값을 갖는 이진 트리입니다. 이를 이용하여 최소값이나 최대값을 빠르게 찾을 수 있습니다. 파이썬 heapq 모듈에 최소힙이 구현되어 있습니다. 최소힙에 원소들을 넣으면 (...복잡한 과정을 거쳐...) 힙트리의 루트에 항상 가장 작은 값이 오게 됩니다. 이러한 성질을 이용해 어떤 배열에 수시로 값이 추가될 때 매번 전체를 정렬하지 않고도 최소값을 빠르게 찾을 수 있습니다. 단, 주의할 것은 첫 번째 요소가 가장 작은 값일 뿐 힙 내부 전체가 정렬되어 있는 것은 아니라는 것입니다.

```python
import heapq # python/Lib/heapq.py
h = []; print(h) # []
heapq.heappush(h, 3); print(h) # [3]
heapq.heappush(h, 2); print(h) # [2, 3]
heapq.heappush(h, 1); print(h) # [1, 3, 2] - push할 때마다 루트(pop 대상)에는 최소값이 있음
p = heapq.heappop(h); print(p, h) # 1 [2, 3]
p = heapq.heappushpop(h, 1); print(p, h) # 1 [2, 3] - push 후 pop
p = heapq.heapreplace(h, 1); print(p, h) # 2 [1, 3] - pop 후 push
h = [3,2,1]; heapq.heapify(h); print(h) # [1, 2, 3] - 배열을 최소힙 배열로 변환(정렬 아님)
h = [[5,3],[1,9]]; heapq.heapify(h); print(h) # [[1, 9], [5, 3]]
```



### 최대힙

약간의 트릭을 사용하면 heapq의 성질을 그대로 이용하여 최대힙을 구현할 수 있습니다. 리스트의 각 요소에 마이너스 부호를 붙여주면 큰 값은 작아지고 반대로 작은 값은 커집니다. 이제 리스트를 힙으로 바꾼 후 가장 작은 값을 뽑아내어 다시 마이너스 부호를 붙이면 가장 큰 값이 됩니다.

```python
import heapq                                        # heapq
h = [1,3,2]; print(h)                               # [1, 3, 2]
h = [ -n for n in h ]; print(h)                     # [-1, -3, -2]
heapq.heapify(h); print(h)                          # [-3, -1, -2]
p = -heapq.heappop(h); print(p, h)                  # 3 [-2, -1]
p = -heapq.heappop(h); print(p, h)                  # 2 [-1]
p = -heapq.heappop(h); print(p, h)                  # 1 []
```

참고로 heapq 모듈에는 다음과 같은 메소드도 있습니다.

- merge : list(merge([1,3,5,7], [0,2,4,8], [5,10,15,20], [], [25])) # [0, 1, 2, 3, 4, 5, 5, 7, 8, 10, 15, 20, 25]
- nsmallest(n, iterable, key=None) : sorted(iterable, key=key)[:n]
- nlargest(n, iterable, key=None) : sorted(iterable, key=key, reverse=True)[:n]



### 그래프

그래프는 딕셔너리로 표현하면 직관적입니다. 키는 각 노드, 밸류는 각 노드에 연결된 노드 리스트입니다.

```python
'''
     A
    / \ 
   B   C
  /   /|\
 D   G H I
/ \      |
E F      J
'''
graph = { 
	'A': ['B', 'C'], 
	'B': ['A', 'D'], 
	'C': ['A', 'G', 'H', 'I'], 
	'D': ['B', 'E', 'F'], 
	'E': ['D'], 
	'F': ['D'], 
	'G': ['C'], 
	'H': ['C'], 
	'I': ['C', 'J'], 
	'J': ['I'], 
}
```



### 그래프 깊이우선탐색(DFS - Depth First Search)

그래프 깊이우선탐색을 위해 스택을 활용할 수 있습니다.

```python
def dfs_stack(graph):
    visited = []
    willvisit = ['A']
    while willvisit:
        node = willvisit.pop() # list.pop() : O(1), 높은 성능
        if node not in visited:
            visited.append(node)            
            willvisit.extend(reversed(graph[node])) # reversed()
    print(visited)
dfs_stack(graph) # ['A', 'B', 'D', 'E', 'F', 'C', 'G', 'H', 'I', 'J']
```



### 그래프 너비우선탐색(BFS - Breadth First Search)

그래프 너비우선탐색을 위해 큐를 활용할 수 있습니다.

```python
from collections import deque
def bfs_queue(graph):
    visited = []
    willvisit = deque(['A'])
    while willvisit:
        node = willvisit.popleft() # deque.popleft() : O(1), 높은 성능
        if node not in visited:
            visited.append(node)            
            willvisit.extend(graph[node]) # willvisit += graph[node]
    print(visited)
bfs_queue(graph) # ['A', 'B', 'C', 'D', 'G', 'H', 'I', 'E', 'F', 'J']
```



### 이분탐색(이진탐색, binary search)

이분탐색은 정렬된 리스트로부터 특정 값의 인덱스를 찾기 위해 사용합니다. 시작점을 첫 번째 인덱스, 끝점을 마지막 인덱스에 둔 후, 탐색을 마칠 때까지 다음 과정을 반복하여 시작점과 끝점을 조정합니다. 중간점은 시작점과 끝점의 중간을 의미합니다.

- 찾으려는 값이 중간점 값과 같으면 탐색 완료
- 찾으려는 값이 중간점 값보다 작으면 탐색 범위를 중간점의 왼쪽으로 조정 : 끝점 조정
- 찾으려는 값이 중간점 값보다 크면 탐색 범위를 중간점의 오른쪽으로 조정 : 시작점 조정

```python
def binarysearch(target, data):
    data.sort()
    left = 0 # 이분탐색 범위의 시작점(인덱스)
    right = len(data) - 1 # 이분탐색 범위의 끝점(인덱스)
    while left <= right: # 범위가 유효한 동안
        mid = (right + left) // 2 # 범위를 두 구간으로 나눌 중간점(인덱스)
        if data[mid] == target: # 찾으려는 데이터가 중간점에 있음
            return mid # 찾은 인덱스를 반환 - 종료
        elif data[mid] > target: # 찾으려는 데이터가 이분 구간의 왼쪽에 있음
            right = mid - 1 # 끝점을 수정하여 범위 조절
        else: # 찾으려는 데이터가 이분 구간의 오른쪽에 있음
            left = mid + 1 # 시작점을 수정하여 범위 조절
    return -1 # 찾으려는 데이터가 없음
print(binarysearch(50, [x * 10 for x in range(10)]))
```



### 피보나치 수열

1,1,2,3,5,8,... 로 시작하는 피보나치 수열은 앞의 두 수를 더한 값들로 계속 이어집니다. 파이썬에서 피보나치 수열을 구현하는 방법은 재귀를 사용하는 방법, 계산 결과를 재활용하는 방법, 행렬을 사용한 수학적 방법 등이 있습니다. 메모이제이션은 계산 결과를 보존하여 같은 계산을 두번 하지 않도록 하는 기법입니다. 아래 코드의 fibo2R, fibo3, fibo4, fibo5, fibo6에서 메모이제이션을 적용하고 있는데, 특히 fibo4와 fibo5에서는 함수 종료 후에도 변수가 유지되는 성질(함수 파라미터 디폴트값이 리스트나 딕셔너리일 경우 및 데코레이터)을 이용하여, 첫 번째 실행 시 생성된 메모이제이션을 두 번째 실행에서도 그대로 이어 받아 시간을 단축하고 있습니다. fibo9는 다른 사람의 코드인데 성능이 워낙 압도적이어서 함께 실어보았습니다([출처](https://shoark7.github.io/programming/algorithm/%ED%94%BC%EB%B3%B4%EB%82%98%EC%B9%98-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98%EC%9D%84-%ED%95%B4%EA%B2%B0%ED%95%98%EB%8A%94-5%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95)).

```python
def elapsed(f): # 시간 측정을 위한 데코레이터
    import time
    def wrapper(p):
        start = time.time()
        r = f(p)
        elapsedtime = time.time() - start
        print(f"{f.__name__}: {elapsedtime:.4f}")
        return r
    return wrapper

@elapsed 
def fibo1R(n): # 단순 재귀
    return fibo1R(n-1) + fibo1R(n-2) if n >= 2 else n

@elapsed
def fibo2R(n, cache=[1,1]): # 메모이제이션을 적용한 개선된 재귀
    if n < 3:
        return 1
    if len(cache) < n: # 새로운 계산
        c = fibo2R(n - 2) + fibo2R(n - 1)
        cache.append(c)
        return c
    else: # 캐시된 계산
        return cache[n - 1]

@elapsed
def fibo3(n): # 계산 결과를 함수 로컬 변수에 저장
    cache = [1,1]
    for i in range(2, n):
        cache.append(cache[i-1] + cache[i-2]) # 메모이제이션
    return cache[n-1]

@elapsed
def fibo4(n, cache=[1,1]): # 계산 결과를 함수 파라미터에 저장
    if n < 3:
        return 1
    for i in range(len(cache), n):
        cache.append(cache[i-1] + cache[i-2]) # 메모이제이션
    return cache[n - 1]

def deco(func): # 계산 결과를 데코레이터 로컬 변수에 저장
    cache = [1,1] # 내부함수가 접근 가능한 데이터(데코레이터 내부에 숨겨져있음)
    print("DECO!!!") # 데코레이터 적용할 때마다 실행(코드에서 @를 만날 때마다 실행됨)
    @elapsed
    def fibo5_(n):
        if n < 3:
            return 1
        for i in range(len(cache), n):
            cache.append(cache[i-1] + cache[i-2]) # 메모이제이션
        return cache[n - 1]
    return fibo5_
@deco
def fibo5(n):
    return

@elapsed
def fibo6(n): # 계산 결과를 함수 로컬 변수에 저장
    n2, n1 = 1, 1 # n-2, n-1
    if n <= 2:
        return 1
    for i in range(2, n):
        n2, n1 = n1, n1 + n2 # 메모이제이션
    return n1

@elapsed
def fibo9(n): # 행렬을 사용한 방법(O(logn))
    SIZE = 2
    ZERO = [[1, 0], [0, 1]] # 행렬의 항등원
    BASE = [[1, 1], [1, 0]] # 곱셈을 시작해 나갈 기본 행렬
    def square_matrix_mul(a, b, size=SIZE): # 두 행렬의 곱을 구함
        new = [[0 for _ in range(size)] for _ in range(size)]
        for i in range(size):
            for j in range(size):
                for k in range(size):
                    new[i][j] += a[i][k] * b[k][j]
        return new
    def get_nth(n): # 기본 행렬을 n번 곱한 행렬을 만듦
        matrix = ZERO.copy()
        k = 0
        tmp = BASE.copy()
        while 2 ** k <= n:
            if n & (1 << k) != 0:
                matrix = square_matrix_mul(matrix, tmp)
            k += 1
            tmp = square_matrix_mul(tmp, tmp)
        return matrix
    return get_nth(n)[1][0]
    

fibo1R(10) # ... (낮은 성능)
fibo2R(995) # ... (낮은 성능)
fibo3(100000) # fibo3: 0.4399
fibo4(100000) # fibo4: 0.3044
fibo4(100001) # fibo4: 0.0000
fibo5(100000) # fibo5_: 0.3732
fibo5(100001) # fibo5_: 0.0000
fibo6(100000) # fibo6: 0.1078
fibo9(100000) # fibo9: 0.0158
fibo9(2000000) # fibo9: 1.8491
```



### 하노이의 탑

```python
def solution(n):
    answer = []
    def hanoi(n, s, t, b):
        if n == 1:
            answer.append([s, t])
            return
        hanoi(n-1, s, b, t)
        answer.append([s, t])
        hanoi(n-1, b, t, s)
    hanoi(n, 1, 3, 2)
    return answer
solution(3) # [ [1,2], [1,3], [2,3] ]
```



### 참고

- [파이썬 자습서 - 자료구조](https://docs.python.org/ko/3/tutorial/datastructures.html)
- [파이썬 위키 - TimeComplexity](https://wiki.python.org/moin/TimeComplexity)
- [파이썬 표준 라이브러리 - heapq](https://docs.python.org/ko/3/library/heapq.html)

