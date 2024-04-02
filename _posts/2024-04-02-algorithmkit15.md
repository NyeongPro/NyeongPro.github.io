---
layout: single
title:  "[Algorithm]프로그래머스 길 찾기 게임 문제[파이썬 Python]"
categories: algorithm
tag: [algorithm, 코테, 코딩테스트, 파이썬, python]
redirect_from:
  - /algorithm/algorithmkit15
---

# [길 찾기 게임] 문제
## 코드
```python
import sys
sys.setrecursionlimit(10**6)

def preorder(node_idx_x_y, preorderResult):
    sortNode_Y = sorted(node_idx_x_y, key=lambda x: -x[2])  # Y를 기준으로 정렬
    highlevelNode = sortNode_Y[0]
    sortNode_X = sorted(node_idx_x_y, key=lambda x: x[1])  # X를 기준으로 정렬
    leftNodes = sortNode_X[:sortNode_X.index(highlevelNode)]
    rightNodes = sortNode_X[sortNode_X.index(highlevelNode)+1:]

    preorderResult.append(highlevelNode[0])
    if leftNodes:
        preorder(leftNodes, preorderResult)
    if rightNodes:
        preorder(rightNodes, preorderResult)
    return
def postorder(node_idx_x_y, postorderResult):
    sortNode_Y = sorted(node_idx_x_y, key=lambda x: -x[2])  # Y를 기준으로 정렬
    highlevelNode = sortNode_Y[0]
    sortNode_X = sorted(node_idx_x_y, key=lambda x: x[1])  # X를 기준으로 정렬
    leftNodes = sortNode_X[:sortNode_X.index(highlevelNode)]
    rightNodes = sortNode_X[sortNode_X.index(highlevelNode)+1:]
    
    if leftNodes:
        postorder(leftNodes, postorderResult)
    if rightNodes:
        postorder(rightNodes, postorderResult)
    postorderResult.append(highlevelNode[0])
    
    return
def solution(nodeinfo):
    answer = []
    preorderResult = []
    postorderResult = []
    node_idx_x_y = []
    for i, xy in enumerate(nodeinfo):
        x, y = xy
        node_idx_x_y.append([i+1, x, y])
        
    preorder(node_idx_x_y, preorderResult)
    postorder(node_idx_x_y, postorderResult)
    return  [preorderResult] + [postorderResult]
```
## 풀이
얼마나 오랫동안 고민을 했는지...  

이진트리에서 전위순회, 후위순회하면서 각 노드의 번호를 검색하면 되는 문제였습니다.
혼자서 고민해서 혼자 풀다보니 코드는 매우 복잡합니다. 최대한 자세히 설명하고 다른 코드를 참고하여 공부하도록 하겠습니다.

### 1) 정보를 담을 리스트들
preorder 결과와 postorder의 결과를 담을 리스트입니다.  
그리고 node_idx_x_y는 이후에 제가 사용할 node의 번호와 x, y 좌표입니다.

예를 들어 예시에서 7번 노드는 [7, 8, 6]으로 담깁니다. [노드번호, x, y] 입니다.

### 2) 리스트에 [노드번호, x, y] 담기
node_idx_x_y 리스트에 nodeinfo에서 얻은 정보들을 모두 담습니다.

### 3) 노드들 중에서 최상위 노드 검색
현재 가지고 있는 노드들 중에서 y 좌표가 최고로 높은 노드를 하나 불러옵니다.
y좌표를 기준으로 내림차순 정렬했기 때문에 첫 번째 인덱스의 정보가 y좌표가 최고로 높은 상위 계층입니다.
이 친구를 기준으로 왼쪽 트리, 오른쪽 트리를 나눠줄 것입니다.

### 4) 왼쪽, 오른쪽 트리 나누기
예를 들어 아무런 것도 적용하지 않은 첫 번째 트리를 살펴보겠습니다.
노드번호 1부터 9까지 모두 있는 트리겠네요.

여기서 최상위 계층 노드는 7번 노드입니다. [7, 8, 6] 이구요.
이 친구를 기준으로 왼쪽 트리, 오른쪽 트리로 나눌 수 있습니다.  

왼쪽 트리는 4, 6, 1, 9, 8 ,5 가 되겠네요.  
오른쪽 트리는 2, 3 이 됩니다.

