## 모의고사


```python
import numpy as np
def solution(answers):
    k1 = divmod(len(answers),5)[0]
    k2 = divmod(len(answers),8)[0]
    k3 = divmod(len(answers),10)[0]
    m1 = np.array([1,2,3,4,5]*(k1+1))
    m2 = np.array([2, 1, 2, 3, 2, 4, 2, 5]*(k2+1))
    m3 = np.array([3, 3, 1, 1, 2, 2, 4, 4, 5, 5]*(k3+1))
    n1=0
    n2=0
    n3=0
    for i in range(len(answers)) :
        if answers[i] == m1[i] :
            n1 += 1
        if answers[i] == m2[i] :
            n2 += 1
        if answers[i] == m3[i] :
            n3 += 1
    d = []
    if n1 == max(n1,n2,n3):
        d.append(1)
    if n2 ==max(n1,n2,n3):
        d.append(2)
    if n3 == max(n1,n2,n3):
        d.append(3)
    answer = d
    return answer
```

## 체육복


```python
def solution(n, lost, reserve):
    for i in range(n):
        if (i+1) in lost:
            if i in reserve:
                lost.remove((i+1))
                reserve.remove(i)
            elif (i+2) in reserve:
                lost.remove((i+1))
                reserve.remove((i+2))

    answer = n-len(lost)
    return answer
```

## K번째수
        


```python
def solution(array, commands):
    d=[]
    for i in range(len(commands)):
        a = sorted(array[(commands[i][0]-1):commands[i][1]])
        d.append(a[commands[i][2]-1])
    answer = d
    return answer
```

## 2016년 0424


```python
def solution(a, b):
    y = [31,29,31,30,31,30,31,31,30,31,30,31]
    w = ['FRI','SAT','SUN','MON','TUE','WED','THU']
    d = sum(y[:a-1]) + b
    answer = w[d %7-1]
    return answer
```

## 가운데 글자 가져오기


```python
def solution(s):
    n =len(s)/2
    if len(s)%2 == 0 :
        answer = s[int(n)-1:int(n)+1]
    else :
        answer = s[int(n)]
    return answer
```

## 같은 숫자는 싫어


```python
def solution(arr):
    new = []
    for i in range(len(arr)-1) :
        if arr[i] != arr[i+1] :
            new.append(arr[i])
            
    new.append(arr[-1])
    answer = new
    return answer
```

## 3진법 뒤집기



```python
def solution(n):
    b=''
    while n > 2 :
        n, r = divmod(n,3)
        b += str(r)
    b = b + str(n)

    answer = int(b,3)
    return answer
```

## 내적


```python
def solution(a, b):
    s = 0
    for i in range(len(a)) :
        s += a[i] * b[i]
    answer = s
    return answer
```

## 음양 더하기


```python
def solution(absolutes, signs):
    s = 0
    for i in range(len(signs)) :
        s += (2*int(signs[i]) - 1) * absolutes[i]
    answer = s
    return answer
```

## 예산


```python
def solution(d, budget):
    i=0
    d.sort()
    while budget >= 0 and i < len(d) :
        budget = budget - d[i]
        i += 1
    if budget < 0 :
        answer = i-1
    else:
        answer = i
    
    return answer 
```

## 나누어 떨어지는 숫자 배열



```python
def solution(arr, divisor):
    l=[]
    for i in arr:
        if i%divisor == 0:
            l.append(i)
    
    l.sort()
    if len(l)==0:
        answer = [-1]
    else :
        answer = l
    return answer
```

## 두 정수 사이의 합


```python
def solution(a, b):
    d = max(a,b)
    c = min(a,b)
    answer = sum(range(c,d+1))
    return answer

def solution(a, b):
    answer =(b+a) * (abs(b-a)+1)/2
    return answer
```

## 폰켓몬


```python
def solution(nums):
    s = set(nums)
    if len(s) >= len(nums)/2 :
        answer = len(nums)/2
    else:
        answer = len(s)
    return answer
```

## 문자열 내 마음대로 정렬하기



```python
def solution(strings, n):
    d=[]
    l = []
    strings.sort()
    for i in strings :
        d.append(i[n])
    d = sorted(list(set(d)))
    for j in d:
        for k in strings:
            if j == k[n]:
                l.append(k)

    answer = l
    return answer
```

## 문자열 내 p와 y의 개수


```python
def solution(s):
    pn=0
    yn=0
    for i in range(len(s)):
        if s[i] in ['p','P']:
            pn += 1
        if s[i] in ['y', 'Y']:
            yn += 1
            
    if pn == yn or (pn==0 and yn==0):
        answer= True
    else: 
        answer = False
    


    return answer
```

## 비밀지도


```python
def solution(n, arr1, arr2):
    l1 = []
    l2 = []
    for i in range(len(arr1)):
        if len(bin(arr1[i])[2:]) < n :
            l1.append( (n-len(bin(arr1[i])[2:])) *'0' + bin(arr1[i])[2:])
        else :
            l1.append(bin(arr1[i])[2:])

        if len(bin(arr2[i])[2:]) < n :
            l2.append( (n-len(bin(arr2[i])[2:])) *'0' + bin(arr2[i])[2:])
        else :
            l2.append(bin(arr2[i])[2:])

    arr3 = []
    for i in range(n):
        w=''
        for j in range(n):
            if (l1[i][j] == '0') and (l2[i][j] =='0'):
                w += ' '
            else :
                w += '#'
        arr3.append(w)
    
    answer = arr3
    return answer
```

## 실패율 0424(토) 나중에 다시 풀어봐야함.


```python
def solution(N, stages):
    result= {}
    denominator = len(stages)
    for stage in range(1,N+1):
        if denominator != 0:
            count = stages.count(stage)
            result[stage] = count / denominator
            denominator -= count
        else:
            result[stage] = 0
    return sorted(result,key = lambda x : result[x], reverse=True)
```

## 다트 게임


```python
import re
    
def solution(dartResult):
    bonus = re.compile('[SDT]')
    scorelist=[0,0,0]
    for i in range(3):
        print(i)
        scorelist[i] = int(dartResult[:bonus.search(dartResult).span()[0]])
        #보너스 점수
        if dartResult[bonus.search(dartResult).span()[0]] == 'D':
            scorelist[i] = scorelist[i]**2
        elif dartResult[bonus.search(dartResult).span()[0]] == 'T':
            scorelist[i] = scorelist[i]**3
        #옵션

        if len(dartResult[bonus.search(dartResult).span()[0]:])>1:
            if dartResult[bonus.search(dartResult).span()[1]] == '#':
                scorelist[i] = scorelist[i]*(-1)
            elif dartResult[bonus.search(dartResult).span()[1]] == '*':
                scorelist[i] = scorelist[i]*(2)
                if i>0:
                    scorelist[i-1] = scorelist[i-1]*(2)
        #삭제
        if len(dartResult[bonus.search(dartResult).span()[0]:])>1:
            if dartResult[bonus.search(dartResult).span()[1]] in '#*':
                dartResult[bonus.search(dartResult).span()[1]:]
                dartResult = dartResult[bonus.search(dartResult).span()[1]+1:]
            else:
                dartResult = dartResult[bonus.search(dartResult).span()[1]:]
    answer = sum(scorelist)
    return answer
```
