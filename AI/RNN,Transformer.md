[주의: 배우는 중, 오늘 배운 내용이 어려..운 관계로 학습 목표에 맞춘 나의 작고 귀여운 휘뚜루 마뚜루 이해]

-통계적 언어 모델 (전통적) 
횟수기반 (확률) -> OOV -> Sparsity (희소성)문제  
- So, 어제 배운 [임베딩 벡터]를 써서 Word2vec, fastText같은 유사성을 끼워넣어 줌.

- This is 신경망 언어 모델 -> 희소성 해소  
---> 이 언어 모델에서 사용되는 것
### **---> :순환 신경망 (RNN)** 

### 얘 특 
- Sequential 한 데이터를 처리 
- 순환(Recurrent) : ->은닉-> 입력->은닉 -> 
   (약간 급식실 줄서는데 오늘 요구르트 나와서 계속 애들이 다시 모른척하고 또 줄서는 느낌) 

- 이론적으로는 무한대로 줄을 늘려서 급식을 설수있긴 하지.. (어떤 길이의 sequential 데이터도 이론적으로는 처리 가능) 
- 근데 무조건 한줄 서기임 두줄 세줄 XX (병렬화 불가능) 

- 그리고 얘가 계속 자기가 요구르트를 이미 먹은걸 까먹거나 100개는 먹었다고 착각해 
  (역전파 과정에서 기울기 소실과 폭발로 인한 장기 의존성) 

###  So, 이 기울기를 조절해보자!! (장단기 기억망)--> **LSTM&GRU**


**[주의: 그...학습 목표를 기준으로 트랜스포머..를 이해하려고 발버둥치려는 과정입네다😜😉💜🤦‍♀️. 작성중]**

- 🏆 학습 목표
- Transformer의 장점과 주요 프로세스인 Self-Attention에 대해 이해하고 설명할 수 있다.
- GPT, BERT 그리고 다른 모델에 대해서 개략적으로 설명할 수 있다.

**Attention 메커니즘 --(극대화)--> 트랜스포머 (기계번역 new 모델)-(기반)---> SOTA(State of the art)(자연어 처리 모델)**

_아 잠시만.. 이 state of the art..? 많이 들어봤는데..?_ 
![image](https://user-images.githubusercontent.com/89775352/147628250-77b2a9af-ce4c-4d48-af0e-7449c17fa6c7.png)
10만 유튜버 개발자 칭규가..내가 포항에서 데려간 물회집에서 이 드립을 쳤을 때..  어..state-of-the-art는 최첨단이라는 뜻 아니야?
라고 말하니까 AI 공부하는 걔가 대충 인공지능 관련한거야 라고 했었다. [오히려 좋아] 이제 이해할 수 있는건가..?

가보자규!

### **트랜스포머(Transformer)**
RNN 단점: 단어가 Sequential 하다(병렬X) -> 계산 시간이 길어진다--> 트랜스포머 두둥 (내가 이걸 해결해줄게!!)
즉, 트랜스포머: 두둥 타라라락 모든 토큰 나한테 동시에 줘! 나는 병렬 연산 할 수 이쪄! -> GPU에 최적화 
![image](https://user-images.githubusercontent.com/89775352/147629387-23a8bc55-628b-483a-91df-5a887a6141ed.png)
 
![image](https://user-images.githubusercontent.com/89775352/147630547-cdc80de5-2629-4362-8694-383e3bb18ab5.png)

### **Self-Attention이란**
: 트랜스포머의 주요 메커니즘 중 하나임.'
'The animal didn't cross the street because **it** was too tired'  --> 의 it 이 뭔지를 알아야 번역 제대로 하겠지
그래서 자기 자신을 활용하는 거야 약간 그 뭐냐 '네 자신을 알라','자기 복제 도마뱀'

근데 **Attention이 뭐냐 Pay Attention!** 
내가 구글에 "저는 왜 눈도 코도 예쁠까요?" 라고 검색한다고 치자. 
Query: "저는 왜 눈도 코도 예쁠까요?" 
Key: 눈, 코, 예쁠까요
Value: 관련한 페이지 

**Self-Attention은 각 Query,Key,Value 벡터가 모두 가중치 벡터**
어떻게 이 셀프 어텐션을 취해주느냐? 어텐션이랑 비슷 
1.특정 단어의 쿼리(q) 벡터와 모든 단어의 키(k) 벡터를 내적-> 나오는 값이 Attention 스코어(Score)
2.이 가중치를 q,k,v 벡터 차원  의 제곱근인  로 나누어 줌-> 계산 보정
3. Softmax를 취해줍니다. -> 단어와 문장 내 다른 단어가 가지는 관계의 비율을 구할 수 있음.
4. 밸류(v) 각 단어의 벡터를 곱해준 후 모두 더함
![image](https://user-images.githubusercontent.com/89775352/147632810-620a8d74-50df-4e1e-bde3-3495a3c60574.png)

### **Multi-Head Attention**
### **:Self-Attention을 동시에 / 병렬적으로 /실행하는 것**
![image](https://user-images.githubusercontent.com/89775352/147632750-d5454389-602f-4c6e-b9f0-139f3448ac8c.png)

![image](https://user-images.githubusercontent.com/89775352/147632969-ded22629-79d4-4dfc-a173-b04b1b699420.png)

트랜스포머의 모든 sub-layer에서 출력된 벡터는 Layer normalization과 Skip connection 거침 
그 다음 FFNN(Feed forward neural network) 활성화 함수(Activation function)으로 ReLU를 사용
### **Masked Self-Attention :디코더 블록에서 사용되는 특수한 Self-Attention**
![image](https://user-images.githubusercontent.com/89775352/147633175-a62769da-2040-4f77-981d-7242a3aec8f8.png)
