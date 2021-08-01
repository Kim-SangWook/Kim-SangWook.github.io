## 124 나라의 숫자
124 나라가 있습니다. 124 나라에서는 10진법이 아닌 다음과 같은 자신들만의 규칙으로 수를 표현합니다.

124 나라에는 자연수만 존재합니다.
124 나라에는 모든 수를 표현할 때 1, 2, 4만 사용합니다.
예를 들어서 124 나라에서 사용하는 숫자는 다음과 같이 변환됩니다.`



```python
def solution(n):
    num = ''
    a,b = divmod(n,3)
    while a >= 3 :
        num += str(b)
        a,b = divmod(a,3)
    num += str(b)
    num += str(a)
    numm = list(num)
    for i in range(len(num)-1):
        if numm[i] == '0':
            numm[i] = '4'                
            numm[i+1] = str(int(numm[i+1])-1)
        if numm[i] == '-1':
            numm[i] = '2'
            numm[i+1] = str(int(numm[i+1])-1)
    if '0' in numm:
        numm.remove('0')
    numm.reverse()
    a=''
    for i in range(len(numm)):
        a += numm[i]
        
    return a
```

# n개의 최소공배수
두 수의 최소공배수(Least Common Multiple)란 입력된 두 수의 배수 중 공통이 되는 가장 작은 숫자를 의미합니다.

예를 들어 2와 7의 최소공배수는 14가 됩니다. 정의를 확장해서, 

n개의 수의 최소공배수는 n 개의 수들의 배수 중 공통이 되는 가장 작은 숫자가 됩니다.

n개의 숫자를 담은 배열 arr이 입력되었을 때 이 수들의 최소공배수를 반환하는 함수, solution을 완성해 주세요.


```python
from math import gcd
def lcm(a,b):
    return int(a*b/gcd(a,b))



def solution(arr):
    for i in range(len(arr)):
        if i==0:
            a = lcm(arr[i],arr[i+1])
        if i>0:
            a = lcm(a,arr[i])
    return a
```

# JadenCase 문자열 만들기
JadenCase란 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열입니다.

문자열 s가 주어졌을 때, s를 JadenCase로 바꾼 문자열을 리턴하는 함수, solution을 완성해주세요.


```python
def solution(s):
    l=[]
    for i in range(len(s)):
        if i==0 and s[i] != ' ':
            l.append(i)
        if i>0 and s[i-1] == ' ' and s[i] != ' ':
            l.append(i)
    s = s.lower()
    ll = list(s)
    for i in l:
        ll[i] = ll[i].upper()
    ss=''
    for i in ll:
        ss += i
    return ss
```

# 행렬의 곱셈
2차원 행렬 arr1과 arr2를 입력받아, arr1에 arr2를 곱한 결과를 반환하는 함수, solution을 완성해주세요.

### 제한 조건
* 행렬 arr1, arr2의 행과 열의 길이는 2 이상 100 이하입니다.
* 행렬 arr1, arr2의 원소는 -10 이상 20 이하인 자연수입니다.
* 곱할 수 있는 배열만 주어집니다.


```python
def solution(arr1, arr2):
    arr3 = []
    for i in range(len(arr1)):
        arr3.append([])

    for i in range(len(arr1)):
        for k in range(len(arr2[0])):
            a=0
            for j in range(len(arr1[0])):
                a += arr1[i][j] * arr2[j][k]
            arr3[i].append(a)
    return arr3
```

# 피보나치수 

피보나치 수는 F(0) = 0, F(1) = 1일 때, 1 이상의 n에 대하여 F(n) = F(n-1) + F(n-2) 가 적용되는 수 입니다.

예를들어

* F(2) = F(0) + F(1) = 0 + 1 = 1
* F(3) = F(1) + F(2) = 1 + 1 = 2
* F(4) = F(2) + F(3) = 1 + 2 = 3
* F(5) = F(3) + F(4) = 2 + 3 = 5

와 같이 이어집니다.

2 이상의 n이 입력되었을 때, n번째 피보나치 수를 1234567으로 나눈 나머지를 리턴하는 함수, solution을 완성해 주세요.


```python
def solution(n):
    list1 = [0,1]
    for i in range(n):
        list1.append(int(list1[i])+int(list1[i+1]))

    return list1[n]%1234567
