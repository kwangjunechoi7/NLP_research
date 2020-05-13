### SQuAD 2.0 변경사항

Squad 1.0 버전 대의 경우 input이 context/question pair로 제공됨. output은 답변 텍스트의 시작과 끝을 indexting하는 정수의 쌍.

QA 모델은 크게 두 부분으로 나뉨. 문서에서 질문을 차적나 생성하는 question generation(OG), 질문에 맞는 답을 찾는 question answer 부분. 방향을 잡은 것은 주어진 문서를 요약하는 방식으로 중요한 문장들을 뽑아내어 이를 답이 되는 문장으로 먼저 사용하고 (qa), 중요 문장을 질문 생성 모델 (qg)에 넣어 질문을 출력함.

모델이 잘 만들어 졌다는 것은 어떻게 판단하는가? 평가지표 선정이 매우 중요. 모델을 잘 만들었어도 그게 어느정도 좋은지 모르면 아무 소용없을 것. qa모델의 경우, 출력값으로 나온 질문과 답변이 주어진 문서에서 나올 수 있고 또 절절한지를 판단해야함. 그리고 질문과 답변이 서로 연관있는지도 봐야함. squad는 F1과 Exact Match를 사용해 모델의 정확성을 측정함.

F1 measures the portion of overlap tokens between the predicted answer F1 measures the portion of overlap tokens between the predicted answer and ground truth, while exact match score is 1 if the prediction is exactly the same as groudtruth or 0 otherwise.


Example _ squad 1.0
——————————————————————————————————————————
Context: Architecturally, the school has a Catholic character. Atop the Main Building's gold dome is a golden statue of the Virgin Mary. Immediately in front of the Main Building and facing it, is a copper statue of Christ with arms upraised with the legend "Venite Ad Me Omnes". Next to the Main Building is the Basilica of the Sacred Heart. Immediately behind the basilica is the Grotto, a Marian place of prayer and reflection. It is a replica of the grotto at Lourdes, France where the Virgin Mary reputedly appeared to Saint Bernadette Soubirous in 1858. At the end of the main drive (and in a direct line that connects through 3 statues and the Gold Dome), is a simple, modern stone statue of Mary.

Question: The Basilica of the Sacred heart at Notre Dame is beside to which structure?

Answer: start_position: 49, end_position: 51
——————————————————————————————————————————


Paragraph: Victoria is a state in south-eastern Australia…Most of its population is concentrated in the area surrounding…its state capital and largest city, Melbourne…
Question: What city is the capital of Victoria?
Answer: Melbourne

Paragraph: Victoria is a state in south-eastern Australia…Most of its population is concentrated in the area surrounding…its state capital and largest city, Melbourne…
Question: What city is the capital of Australia?
Answer: <No Answer>

Extracting reading comprehension systems는 context document에 대한 질문의 대답을 찾을 수 있으나, context에 명시되지 않은 정답에 대해 신뢰할 수 없는 예측을 하는 경향이 많다. 그래서 기존 데이터 세트는 답변 가능한 질문에만 초점을 맞추거나, 식별하기 쉬운 automatically generated unanswerable question를 사용함.

——————————————————————————————————————————
	⁃	BERT _ 초간단 설명
	+ sentences 중에서 무작위로 가려진 단어를 에측하고
	+ sentence B가 sentence A의 뒤에 따라오는지 판별하는 방식으로 학습
	+ 그 결과 단어들을 포함시키는 pre-trained된 encoder로 주 변 상황을 파악함.
	+ fully connected layer로 후가적으로 보완했을 때 BERT는 놀라운 성능을 보일 수 있었음.
——————————————————————————————————————————


Example _ squad 2.0
——————————————————————————————————————————
Context: BeyoncГ©'s first solo recording was a feature on Jay Z's "'03 Bonnie & Clyde" that was released in October 2002, peaking at number four on the U.S. Billboard Hot 100 chart. Her first solo album Dangerously in Love was released on June 24, 2003, after Michelle Williams and Kelly Rowland had released their solo efforts. The album sold 317,000 copies in its first week, debuted atop the Billboard 200, and has since sold 11 million copies worldwide. The album's lead single, "Crazy in Love", featuring Jay Z, became BeyoncГ©'s first number-one single as a solo artist in the US. The single "Baby Boy" also reached number one, and singles, "Me, Myself and I" and "Naughty Girl", both reached the top-five. The album earned BeyoncГ© a then record-tying five awards at the 46th Annual Grammy Awards; Best Contemporary R&B Album, Best Female R&B Vocal Performance for "Dangerously in Love 2", Best R&B Song and Best Rap/Sung Collaboration for "Crazy in Love", and Best R&B Performance by a Duo or Group with Vocals for "The Closer I Get to You" with Luther Vandross.

