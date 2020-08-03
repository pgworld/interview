# **Computer Algorithm Interview**
## Algorithmic Analysis
### Correctness if Insertion Sort
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

### Analyzing Runtime
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

