# **Computer Algorithm Interview**
## 1. Algorithmic Analysis
### 1.1. Correctness if Insertion Sort
- 선택 정렬 vs 삽입 정렬
  - 선택 정렬은 최소값 찾아서(**선택**해서) 계속 앞으로 가져오는 과정(힙소트의 하위호환), 삽입 정렬은 앞에서부터 쭉 돌면서 차근차근 정렬함( [**4**,2,3,1,5] -> [**2,4**,3,1,5] -> [**2,3,4**,1,5] -> [**1,2,3,4**,5] -> [**1,2,3,4,5**] )
  
  
**Inductive Hypothesis** The loop invariant holds after the ith iteration.

**Base case** The loop invariant holds before the first iteration.(initialization)

**Inductive step** If the loop invariant holds after the ith iteration, then it holds after the (i+1)st iteration.(maintenance)

**Conclusion** If the loop invariant holds after the last iteration, then the algorithm is correct!(termination)

쉽게 말해서, 수학적 귀납법의 증명과정과 똑같다. 우선 Inductive hypothesis를 정의해서 계속해서 loop가 돌더라도 유지가 되는 성질을 찾아내고(예측하고) i = 0일 때(수학적 귀납법에서 보자면 수열의 첫번째 항) 그 성질이 유지되는가. i번째에서 성질이 유지될 때, i+1번째 것도 유지가 되는지를 증명을 하고 결론적으로 n번째(최종 결과)에서도 원하는 성질이 유지가 되는지 귀납적으로 증명이 되었음을 보이면 된다. 추가적으로 후술할 Divide and Conquer 알고리즘과 correctness 증명의 차이를 한번 살펴보자.
```python
  def proof_of_correctness_helper(algorithm):

    if algorithm.type == "iterative":
      # 1) Find the loop invariant
      # 2) Define the inductive hypothesis
      #    (internal state at iteration i)
      # 3) Prove the base case (i=0)
      # 4) Prove the inductive step (i => i+1)
      # 5) Prove the conclusion (i=n => correct)

    elif algorithm.type == "recursive":
      # 1) Define the inductive hypothesis
      #    (correct for inputs of sizes 1 to i)
      # 2) Prove the base case (i < small constant)
      # 3) Prove the inductive step (i => i+1 OR
           {1,2,...,i} => i+1)
      # 4) Prove the conclusion (i=n => correct)

    # TODO
```

자 이제 어떤 알고리즘이 잘 적용되는지 알았다면(inductive로 증명 가능한 알고리즘) 그것이 충분히 빠른지에 대해서 논의할 필요가 있을 것이다.

### 1.2. Analyzing Runtime
- Big O notation
Big O notation의 수학적인 정의를 알아보기 전에, 직관적인 의미를 살펴보자. Big O notation은 upper bound라는 개념을 상상하면 수식도 헷갈일 일 없이 쉽게 외워질 것이다. upper bound라는게 무엇이냐? 아무리 커봐야 이거보단 크지않다는 것이다. "T(n) = O(g(n)) iff ∃c, n0 > 0 s.t. ∀n ≥ n0, 0 ≤ T(n) ≤ c⋅g(n)"이게 수학적인 정의이다.
쉽게 말해서 흔히 우리가 insertion sort(다시 설명하자면 10개의 숫자가 있을 때, 앞에서부터 천천히 1개, 2개, 3개 ... 10개까지 sort를 수행하는 것이다. 즉 i번째 iteration에서는 i번째까지는 sorting이 되었다는 것! 이때 sort를 하는 방법은 i번째에서 새로운 숫자가 들어왔을 때, 적당한 그 친구의 위치로 그 숫자를 넣어주는 것)의 수행시간이 O(n^2)이라고 하는데, Big O notation으로 나타내면 O(n^3)일 수도 있고, O(n^4)일 수도 있다는 것이다.

