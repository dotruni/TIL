**\[주의: 그...학습 목표를 기준으로 트랜스포머..를 이해하려고 발버둥치려는 과정입네다😜😉💜🤦‍♀️. 작성중\]**
​
-   🏆 학습 목표
-   Transformer의 장점과 주요 프로세스인 Self-Attention에 대해 이해하고 설명할 수 있다.
-   GPT, BERT 그리고 다른 모델에 대해서 개략적으로 설명할 수 있다.
​
**Attention 메커니즘 --(극대화)--> 트랜스포머 (기계번역 new 모델)-(기반)---> SOTA(State of the art)(자연어 처리 모델)**
​
아 잠시만.. 이 state of the art..? 많이 들어봤는데..?
​
[##_Image|kage@lDNVX/btro7BQrFYb/S6GYwClVrDYHV0vaXiiJVK/img.png|CDM|1.3|{"originWidth":928,"originHeight":594,"style":"alignCenter"}_##]
​
  
10만 유튜버 개발자 칭규가 내가 포항에서 데려간 물회집에서 이 드립을 쳤을 때..
​
내가 어..state-of-the-art는 최첨단이라는 뜻 아니야?  
라고 말했는데 AI 공부하는 걔가 대충 인공지능 관련한거야 라고 했었다. \[오히려 좋아\] 이제 이해할 수 있는건가..?
​
**가보자규!**
​
### **트랜스포머(Transformer)**
​
RNN 단점: 단어가 Sequential 하다(병렬X) -> 계산 시간이 길어진다--> 트랜스포머 두둥 (내가 이걸 해결해줄게!!)  
즉, 트랜스포머: 두둥 타라라락 모든 토큰 나한테 동시에 줘! 나는 병렬 연산 할 수 이쪄! -> GPU에 최적화
​
[##_Image|kage@oSV0c/btrpdnqBXfq/7ivaeAJ6QqSqYvwZGpv2E1/img.png|CDM|1.3|{"originWidth":882,"originHeight":674,"style":"alignCenter"}_##][##_Image|kage@bHKavu/btro9Hpnees/RIGCW3XNDXTUFXbtEvPkB1/img.png|CDM|1.3|{"originWidth":965,"originHeight":671,"style":"alignCenter"}_##]
​
### **Self-Attention이란**
​
: 트랜스포머의 주요 메커니즘 중 하나임.'  
'The animal didn't cross the street because **it** was too tired' --> 의 it 이 뭔지를 알아야 번역 제대로 하겠지  
그래서 자기 자신을 활용하는 거야 약간 그 뭐냐 '네 자신을 알라','자기 복제 도마뱀'
​
근데 **Attention이 뭐냐 Pay Attention!**  
내가 구글에 "저는 왜 눈도 코도 예쁠까요?" 라고 검색한다고 치자.  
Query: "저는 왜 눈도 코도 예쁠까요?"  
Key: 눈, 코, 예쁠까요  
Value: 관련한 페이지
​
**Self-Attention은 각 Query,Key,Value 벡터가 모두 가중치 벡터**  
어떻게 이 셀프 어텐션을 취해주느냐? 어텐션이랑 비슷  
1.특정 단어의 쿼리(q) 벡터와 모든 단어의 키(k) 벡터를 내적-> 나오는 값이 Attention 스코어(Score)  
2.이 가중치를 q,k,v 벡터 차원 의 제곱근인 로 나누어 줌-> 계산 보정  
3\. Softmax를 취해줍니다. -> 단어와 문장 내 다른 단어가 가지는 관계의 비율을 구할 수 있음.  
4\. 밸류(v) 각 단어의 벡터를 곱해준 후 모두 더함
​
[##_Image|kage@9O4Hr/btrpjwNARKh/jFOT3ZCabyNsRHQjzgKpWK/img.png|CDM|1.3|{"originWidth":823,"originHeight":373,"style":"alignCenter"}_##]
​
### **Multi-Head Attention**
​
### **:Self-Attention을 동시에 / 병렬적으로 /실행하는 것**
​
[##_Image|kage@0WU7L/btrpjwz3IND/M81LlVjQH8PfyuVdQmXlXk/img.png|CDM|1.3|{"originWidth":940,"originHeight":824,"style":"alignCenter"}_##][##_Image|kage@mJdE5/btrpcMqoihh/K5JcmoR5Hp5G0Y67nbwkk1/img.png|CDM|1.3|{"originWidth":335,"originHeight":305,"style":"alignCenter"}_##]
​
트랜스포머의 모든 sub-layer에서 출력된 벡터는 Layer normalization과 Skip connection 거침  
그 다음 FFNN(Feed forward neural network) 활성화 함수(Activation function)으로 ReLU를 사용
​
### **Masked Self-Attention :디코더 블록에서 사용되는 특수한 Self-Attention**
​
[##_Image|kage@bAhab5/btrpiu3xtr3/Y3tgPzjLSsUh0BkJcAkpn1/img.png|CDM|1.3|{"originWidth":407,"originHeight":359,"style":"alignCenter"}_##]
