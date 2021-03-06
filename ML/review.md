# ML review

- 머신러닝 딥러닝의 이론들은 100년전, 1958년 부터 나왔으나 시대에 맞지않아 흥하지 못했다.

- 문제해결, 컴퓨터 리소스의 발전등으로 인해 ML과 DL의 중흥기를 맞게 되었다.

  

## 인공지능이란?

현재 인공지능이란 단어 자체는 대중에게 친숙하게 다가가기 위한 마케팅적인 요소가 크다. ( 아직 일반적으로 사용할 수 있는 AGI의 개발도 제대로 이루어지지 않았다고 볼 수 있다.)

인공지능을 적용한다는 것은 어떤 **데이터**를 활용하여 **모델**을 만들고 어떠한 **기능**을 만드는 것이다.

  

인공지능이라는 말을 두 가지 측면으로 나눠본다면

- 어떤 모델을 만들 것인가.

- 어떤 기능을 만들 것인가.

  

범용적 모델을 만드는 것은 어렵기 때문에 고객의 needs를 통해 어떤 기능을 만들지 정하고 그에 맞는 모델을 선택한다.

  

### 모델이란?

  

몸에 맞는 사이즈를 체형에 대한 모델로 표현할 수 있다.

> 나에게 맞는 사이즈를 찾는 과정

>

>데이터에게 맞는 설명 방법을 찾아가는 과정

= 데이터를 가장 잘 표현하는 모델을 찾는 과정 (**Model fitting**)

  

$y = a * x + b$ 의 꼴을 모델이라 생각할 수 있다.

  

그렇다면 데이터를 가장 잘 설명하는 모델을 어떻게 찾을 것인가?

1. 가설 모델에 데이터를 넣는다.

2. 결과를 평가한다. (eval metrics)

3. 결과를 개선하기 위해 모델을 수정한다.

- 모델 내부의 Parameter 수정

- $y = a * x + b$ 에서 a, b가 모델의 parameter이고 모델의 모양을 결정짓는다.

- 파라미터를 찾는 과정이 ML학습이다.

- 모델의 종류 변경

- 어떤 모델을 쓸지 정하는 것은 컴퓨터가 하기 힘드므로 hyperparameter라 볼 수 있다.

- for문의 형태로 성능을 비교한다면 이것 또한 컴퓨터가 정할 수 있기는 하다.

- 그런 의미에서 나온것이 AutoML이다.

  

### 학습이란?

실제 정답과 예측 결과 사이의 오차를 줄여나가는 최적화 과정, parameter를 찾는 과정이다.

  

### 결국은 모두 다 정형 데이터

- structured data

- relational datadase

- spread sheet

- semi-structured data

- system logs

- sensor data

- HTML

- unstructerd data

- image/video

- sound

- document

결국 모든 데이터들을 일차적으로 정형데이터로 바꿔 모델에 전달한다.

  

#### 머신러닝을 적용한 인공지능을 활용한다는 것은

1. 각종 (정형/비정형)데이터를 활용하여

2. 데이터를 가장 잘 설명할 수 있는 함수의 후보를 정하고

3. 함수에 포함된 파라미터를 컴퓨터를 통해 구하고

4. 모델을 적용한 기능을 만든다.

  

## 학습(learning)이란?

**실제 정답과 예측 결과 사이의 오차를 줄여나가는 최적화 과정이다.**

그러나 이 과정에서 모델의 포용력 **Capacity**를 생각해볼 필요가 있다. 모델의 capacity는 해당 모델이 **복잡도를 얼마나 담을 수 있는가**를 나타낸다.  복잡한 현실을 모두 나타낼 수록 모델 또한 복잡해진다. 그러나 이러한 경우 Overffing의 문제가 발생할 수 있다. 

> Overffing이 되고 있는지 아닌지는 모델이 복잡해짐에 따라 변화하는 training error와 test error의 추이를 보고 판단할 수 있다. 혹은 그 둘의 차이와 실제 y의 구간의 상대적인 비교를 통해서도 생각해볼 수 있다.

Overffitng이 된다면 Train data에 대해서만 높은 성능을 보이고 general한 경우에 좋은 성능을 보일 수 없다. 따라서 Overffitng을 방지하기 위한 방법들을 사용해야한다.

Cross Validataion, K-FoldCV, Regulariztion term, Drop-out & Batch Normalization 등

Training Data를 많이 확보하는 것도 좋은 방법이다. (+ Data augmentation)

## 머신러닝 알고리즘

### Linear Regression
종속변수 y와 한개 이상의 독립변수 x 사이의 선형 상관관계를 모델링하는 회귀분석 기법

