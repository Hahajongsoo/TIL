# DL Optimization

딥러닝은 feature selection, feature extraction에 힘을 쏟지 않아도 된다는 장점이 있지만, overfitting에 항상 조심해야한다는 문제점이 있다. 그렇기 때문에 Optimization을 진행하더라도 Overfitting이 되었는지는 항상 의심해야한다. 

## Avoding Overfitting

### DropOut

#### Co-adaptation

dropout은 co-adaptation을 극복하려는 목표에서 나오게 되었다. 자연에서와 마찬가지로 node들은 잘 예측하는 node에 가중치를 더 부여하고 그렇지 못한 node에 가중치를 덜 부여한다. 그런데 node 들은 서로 영향을 받기 때문에 한 node의 weight가 커지게 되면 다른 node는 해당 node에 의존적이게 될 수 있다. 자연에서 유성생식의 의미는 바로 이러한 곳에 있는데, 유성생식을 함으로써 새로운 유전자가 발전할 수 있는 여지를 줄이는 co-adaptation을 줄일 수 있기 때문이다. 이와 유사한 역할을 인공신경망에서 수행하는 것이 바로 **Dropout** 이다.

#### Dropout

dropout은 학습을 진행할 때, **batch마다** layer별로 **일정 비율만큼의 perceptron을 꺼뜨리는 방식**의 간단한 방식으로 일반화 성능을 올려줄 수 있는 효율적인 방법이다.  그러나 Test 단계에서는 전체 perceptron이 켜져있는 상태로 진행해야한다.

> 물론 우리는 hidden layer에서 모델이 **어떤** 특성을 **왜** 바라보고 있는지는 알 수 없다. 하지만 이전에 설명한 것 처럼 layer의 동작을 low level에서는 국지적인 특성을, high level에선 좀 더 큰 구조를 추상화하고 있다고 생각, 상상할 수 있기 때문에 Dropout의 원리를 설명하고 이해할 수 있을 것이다.

<center><img src="https://www.researchgate.net/publication/340700034/figure/fig3/AS:881306405724163@1587131229956/Dropout-Strategy-a-A-standard-neural-network-b-Applying-dropout-to-the-neural.ppm" width='60%' > </center>


dropout의 장점은 바로 model ensemble과 같은 효과를 낸다는 점에 있다. batch 마다 켜져있는 perceptron이 다르기 때문에 그 때마다 다른 특성을 바라보게 되고 그 결과, 각각 다른 model이 학습하는 것 처럼 볼 수 있다. 

dropout은 overfitting을 유발하는 co-adaptation을 줄일 수 있다는 것과 제한된 데이터와 model에서 효율적으로 voting을 수행할 수 있다는 점에 있다.

> https://www.cs.toronto.edu/~hinton/absps/JMLRdropout.pdf

## Batch Normalization

### Gradient Vanishing 문제

인공신경망이 학습하는 것은 결국 최적의 loss function을 만드는 파라미터를 구하기 위해 미분값인 gradient의 계산을 반복하는 것에 있다. 그런데 sigmoid 함수를 생각해보면 입력 값들의 분포가 sigmoid 함수의 값이 0과 1이 되는 부분에 몰려있다면, gradient가 0으로 나오게 되어 이후 파라미터를 update를 할 수 없는 gradient vanishing 현상이 일어나게 된다. 물론 이를 해결하기 위해 ReLU 함수를 이용할 수 있지만 학습 과정 자체를 전체적으로 안정화하여 학습 속도를 향상시킬 수 있는 근본적인 방법인 Batch Normaliztion을 활용하는 것이 더 좋다.

### Covariate Shift

Covariate Shift는 대표적으로 train set과 test set의 분포가 달라서 발생하는 문제이다. train set과 test set의 분포가 다르다면 train set에 대해서 학습한 모델은 test를 잘 예측하지 못할 것이다.

#### Internal Covariate Shift

ICS는 바로 이 Covariate Shift가 layer 마다 일어나는 것을 의미한다. 이 현상은 Layer가 깊어질수록 더 심해지기 때문에 처음과 완전히 달라진 Input data의 분포 때문에 제대로 학습하지 못하게 된다.