- Big Ω notation
이는 Big O notation과는 반대로 lower bound라는 생각을 가지면 된다. 아무리 작아봐야 이거보단 작지 않다 것이다. "T(n) = Ω(g(n)) iff ∃c, n0 > 0 s.t. ∀n ≥ n0, 0 ≤ c⋅g(n) ≤ T(n)"이 수학적인 정의이다. 앞서 예를 들었던 insertion sort는 Ω(n^2)이라고도 할 수 있겠지만 Ω(n), Ω(1)이라고 할 수 있다는 것이다. 즉 insertion sort가 아무리 짧게 걸려봐야 n보다 짧게는 안걸린다는 의미와 일맥상통한다.

- Big Θ notation
이것은 Big O notation과 Big Ω notation의 교집합이라고 보면 된다. 이는 존재하지 않을 수도 있고, 만약 존재한다면 단 하나의 표현만 존재하게 될 것이다. 즉, insertion sort는 Θ(n^2)이다.

- Best-case vs. worst-case
쉽게 말해서 운 좋으면 best case, 운 존나 나쁠 때 worst case.

- 여기서 헷갈리면 안되는 것
worst case, best case의 경우와 아까 말했던 upper bound, lower bound를 헷갈리면 안된다. "insertion sort는 운 존나 좋을 땐 n만큼 시간 걸리는데, 그러면 Ω(n^2)이라고 하면 안되는거 아님? 아무리 작아봐야 n^2보다 작지는 않다며?"라고 이야기하면 뒤진다. 그냥 Big O notation과 Big Ω notation은 복잡한 수식을 간단하게(근사해서) 나타내는 하나의 "표현"일 뿐이다. 그 표현을 빌려서 우리는 알고리즘의 수행시간을 나타내는 것일 뿐이고. "그러면 무조건 Big Θ notation은 존재하는거 아냐? 존재하지 않을 수도 있는 경우는 뭐임?"이라고 질문한다면 아주 간단한 반례를 하나 들 수 있을 것이다. 만약 x가 홀수일 때는 x, x가 짝수일 때는 x^2가 걸리는 알고리즘이 있다고 가정하자. 이러한 알고리즘은 정의에 의하여 O(n^2)이고 Ω(n)일 것이다. 그렇다면 어떻게 되는가? Big Θ notation은 존재하지 않게 될 것이다.

## 2. Divide and Conquer
### 2.1. Analyzing Runtime
다른거 다 알 필요없다. 다른건 그냥 tree 이용해서 하는 방법(n(tree 너비)  * logn(tree 높이))과 수학적인 방법이 있는데, master method쓰면 그냥 다 된다.

- master method
Suppose T(n) = a⋅T(n/b) + O(n^d). The Master method states:
```
T(n) = O(n^d logn)   if a = b^d
       O(n^d)        if a < b^d
       O(n^log_b(a)) if a > b^d
```
이거 외우기 힘들면 그냥 면접볼 때 포스트잇에 적어놓고 노트북에 붙여놓자.

### 2.2. Linear-Time Selection
미안하다 이거 한시간동안 봤는데 이해 못했다.

## 3. Linear Time Sorting

### 3.1. comparison-based sorting algorithms
비교를 기반으로 한 알고리즘은 아무리 잘 짜도 최소 O(nlogn) 시간을 필요로 하게 된다. 그 이유는 decision tree의 노드 수는 n!이고 해당 트리의 최소 depth는 log(n!)이 된다. 이때 O(log(n!)) = O(nlogn)이므로 최소 nlogn의 시간을 필요로 하게 된다.

### 3.2. Linear Time Sorting
- Counting Sort
- Bucket Sort
- Radix Sort

## 4. Randomized Algorithms
### 4.1. Concept
- Good average-case behavior (평균적으로 좋은 성능을 보인다)
- Getting exact answers with high probability (높은 확률로 제대로된 값을 뱉어낸다)
- Getting answers that are close to the right answer (정답값에 가까운 값을 뱉어낸다)

### 4.2. Monte Carlo vs. Las Vegas
- **Las Vegas algorithms** guarantee correctness, but not runtime. We’ll focus on these algorithms today. (정답 맞추기 > 빨리 풀기)
- **Monte Carlo algorithms** guarantee runtime, but not correctness. We’ll revisit this next week when we see Karger’s algorithm. (빨리 풀기 > 정답 맞추기, 그렇더라도 정답에 가까운 값을 가져옴)

