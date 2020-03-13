Mixture Content Selection for Diverse Sequence Generation
https://arxiv.org/pdf/1909.01953.pdf

method to explicitly separate diversification from generation using a general plug-and-play module (called SELECTOR) that wraps around and guides an existing encoder-decoder model.
*focus* : latent variable, distribution을 *select*, *generate*로 나눔

diversification stage : smple different binary mask on the source sequence for diverese conent selection / map the source to multiple sequences, each mapping modeled by focusing on different tokens in the source

model a conditional multimodal distribution for p(y|x) 이거할때 보통의 encoder-decoder는 모든 valid mapping에 대한log probability의 예상값을 줄여서 부적절하다

*focus*로 multimodal distribution을 factorize함  
*select* stage : sample several meaningful *focus*, source sequence의 어느 부분이 중요하게 간주되는지 나타냄  
*generate* stage : 각 sampled focus는 생성과정이 fucused content에 conditioned되도록 함  
*focus*는 input token의 각 sequence에 대응되는 sequence of binary variable로 모델링, m={m_1...m_s} \in {0,1}^S
