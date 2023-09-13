# Inverted Indexing(역 인덱싱)
## 💡 배열에 1,000,000 개의 수가 있다고 가정할 때, 여기에서 원하는 수가 몇 번째 인덱스에 위치해 있는지 확인하는 프로그램을 작성해 보세요. (단, 원하는 수가 하나가 아닐 수 있습니다.)
<br>
일반적으로 선형 탐색 시 비교적 효율적으로 활용할 수 있는 이분 탐색 혹은 투 포인터 등을 활용하기 위해서는 최소 O(NlogN)의 정렬을 활용해야하며, 정렬 과정에서 인덱스도 바뀌어버려 원하는 수가 몇 번째 인덱스에 위치해 있는지도 확인이 어렵습니다.
<br> <br>

이를 위해 `Inverted Indexing(역 인덱싱)`을 활용할 수 있습니다. 역 인덱싱은 검색 엔진 분야에서 주목 받고있는 Elastic Search 등의 주요 특징 중 하나입니다.

일반적으로 특정 문서 내 키워드의 위치를 탐색하는 색인 과정과 달리, 역 인덱싱은 키워드를 이용해 역으로 문서를 찾는 과정입니다. 이러한 특징을 문제에 적용시켜 보면, **우리가 찾고자 하는 수를 미리 dictionary 등의 자료구조에 {숫자 : [인덱스]}와 같은 형태로 저장**해두면 됩니다. 그러면 최소 시간 복잡도 O(N)으로 해결이 가능합니다.

예시 코드는 아래와 같습니다.

#### dictionary를 활용하는 방식
``` python
inverted_index = {}
arr = [1, 2, 3, 4, 2, 5, 6, 2, 7, 8, 2, 5, 5]

# array에 등장하는 수(key)가 몇 번째 인덱스에 등장하는지를 관리
for idx, key in enumerate(array):
    if num in index_dict:
        index_dict[num].append(idx)
    else:
        index_dict[num] = [idx]

targets = [2, 5, 9] # 찾고자 하는 타겟 값이 주어지면

for target in targets:
    # 해당 타겟이 몇 번째 인덱스에 등장하는지를 바로 구할 수 있다.
    print(inverted_index[target] if target in inverted_index else '탐색 실패')
```
#### defaultdict를 활용하는 방식
```python
from collection import defaultdict

inverted_index = defaultdict(list)
arr = [1, 2, 3, 4, 2, 5, 6, 2, 7, 8, 2, 5, 5]

# array에 등장하는 수(key)가 몇 번째 인덱스에 등장하는지를 관리
for idx, key in enumerate(array):   
    inverted_index[key].append(idx)

targets = [2, 5, 9] # 찾고자 하는 타겟 값이 주어지면

for target in targets:
    # 해당 타겟이 몇 번째 인덱스에 등장하는지를 바로 구할 수 있다.
    print(inverted_index[target] if target in inverted_index else '탐색 실패')
```

<br>

## 📚 Reference
[티스토리 - [데이터 색인] 역색인 구조 (역 인덱스; Inverted Index)](https://the-dev.tistory.com/30)
