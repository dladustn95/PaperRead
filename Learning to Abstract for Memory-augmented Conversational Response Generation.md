Learning to Abstract for Memory-augmented Conversational Response Generation

P(r|q, M) (M개의 그룹), q-r 퀄리티에 덜 민감
abstract query-response relationship hidden in those q-r pairs, save them to the memory and use those relationships for response generation.
autoencoder and clustering model joint

model : memory module, generative mdel
memory module : the training corpus into multiple groups of query-response pairs, extracts and memorizes the essential query-response correspondence information hidden in each group of pairs.
generative model : takes information stored in the memory module into consideration

memory slot : key-value (real-value vector, embedding) K개
input이 들어오면 가장 유사한 key 찾고 value값 뱉음 / 유사한거 찾기위해 dot product함
2가지 방법 soft read, hard read
soft read는 모든 memory에 대해 k*e를softmax한거에 value 곱해서 더함
hard read는 k*e가 가장큰거의 v 사용