이렇게 나누려면 x 좌표를 기준으로 정렬하면 됩니다.
왜냐하면 왼쪽 트리는 현재 최상위 노드(7번 노드) 기준으로 x 좌표가 작은 것들을 골라주면 되기 때문입니다.
오른쪽 트리도 마찬가지로 7번 노드의 x 좌표보다 큰 것들을 모으면 오른쪽 트리가 됩니다.  

따라서 x좌표 기준으로 정렬하고 7번노드 인덱스 기준으로 왼쪽을 전부 slice 하면 왼쪽 트리가 되고,
오른쪽을 전부 slice하면 오른쪽 트리가 됩니다.

### 5) preorder 구현
이 부분 코드에서 순회의 순서를 정할 수 있습니다.    
전위순회는 [본인찍기 -> 본인 기준 왼쪽 트리 전부 찍기 -> 본인 기준 오른쪽 트리 전부 찍기]입니다.
그렇기에 append를 통해 본인 노드(최상위 노드)를 먼저 삽입해줍니다.  
그리고 왼쪽 트리에 대해 다시 재귀함수로 preorder를 불러 왼쪽에 대해 전부 처리합니다.  
그리고 오른쪽 트리에도 preorder 함수를 다시 불러 오른쪽에 대해 전부 처리합니다.  
이렇게 하면 본인 -> 왼쪽 전부 -> 오른쪽 전부 를 해결할 수 있습니다.

### 6) postorder 구현
여기까지 구현했다면 postorder는 순서만 바꿔주면 됩니다.

postorder는 왼쪽 레벨 끝까지 가서 왼쪽 -> 오른쪽 -> 본인 입니다.  

그렇기에 예시에서 9 -> 6 -> 5 -> 8 -> ... 이 찍히겠죠.  
만약 6 왼쪽에 숫자가 있었다면 그 숫자부터 찍히고 9 -> 6 -> ... 이 찍혔을 것입니다.

그래서 구현은 간단합니다.  
먼저 왼쪽으로 최대한 가고, 오른쪽으로 최대한 간 뒤에 본인이 됐을 때 append 해주면 됩니다.
preorder와 순서만 다릅니다.

### 7) 재귀 제한 조정
이 구문을 추가안하면 테스트 6, 7을 통과하지 못합니다...  

처음에는 어떤 테스트 케이스가 저와 맞지 않는 줄 알았지만 실패가 아니라 런타임에러더군요.  

그래서 조사해보니 재귀함수를 사용할 때 런타임에러는 대부분 재귀함수 제한을 넘어서서 그런거라고 합니다.  

따라서 재귀함수를 사용할 때는 무조건 이 구문을 넣어주고 시작해야 제출했을 때 런타임 에러로
고생하지 않을 것 같아요.

> preorder(전위 순회) 본인 -> 왼쪽 -> 오른쪽  
> postorder(후위 순회) 왼쪽 -> 오른쪽 -> 본인
> 
> 재귀함수 사용 시 limit 제한 설정하기
> ```python
> import sys
> sys.setrecursionlimit(10**6)
> ```

## 추가

### 다른 코드
```python
import sys
sys.setrecursionlimit(10**6) # 파이썬 재귀 limit 늘려주는 코드

# 1) find_root
def find_root(nodes):
    return max(nodes, key=lambda x:x[2])

def divide_nodes(nodes, y):
    for i, node in enumerate(nodes):
        if y == node[2]:
            return nodes[:i], nodes[i + 1:]

def solution(nodeinfo):
    nodes = [(i + 1, x, y) for i, (x, y) in enumerate(nodeinfo)]
    pre_list, post_list = [], []
    
    def make_tree(nodes):
        if not nodes: return
        i, x, y = find_root(nodes)
        pre_list.append(i)
        x_nodes = sorted(nodes, key=lambda x:x[1])
        left_nodes, right_nodes = divide_nodes(x_nodes, y)
        make_tree(left_nodes)
        make_tree(right_nodes)
        post_list.append(i)
    
    make_tree(nodes)
    
    answer = [pre_list, post_list]
    return answer
```

### 풀이

#### 1) root 찾기
##### 내 코드
```python
sortNode_Y = sorted(node_idx_x_y, key=lambda x: -x[2])  # Y를 기준으로 정렬
highlevelNode = sortNode_Y[0]
```

##### max 활용 코드
```python
highlevelNode = max(node_idx_x_y, key=lambda x: x[2])
```
나는 sort를 활용해서 내림차순으로 정렬한 후 첫 번째 인덱스가 최대값이므로 이를 활용했다.  
정렬한 후 max를 이용해서 같은 효과를 얻을 수 있었다.

