### Textrank note

- textrank는 Mihalcea(2004)이 제안한 알고리즘으로 텍스트에 관한 graph-based ranking model 로써, google의 pagerank를 활용한 알고리즘임


- PageRank는 Brian and Page(1998)이 제안한 알고리즘으로 하이퍼링크를 가지는 웹 문서에 상대적 중요도에 따라 가중치를 부여하는 방법


- 서로간의 인용과 참조로 연결된 임의의 묶음에 적용할 수 있음
- (wikipedia) PageRank가 높은 웹페이지는 다른 웹사이트로부터 링크를 받음
- 즉 다른 사이트가 참조를 많이 한 것으로 해석할 수 있음
- 이러한 pagerank 알고리즘을 활용한 것이 바로 textrank임
- textrank는 pagerank의 중요도가 높은 웹사이트는 다른 많은 사이트로부터 링크를 받는다는 점에 착안하여 문서 내의 문장(or 단어)를 이용해 문장의 ranking을 계산하는 알고리즘
- textrank는 pagerank의 중요도가 높은 웹 사이트는 다른 많은 사이트로부터 링크를 받는다는 점에 착안하여 문서 내의 문장(or 단어)을 이용해 문장의 ranking을 계산하는 알고리즘임

$$TR(V_{I} = (1-d) + d * \sum_{V_{j}\in In(V_{i})}\frac{W_{ji}}{\sum_{V_{k}\in Out(V_{j})}W_{jk}}TR(V_{j})$$

- TR(V_i) : 문장 또는 단어 (V)i에 대한 TextRank 값
- d : damping factor, PageRank에서 웹 서핑을 하는 사람이 해당 페이지를 만족하지 못하고 다른 페이지로 이동하는 확률, TextRank에서도 그 값을 그대로 사용(0.85로 설정)
- TextRank TR(V_i)를 계산한 뒤 높은 순으로 정렬
- TextRank를 통해 문서 요약 프로세스 구현