### 4.3. Bogo sort
"Bogo"는 멍청이라는 뜻. 그냥 랜덤으로 숫자들 배열한다음에 정렬되어있는지 확인한다. worst case에서는 무한대의 시간이 걸릴 수도 있고, 기댓값은 O(n * n!)이다. (n이 곱해져 있는 이유는 정렬되어있는지 확인해야하니까)

### 4.4. Quick sort
그냥 pivot값을 랜덤으로 뽑음. 그냥 배열 중간에 있는거 가져오는거랑 무슨 차이냐? 라고 할 수 있는데, quick sort에서 중앙에 있는걸 가져왔는데 그게 항상 각 파티션에서 최대 혹은 최소값일 경우 worst case가 도출될 수 있다. 이러한 상황을 최소한으로 확률적으로 줄이기 위해 랜덤으로 뽑으면 더 좋다고 한다. (인풋에 의해서 좌우되지 않기 위해서!)

### 4.5. Majority Element
최빈값(여기서 말하는 최빈값은 단순히 많이 나오는 값이 아니라 배열 크기의 절반 이상을 차지하는 값)을 가져오기 위해서 우리는 어떤 생각을 하는가? 각각의 값들의 빈도수를 구해서 빈도수가 가장 많은 것을 가져오면 되지 않을까? 라고 생각을 하곤 한다. 하지만 그렇게 되면 그것 역시 결국은 sort와 비슷한 작용을 하기때문에 nlogn의 시간을 필요로 하게 된다. 하지만 bogo sort의 컨셉을 사용하게 된다면 기대 시간 복잡도가 O(n)에 수렴하게 된다. 그냥 이렇게 생각하면 된다. 확률적으로 최빈값이니까 대충 찍었을 때 나오는 값이 최빈값일 확률이 굉장히 높지 않을까? 라는 컨셉에서 시작을 하면 된다. 배열 크기의 절반 이상을 차지하는 값을 찾는 것이므로(물론 어떤 값이 배열 크기의 절반 이상을 차지할 정도로 많이 등장한다는 전제가 필요하다) 1/2 이상의 확률로 정답값을 찾게 될 것이다. 따라서 대충 뽑고 그것이 Majority Element인지 확인만 하면(이때 n의 시간이 들게 된다) 되기 때문에 기대 시간 복잡도는 n이다. 대충 6번만 찍어도 그것이 Majority Element일 확률이 99.6% 이상이므로 충분히 제대로 작동할 수 있을 것이라 예측할 수 있다.

## 5. Hash
### 5.1. Universe
U는 가능한 모든 key의 경우의 수라고 생각하면 된다. 예를 들어 모든 64비트 문자열의 경우, U의 크기는 2^64가 될 것이다. 이때 모든 key를 커버할 수 있으면 좋겠지만, (2^64개의 bucket을 만들면 좋겠지만) 일반적으로 U의 크기는 매우 크기 때문에 현실적으로 가능하지 않다. 그래서 정해진 bucket의 크기를 정해두고 그 곳에 key들을 집어넣게 된다. 즉, 다량의 key를 bucket에 넣을 때는 비둘기집 원리에 의해 다대일 매핑 구조가 될 수 밖에 없다는 것이다. 이때 하나의 bucket에 여러개의 key가 존재할 때는 ```chaining```을 이용해서 해결하게 된다. 

