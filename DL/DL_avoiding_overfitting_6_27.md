# DL Optimization

딥러닝은 feature selection, feature extraction에 힘을 쏟지 않아도 된다는 장점이 있지만, overfitting에 항상 조심해야한다는 문제점이 있다. 그렇기 때문에 Optimization을 진행하더라도 Overfitting이 되었는지는 항상 의심해야한다. 

## Avoding Overfitting

### DropOut

#### Co-adaptation

dropout은 co-adaptation을 극복하려는 목표에서 나오게 되었다. 자연에서와 마찬가지로 node들은 잘 예측하는 node에 가중치를 더 부여하고 그렇지 못한 node에 가중치를 덜 부여한다. 그런데 node 들은 서로 영향을 받기 때문에 한 node의 weight가 커지게 되면 다른 node는 해당 node에 의존적이게 될 수 있다. 자연에서 유성생식의 의미는 바로 이러한 곳에 있는데, 유성생식을 함으로써 새로운 유전자가 발전할 수 있는 여지를 줄이는 co-adaptation을 줄일 수 있기 때문이다. 이와 유사한 역할을 인공신경망에서 수행하는 것이 바로 **Dropout** 이다.

#### Dropout

dropout은 학습을 진행할 때, **batch마다** layer별로 **일정 비율만큼의 perceptron을 꺼뜨리는 방식**의 간단한 방식으로 일반화 성능을 올려줄 수 있는 효율적인 방법이다.  그러나 Test 단계에서는 전체 perceptron이 켜져있는 상태로 진행해야한다.

> 물론 우리는 hidden layer에서 모델이 **어떤** 특성을 **왜** 바라보고 있는지는 알 수 없다. 하지만 이전에 설명한 것 처럼 layer의 동작을 low level에서는 국지적인 특성을, high level에선 좀 더 큰 구조를 추상화하고 있다고 생각, 상상할 수 있기 때문에 Dropout의 원리를 설명하고 이해할 수 있을 것이다.

<center>
  <img src="https://miro.medium.com/max/875/1*EinUlWw1n8vbcLyT0zx4gw.png" width='60%'>
</center>


dropout의 장점은 바로 model ensemble과 같은 효과를 낸다는 점에 있다. batch 마다 켜져있는 perceptron이 다르기 때문에 그 때마다 다른 특성을 바라보게 되고 그 결과, 각각 다른 model이 학습하는 것 처럼 볼 수 있다. 

dropout은 overfitting을 유발하는 co-adaptation을 줄일 수 있다는 것과 제한된 데이터와 model에서 효율적으로 voting을 수행할 수 있다는 점에 있다.

>참고 :  https://www.cs.toronto.edu/~hinton/absps/JMLRdropout.pdf

## Batch Normalization

### Gradient Vanishing 문제

인공신경망이 학습하는 것은 결국 최적의 loss function을 만드는 파라미터를 구하기 위해 미분값인 gradient의 계산을 반복하는 것에 있다. 그런데 sigmoid 함수를 생각해보면 입력 값들의 분포가 sigmoid 함수의 값이 0과 1이 되는 부분에 몰려있다면, gradient가 0으로 나오게 되어 이후 파라미터를 update를 할 수 없는 gradient vanishing 현상이 일어나게 된다. 물론 이를 해결하기 위해 ReLU 함수를 이용할 수 있지만 학습 과정 자체를 전체적으로 안정화하여 학습 속도를 향상시킬 수 있는 근본적인 방법인 Batch Normaliztion을 활용하는 것이 더 좋다.

### Covariate Shift

Covariate Shift는 대표적으로 train set과 test set의 분포가 달라서 발생하는 문제이다. train set과 test set의 분포가 다르다면 train set에 대해서 학습한 모델은 test를 잘 예측하지 못할 것이다.

### Normalization
그렇다면 서로 다른 분포를 갖는 데이터의 분포를 같게 만들어주는 방법에 대해서 생각해 볼 수 있을 것이다. 

