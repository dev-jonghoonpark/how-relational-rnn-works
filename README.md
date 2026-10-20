# How Relational RNN Works

**메모리 칸끼리 어텐션으로 대화하게 하기.** Santoro 외(2018)의 「Relational Recurrent Neural Networks」 3절 식 (1)~(9)를 브라우저에서 직접 계산해 보는 인터랙티브 교육 자료입니다. 메모리를 여러 슬롯으로 두고, 매 스텝 [M; x] 위의 multi-head attention 한 번으로 새 입력을 받아들이는 일과 기억끼리 비교하는 일을 함께 하는 Relational Memory Core(RMC)를 따라갑니다.

🔗 **[데모 바로가기](https://dev-jonghoonpark.github.io/how-relational-rnn-works/)**

## 다루는 내용

1. **관계 추론: 속성이 아니라 관계** · 점은 그대로 두고 기준점 m만 바꾸면 답이 바뀌는 “N번째로 먼 점”
2. **칸끼리 대화하지 않는 기억** · LSTM / NTM / RMC가 한 스텝 안에 비교하는 대상(키 ↔ 칸 vs 칸 ↔ 칸)
3. **기억끼리 어텐션 — 식 (1)** · 슬롯 수·헤드 수 슬라이더와 헤드별 어텐션 가중치 히트맵
4. **새 입력도 같은 어텐션으로 — 식 (2)** · N × (N + 1) 어텐션, Sonnet 구현(전체 셀프 어텐션 후 마지막 행 버리기)과 값이 같음을 확인
5. **MLP와 게이트 — 식 (3)~(9)** · 망각 편향 +1이 반감기를 1 → 2.2스텝으로 늘리는 것, unit / memory 게이팅
6. **슬롯 수, 슬롯 크기, 헤드 수** · 슬롯 수와 무관한 파라미터 계산기(언어 모델 설정 코어 약 7,600만), 같은 값으로 시작한 슬롯은 영원히 같다는 대칭 실험
7. **N번째로 먼 벡터** · 내적이 거리²가 되게 손으로 배선한 어텐션, 브라우저에서 학습하는 어텐션 한 층([x, 1] 78% / [x, ‖x‖², 1] 99% / 슬롯별 MLP 94%), 논문 결과(LSTM·DNC 30% 미만 vs RMC 91%), 직접 붙여 본 RMC vs LSTM 비교(장난감 규모에서는 재현되지 않음 — LSTM이 더 빨리 배우고, 어려운 설정에서 RMC는 찍기 수준의 평평한 구간에 머묾)과 그 이유
8. **프로그램 평가와 강화학습** · Learning to Execute 형식 문제 생성기와 글자 단위 채점, 표 1, Mini PacMan(677 vs 550), 직접 풀어 보는 viewport BoxWorld(98% vs 73%)
9. **언어 모델링** · 표 2·3 퍼플렉시티(WikiText-103 31.6), 흔한 단어는 좋아지고 드문 단어는 나빠진 것, 실험이 슬롯 1개였다는 단서
10. **한계와 그다음** · 왜 좋아졌는지 모른다는 저자들의 단서, 고정 메모리 vs 커지는 버퍼의 비용
11. **퀴즈** 6문항

## 참고

- Santoro, Faulkner, Raposo 외 (2018), *Relational recurrent neural networks*, NeurIPS 2018 — [arXiv:1806.01822](https://arxiv.org/abs/1806.01822)
- DeepMind Sonnet, [`relational_memory.py`](https://github.com/deepmind/sonnet/blob/v1/sonnet/python/modules/relational_memory.py)

[how-ai-works](https://github.com/dev-jonghoonpark/how-ai-works) 시리즈의 일부입니다.
