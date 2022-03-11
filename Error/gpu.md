### Error: root:Internal Python error in the inspect module

nlp  도중 구글 드라이브 마운트하고 cd 하는데에 이 에러가 떠서 당황했다..

아직 빈도수 낮은 단어들만 제거해주고 토큰화 하는 정도라 GPU 쓸 필요 없다고 생각해서

하드웨어 가속기 None 으로 썼었는데,,, 이 에러는 tensorflow 버전 문제라 하기에 생각이 나서

**런타임 유형을 GPU로** **변경**해 주니 해결되었다! 

![](https://t1.daumcdn.net/keditor/emoticon/friends1/large/006.gif)

#### + 추가 에러/ 대부분 에러는 오타에서 발생한다/ 딱 봤을 때 오타 모르겠다면,, s안붙였는지 확인하기..

#### + 함수에서 혹시 : 안썼는지 확인 하기 
