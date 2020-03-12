Learning to Abstract for Memory-augmented Conversational Response Generation
https://www.aclweb.org/anthology/P19-1371.pdf

P(r|q, M) (M개의 그룹), q-r 퀄리티에 덜 민감
abstract query-response relationship hidden in those q-r pairs, save them to the memory and use those relationships for response generation.
autoencoder and clustering model joint

model : memory module, generative mdel
memory module : the training corpus into multiple groups of query-response pairs, extracts and memorizes the essential query-response correspondence information hidden in each group of pairs.
generative model : takes information stored in the memory module into consideration

memory slot : key-value (real-value vector, embedding) K개
input이 들어오면 가장 유사한 key 찾고 value값 뱉음 / 유사한거 찾기위해 dot product함
2가지 방법 soft read, hard read
soft read는 모든 memory에 대해 k \dot e를softmax한거에 value 곱해서 더함
hard read는 k \dot e가 가장큰거의 v 사용

k_i는 비슷한 q를 가진다. 
v_i는 q가 비슷한 e_r의 일반적 특징이다. 
k_i, v_i는 i번째 클러스터의 q,r의 추상화이다. 

M-Seq2Seq은 response generation으로 사용
conditional autoencoder response representation for memory writing

M-seq2seq : q -> e_q = Encoder(q), e_rm = Memory(e_q) -> r^' = Decoder(MLP(e_q, e_rm))
CAE : r -> e_r = Encoder(r) -> r^ = Decoder(MLP(e_q,e_r))
CAE는 e_q에 의존하여 e_r을 만들고  두 브랜치가 동시에 작동하게 하려고 e_q를 feed함
e_rm은 e_r의 rough estimation으로 볼 수 있다. 

loss = logP(r|q,e_rm) + ㅅlogP(r|q,r)
prediction loss + reconstruction loss
1st term : memory-augmented generative model에서 파생 maxP(r|q,e_rm)
2nd term : improve the memory module = improve e_rm

Joint training 함
하나 epoch 하나 update
testing에서는 M-seq2seq만 사용, 
