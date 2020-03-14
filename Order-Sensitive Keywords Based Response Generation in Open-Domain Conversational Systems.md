Order-Sensitive Keywords Based Response Generation in Open-Domain Conversational Systems
https://dl.acm.org/doi/10.1145/3343258

overview
input을 encoder와 keyword 리스트를 만드는 keywords generator로 보냄
keyword리스트는 inpur과 가장 잘 맞는 순서로 재배치됨
keyword update mechanism에 따라 decoder를 condition함
decoder는 keyword를 예측할지 담에 할지 keyword gating function으로 결정

encoder
biGRU를 사용해 context vector c로 요약
c = g(h_1,h_2,...,h_{N_m}), g는 attention mechanism으로 j번째 decoding timestep에서 c_j를 구함

Keywords Generation
training process에서는 ground truth에서 keyword 추출, LDA 모델 사용, ground-truth에 있는 단어는 keyword 아님 background. 
inference process는 input message에서 keyword 추출. 각 message에 대해 LDA로 topic할당, topic아래의 topic word가 keyword 후보.
		N_k개의 keyword를 PMI 점수에 따라 후보에서 뽑음

Keywords Reordering
training process에서는 ground truth에서 keyword 추출하니까 순서 상관 없음, 다른순서는 0, 원래순서는 1로 학습
inference process는 Enhanced Sequential Inference Model 사용
message, keyword biLSTM으로 인코딩. 어텐션하고 그거 모음을 BiLSTM하고 그걸 average, max pooling, 
pooling 한 것을 message랑keyword끼리 concat해서 MLP에 넣어서 softmax
모든 순서중 가장 높은 점수 선택

Decoder
encoder hiddenstate랑 reordred keyword list로 생성.
1) Keyword Update Mechanism
keyword가 training process에서는 groundtruth랑 비교하니까 적당히 바뀌는데 inference는 안바뀔 수도 있으니까 M번 하면 바꾸자 
inference process
2)  Keyword Gating Function
GRU에 k_t 넣음
