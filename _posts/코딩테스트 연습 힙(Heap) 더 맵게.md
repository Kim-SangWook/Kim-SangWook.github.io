# 더 맵게

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다.

모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.

Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때,

모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요

## 내 풀이


```python
def solution(scoville, K):
    i=0
    while min(scoville) < K:
        # print(i)
        scoville.index(min(scoville))
        a = scoville.pop(scoville.index(min(scoville)))
        b = scoville.pop(scoville.index(min(scoville)))
        scoville.append((a + b*2) )
        i += 1
        if len(scoville) == 1 and min(scoville) < K:
            i = -1
            break
    answer = i
    return answer
```

정답은 맞았지만, 효율성테스트에서 다 떨어짐,, 시간문제

알고보니 sort, min 함수가 시간복잡도 잡아먹는다. 또 문제의 카테고리가 힙(heap)이였음

heap 자료구조를 사용하자. (처음들어보는 자료구조임)

### Heap

힙은 특정한 규칙을 가지는 트리로, 최댓값과 최솟값을 찾는 연산을 빠르게 하기 위해 고안된 완전이진트리를 기본으로 한다. 

힙 property : A가 B의 부모노드이면 A의 키값과 B의 키값 사이에는 대소 관계가 성립한다

+ 최소 힙: 부모 노드의 키값이 자식 노드의 키값보다 항상 작은 힙
+ 최대 힙: 부모 노드의 키값이 자식 노드의 키값보다 항상 큰 힙

### 힙 함수 활용하기

+ import heapq : 힙 모듈 불러오기
+ heapq.heappush(heap, item) : item을 heap에 추가
+ heapq.heappop(heap) : heap에서 가장 작은 원소를 pop & 리턴. 비어 있는 경우 IndexError가 호출됨. 
+ heapq.heapify(x) : 리스트 x를 즉각적으로 heap으로 변환함 (in linear time, O(N) )

##  힙을 이용한 풀이


```python
import heapq
def solution(scoville, K):
    heapq.heapify(scoville)
    i=0
    a = heapq.heappop(scoville)
    while a < K:
        if len(scoville) == 0 :
            i = -1
            break
        b = heapq.heappop(scoville)
        heapq.heappush(scoville,(a + b*2))
        i += 1
        a = heapq.heappop(scoville)
    answer = i
    return answer
```

### 정확도와 효율성 모두 통과.
