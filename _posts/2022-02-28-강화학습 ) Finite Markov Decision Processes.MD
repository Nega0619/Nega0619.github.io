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
- 매 time step마다 































