### 5.2. Choice of size of hash table&hash function(해쉬 테이블은 아까 언급했던 bucket과 똑같은 의미이다.)
해쉬 테이블의 크기는 일반적으로 해쉬 테이블의 크기가 적어도 한 시점에 저장되어야하는 최대 키의 수만큼 크도록 선택된다. 이 조건을 위반한다면 일반적으로 구현했을 때 테이블의 크기가 증가하게 되고(rehash) 테이블의 크기가 증가하게 되면 더 큰 테이블에 매핑하여 새 테이블을 처음부터 다시 계산하게 된다.
이때 매핑 작업을 효율적으로 구현하기 위해서는 키가 해시테이블의 버킷에 균일하게 분산되어야 할 것이다. 이러한 분산을 구현하기 위해 우리는 ```completely random hash function```을 찾아야 할 것이다. 하지만 현실적으로 해쉬함수의 경우의 수는 n^|U|가 되고 이는 굉장히 큰 값이다. 즉 배보다 배꼽이 더 큰 상황이 되는데, 그럼에도 불구하고 ```completely random hash function```이 존재한다고 가정하고 hashing을 분석하고 이에 가장 가깝게 접근할 수 있는 함수를 찾을 수 있도록 할 것이다.

### 5.3. Universal Hash Function
다른 키 값에 대해서 동일한 해쉬값을 갖는 해쉬 함수들의 개수가 |F|/n보다 작거나 같으면 이를 universal하다고 한다. (|F|는 hash function의 모임, n은 bucket의 크기) universal hash function을 사용하는 이유는 어떤 방식으로든 해쉬 함수가 이미 정해져버리면 악의적 사용자가 collision을 유도하기 더 쉬워진다. 이를 피하기 위해 해쉬 함수를 random하게 정해지게하는 방식이 유니버셜 해싱이다. 

## 6. Tree Algorithm
자료구조와 완전 겹치는 내용(오히려 하위호환)이라 생략. 물어볼만한 질문이 있다면 "왜 RB tree의 네 조건을 가지게 되면 높이가 logn에 수렴하느냐?"일 것.

- ***Data Structure Trade-offs***
  - When to use a hash table vs. binary search tree vs. red-black tree vs. sorted array vs. sorted linked list?

  - When to use an adjacency matrix vs. adjacency list?

## 7. Graph
### 7.0. Simple Question
- DFS, BFS?
- What is Topological Ordering?
- What is Bipartite Graph?
- What is SCC(Strongly Connected Components)?
### 7.1. Kosaraju's Algorithm
어떤 그래프에서 SCC인 subgraph를 찾는 알고리즘이다. DFS를 응용한다. DFS를 우선 수행하고, 모든 그래프 엣지들의 방향을 반대로 한 역방향 그래프에서 다시 DFS를 수행하여(먼저 수행한 DFS의 end_node부터) SCC를 잡아낸다. 이때 이것이 정당한 이유는 DFS를 수행하면 u -> v로 가는 경로를 찾게 되는 것인데, 만약 u,v가 SCC에 포함되는 노드였다면 v -> u로의 경로도 존재할 것이므로 역방향 그래프에서도 경로로 포함이 될 것이다.

## 8. Dynamic Programming
### 8.0. Divide and Conquer와의 공통점과 차이점
  - 공통점
    - 문제를 잘게 쪼개서, 가장 작은 단위로 분할한다.
  - 차이점
    - 동적계획법
      - 부분 문제는 중복되어, 상위 문제 해결 시 재활용된다.
      - Memoization 기법을 사용한다.(부분 문제의 해답을 저장해서 재활용하는 최적화 기법으로 사용)
    - 분할 정복
      - 부분 문제는 서로 중복되지 않는다
      - Memoization 기법을 사용하지 않는다.
