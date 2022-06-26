# 딥러닝 최적화

> playground.tensorflow 에서 layer들과 기본 연산과 학습에 필요한 hyper parameter 들을 바꿔가면서 딥러닝이 어떻게 동작하는지 확인해 볼 수 있다.
> > layer가 많다고 해서, perceptron이 많다고 해서 학습이 잘 되는 것이 아니다. 바로 이러한 점이 바닥부터 모델을 만들 때 어려운 점이다. 어떻게 layer와 node를 정해야하는지 알 수 없기 때문. 이러한 어려운 점을 넘기기 위해 전이학습이 방법이 될 수 있다.

train, test error plot의 경우 계단 모양을 나타내는 경우가 많다. 즉 error가 줄어들지 않다가 어느 순간을 넘기면 확 낮아지는 것. 그렇기 때문에 error가 줄어들지 않는다고 학습을 섣불리 중단해서는 안된다. 이와 관련하여 early stopping을 사용하지 않는 것이 좋다. 시간이 허용하는 한 학습을 진행하고 plot을 확인 후 모델을 정해야한다.

## Gradient Descent를 활용한 학습과정

1. 모든 파라미터 $\theta$ 를 초기화하고 (초기 위치를 정한다.)
2. 손실함수 상의 가장 낮은 곳을 향해 나아가며 
3. 선택한 gradient descent algorithm 을 적용해 $\theta$를 업데이트한다.

### Weight Intialization

gradient descent의 첫 단계는 바로 초기 $\theta$ 값을 지정해 주는 것이다. 출발 위치에 따라서 도착 위치가 달라질 수 있으므로 더 나은 초기화 방법을 모색해야한다. 

![enter image description here](https://tensorflowkorea.files.wordpress.com/2016/10/gradient-descent.png)

실제로 어떤 초기 $\theta$ 를 설정하느냐에 따라서 error를 찾는 과정이 수렴하지 못할 수도 있고 local minima들로 수렴할 수 있다. 이전 layer의 node들에서 값을 받는다는 점을 생각해보면 이전 layer의 node개수가 linear combination 결과에 큰 영향을 미칠 수 있다고 생각해볼 수 있다. 이러한 문제를 생각한 initialization들을 사용하면 보다 나은 결과를 얻을 수 있다.

#### Xavier Initialization
활성화 함수로 Sigmoid 함수나 tanh 함수를 사용할 때 적용한다. 다수의 딥러닝 라이브러리들에 기본으로 적용되어 있다. 이 방법은 표준편차가 $\sqrt{1\over{n}}$ 인 정규분포를 따르도록 가중치를 초기화 하는 것이다. (node 수에 반비례하게 가중치를 적용한다고 볼 수 있다.)

#### He Initializtion
활성화 함수가 ReLU일 때 적용한다. 표준편차가 $\sqrt{2\over{n}}$ 인 정규분포를 따르도록 가중치를 초기화 한다.

### Weight Regularization
training data만 고려된 Cost function을 최소화 하는 것은 Overfitting의 위험성이 있다. 모델이 복잡해질수록 모델은 주어진 데이터에 대해서만 잘맞히도록 학습하게 되고 general한 성능을 뽑지 못하게 된다.  그런데 이때 $\theta$ 의 수가 늘어날 수록 $\left\vert \theta \right\vert$가 커지는 경향을 이용한다.

기존의 cost function에 $\theta$의 크기들을 더해줘서 cost function에 제한을 주는 역할을 하게 한다. 가중치의 감퇴, 감소를 일으킬 수 있다.

#### L1 Regularization
가중치의 절대값의 합에 비례하여 가중치에 페널티를 준다. 관련성이 없거나 매우 낮은 특성의 가중치를 정확히 0으로 유도하여 모델에서 해당 특성을 배제하는데 도움이 된다.(feature selection의 효과)

#### L2 Regularization 
가중치의 제곱의 합에 비례하여 가중치에 페널티를 준다.

이때, regularization rate(정규화율) $\lambda$ 에 따라서 모델이 underfitting, overfitting이 될 수 있으므로 실제로 극단적인 값들을 넣어보며 그 결과를 확인하고 $\lambda$ 값을 정하면 된다.

### Advanced gradient descent algorithm
 다음 지점을 정하는데 있어 얼만큼의 데이터를 사용할지에 따라 방법이 나뉠 수 있다. 사용하는 데이터의 양에 따라 연산량이 달라지고 소요되는 시간에서 큰 차이가 나기 때문에 고려해야할 요소이다.
#### Full batch gradient descent
모든 Training Data에 대해서 gradient descent를 적용하는 방법이다. 이 방법의 경우 모든 data를 사용하기 때문에 연산량이 많고 학습시간이 오래걸린다.

#### Stochastic Gradient Descent(확률적 경사하강법)
하나의 Training Data(batch size = 1) 마다 gradient descent를 적용하는 방법이다. 하나의 training data에 대해서 적용하는 것이기 때문에 신경망의 성능이 들쑥날쑥 변한다. 즉 loss 값이 안정적으로 줄어들지 않는다. 

#### Mini Batch Stochastic Gradient Descent
앞의 두 가지 방법의 중간 지점으로 볼 수 있다. Training Data에서 일정한 크기의 데이터(= Batch Size)를 선택해서 gradient descent를 적용한다. GPU 기반의 효율적인 병렬연산을 할 수 있다.

> 학습은 epoch에 대한 반복문과 batch에 대한 반복문의 이중 반복문으로 진행된다고 볼 수 있다. epoch는 전체 학습 데이터를 한 번씩 모두 학습시킨 횟수이고 batch size는 우리 가정한 data size이며 batch size만큼의 데이터에 대해서 gradient descent를 적용한 횟수를 iteration으로 본다.

![enter image description here](https://image.slidesharecdn.com/random-170910154045/85/-49-320.jpg?cb=1505089848)
데이터의 사이즈 말고도 gradient descent의 방향에 대해서도 조절하는 방법도 있다. step에 momentum을 적용하여 관성을 적용한다면 얕은 local minima의 경우 빠져나올 수 있게 된다. 

