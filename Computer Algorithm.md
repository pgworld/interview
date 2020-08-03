# **Computer Algorithm Interview**
## Algorithmic Analysis
### Correctness if Insertion Sort
**Inductive Hypothesis** The loop invariant holds after the ith iteration.

**Base case** The loop invariant holds before the first iteration.(initialization)

**Inductive step** If the loop invariant holds after the ith iteration, then it holds after the (i+1)st iteration.(maintenance)

**Conclusion** If the loop invariant holds after the last iteration, then the algorithm is correct!(termination)

쉽게 말해서, 수학적 귀납법의 증명과정과 똑같다. 우선 Inductive hypothesis를 정의해서 계속해서 loop가 돌더라도 유지가 되는 성질을 찾아내고(예측하고) i = 0일 때(수학적 귀납법에서 보자면 수열의 첫번째 항) 그 성질이 유지되는가. i번째에서 성질이 유지될 때, i+1번째 것도 유지가 되는지를 증명을 하고 결론적으로 n번째(최종 결과)에서도 원하는 성질이 유지가 되는지 귀납적으로 증명이 되었음을 보이면 된다.

자 이제 어떤 알고리즘이 잘 적용되는지 알았다면(inductive로 증명 가능한 알고리즘) 그것이 충분히 빠른지에 대해서 논의할 필요가 있을 것이다.

### Analyzing Runtime
- Big O notation
