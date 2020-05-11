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