$y = \theta_0 +\theta_1x_1 + \dots + \theta_n x_n$ 의 선형 결합 꼴로 나타내진다.
- $\theta_0$는 bias를 나타내고 보정치라 할 수 있다.
- 나머지 $\theta_n$은 weight인 가중치로 해당 feature를 얼마나 중요하게 여겨야 하는지를 나타낸다.

**가장 적합한 $\theta$들을 찾는 것이 목표이다.**

#### Cost Function

그렇다면 가장 적합한  $\theta$ 를  찾는 것은 어떻게 할 수 있을까? 모델이 기존 데이터를 잘 설명하려면 모델이 예측한 값과 실제 값의 차이인 error가 작아야 한다. 이때 error의 정도를 정의하는 함수를 **Cost Function** 이라 할 수 있고 해당 함수가 작아지도록 하는 $\theta$를 찾는 것이 목표가 된다.

#### Gradient Descent

어떤 함수가 최소가 된다라는 것은 미분을 이용하여 구할 수 있을 것 같다. 이때 사용 되는 방법이 바로 **Gradient Descent Algorithm** 이다. 데이터의 차원이 낮다면 해석적으로 그 해를 구할 수 있지만 데이터의 차원이 높다면 그럴 수 없다. 따라서 수치적으로 그 위치를 찾아야 하며 그 위치를 찾는 방법은
	
- 임의의 위치($\theta$)를 지정한다.
- 해당 위치에서의 Cost function의 gradient, 경사도를 구한다.
- gradient의 음의 방향으로 움직여 해당 위치를 다음 $\theta$ 값으로 지정한다.
	> 이때 움직이는 정도, 보폭을 step size, learning rate라고 한다. 너무 작지도 않고 크지도 않은 값을 설정해줘야 한다. 너무 작을 경우 학습 속도가 느리고 너무 클 경우 최소 지점을 찾지 못하거나 발산할 수 있다.
- 이를 반복하여 cost function이 최소가 되는 지점의 $\theta$를 구한다.

그러나 이러한 방법으로 찾은 지점이 global minimun인지는 알 수 없다. 그래서 관성을 이용하는 방법론을 사용하기도 하며 실질적으로 local minima가 global minimun과 큰 차이가 없다는 의견들이 있다.

### Logistic Regression
이진 분류 문제를 해결하기 위한 모델로 시그모이드 함수 중 하나인 로지스틱 함수에 선형 회귀식을 입력으로 넣어준 것 이다.
>Logistic function: 베르누이 시행에서 1이 나올 확률 $\mu$ 과 0이 나올 확률 $1-\mu$ 의 비율인 odd ratio인 승산비 를 로그 변환한 것이 logit 함수이다.  
>$logit(odd ratio)=\log\left( \mu \over{1 - \mu}\right)$ 
>그리고 이 logit 함수의 역함수가 Logistic 함수이다. $-\infty부터+\infty$의 입력을 가지고 0부터 1사이의 출력을 가지는 함수로 변환하는 것이다.

로지스틱 함수로 input data가 특정 class에 속할 확률을 계산하고  cut off에 따라 class로 분류한다.

성능지표로는 Cross-entropy를 사용한다. (두 분포의 거리, 한 분포에서 한 데이터를 다른 분포로 옮기는 거리) 
: 해당 클래스 값과 해당 클래스가 나올 확률에 로그를 취한 값의 총 합

### Softmax Algorithm

모델의 output에 해당하는 logit(socore)를 각 클래스에 소속될 확률에 해당하는 값들의 벡터로 변환해준다. 다중 클래스 분류 문제 해결에 사용할 수 있다.

### Support Vector Machine

margin을 최대화 하는 descision boundary를 찾는 기법이다.

soft margin의 경우 슬랙변수 만큼 오차를 허용해주지만 너무 슬랙변수의 수가 많아지면 성능이 저하될 수 있다. 따라서 이를 방지하기 위해 변수 C를 추가해주며 C가 커질수록 슬랙변수 $\xi$의 수가 줄어들게 된다. 
soft margin의 경우 robust하다. 

#### kernel 함수
데이터가 선형적으로 분리되지 않는 경우에는 데이터가 놓여져있는 차원을 비선형 매핑으로 고차원의 공간으로 변환하는 것으로 해결할 수 있다. 원래의 저 차원에서는 분리할 수 없던 데이터들이 고차원에서는 쉽게 분리될 수 있는 경우가 있다.  이때의 kernel 함수는 hyper parameter이다.
고차원에서 경계면을 찾은 후 다시 원래 차원으로 돌아온다. 이때 경계면은 비선형적이다. 
