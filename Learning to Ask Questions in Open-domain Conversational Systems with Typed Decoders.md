1. 서론
뭘 질문할지 정하는건 얼마나 이해했냐에 따른다. 질문하는거는 대화를 더 깊게 이끌 수 있는 중요한 주도적 행위다.
open domain에서 Question Generation(QG)는 인간-기계 간 상호적용적이고 계속되는 상호작용을 강화하는 것이다.
tradition QG는 생성된 질문을 통해 정보를 찾는 것이 주 목적이다.

두가지 측면에서 unique하다
같은 input에 대해 다양한 질문 패턴이 존재한다.
주어진 입력에서 많은 주제전환 topic을 보내야한다. -> scene understanding, comprehend a scenario를 해야 input에 대한 주제를 설명할 수 있다.

좋은 질문은 다양한 패턴으로 질문하고 주제 전환 topic을 자연스럽게 보내야함
의문사 + topic word + ordinary word로 구성됨
의문사는 질문 패턴, topic word는 주제 전환을 위한 key information을 보냄, ordinary word는 문법, 구문론적 역할

이렇게 3가지로 나눔
STD, HTD 두가지 디코더 고안
decoding 단계에서 단어들의 type 분포를 측정한다.

STD는 mixture of type-specific generation distribution
HTD는 Gumble softmax, modulates the generation distirbution by type probabilityies.

conversation system setting에서 question generation하는 최초의 연구
단어의 type을 포착하는 typed decoder 고안

2. 관련연구

3. methdology
Y* = argmax_y P(Y|X)
training 동안 각 단어의 type은 자동적으로 정해진다.
20개의 의문사를 모음
동사와 명사는 topic word로 취급
다른 word는 ordinary word
PMI 사용해서 주어진 post의 몇개의 topic word 예측

STD : word type에 대한 type distribution과 3가지 type-specific generation distirbution을 추정하여 word generation을 위한 mixture of type-specific distirbution을 구함 
HTD :  gumble softmax를 사용해서 argmax를 근사화하여 decoding process를 더 explicitly하게 함
final generation probability of a word가 word type에 의해 조절됨

STD
P(y_t|y_{<t},X)=\sigma^k_{i=1}P(y_t|ty_t=c_i,y_{<t},X)\dotP(ty_t=c_i|y_{<t},X)
첫번째 term은 mixture of the type-specific generation probability이다. 두번째 term을 가중치로 가짐
두번째 term은 probability of the type distribution
여기서는 word type을 ㅁ여시적으로 구분할 필요가 없어 word type이 latent이다. 
모든 word는 주어진 context에 따라 다른 확률을 가지면서 3가지 type에 모두 속할 수 있다.
probability distribution over word types C={c_1,...,c_k}(k=3)는 아래와 같이 주어진다.
decoder hidden state에 fc layer 거쳐서 softmax한 거
type-specific generation distribution은 다음과 같다.
distribution for each word type은 각각 parameter를 가짐
multiple type-specific generation distribution을 적용해서 decoder를 더 풍부하게 한다.
time step에서 이게 어떤 type 일지 확률에 time step에서 어떤 type c에 대한 전체 단어들의 확률 분포를 곱해서 구함
즉 time step에서 각 type마다 단어 확률이 나오면 그걸 더해
그래서 결과값이 어떤 type인지는 몰라
generation distribution은 같은 word에 대해 존재하므로 word type을 명시적으로 분류할 필요는 없다.

3.6 HTD
단어가 각 post마다 3가지 중 하나로 분류된다.
디코더는 각  위치에 대한 type distribution을 추정하고 가장 높은 type probability를 가지는 단어를 생성한다.

argmax는 두가지 문제가 생김
1. cascade decision process(먼저 타입 정하고, 단어를 정함)는 처음 잘못 선택하면 문법오류가 자주 생김
2. argmax는 미분 불가능함 역전파 안됨

gumbel-softmax로 해결
특정 type probability는 그 type의 단어에만 영향
각 post에서 topic word는 PMI로 의문사는 small dictionary로 나머지는 ordinary word로 구분

3.5 Loss function
NLL쓰는데 type도 같이 학습

3.6 Topic Word Prediction
