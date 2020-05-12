## Open source trends

1. Transformers
Compression, fine-tuning, adversarial attach, explainability 등 모델이 pre-trained 된 상태에서 시작해야하는 연구들의 경우 transformers라이브러리가 많이 활용됨. 혹은 새로운 모델 연구의 베이스 라인 성능 측정을 위한 도구로 활용.


2. fairseq( FAIR)
인코딩과 디코딩작업이 수반되어야 하는 seq2seq 아키텍처를 활용한 연구를 위해 많이 사용되는 라이브러리.
Summarization , GEC 등의 연구에 자주 활용되며 NMT 연구에 많이 활용됨. 프레임워크에 기작성딘 모델들 뿐만 아니라 editable 옵션을 통해 자신들의 실험 모델을 직접 구현하고 연구에 활용함.


3. spaCy(explosion.ai)
spaCy는 시퀀스 태깅 연구에 많이 활용됨. spaCy 모델을 활용해 그 위에 모델을 올린다던가 혹은 자신들이 연구한 태깅 모델을 spacy API에 맞추어 제공해준다던가 등과 같은 방식으로 활용됨.

4. Stanford CoreNLP (by. Stanford Univ)
CoreNLP는 주로 토크나이저 및 파서로 실험에 활용되는 경우가 많음. 특히 Dependency, Constituency, POS 등 다양한 파싱 결과를 모델에 함께 적용하고자 할 때 많이 활용되고 있음. 최근에는 파이썬으로 래핑된 라이브러리 Stanza를 출시하기도 했으니, 사용법이나 라이브러리의 출력물 등을 익혀두시면 좋을 것 같음.

5. Allen NLP (by. Allen AI)
Allen AI 에서 관리하는 Allen NLP의 경우 대개 Allen AI의 연구에 많이 활용되고 있음. 최근에 페이지를 통해 소개드린 Longformer, SledgeHammer 등이 Allen NLP를 활용해 연구된 논문. Allen AI의 자연어 처리 연구는 그 영향력이 상당히 큰 편이기 때문에 Allen NLP 역시 알아두시면 좋으리라 생각됨. 이와 별개로 Pre-trained ELMo 임베딩을 활용하는 연구들에서도 Allen NLP를 활용하고 있음.

6. FAISS (by. FAIR)
인퍼런스 속도를 개선하기 위해 임베딩할 수 있는 데이터 인스턴스들을 모두 임베딩해 저장한 후, 입력 쿼리와 임베딩 스페이스에서 가장 가까이 위치한 Nearest Neighbor Search를 수행하는 연구들에는 페이스북 리서치의 FAISS가 자주 활용됨. FAISS는 비단 이런 유형의 연구들 뿐만 아니라 어플리케이션에 시맨틱한 Retrieval을 적용해야 하는 개발자들에게도 많은 사랑을 받고 있음.