```




    3



# 최솟값 만들기

길이가 같은 배열 A, B 두개가 있습니다. 각 배열은 자연수로 이루어져 있습니다.

배열 A, B에서 각각 한 개의 숫자를 뽑아 두 수를 곱합니다. 이러한 과정을 배열의 길이만큼 반복하며,

두 수를 곱한 값을 누적하여 더합니다. 이때 최종적으로 누적된 값이 최소가 되도록 만드는 것이 목표입니다. 

(단, 각 배열에서 k번째 숫자를 뽑았다면 다음에 k번째 숫자는 다시 뽑을 수 없습니다.)

예를 들어 A = [1, 4, 2] , B = [5, 4, 4] 라면

* A에서 첫번째 숫자인 1, B에서 첫번째 숫자인 5를 뽑아 곱하여 더합니다. (누적된 값 : 0 + 5(1x5) = 5)
* A에서 두번째 숫자인 4, B에서 세번째 숫자인 4를 뽑아 곱하여 더합니다. (누적된 값 : 5 + 16(4x4) = 21)
* A에서 세번째 숫자인 2, B에서 두번째 숫자인 4를 뽑아 곱하여 더합니다. (누적된 값 : 21 + 8(2x4) = 29)

즉, 이 경우가 최소가 되므로 29를 return 합니다.

배열 A, B가 주어질 때 최종적으로 누적된 최솟값을 return 하는 solution 함수를 완성해 주세요.



```python
def solution(A,B):
    A.sort()
    B.sort()
    B.reverse()
    s= 0
    for i in range(len(A)):
        s += A[i] * B[i]

    return s
```

# 최댓값과 최솟값

문자열 s에는 공백으로 구분된 숫자들이 저장되어 있습니다.

str에 나타나는 숫자 중 최소값과 최대값을 찾아 이를 "(최소값) (최대값)"형태의 문자열을 반환하는 함수, solution을 완성하세요.

예를들어 s가 "1 2 3 4"라면 "1 4"를 리턴하고, "-1 -2 -3 -4"라면 "-4 -1"을 리턴하면 됩니다.


```python
def solution(s):
    list1 = s.split()
    neg=[]
    pos=[]
    for i in range(len(list1)):
        if '-' in list1[i]:
            neg.append(list1[i][1:])
        if '-' not in list1[i]:
            pos.append(list1[i])

    pos =  list(map(int, pos))
    neg = list(map(int,neg))
    if len(neg) == 0 :
        answer= str(min(pos)) + ' ' + str(max(pos))
    elif len(pos) == 0 :
        answer= '-'+str(max(neg)) + ' ' + '-'+str(min(neg))
    else:
        answer = '-'+str(max(neg)) + ' ' + str(max(pos))
    return answer
```


```python
# 정답 
def solution(s):
    s = list(map(int,s.split()))
    return str(min(s)) + " " + str(max(s))