> 이 분은 root 노드를 찾는 것을 함수로 따로 만들어서 적용하였다.
> 더 깔끔해보임!

#### 2) 노드 기준으로 왼쪽 오른쪽 나누기.
nodes는 현재 가져온 트리에 대한 노드 정보들이다.
이 분도 나와 마찬가지로 [노드번호, x좌표, y 좌표] 대한 정보를 nodes에 저장했다.
나와 다른 점은 나는 리스트안에 배열(또는 리스트) 형태로 담았고 이 분은 튜플 형태로 담았다.  

튜플은 튜플 내에 담긴 정보가 수정되거나 삭제될 일이 없기 때문에 이러한 경우에 사용하면 좋다.  
노드 번호와 좌표값을 변경하거나 삭제할 일이 없기 때문이다.  

이제 가져온 트리에 대한 노드 정보들(nodes)와 y를 가지고 왼쪽 트리와 오른쪽 트리로 나눠준다.  
여기서 y는 find_root 함수를 거쳐서 나온 y 이기 때문에 이 y를 가진 노드를 기준으로 왼쪽 오른쪽으로 나눈다.  
위에서 나와 개념이 완전히 같다.  

그리고 nodes는 들어가기전 x 좌표를 기준으로 정렬 후 들어간다. 그렇기에 root 노드 인덱스 기준으로 왼쪽 오른쪽을 나눴을 때
잘 slice 될 수가 있다.  

이것을 보니 처음에 내가 코드를 짤 때 매우 정확한 방향성을 가지고 짰던 것 같다.  

#### 3) 트리 만들기
이 분은 내가 preorder 함수와 postorder 함수를 따로 만든 것과 비교해서
make_tree에서 한 번에 해결한다.  
어차피 내가 만든 pre, post 함수도 append 하는 순서만 다르고 append 하는 리스트 조차 다르기 때문에
합쳐도 되는 부분이다. 그것을 make_tree에서 하나로 합친 모습이다.  

먼저 nodes를 받아오고 이것이 존재하지 않는다면 return 한다.  

slice의 강점은 인덱스를 신경쓰지 않고 자를 수 있는 점이 강점이다.
예를 들어 내가 리스트의 길이를 몰라도 [0:] 라고 구간을 지정하면 알아서 0번째부터 끝까지 잘라낸다.  

그리고 이것은 아무것도 잘라내지 못했을 때도 [] 와 같이 빈 리스트를 잘 뱉어낸다. 오류를 반환하지 않는다.  

따라서 위에서 왼쪽 트리와 오른쪽 트리를 자르다가 아무것도 
없어서 빈 리스트를 뱉어냈을 것이고, 빈 리스트라면 if 절에서 벗어나기 때문에 조건을 잘 걸어줄 수 있다. 
if 빈리스트: 라고 조건을 걸면 if 절을 벗어나게 된다. 
이를 이용해 if not nodes:라고 해서 nodes가 빈 리스트를 잘라왔다면 바로 return하게 만든다. 
그리고 리스트에 무엇이라도 존재했다면 아래 구문들을 진행한다.

아래 구문을 살펴보면 find_root를 이용해 현재 트리(nodes)에서 root를 찾아낸다.  
그리고 preorder는 다시 다음 트리(또는 왼쪽 트리, 오른쪽 트리)로 넘어가기 전에 본인(root)를
찍어야한다. 그래서 pre_list에 append 하게 된다.  

그리고 나서 왼쪽 트리와 오른쪽 트리를 slice하기 위해 현재 트리를 x 기준으로 정렬한다.
정렬하면 x 기준으로 오름차순 정렬이 되고 현재 root의 x좌표는 이미 구했으므로 이 x 좌표를 기준으로 
왼쪽 오른쪽을 잘라내면 된다.

이제 왼쪽 트리는 다시 make_tree에 들어가고 오른쪽 트리는 make_tree에 또 들어가게 된다.

이제 postorder는 왼쪽, 오른쪽이 전부 끝난 뒤에 본인 root를 찍으므로 이때 append 한다.


# 참고 사이트
[[파이썬 코딩테스트] sys.setrecursionlimit](https://velog.io/@ch980113_/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%BD%94%EB%94%A9%ED%85%8C%EC%8A%A4%ED%8A%B8-sys.setrecursionlimit)  

[[프로그래머스] 길 찾기 게임](https://velog.io/@yunu/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EA%B8%B8-%EC%B0%BE%EA%B8%B0-%EA%B2%8C%EC%9E%84)
