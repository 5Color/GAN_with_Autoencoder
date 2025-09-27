# GAN_with_Autoencoder

## Autoencoder
<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/7207ab2c-c75f-4eec-8073-6ca7786419bb" />

### 위 자료는 오토인코더를 이해하기 쉽게 시각화 한것이다.

오토인코더의 원리는 간단하다 우선 input값 x를 넣어서 encoder에 넣어서 데이터를 찌뿌(압축)시켜 잠재벡터로 만들고, 다시 잠배벡터를 확장시키는 디코더에 넣어서 재구성한다.
그럼 1이미지를 넣었다 치면 1과 비슷하게 재구성된다.
다음과 같은 결과가 나온다.
<img width="855" height="398" alt="image" src="https://github.com/user-attachments/assets/6c58c1bd-6923-475d-aaf2-53e411ea5b65" />

## GAN
<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/f6aa10b7-097b-43fd-93da-f0fde2810647" />
이제 GAN에 대해서 알아볼것이다 GAN은 (Generative Adversarial Network) 즉 생성적적대신경망의 약자로 
생성자 클래스와 판별자 클래스가 있는데 생성자 클래스는 random_noise로부터 FakeImage를 생성,
판별자 클래스는 사전에 진짜 이미지를 학습하여 생성자클래스가 반환하는 FakeImage로부터의 거짓/진실 여부를 판단한다.
물론 판별자는 랜덤으로 실제 이미지와 FakeImage가 들어오는데 그걸 올바르게 판별하는게 판별자의 역할이며
생성자는 판별자를 더 잘 속여야 하는 그런 알고리즘이다.

간단하게 요약하면 생성자와 판별자가 속고 속이는 그런 티키타카 게임이다!

## GAN구현코드 설명
### import
torch, torchvision, matplotlib 

### 데이터 로더

### 생성자 클래스 구현
노이즈로부터 이미지를 생성하는것이기에 input_size=100으로 설정하고, 
마지막 출력층은n.Linear(1024, 28*28), # 784로 해준다
output_layer의 활성화함수는 하이퍼볼릭탄젠트 함수를 사용한다. => Tanh()

HiddenLayer는 Linear모델과 활성화함수는 LeakyRelu를 쓰고,
layer가 골구로 학습될수 있게 0.n =n*10%의 확률로 h가 비활성화돼는
Dropout()함수를 쓸것이다
순전파 함수 정의해준다음

### 판별자 클래스 구현
Dropout()함수 기능도 똑같이 추가해 학습을 더욱더 개선시킨다.
실제 이미지를 받아 잠재벡터로 만들어 축소시킨다음 sigmoid함수를 이용해 픽셀값을 0~1값으로 반환해준다.
순전파 정의,

<img width="698" height="307" alt="image" src="https://github.com/user-attachments/assets/6a1ac06c-17e3-44c5-979f-1f0577e0df5e" />

#### 생성자, 판별자 객체 생성하고, 손실함수와 옵티마이저를 설정하는 모습

옵티마이저는 Adam보다 NAdam이 무난하고 좋았다.

### 학습
epoch은 70으로 과적합 되지않고 딱 좋았다.

#### loss값 설명
##### d_loss: 판별자(discriminator)의 손실 값
##### g_loss:생성자(generator)의 손실 값
##### D(x): 판별자가 실제 이미지를 진짜라고 판단한 평균 확률. 이 값이 1에 가깝다는 것은 판별자가 실제 이미지를 잘 인식하고 있다는 것을 의미
##### D(G(z)):판별자가 생성자가 만든 가짜 이미지를 진짜라고 판단한 평균 확률

## 학습 결과
<img width="500" height="700" alt="image" src="https://github.com/user-attachments/assets/123d2a16-87d3-41a0-99b4-184b040712be" />

와우  꽤 잘나왔네? loss값이 서로 수렴된다.

그리고 생성된 이미지도 꽤 괜찮았다
<img width="1135" height="997" alt="image" src="https://github.com/user-attachments/assets/c471aa3c-49f0-4fe9-bdf3-7550d4ac2678" />

## 느낀점
GAN은 참 신기하고 독특한 신경망 알고리즘이라고 생각한다.
그리고 재밌다. 하지만 loss값을 서로 완벽히 수렴시키는게 어려웠다.