### 8.1. Bellman-Ford Algorithm
최단 경로를 찾는 알고리즘. 이건 그림 한번 보면 이해될 것 같긴한데(https://docs.google.com/presentation/d/1NMADbRbELNZoZaqI466w86-CL2EgzzQfVM3vUEXB0Q0/edit#slide=id.g24bd18724f_0_2472) 이렇게 지엽적인 내용을 묻지는 않을 것 같음. 결국은 이전의 결과를 memoization해서 그 다음에 이용한다는 것.

### 8.2. Floyd-Warshall
이건 벨만포드와 비슷한데 엣지의 웨이트가 음수일 때 사용한다. 이건 행렬을 두개 그려야해서 문제 풀어라고 나오지는 않을 듯. 우리 시험 때도 이거 안나왔음.

### 8.3. Knapsack
가방에 최대한 물건을 담는 알고리즘. 문제를 푸는데 짧게 걸리는 문제가 아니라서 단지 가방의 최대 한도 무게까지 하나하나 늘려가면서 그 전의 결과를 저장하고 그 다음에 조금씩 한도 무게를 목표 한도 무게까지 증가시키면서 물건을 바꿔넣는 방식으로 알고리즘이 진행된다.

## 9. Greedy Algorithm
### 9.1. What is Greedy Algorithm?
미리 정한 기준에 따라서 매번 가장 좋아보이는 답을 선택하는 알고리즘. 근시안적으로 해를 구함.
### 9.2. Minimum Spanning Trees
그래프 내의 모든 정점을 최소의 비용으로 연결하는 트리. greedy algorithm을 이용해서 MST를 구성하기 위해서는 하나의 lemma가 필요하다. lemma는 다음과 같다.(이 lemma는 cut property라고 한다. https://daily-life-of-bsh.tistory.com/39) (https://1ambda.github.io/algorithm/algorithm-part2-1/)
```
그래프 G의 MST의 엣지의 집합 T에 대하여 T의 부분집합 A가 있다고 가정하자.(이때 A는 graph cut에 의해 생겨난 edge set이다.) 이때 G에서 A를 제외한 edge 중에서 가장 웨이트가 낮은 엣지는 MST에 포함된다.
```
### 9.3. Prim's Algorithm
진짜 완전 탐욕적인 알고리즘. 어떤 노드를 선택해서 거기서 출발해가지고 가장 웨이트 작은 쪽으로만 가면서 루프만 안생기게(루프가 생기면 트리가 아니니까) 트리를 구성한다. 그걸 리턴한다.

### 9.4. Kruskal's Algorithm
위에서 언급했던 lemma를 여실히 사용해버리는 알고리즘. 그림을 보면 쉽게 이해가 된다. 묶음을 만들어나간다고 생각하면 쉬울듯. (https://docs.google.com/presentation/d/1-PNLi9UcHUXX_XjN2pOfJCVZFEGNQvjgy5RBYtrROio/edit#slide=id.g40119670e5_0_551)

## 10. NP, P problem(면접에 나온다면 앞의 것들보다 이게 나올 가능성 농후)

### 10.0. 결정문제
답이 YES 아니면 NO로 반환되는 문제를 결정 문제라고 한다. 예를 들어, 'a는 b의 배수인가?'와 같은 질문은 결정 문제이다. P와 NP 모두 결정 문제의 분류에 해당한다.

### 10.1. P problem
P 문제는 결정 문제들 중에서 쉽게 풀리는 것을 모아 놓은 집합이다. 어떤 결정 문제가 주어졌을 때, 다항식(Polynomial) 시간 이내에 그 문제의 답을 YES와 NO 중의 하나로 계산해낼 수 있는 알고리즘이 존재한다면, 그 문제는 P 문제에 해당된다. n자리 이하의 수 a와 b가 주어졌을 때, a가 b의 배수인지를 판정하는 것은 유클리드 호제법을 사용하면 n에 대한 다항식 시간에 계산할 수 있으므로, 'a는 b의 배수인가?'하는 문제는 P 문제에 해당된다.

### 10.2. NP problem
NP 문제는 결정 문제들 중에서 적어도 검산은 쉽게 할 수 있는 것을 모아 놓은 집합으로도 정의할 수 있다. 정확히 말하면, 어떤 결정 문제의 답이 YES일 때, 그 문제의 답이 YES라는 것을 입증하는 힌트가 주어지면, 그 힌트를 사용해서 그 문제의 답이 정말로 YES라는 것을 다항식 시간 이내에 확인할 수 있는 문제가 바로 NP 문제에 해당된다. 예를 들어,{-5,,6,1,2,-10,-7,13}과 같이 정수 n개로 이루어진 집합이 주어졌다고 할 때, '이 집합의 부분집합들 중에서 원소의 합이 0이 되는 집합이 존재하는가?'라는 문제는 아직까지 다항식 시간 알고리즘이 알려져 있지 않다. 곰곰히 생각해보아도, 그냥 모든 부분집합을 다 테스트해보지 않는 이상 답이 YES인지 NO인지 답하기가 어렵다는 것을 알 수 있다. 그렇지만 누군가가 우리에게 {6,1,-7}이라는 힌트를 제공하였다면, 우리는 먼저 이 집합이 원래 집합의 부분집합이라는 사실을 확인하고, 이 집합의 원소의 합이 00이라는 사실을 확인함으로써 원래 문제의 답이 YES라는 것을 어렵지 않게 확인할 수 있다. 따라서 '크기가 n인 어떤 정수 집합이 주어졌을 때, 이 집합의 부분집합들 중에서 원소의 합이 0이 되는 집합이 존재하는가?'라는 문제는 적어도 NP 문제인 것은 확실하지만, 이것이 P 문제인지 아닌지는 아직 모르는 상황이라고 할 수 있다.

### 10.3. P = NP?
만약 어떤 P 문제가 주어졌고, 그 문제의 답이 YES라면, 우리는 그 문제의 답에 관한 힌트가 주어지면 곧바로 그 문제의 답이 YES라는 것을 쉽게 확인할 수 있을 것이므로, 모든 P 문제는 저절로 NP 문제도 된다. 즉, P⊂NP임을 알 수 있다. 하지만 그 역방향인 NP⊂P에 대해서는 참인지 거짓인지 아직 알려져 있지 않다. 만약 모든 NP 문제가 P 문제인 경우, 즉 모든 NP 문제가 다항 시간에 풀 수 있는 알고리즘이 존재함을 증명할 경우 P=NP라는 결론이 된다. 그래서 P=NP인지, 아니면 P≠NP인지를 묻는 것이 바로 P-NP 문제이다.

사실 많은 컴퓨터공학자들은 절대로 P=NP일리가 없다고 믿고 있다. 왜냐하면, P=NP가 의미하는 바는, 만약 어떤 문제가 주어졌을 때, 그 문제의 답안을 쉽게 검산할 수 있다면, 그 문제 자체도 쉽게 풀 수 있다는, 너무나도 강력한 주장이기 때문이다. 이는 우리가 지금까지 열심히 수학을 공부하면서 몸에 체득해온 직관과는 배치되는 일이다. 일부 방정식의 경우 해를 직접 구하는 것은 어렵지만, 남이 미리 풀어서 구한 답을 방정식에 대입해서 그게 맞는지 확인하는 일은 훨씬 쉬운 경우가 많다. 종종 NP 문제에 대해서 "P 문제가 아닌 것"으로 설명하는 경향이 있지만, P 문제는 다항식 시간 동안 '답을 찾을 수 있는' 문제이고 NP 문제는 다항식 시간 동안 '주어진 답이 맞는지 확인할 수 있는' 문제이므로 이 둘은 상호배타적인 관계가 아니다.

참고로 '어떤 문제를 다항식 시간 내에 해결할 수 있는가 아닌가'는 '그 문제를 해결하는데 평균적으로 얼마나 시간이 걸리는가?'가 아니라 '최악의 경우(worst case)에 시간이 얼마나 걸리는가?'를 기준으로 한다. 즉, '다항식 시간 내에 해결할 수 없는 경우'가 하나라도 있으면 그것은 P 문제가 아니며, P 문제가 아니라고 생각되는 NP 문제라고 하더라도 평균적으로는 다항식 시간 내에 해결할 수도 있다.

## 11. Minimum Cut
### 11.0. Minimum Cut?
s랑 t 사이에 갈 길을 없애버리는데, 자를 때 좀 덜 힘들게 자르자!

### 11.1. Karger's Algorithm
https://docs.google.com/presentation/d/11zSwMINasKaXBDsJogUBUWap7GqkV8e3tYxK-t-DFe4/edit#slide=id.g3fa61439f7_0_906

### 11.2. Ford-Fulkerson Algorithm
https://ratsgo.github.io/data%20structure&algorithm/2017/11/29/maxflow/
https://blog.naver.com/PostView.nhn?blogId=kks227&logNo=220804885235
