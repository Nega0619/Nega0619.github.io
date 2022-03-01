오늘은 강화학습 분야에서 Finite Markov Decision Processes에 대하여 공부한 것을 정리해 보았습니다.



---

###### 참고사이트

- https://towardsdatascience.com/introduction-to-reinforcement-learning-rl-part-3-finite-markov-decision-processes-51e1f8d3ddb7

----

###### 핵심개념

- RL Problem이 MDP 프레임워크에 어떻게 적용이 되는가
- Markov Property란 무엇인가
- 전환 확률 transition probabilities란 무엇인가
- 미래 보상 할인이란 무엇인가 Discounting future rewards
- 일시적인 작업과 연속작업
- 벨만 최적 방정식 Bellman optimality Equations을 사용하여 최적의 정책 optimal Policy Function및 가치함수 optimal Value Function 풀기

---



# 강화학습 정리

- 강화학습에서 learner, decision-maker는 Agent입니다.
- Agent 외부의 모든 것을 포함하여 상호작용 하는 것은 Environment입니다.
- 매 time step마다 environment는 agent에게 state를 보냅니다.
- state를 기초로 하여 agent는 Action을 선택합니다.
- 다음 time step에서 Agent는 reward를 받게 되고, 다음 단계의 state를 받게 됩니다.
- 매 time step마다 Agent는 현재 State에 따라 가능한 Action들을 선택할 확률에 대해 맵핑 알고리즘을 따르게 됩니다.
  - At each time step, the agent follows a mapping from its current state to probabilities of selecting each possible action in that state.


- 위에서 설명한 맵핑 알고리즘을 Agent의 Policy라고 얘기하며 πt(A|S)라고 표기합니다. time step = t에서 State가 S일 때 Action A를 선택할 확률입니다.

- Agent의 목표는 장기적으로 reward를 최대화 하는 것이 목표이며 reward를 최대화 하는 방향으로 학습하게 됩니다.



# 보상

- time step t 이후,  agent가 reward를 받게될 보상을 `R(t+1), R(t+2), . . .`라고 한다면, Agent는 기대보상값  **E**(**Gt)**를 최대화 하는 방법을 학습하게 됩니다.
  - Gt = R(t+1) +R(t+2) +R(t+3) + · · · +R(T)
  - T는 마지막 time step
- 기대 보상값을 최대화 할 수 있는 경우는 실제 최동 단계인 T가 존재할 경우에만 유의미합니다.

























































