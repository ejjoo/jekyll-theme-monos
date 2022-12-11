---
title:  "Recurrent Neural Network"
excerpt: "Deep Learning Summary"

categories:
  - A.I

tags:
  - [deep learning, machine learning, AI, School Summary]

toc: true
toc_sticky: true
layout: post
use_math: true
 
date: 2022-11-21
last_modified_at: 2022-11-21
---

# Recurrent Neural Network

### 순차데이터

- 특징이 순서를 가지는 시간성 데이터
- 동적이며 보통 가변 길이의 데이터
- **순차데이터의 얘시**

    ![퍼셉트론의 공간 분할](/assets/img/%EC%88%9C%EC%B0%A8%EB%8D%B0%EC%9D%B4%ED%84%B0%20%EC%98%88%EC%8B%9C.png)
    
  - 온라인 숫자의 경우 글씨 시작점부터 해당하는 방향 벡터의 번호를 나열하여 표현
  - 심전도 신호의 경우 각각의 그래프에서 시간에 따른 값을 나열

- **순차데이터의 표현**
  
  - 벡터를 통한 표기<sup>벡터의 벡터</sup>
    - 일반적으로 순차 데이터를 나타낼때 사용하는 표현법
    - 특정 시간이나 순서에 해당하는 각각의 특징벡터들을 순서에 따라서 하나의 벡터에 넣어 표현
      > $x= \Bigg(\Big(\begin{matrix}0.3\\0.1\\0.2\end{matrix}\Big), \Big(\begin{matrix}0.5\\0.6\\0.4\end{matrix}\Big), ... \Bigg)^T$
    - 훈련집합 표현법
      > $
          x=(x^{(1)}, x^{(2)}, x^{(3)}, ...\ , x^{(T)})^T\\
          y=(y^{(1)}, y^{(2)}, y^{(3)}, ...\ , y^{(L)})^L
        $

  - 사전을 이용한 텍스트 순차 데이터의 표현
    - 단어가방
      - 각각의 데이터마다 등장하는 단어 각각의 빈도수를 세어 m차원의 벡터로 표현 m은 전체 단어의 갯수
      - 문장에서 특정 단어가 어딨는지 순차/시간 정보가 소실된다
    - 원핫코드
      - 단어를 원핫코드로 변경하여 표현하는 방법
      - 단어 하나를 나타내기 위해 전체 단어의 갯수 차원만큼의 공간이 필요하다
      - 단어 사이의 상관관계를 나타낼 수 없음
    - 단어 임베딩
    ![단어 임베딩](/assets/img/%EB%8B%A8%EC%96%B4%20%EC%9E%84%EB%B2%A0%EB%94%A9.png) ![단어 임베딩](/assets/img/%EB%8B%A8%EC%96%B4%20%EC%9E%84%EB%B2%A0%EB%94%A92.png)
      - 단어 사이에 상호작용을 분석, 새로운 공간으로 변환
      - 변환 과정은 학습이 말뭉치를 훈련집합으로 사용해 알아냄
      - 특정 단어들 사이에 같이 나타나는 빈도를 분석

- **순차데이터의 특성**
  - 순서가 바뀌면 의미가 크게 훼손된다
  - 샘플마다 길이나 데이터의 크기가 다르다
  - 데이터 내부에서 멀리 떨어져 있는 두 데이터 사이에 **장기 의존성**이 존재하기도 함

### 순환 신경망<sup>Recurrent Neural Network</sup>
- **표준신경망과 차이**
  - 표준신경망은 커널 이전의 데이터를 계산에 사용하지 못한다
  - 순환 신경망운 순차 데이터를 활용할 수 있도록 표준신경망의 구조를 변경
  - 즉 RNN은 표준신경망의 입력에 timestamp차원이 추가
  - ![RNNvsDMLP](/assets/img/%08DMLPvsRNN.png)

- **특징**
  - 가변길이의 입력을 처리
  - 장기 의존성 추적
  - 순서에 대한 정보 유지
  - 시퀸스 전체의 파라미터가 공유
  
- **RNN Unit의 구조**
  - RNN은 RNN유닛의 재귀적 연결로 구성

    ![RNN 구조](/assets/img/RNN%20Model.png)
  
  - 입력 벡터 : $x_t$
  - 출력 벡터 : $h_t=f(W_{xy}h_t)$
  - 은닉 상태 : $h_t=f(W_{hy}x-t + W_{hh}h_{t-1})$
  - rnn은 총 3개의 가중치 행렬을 가지며 각각 입력벡터와 곱하여 새로운 은닉상태를 만드는 $W_xh$, 이전 은닉상태와 곱하여 새로운 은닉상태에 더하여 의존성을 나타 낼 $W_{hh}$, 그리고 새로운 은닉상태와 곱하여 출력값을 만드는 $W_hy$가 존재