```

# 숫자의 표현

Finn은 요즘 수학공부에 빠져 있습니다.

수학 공부를 하던 Finn은 자연수 n을 연속한 자연수들로 표현 하는 방법이 여러개라는 사실을 알게 되었습니다.

예를들어 15는 다음과 같이 4가지로 표현 할 수 있습니다.

* 1 + 2 + 3 + 4 + 5 = 15
* 4 + 5 + 6 = 15
* 7 + 8 = 15
* 15 = 15

자연수 n이 매개변수로 주어질 때,

연속된 자연수들로 n을 표현하는 방법의 수를 return하는 solution를 완성해주세요.


```python
def solution(n):
    l = []
    a=0
    for i in range(n//2):
        l.append([])
        while sum(l[a]) != n:
            i += 1
            l[a].append(i)
            if sum(l[a])>n:
                break
        a += 1

    b=[]
    for i in range(len(l)):
        if sum(l[i])==n:
            b.append(1)
        else :
            b.append(0)
    answer = sum(b)+1
    return answer
```


```python
#답 이해해보도록하자
def expressions(num):
    return len([i  for i in range(1,num+1,2) if num % i is 0])
```

# 땅따먹기 어려움... dp 

땅따먹기 게임을 하려고 합니다.

땅따먹기 게임의 땅(land)은 총 N행 4열로 이루어져 있고, 모든 칸에는 점수가 쓰여 있습니다.

1행부터 땅을 밟으며 한 행씩 내려올 때, 각 행의 4칸 중 한 칸만 밟으면서 내려와야 합니다.

단, 땅따먹기 게임에는 한 행씩 내려올 때, 같은 열을 연속해서 밟을 수 없는 특수 규칙이 있습니다.

예를 들면,

| 1 | 2 | 3 | 5 |

| 5 | 6 | 7 | 8 |

| 4 | 3 | 2 | 1 |

로 땅이 주어졌다면, 1행에서 네번째 칸 (5)를 밟았으면, 2행의 네번째 칸 (8)은 밟을 수 없습니다.

마지막 행까지 모두 내려왔을 때, 얻을 수 있는 점수의 최대값을 return하는 solution 함수를 완성해 주세요.

위 예의 경우, 1행의 네번째 칸 (5), 2행의 세번째 칸 (7), 3행의 첫번째 칸 (4) 땅을 밟아 16점이 최고점이 되므로 16을 return 하면 됩니다.


```python
def solution(land):
    for i in range(len(land)-1):
        m = max(land[i])
        idx = land[i].index(m)
        for j in range(4):
            if j == idx:
                land[i].sort()
                land[i+1][j] +=  land[i][2]
            else:
                land[i+1][j] +=  m

    answer = max(land[-1])
    return answer
```

# 다음 큰 숫자

자연수 n이 주어졌을 때, n의 다음 큰 숫자는 다음과 같이 정의 합니다.

* 조건 1. n의 다음 큰 숫자는 n보다 큰 자연수 입니다.
* 조건 2. n의 다음 큰 숫자와 n은 2진수로 변환했을 때 1의 갯수가 같습니다.
* 조건 3. n의 다음 큰 숫자는 조건 1, 2를 만족하는 수 중 가장 작은 수 입니다.

예를 들어서 78(1001110)의 다음 큰 숫자는 83(1010011)입니다.

자연수 n이 매개변수로 주어질 때, n의 다음 큰 숫자를 return 하는 solution 함수를 완성해주세요


```python
def solution(n):
    aa = n
    list1 = []
    while n>2:
        n,b = divmod(n,2)
        list1.append(b)
    a,b = divmod(n,2)
    list1.append(b)
    list1.append(a)
    list0 = list1

    for i in range((aa+1),(2*aa)):
        c = i
        list1 = []
        #print(i)
        while i>2:
            i,b = divmod(i,2)
            list1.append(b)
        a,b = divmod(i,2)
        list1.append(b)
        list1.append(a)
        if sum(list0) == sum(list1):
            answer = c
            break

    #print(answer)
    return answer
```

# 가장 큰 정사각형 찾기

1와 0로 채워진 표(board)가 있습니다. 

표 1칸은 1 x 1 의 정사각형으로 이루어져 있습니다. 

표에서 1로 이루어진 가장 큰 정사각형을 찾아 넓이를 return 하는 solution 함수를 완성해 주세요.

(단, 정사각형이란 축에 평행한 정사각형을 말합니다.)


```python
board = [[0,1,1,1],[1,1,1,1],[1,1,1,1],[0,0,1,0]]
```


```python
def solution(board):
    for row in board: # 정답의 최소값이 0인지 1인지 먼저 판별
        if sum(row):
            answer = 1
            break
    else:
        return 0
	# 1행 1열부터 board를 2x2 정사각형으로 탐색하면서 우측 아래 값 최신화
    for i in range(1, len(board)): # 행
        for j in range(1, len(board[0])): # 열
            if board[i-1][j-1] and board[i-1][j] and board[i][j-1] and board[i][j]:
                board[i][j] = min(board[i-1][j-1], board[i-1][j], board[i][j-1]) + 1
                answer = max(answer, board[i][j])
    return answer ** 2
```

# 올바른 괄호


```python
def solution(s):
    l=0
    r=0
    for i in s:
        #print(i)
        if i == "(":
            l += 1
        if i == ")":
            if l ==0 :
                return False
            else:
                l += -1
        if l == -1 :
            return False
    
    if l == r :
        ans1 = True
    else :
        ans1 = False
    answer = ans1
    return answer
```
