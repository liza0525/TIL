# 2019_0807_Algorithm_Day3

## 지난 시간 코드 리뷰

### min max

```python
def find(a,x):
    max_value = a[0]
    min_value = a[0]
    
    for i in range(1, n):
        if a[i] > max_value:
            max_value = a[i]
        if a[i] > min_value:
            min_value = a[i]
    return max_value - min value

# 코드 생략
```



### 전기 버스

```python
T = int(input())
for tx in range(1, T+1):
    k,n,m = map(int, input().split())
    charging_station = list(map(int, input().split())
    stations = [0]* (n+1)
    for i in range(m):
    	stations[charging_station[i]] = 1
                            
    cnt = cur = 0
    while True:
        pre = cur
        cur += k
        if cur >= n:
        break
        
        if stations[cur] = 1:
        cnt+= 1
        else:
            for i in range(1, k+1):
                if station[cur - i] == 1:
                cur -= i
                cnt += 1
                break
            if cur == pre:
                cnt = 0
                break
    print("#%d" %tc, cnt)
```



### 숫자 카드

```python
t = int(input())
for tc in range(1, t+1):
    n = int(input())
    cards = input()
    cnt = [0] * 10

    for i in range(n):
        cnt [int(cards[i])] += 1

    maxl = 0

    for i in range(10):
        if cnt[maxl] <= cnt[i]:
            maxl = i
    
    print("#%d %d %d" % (tc, maxl, cnt[maxl]))
```



### 구간합

```python
t = int(input())
for tc in range(1,t+1):
    n, m = map(int, input().split())
    v = list(map(int, input().split()))

    sum = 0
    for i in range(m):
        sum += v[i]

    minv = maxv = sumv

    for i in range(1, n-m+1):
        sumv = 0
        for j in range(i, i+m):
            sum += v[j]
        if maxv < sum: maxv = sum
        if minv > sum: minv = sum
    print("#%d %d" % (tc, maxv-minv))
```



### flatten

```python
def find_minmax():
    max_idx = min_idx = 0
    for i in range(len(box_heights)):
        if box_heights[i] > box_heights[max_idx]: max_idx = i
        if box_heights[i] < box_heights[min_idx]: min_idx = i

    return max_idx, min_idx

tc = 10
for tc in range(1, tc+1):
    dump_cnt = int(input())
    box_heights = list(map(int, input().split()))

    for i in range(dump_cnt):
        maxl, minl = find_minmax()
        box_heights[maxl] -= 1
        box_heights[minl] += 1

    maxl, minl = find_minmax()

    print("#%d" % tc, box_heights[maxl] - box_heights[minl])
```





## 배열 2(Array2)

- 2차 배열 순회 : 행 우선 순회

  보통 이중 for문을 이용하여 순회한다.

  - 행 우선 순회
  - 열 우선 순회
  - 지그재그 순회

- 델타를 이용한 2차 배열 탐색

  dx = [0, 0, -1, 1]

  dy = [-1, 1, 0, 0]

  각 index의 dx, dy를 계산하면 기준 index의 상, 하 , 좌, 우의 인덱스에 접근할 수 있다.

  

### 부분집합 합(Subset sum) 문제

- 후에 배낭 문제(O(2^n))에 이용될 문제

- 원소의 개수가 n개인 집합의 부분집합 개수 **2^n**

  : 각 원소가 있냐 없냐의 경우의 수를 모두 곱했을 때의 결과

- 이를 위해 for문을 쓰면 복잡해진다. ==> 따라서 비트연산을 이용한다.

- 비트 연산을 쓰면 이진수의 2배 또는 1/2배가 된다.

|       <<        |        >>         |         &         |        \|        |
| :-------------: | :---------------: | :---------------: | :--------------: |
| 비트를 왼쪽으로 | 비트를 오른쪽으로 | 비트끼리 and 연산 | 비트끼리 or 연산 |

- 모든 부분집합 구하는 코드

```python
arr = [3,6,7]

n = len(arr)

for i in range(1<<n):
    for j in range(n): # i의 j번째 비트
        if i & (1<<j): # i의 j번째 비트가 1이면 j번째 원소 출력
            print(arr[j], end=", ")
    print()
print()
```



### 검색

- 이중 검색 : 검색 후 검색 범위를 1/2로 계속 줄인다. 따라서 시간 복잡도가 O(log(n))



### 선택 알고리즘

- 최소값, 최대값 혹은 중간값을 찾는 알고리즘(또는 k번째로 큰, 또는 작은 원소 찾기)

- 정렬 알고리즘 이용하여 자료 정렬 후 원하는 순서에 있는 원소 가져온다.(O(kn), k가 비교적 작을 때 유용)