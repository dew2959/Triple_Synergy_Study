# 03
## 03 - 1 | 시간 초과의 원인을 찾기

##### 입출력 데이터의 양이 많아질 수록 input/print와 sys.stdin.readline/sys.stdout.write 성능 차이가 생긴다

```python
a = int(input())
print(a)

import sys
b = int(sys.stdin.readline())
sys.stdout.write(str(b) + '\n')
```

- sys.stdout.write은 문자열만 출력이 가능하다
- print와 달리 자동으로 줄바꿈을 해주지 않아서 '\n'을 뒤에 붙여줘야 한다

## 03 - 2 | 인덱스에 의미 부여하기

##### 인덱스가 단순한 위치가 아니라 특정한 의미를 지닌 값으로 활용 (해싱 개념)
- A[1]의 의미
    
    1. 몇 번째 데이터인지 순서를 의미하는 경우 -> 첫번째 데이터 저장
    2. 숫잣값으로 의미를 부여한 경우 -> 1이라는 값이 몇 개 있는지를 저장 

```python
import sys

N = int(sys.stdin.readline())
count = [0] * 1001
numbers = list(map(int, sys.stdin.readline().split()))

for number in numbers:
    count[number] += 1

for i in range(1001):
    if count[i] != 0:
    for j in range(count[i]):
        sys.stdout.write(str(i) + ' ')
```
#### 계수정렬
- for number in number에서 numbers 리스트의 값을 number가 꺼내옴
- number는 중복이 있어서 나올 때마다 1을 더해주면서 값을 누적
- 이렇게 하면 비교 연산을 따로 하지 않아도 빠르게 정렬 가능

## 03 - 3 | 나머지 연산의 중요성
- 나머지 연산을 활용하는 문제는 단순한 출력 형식의 지시가 아니라 큰 수의 연산을 효율적으로 처리하고, 나머지 연산의 수학적 성질을 활용할 수 있는 지 확인하려는 의도
### 분배 법칙
- 덧셈: (20 + 6) % 3 == (20 % 3 + 6 % 3) % 3 == 2
- 뺄셈: (20 - 6) % 3 == (20 % 3 - 6 % 3) % 3 == 2
- 곱셉: (20 * 6) % 3 == (20 % 3) * (6 % 3) % 3 == 0
- 나눗셈: (20 / 6) % 3 != (20 % 3) / (2 % 3) % 3

#### 분배 법칙을 적용하지 않은 나머지 연산
- Python은 int 형이 자동으로 확장되지만, 숫자가 매우 커지면 계산 속도가 급격히 느려져 시간 초과가 될 수 있음
```python
import time

answer = 1
start = time.time()

for i in range(1, 100001):
    answer *= i

result = answer % 1000000007

end = time.time()
print("결과: ", result)
print("수행 시간: {:.6f}초".format(end - start))
```
#### 분배 법칙을 적용한 나머지 연산
- 중간 과정마다 나머지 연산의 분배 법칙을 적용하여서 계산 시간을 줄일 수 있음
```python
import time

answer = 1
start = time.time()

for i in range(1, 100001):
    answer = (answer *= i ) % 1000000007

end = time.time()
print("결과: ", result)
print("수행 시간: {:.6f}초".format(end - start))
```
## 03 - 4 | 정렬 기초 다지기
- 코딩 테스트는 기본적으로 다양한 데이터를 효과적으로 다루는 것에서 시작
- 이진 탐색과 같은 특정 알고리즘은 정렬된 데이터에서만 적용 가능
#### 오름차순 정렬
- 가장 작은 값에서 시작하여 점차 큰 값으로 나열
- sort(): 원본을 변경, 반환 X
- sorted(): 원본은 유지, 복사본 반환 O

```python
A = [5, 4, 3, 2, 1]

A.sort()
print("sort() 결과", A)

A = [5, 4, 3, 2, 1]

B = sorted(A)
print("원본 리스트: ", A)
print("sorted() 결과: ", B)
```
#### 내림차순 정렬 1
- 가장 큰 값에서 시작하여 점차 작은 값으로 나열
- sort(), sorted()에 reverse=True 옵션 사용하여 구현 가능
```python
A = [5, 4, 3, 2, 1]

A.sort(reverse=True)
print("내림차순", A)

B = sorted(A, reverse=True)
print("내림차순 복사본: ", B)
```

#### 내림차순 정렬 2
- 정렬 함수를 직접 제어(reverse=True)할 수 없을 때
- 모든 데이터를 음수로 변환 후 sort() 함수로 오름차순 정렬, 이후 되돌린다
```python
A = [5, 4, 3, 2, 1]

A = [-x for x in A]
A.sort()
A = [-x for x in A]

print("부호 반전 방식 내림차순:", A)
```

## 03 - 5 | 정렬 기초 다지기
- 데이터를 여러 기준에 따라 정렬해야 할 때 사용 시 여러 조건을 동시에 적용하여 원하는 순서대로 정렬 가능
#### 튜플 기반 정렬
- key 에는 함수를 넣어줘야 한다
```python
scores = [
    (80, 100),
    (100, 50),
    (70, 100),
    (80, 90)
]

scores.sort(key = lambda x: (-x[0], -x[1]))

for s in scores:
    print(f"english={s[0]}, math = {s[1]}")

# sort()는 오름차순 정렬을 하기 때문에 음수로 바꿔줘야 한다
# 첫번째 -x[0]을 기준으로 내림차순 하는데 이 값이 동일하면 두번째 -x[1]을 기준으로 내림차순 정렬을 함
```

#### 딕셔너리 기반 정렬
```python
scores = [
    {'english': 80, 'math': 100},
    {'english': 100, 'math': 50},
    {'english': 70, 'math': 100},
    {'english': 80, 'math': 90},
]

scores.sort(key = lambda x: (-x['math'], -x['english']))

for s in scores:
    print(s)

# key 함수는 단 하나의 값만 반환해야 한다 
# 따라서 정렬 기준이 두 개 이상이면 튜플로 묶어야 함
# 튜플은 순차 비교로 앞에서부터 차례대로 비교하는 방식
# 딕셔너리 하나에서 두 기준을 사용하려면 하나의 값으로 묶어야 함
```
## 03 - 6 | 이차원 리스트 다루기
- 그래프의 구조를 표현하는데 많이 사용하는 것이 이차원 리스트
- 인접 리스트 방식으로 그래프 선언 시, 이차원 리스트의 선언, 데이터 저장, 활용 방법을 알아야 함 

#### 이차원 리스트 선언과 초기화
- 파이썬은 list in list 방식으로 2차원 리스트 구성
```python
# 보통 노드 번호는 1번부터 시작 (N + 1)
N = 3
graph = [[] for i in range(N + 1)] 
```
#### 그래프 데이터 저장하기
- 이런 그래프는 코테에서 아래 입력값으로 주어짐
```python
3 4     # N, E
1 2 4   # s, e, w
2 1 10
1 3 7
3 2 6
```
- 이차원 리스트에 연결 정보 (도착 노드, 가중치) 형태는 튜플로 저장 
- 여러 데이터를 하나로 묶어서 다루기 위해 튜플로 저장한다. 
- 튜플이 리스트보다 가볍고 빠르다
- 정렬, 우선순위 큐 등에서 자연스럽게 비교 가능 (앞 요소부터 비교해줌)
```python
N, E = map(int, input().split())

for i in range(E):
    s, e, w = map(int, input().split())
    graph[s].append((e, w))
```

#### 그래프 데이터 가져오기
```python
for nextNode, weight in graph[1]:
    print(f"next Node {nextNode}, weight = {weight}")
```
