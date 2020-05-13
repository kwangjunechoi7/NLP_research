## Word piece embedding


자연어처리 딥러닝은 embedding을 해야한다는 점에서 이미지 딥러닝과 차이가 있음. embedding이란 문장을 신경망이 이해할 수 있는 벡터로 변환하는 것을 말함. 임비뎅에는 단어를 기준으로 하는 워드 임베딩과 글자를 기준으로 하는 캐릭터 임베딩이 있음. 보통 워드 임베딩 성능이 놓음. 의미가 더 함축적이기 때문. 그러나 oov에 약함. 이 두가지 임베딩을 섞은 것이 wordpiece임. 먼저 캐릭터 당뉘로 분리, 그 다음 자주 나오는 캐릭터들을 병합하여 하나의 토큰으로 만듬. 이렇게 하면 의미있는 캐릭터들끼리 묶여지기 때문에 두 임베딩의 장점이 합해지며 형태소분석또한 필요가 없어서 다양한 언어에 적용할 수 있음.

https://github.com/qbxlvnf11/data-processing/blob/master/Common/tf.data.Dataset%2C%20tensorflow_datasets.ipynb

학습데이터를 사용하지않으면서도 unit을 구성하는 모호성을 없애며 이용하는 방법은 heuristics가 있음.
개막공연이라는 단어가 있다고 가정하자, 만약 공연-의 빈도수가 개막공연- 빈도수와같다면 공연-이나 개막-은 유닛으로 사용하지 않아도 어차피 세 유닛은 모두 개막공연을 나타내기 위한 부분들이 된다,.

자주 쓰이는 단어는 그 자체가 unit이 됨. Rare words가 subword units으로 나뉘어짐.

원문의 띄어쓰기는 _ underbar로 치환되고 단어는 subword 로 통계에 기반하여 띄어쓰리고 분리됨.
기존 띄어쓰기를 언더바로 치환하는 이유는 차후 다시 문장 복원을 위한 장치. 본래의 띄어쓰기를 언더바로 치환하여 두지 않으면 기존 문장을 복원할 수 없음.

- 언어와 상관없이 tokenization을 하려는 시도.
- BPE와 유사
- Rare words를 처리하기 위함.
- 모델을 학습하기 전에 special word boundary symbol을 기존 word에 추가하고 decoding 때는 이를 이용하여 복원함.
    - Word : Jet makers feud over seat width with big orders at stake
    - Wordpiece : _J et _makers _fe ud _over _seat _width _with _big _orders _at _stake
- 띄어쓰기로 일단 word를 구분하는 것 _로 하고 한 word 내에서 자주 발생하는 것으로 나눈다는 개념.
- Jet라는 단어는 rare word로 판단하여 J와 et로 나뉘는 것.
- Workpiece 모델은 data-driven 모델로 maximize the language-model likelihood방법임
    - 주어진 데이터에 따라 모델이 달라진다는 말
    - 그리고 LM모델인데 maximize likelihood로 학습했다고 함.
    - BPE처럼 정해진 개수의 token으로 빈도수로 쪼갬. 그 다음 이 token으로 RNN LM모델로 MLE 방법으로 학습.
- 즉 데이터가 주어지고 원하는 tokens D가 주어지면 D개로 segmentation하는 개념.