- **RNN의 유형**
  |유형|1대1|1대N|N대1|N대N|
  |:---|:---|:---|:---|:---|
  |설명|RNN 유닛이 재귀적으로 연결되지 않은 경우<br>Vanilla Neural Network이라고 한다.|하나의 입력에 많은 수의 출력이 존재<br>|일련의 입력을 받아 단일 출력을 생성|많은 수의 입력을 받아 많은 수의 출력을 생성|
  |구조|![일대일 구조 rnn](/assets/img/rnnExample1.png)|![일대다 구조 rnn](/assets/img/rnnExample2.png)|![다대일 구조 rnn](/assets/img/rnnExample3.png)|![다대다 구조 rnn](/assets/img/rnnExample4.png)|
  |사용 예|일반적인 신경망|이미지 캡션 생성<br>이미지 입력에 따른 이미지 설명 출력|문장의 감정 분석 신경망|기계 번역<br>단어 입력에 따라 다른 언어 단어들을 출력|

- **BPTT<sup>Backpropagation Through Time</sup>**
  - 순환 신경망에서는 계층을 통해 오류를 역전파하는 대신, 시간을 거슬러 올라가면서 오류를 역전파

  - ![BPTT](/assets/img/BPTT.png)

- **Gradiant 소멸/폭증**
  - RNN은 기존 신경망과 다르게 계속 같은 가중치를 곱하고, 입력에 따라 신경망의 깊이가 계속 깊어질 수 있어 Gradiant 소멸, 폭증 문제가 다른 신경망보다 더 심각함
  - **Gradiant 소실문제 해결책**
    - 활성함수로 Relu 사용
    - 가중치를 단위행렬, bias를 0으로 초기화
    - 기존 RNN 유닛 대신 LSTM, GRU등의 복잡한 순환유닛 사용
  - **Gradiant 폭증문제 해결책**
    - Gradiant의 최대값 상한선을 설정

### LSTM<sup>long short term memory</sup>

- 장기의존성을 잘 나타낼 수 있어 매우 긴 순차 데이터를 처리하는데 효과적인 신경망
- 기존의 rnn에 입력 게이트, 출력 게이트, 삭제 게이트 그리고 셀을 추가한 신경망
  
- **LSTM의 구조**
  - LSTM 유닛 구조
  ![LSTM](/assets/img/LSTM.png)
    > $f_t = \sigma(W_{xh\_f}x_t + W_{hh\_f}h_{t-1}+b_{h\_f})\\i_t = \sigma(W_{xh\_i}x_t + W_{hh\_i}h_{t-1}+b_{h\_i})\\o_t = \sigma(W_{xh\_o}x_t + W_{hh\_o}h_{t-1}+b_{h\_o})\\g_t = \tanh(W_{xh\_g}x_t + W_{hh\_g}h_{t-1}+b_{h\_g})\\c_t = f_t ⊙ c_{t-1} + i_t ⊙ g_t\\h_t = o_t ⊙ \tanh(c_t) $

  - **cell state** : $c_t$
    - 일종의 컨베이어 벨트 역할을 하여 timeStamp가 경과 되더라도 그레디언트를 유지해주는 변수
  - **Input Gate** : $i_t ⊙ g_t$
    - ‘현재 정보를 기억하기’ 위한 게이트
    - 이전 은닉상태 $h_{t-1}$와 현재 시점의 입력 $x_t$ 두개의 각각 가중합 결과를 요소별 곱을 취한 뒤 하이퍼볼릭 탄젠트, Sigmoid 활성함수를 통과시켜 요소곱을 한다.
    - sigmoid는 0~1, 하이퍼볼릭 탄젠트는 -1~1값을 갖기 때문에 각각 기억을 얼마나 할지(신호의 강도)와 어떤 기억을 할지(신호의 방향)의 곱 결과를 기억
  - **Forget Gate** : $f_t$
    - ‘과거 정보를 잊기’를 위한 게이트
    - 이전 은닉상태 $h_{t-1}$와 현재 시점의 입력 $x_t$ 두개의 각각 가중합 결과를 요소별 곱을 취한 뒤 Sigmoid연산을 하여 0~1사이의 값을 취하게 된다.
    - 0과 1 사이의 값을 가지게 되므로 해당 값을 cell state에 곱하면 cell state의 가중치 값이 낮아지면서 일부 기억이 삭제
  - **Update Gate** : $c_t$
    - '선택적으로 이전기억 보존'을 위한 게이트
    - 이전 cell state와 Forget Gate의 입력과 곱하여 일부 기억을 삭제 한 후 Input Gate의 출력을 더해 Input Gate에서 선택한 기억을 cell state에 저장
  - **Output gate** : $h_t$
    - 다음 시점의 은닉상태를 출력하는 게이트
    - 현재시점의 입력값과 이전 시점의 은닉 상태와 현재 시점의 cell state를 통해 다음 시점의 은닉상태를 계산
  - **Gradiant banishing**
    - Update Gate를 통해 gradian 정보가 중단되지 않음
논문 설명에서 모델 크기 조절 설명할수 있어야됨