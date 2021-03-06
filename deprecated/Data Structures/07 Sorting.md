# 7 Sorting

## 7.1 Motivation

- List : 하나 이상의 필드를 가지는 자료(레코드)들의 집합
- Key : 특정 레코드를 찾기 위해서 사용되는 필드(들)

### Search

- List에 있는 레코드를 찾는 문제 (Search)
- 가장 기본적인 방법은 sequential search (순차 탐색)
  - 코드를 수월하게 짜기 위해 (혹은 헤더 등으로 활용하게) 정보를 담는 배열의 크기는 $n+1$로 했다.
  - 시간 복잡도가 $(n+1)/2$가 된다.
- **정렬이 돼있다면** sequential search도 빠르게 할 수 있다.
  - 찾고자 하는 key가 list에 없을 경우
  - Binary search
    - 중간을 보고 앞으로 갈지 뒤로 갈지 판단

### Match

- (예시) 고용인과 피고용인이 각자 국세청에 소득 신고를 했다. 둘 다 제대로 했는지(내용이 같은지) 확인이 필요하다.
- 정렬이 되어 있다면 안되어 있을 때보다 빠르다.

### A Formal Definition of Sorting

- $R_n$은 레코드, $K_n$은 키라고 하자
- 키 사이에는 정렬이 가능해야 한다. (순서가 정해질 수 있어야 한다.)
- $\sigma$를 permutation(순열)이라고 하자
  - E.g., 1,2,3,4,5 -> 5,2,4,3,1
  - 한 $\sigma$에서 $K_1<=K_2<=...<=K_n$인 경우 정렬이 된 것이다. (satisfying)
- Stable한 sorting 알고리즘
  - 키값이 같은 경우 레코드의 순서가 바뀌어도 상관은 없지만, 원래 있던대로 냅두는 경우 stable한 알고리즘이라 한다.
  - 답이 하나만 나온다는 것 ($\sigma_s$)

### Internal / External methods

- Internal : 명령 자체가 데이터 처리가 메모리 상에서 다 돌 수 있는 경우
- External : 메모리에 다 못올려서 파일에 남겨야 하는 (남아 있는) 경우

## 7.2 Insertion Sort

- Insert라는 과정을 반복함으로써 정렬시키는 알고리즘
- Insert : 이미 정렬되어 있는 배열에 새로운 element를 넣는데, 정렬 상태가 유지되도록 제자리를 찾아 넣어주는 것
- Insert를 앞에서부터 반복해서 쭉 하게 되면 결국 전체가 정렬된 배열이 나온다. (`InsertionSort()`)
- 가장 큰 값부터 비교하며 자리를 찾는다. (나보다 크면 오른쪽으로 한 칸 민다.)
- Complexity
  - Worst-case (정렬 결과와 반대 순서로 들어온 경우)
    - `Insert`의 복잡도는 $1$에서 $n$까지 더한 것
    - `InsertionSort`는 for문이 $n-1$번 돌기에 도합 $O(n^2)$이 됨
  - 내가 지금 들어갈건데 나보다 큰 놈이 몇 놈이냐? ~~싸우자~~
- 정렬을 자주 해야 하는데 어느 정도 정렬이 되어 있는 경우 이 방법이 좋다.

## 7.3 Quick Sort

- Pivot을 하나 뽑아서 그걸 기준으로 작은 건 왼쪽에, 큰 건 오른쪽에 넣는다.
  - 책 내용은 다른 배열을 따로 만들지 않고 한 배열에서 계속 swap
- Pivot을 누굴 뽑냐가 러닝타임에 영향을 줄 수 있다.
  - 보통 recursion이 $log_2n$번 일어난다. (절반씩 쪼개진다고 보면; best-case)
  - 이 땐 $O(n\ logn)$
- 책의 경우 (pivot이 [1]일 경우) best-case는 사실 오름차순 순서대로 들어올 때다.
  - Worst-case는 반대로 들어올 때
- Pivot을 뭘 잡으면 좋을지 결국 알 수 없다. (평균적인 시간 복잡만 계산될 뿐)

## 7.4 How Fast Can We Sort?

- 이정도 시간 복잡도를 가지는 알고리즘을 만들자!라고 했을 때 과연 그게 가능한지 따질 수 있을까
- 가장 좋은, 효율적인 시간 복잡도는 얼마까지 가능할까
- **과연 이 알고리즘이 얼마나 빨라질 수 있는가?**
- 비교와 swap 연산 2가지를 갖고 어떻게 정렬을 할 수 있을까?
  - 정렬 과정을 트리로 그려보자 (decision tree)
  - 결과가 T/F이므로 차수가 2인 binary tree이다.
  - 결정 트리의 leaf수가 최소 permutation값($n!$)이 나와야 한다.
    - 그렇지 않다면 결정 트리가 틀린 것 (Cover 못하는 경우가 있음)
  - 트리의 **높이**($k$)가 실행 시간과 관련이 있다.
  - $1+log_2n!<=k$
  - $n!>=(n/2)^n$$^/$$^2$에서 결국 $\Omega(nlogn)$임을 알 수 있다.

## 7.5 Merge Sort

- 두 개의 정렬이 되어 있는 리스트를 합치는 것
- 리스트가 홀수 개 있을 땐? (`MergePass()`)
  - `s` : Sublist. 몇 개씩 묶을 건지
  - `s`씩 2개씩 묶으면서 지나가자 (2번째 묶음의 마지막 인덱스 $i+2s-1$이 $n$보다 큰지 확인해야 한다.)
  - 레코드가 남았으면? ($2s$보단 적다.)
    - $s$보다 많이 남아있으면 그 두 개를 합치면 된다.
    - 아니면 그냥 내리고
  - 과정을 뒤집으면 binary tree
  - Depth가 $log\ n$이고 배열 크기가 $n$이니까 $nlog\ n$