QuestionAbswerSets : {<|"Question" -> "How many albums did Beyonce sell in the first week when she released her second album?", "Answers" -> {"541,000"}, "AnswerPositions" -> {133}, "Answerable" -> True, "QuestionID" -> "56be932e3aeaaa14008c90f9"|>, <|"Question" -> "The lead single from the album was which song?", "Answers" -> {"DГ©jГ  Vu"}, "AnswerPositions" -> {304}, "Answerable" -> True, "QuestionID" -> "56be932e3aeaaa14008c90fa"|>, <|"Question" -> "How many countries did her song \"Irreplaceable\" get number one status in?", "Answers" -> {"five"}, "AnswerPositions" -> {347}, "Answerable" -> True, "QuestionID" -> "56be932e3aeaaa14008c90fb"|>, <|"Question" -> "How many singles did her second album produce?", "Answers" -> {"five"}, "AnswerPositions" -> {347}, "Answerable" -> True, "QuestionID" -> "56be932e3aeaaa14008c90fc"|>, <|"Question" -> "What birthday did Beyonce's album B'Day celebrate?", "Answers" -> {"twenty-fifth birthday"}, "AnswerPositions" -> {102}, "Answerable" -> True, "QuestionID" -> "56bf940da10cfb1400551189"|>, <|"Question" -> "What artist did Beyonce duet with in the single, \"Deja Vu''?", "Answers" -> {"Jay Z"}, "AnswerPositions" -> {324}, "Answerable" -> True, "QuestionID" -> "56bf940da10cfb140055118a"|>, <|"Question" -> "How high did ''Deja Vu'' climb on the Billboard chart?", "Answers" -> {"top five"}, "AnswerPositions" -> {343}, "Answerable" -> True, "QuestionID" -> "56bf940da10cfb140055118b"|>, <|"Question" -> "What is the name of BeyoncГ©'s second album?", "Answers" -> {"B'Day"}, "AnswerPositions" -> {29}, "Answerable" -> True, "QuestionID" -> "56d4bc642ccc5a1400d83190"|>, <|"Question" -> "How many copies did B'Day sell during the first week of its release?", "Answers" -> {"541,000"}, "AnswerPositions" -> {133}, "Answerable" -> True, "QuestionID" -> "56d4bc642ccc5a1400d83191"|>, <|"Question" -> "Who collaborated with BeyoncГ© on the single, Deja Vu?", "Answers" -> {"Jay Z"}, "AnswerPositions" -> {324}, "Answerable" -> True, "QuestionID" -> "56d4bc642ccc5a1400d83192"|>, <|"Question" -> "Which single from B'Day was only released in the U.K.?", "Answers" -> {"Green Light"}, "AnswerPositions" -> {636}, "Answerable" -> True, "QuestionID" -> "56d4bc642ccc5a1400d83193"|>}——————————————————————————————————————————

	⁃	Extractive reading comprehension system 들은 본문 내에서 질문에 대한 정확한 답을 찾아주기도 하지만, 제대로 된 답이 본문에 없을 경우 부정확한 추측을 리턴하기도 함.
	⁃	현존하는 reading comprehension 데이터 셋들은 주로 answerable(대답가능한) 질문들에 초점을 맞추거나 쉽게 판별이 가능한 가짜 unanswerable 대답 불가능한 질문들을 자동으로 생성해왔음.
	⁃	이러한 약점을 보강하기 위해 등장한 것이 squad 2.0
	⁃	기존 데이터 셋에 새로운 5만개 이상의 unanswerable questions을 병합함.
	⁃	Unanswerable question은 온라인 crowd worker들이 직접 생성. 기계적으로 생성한 것이 아니라 인간이 생성했기 때문에 질이 더 높다고 가정)
	⁃	worker들에 의해 생성된 unanswerable question은 응답 가능한 질문들과 유사하여 기계적으로 판별이 어려움.
	⁃	이제 task가 단순히 정답이 가능할 때만 리턴하는 것이 아니라 주어진 본문에 정답이 없는 경우에도 그 여부를 리턴하는것으로 더 어려워짐. (Squad 1.1에서 f1이 86%였던 신경망이 squad 에서는 66%)

Dataset
	⁃	저자들은 새로운 데이터 셋 제작 프로젝트에 있어 일반적인 large size, diversity, low noise에도 unanswerable question에 대한 두 가지 요구사항(desiderata)를 정의.
	⁃	1. Relevance 연관성 응답 불가능 질문들을 주어진 본문과 주제적으로 연관이 있어야함. 그렇게 않으면 단순한 휴리스틱으로도 응답 가능/불가능 여부를 판별할 수 있게 됨.
	⁃	2. Existence of plausible answers 그럴듯한 정답의 존재 : 본문 내에 응답 불가능 질문이 요구하는 정답과 타입이 일치하는 그럴듯한 정답 구절이 포함되어야 함.
	⁃	예를들어 1992년에 설립된 회사는 무엇인가? What company was founded in 1992?라는 질문이 있을 경우 본문 내에 어떠한 회사명이 존재해야함. 그렇지 않으면 type-matching 휴리스틱이 응답/가능 불가능 여부를 판별할 수 있기 때문.