Standardization
:  데이터가 평균 0, 표준편차 1의 분포를 갖게 하는 정규화이다. shifting 하고 scaling한다.


Whitening
: 데이터가 평균 0, 공분산 행렬이 단위행렬을 갖게 하는 정규화이다. PCA로 decorrelated하게 만들고 scaling 한다. 

<center><img src=https://ars.els-cdn.com/content/image/1-s2.0-S156625352100230X-gr14.jpg></center>

### Internal Covariate Shift

그러나 Covariate Shift가 layer 마다 일어날 수 있다.. 이 현상은 Layer가 깊어질수록 더 심해지기 때문에 처음과 완전히 달라진 Input data의 분포 때문에 제대로 학습하지 못하게 된다.

### Batch Normalization

이러한 ICS를 해결하기 위한 방법이 바로  Batch Normalization이고 layer마다 normalization을 적용하여 이를 해결한다.


<center>
  <img src="https://miro.medium.com/max/1153/1*xQhPvRh08oKFC63swgWr_w.png" width='40%'>
</center>

1. mini-batch의 평균을 구하고
2. mini-batch의 분산을 구하고
3. 구한 값으로 normalize를 진행하고($\epsilon$ 의 경우 분산이 0이 가깝게 되어 0으로 나누는 것을 방지하기 위함이다.)
4. 3번의 결과에 $\gamma$를 곱하고 $\beta$를 더한다. (scale, shift)

결과적으로 batch normalization layer를 놓으면 모든 노드에 대해서 training 해야하는 parameter $\gamma,\beta$ 가 생기게 된다.

> 원래 $g(W\cdot x + b)$ 의 꼴로 학습하게 되는데 batch normalization에서 bias를 학습하므로 실제로는 $b$를 학습하지 않아도 된다. $g(batchnormal(W\cdot x ))$ 의 꼴

#### Test, Inference

test 시에는 앞서 구한 sample 평균과 분산의 평균($E[\mu_B], E[\sigma_B]$)을 이용하여 test를 한다.  batch의 평균과 분산자리에 대신 구한 값을 넣고  $\gamma,\beta$는 학습한 값을 넣고 test를 진행한다.
이때 test시의 분산에는 $m\over{1+m}$을 곱해줘야 한다. 원래 분산을 구할 때 불편 추청량으로 구하지 않았으므로 이를 보정해줘야 한다.
$E$는 moving average를 이용하여 구한다. 이전 초기 mini-batch에 대한 값에 가중치를 덜 부여하는 exponetial moving average도 있다.

batch-normalizer는 batch 의 분포를 다른 위치와 다른 스케일로 바꾸는 것으로 생각할 수 있다. 이때 앞서 말한 gradient vanishing 문제를 해결할 수 있다. 그렇다면 $\gamma,\beta$는 필요하지 않을 것으로 생각할 수 있지만, 0에 가까운 지점에서 sigmoid 함수는 선형적이기 때문에 $\gamma,\beta$ 를 이용하여 shift, scaling해줄 필요가 있다.

#### learning rate
초기 $\gamma=1, \beta=0$으로 주어지기 때문에, 어떤 데이터든 뿌려지는 위치와 정도가 비슷하다. 그러므로 update를 해야하는 정도가 다 비슷하기 때문에 learning rate를 작게 주지않고 키워도 된다는 것이 저자의 주장이다. 그리고 실제로 같은 성능 대비 학습속도가 상당히 빠른 것을 실험을 통해 알아냈다. 또한 특정 parameter의 영향력이 크다면 valley가 생기게 되고 작은 learning rate를 적용해야 수렴할 수 있다.

#### batch-normalization의 장점
1. 학습속도의 향상(learning rate를 크게 잡아도 된다.)
2. 학습의 안정성(gradient vanishing을 해결한다.)
3. 정규화(Overfitting을 억제한다.)

