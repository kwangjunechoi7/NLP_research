### Transformer note

- 가장 경쟁력있는 neural sequence transduction models은 encoder-decoder 구조를 이용함.
- 그래서 encoder는 input sequence(x_1, .. x_n)을 continuous representations z = (z_1, .., z_n)에 mapping됨. 주어진 z로 output sequence이 생성됨.
- 각 step은 input data와 이전에 생성된 symbols를 사용하는 (input data+hidden state) auto-regressive임.
- Transformer의 전체적인 architecture는 encoder와 decoder에 stacked self-attention, point-wise, fully connected layers를 사용함.


- encoder는 input으로 vector들의 리스트를 받는데, 이 리스트를 먼저 self-attention layer(continuous representations)를 거치고, 그 다음 feed-forward 신경망에 통과시키고 그 결과물을 encoder에 전달.
- Decoding 단계의 각 step은 output sequence의 한 element를 출력. (번역 시)
- Decoding step은 decoder가 출력을 완료 했다는 special 기호인 <eos> (end of sentence)를 출력할 때 까지 반복됨.
- 각 step 마다 출력된 단어는 다음 step의 가장 밑단의 decoder에 들어가게 되고, 마찬가지로 여러 개의 decoder를 거침. Encoder의 input에 했던 것과 동일하게 embedding 한 후 positional encoding을 추가해 각 단어의 위치 정보를 더해줌.

* Query(Q) : 영향을 받는 단어 A를 나타내는 변수입니다.
* Key(K) : 영향을 주는 단어 B를 나타내는 변수입니다.
* Value(V) : 그 영향에 대한 가중치를 나타냅니다.

## Attention note

- Encoder로 들어온 inputd은 일단 먼저 self-attention layer를 지나게 됨. 이 layer는 encoder가 하나의 특정한 단어를 encode 하기 위해서 입력 내의 모든 단어들과의 관계를 살펴봄.
- 입력이 self-attention 층을 통과하여 나온 출력은 다시 feed-forward 신경망으로 들어감. 똑같은 feed-forward 신경망이 각 위치의 단어마다 독립적으로 적용돼 output을 생성.
- decoder 또한 encoder에 있는ㄴ 두 layer를 모두 갖고 있음. 그러나 두 층 사이에 seq2seq model의 attention과 비슷한 encoder-decoder attention이 포함되어 있음. 이는 decoder가 입력 문장 중 각 타임 스템에서 가장 관련있는 부분에 집중할 수 있게 함.
- transfomer 역시 기타 word embedding algorithme처럼 input words를 vector로 바꾸는 작업부터 시작.
- (512 가정) 각 단어들은 크기 512의 벡터 하나로 embedding됨.
- 이 embedding은 가장 밑단의 encoder에서만 일어남. 문장의 단어들을 embedding한 뒤에 각 단어에 해당하는 벡터들은 encoder내의 두 개의 sub-layer로 들어감.
- 각 단어가 그만의 path를 통해 encoder에서 흘러간다는 transformer 모델의 주요 성질을 볼 수 있음. self-attention층에서 이 위치에 따른 path들 사이에 다 dependency가 있음. 반면, feed-forward 층은 이런 dependency가 없기 때문에 feed-forward layer 내의 다양한 path들은 병렬처리 될 수 있음.
- encoder는 입력으로 vertor들의 list를 받음. 이 list를 먼저 self-attention layer로 보냄. 그 뒤 feed-forward신경망에 통과시키고 그 결과물을 다음 encoder에게 전달함. 각 단어들은 각각 다른 self-encoding과정을 거침. 그 다름으로는 모두에게 같은 과정인 feed-forward 신경망을 거침.
- how to calculate self-attention? self-attention 연산의 첫 단계는 encoder에 입력된 벡터들(보통 각 단어의 embedding 벡터)에게서부터 각 3개의 vector를 만들어 내는 일. 각 단어에 대한 query, key, Value vectors.
- x1를 weight 행렬인 WQ로 곱하는 것은 현재 단어와 연관된 query 벡터인 q1를 생성함. 같은 방법으로 우리는 입력 문장에 있는 각 단어에 대한 query, key, value 벡터를 만들 수 있음.
- what does those meaning? attention에 대해서 생각하고 계산하려고 할때 사용하는 추상적인 개념.
- self-attention 계산의 두 번째 스텝은 점수를 계산하는 것. 아래 예시의 첫 번째 단어인 thinking에 대해서 self-attention을 계산한다고 가정. 이 단어와 입력 문장 속의 다른 모든 단어들에 대해서 각가 점수를 계산함. 이 점수는 현재 위치의 이 단어를 encode 할 때 다른 단어들에 대해서 얼마나 attention하는지를 결정.
- score는 현재 단어의 query vector와 점수를 매기려하는 다른 위치에 있는 단어의 key vector의 내적으로 계산됨. 다시 말해 어떤 단어에 대한 self-attention을 계산한다 했을 때, score는 q1과 k1의 내적일 것.

##### tensor2tensor colab notebooks
https://colab.research.google.com/github/tensorflow/tensor2tensor/blob/master/tensor2tensor/notebooks/hello_t2t.ipynb