Existing datasets
	⁃	저자들은 이러한 조건 (criteria)들을 염두에 두면서 현존하는 다른 기계독해 데이터셋을 조사함. 응답 불가능한 지룸ㄴ과 짝을 이루는 본문을 지칭하기 위해 negative example이라는 용어가 사용됨.

	0.	Extractive datasets
	⁃	추출식 기계독해 extractive reading comprehension 데이터셋에서는 주어진 본문 내에서 시스템이 정답을 추출함.
	⁃	2017 zero-shot relation extraction dataset
	⁃	2017 triviaQA
	⁃	2017 negative examples for SQuAD
	⁃	2017 NewsQA

	0.	Answer sentence selection datasets
	⁃	정답 문장 선택 데이터 셋은 시스템이 질문에 답하는 문장을 그렇지 않은 문장보다 높게 순위를 매길 수있는지 테스트 합.
	0.	Multiple choice dataset


SQuAD 2.0

	0.	Dataset creation

	⁃	응답 불가능 질문을 생성하기 위해 저자들은 crowdworker를 고용.
	⁃	데이터 생성에는 SQuAD 1.1의 모든 본ㅁ누이 사용됨.
	⁃	본문의 각 문단마다 작업자들은 주어진 문단 단ㄷ고으로는 대답할 수 없는 응답 불가능 질문들을 최대 다섯 개 까지 생성하도록 지시 받음.
	⁃	이 과정에서 본문내의 개체를 질문에 사용하고, 그럴 듯한 진짜 질문이 존재하는지 여부를 확인함.
	⁃	Squad 1.1의 응답 가능 질문들이 참고로 제시되었고, 이를 통해 작업자들은 응답 불가능 질문을 더욱 진짜 처럼 만들 수 있었음.



기존의 언어 모델, left-to-right
"남자는 가게에 갔다" ("the man went to a store")
P(the | <s>)*P(man | <s> the)*P(went | <s> the man)*...

문제는 일반적으로 세부 task의 경우 언어 모델을 만드는 것을 원하는 것이 아니라 특정 상황 context에 맞는 특정 단어의 상황별 연어 포현을 원한다는 것. 각 단어가 왼쪽의 맥락만 된다면 많은 것이 누락될 것. 그래서 사람들이 사용한 트릭이 오른쪽에서 왼쪽으로도 모델을 훈련하는 것.
P(store|</s>)*P(a|store </s>)*…

이렇게 각 단어들에 대해 두 개의 언어 표현이 생김. 하나는 왼쪽에서 오른쪽으로 다른 하나는 오른쪽에서 왼쪽으로, 그리고 세부 task를 위해 이들은 concatenation하여 사용함.

하지만 직관적으로 양방향성이 강한 deeply bi-directional 단일 모델을 훈련시키는 것이 훨씬 더 나음.

불행히도 일반적인 LM과 같이 깊게 양방향성을 지닌 모델을 훈련시키는 것은 불가능함. 이는 단어들이 간접적으로 자기 자신을 바라보는 see themeselves 사이클을 만들어 예측이 비정상적 trivial  발생할 수 있기 때문. 

대신 우리가 할 수 있는 것은 입력에서 단어의 일부분을 mask 하고 문맥 내에서 해당 단어를 재구성하는데 사용된 de-noising auto-encoder라 불리는 간단한 트릭 사용. 이것을 masked LM이라고 부름.

	⁃	Task 1: Tased LM
Input:
The man [MASK1] to [MASK2] store
Label:
[MASK1] = went; [MASK2] = a

입력값을 deep transformer encoder에 넣고 masked  위치에 해당하는 최종 hidden states를 사용하여 masked 단어를 예측함. 이는 언어모델을 학습시키는 방식과 동일.

LM에서 빠진 또 다른 것은, 많은 NLP task에서 중요하게 다루어져야 할 문장 간의 관계를 이해하지 못한다는 것. 문장 관게 모델을 사전 학습 시키기 위해 매우 간단한 이진 분류 task를 수행. 이 작업은 두 문장 A와 B를 연결하고 B가 원문 텍스트에서 A 다음에 실제로 오는지 예측하는 것.

	⁃	Task 2: Next sentence prediction
Input:
The man went to. He. Tore [sep] he bought a gallon of milk
Label:
IsNext

Input:
The man went to the store [sep] penguins are flightless birds
Label:
NotNext



##### BERT
	⁃	MLM
	⁃	Close task에서 영향을 받음
	⁃	무작위로 input 토큰 중 일부를 가리고,그 문맥만 이용하여 가려진 단어의 id를 예측
	⁃	Left-to-right 언어 모델 사전 학습과 달리 MLM objective는 깊은 양방향 트랜스포머 모델을 학습함으로써 왼쪽과 오른쪽 맥락을 융합.
	⁃	저자들이 기여한 점
	⁃	Representation에 있어 양방향 사전학습의 중요성을 증명함.
	⁃	사전 학습된 언어 표현은 특정 task를 목적으로 무겝게 제작된 모델 구조의 필요성을 없애준다는 것을 증명
	⁃	BERT 모델은 11개의 NLP task에 대해 현존 최고 수준을 보임.
