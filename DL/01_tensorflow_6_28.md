# TensorFlow

tensorflow is an open source library for numerical computation and large-scale machine learning 

딥러닝을 위한 다른 라이브러리들이 많이 존재하나 현재 구글의 TensorFlow와 페이스북의 Pytorch를 주로 쓰는 추세이다.
TensorFlow는 현재 javascript로 구동할 수 있다는 것, 모바일 환경을 위해 가볍게 만든 버전도 성능이 올라왔다는 점에서 웹환경, 모바일환경에서 구동할 수 있다는 장점이 있다. 또한 TensorBoard를 이용하여 시각화와 로그를 확인할 수 있다는 장점도 있다.

tensorflow는 기본적으로

1. Building a TensorFlow Graph
2. Executing the TensorFlow Graph

의 두 단계를 통해 계산을 수행한다. 여기서 Graph는 자료구조에서 다루던 모양의 그래프를 의미하고 1번에서는 Tensor들의 연산 관계를 그래프로 정의하고 2번에서는 관계가 정의된 연산들을 실제로 수행한다. 이때 계산은 C계열 언어로 수행되며 그 속도가 빠르다.

tensor들의 연산을 함수로 정의하고나서 연산을 수행할 때는 Session 클래스로 수행한다. ( tf.Session() ) Session은 함수가 정의되어 있더라도 Session을 통해 실행되는 함수만 실행되게 하기 때문에 효율적이다. 그런데 Session은 마치 스레드처럼 동작하기 때문에 모든 일을 마치고나서는 종료시켜줘야한다.


### Linear Regression
일련의 과정은 지금까지 배워온 과정과 똑같다.
1. 모델을 정의하고 (parameter 지정)
2. loss 함수를 지정하고
3. optimizer 함수를 이용하여 최적의 parameter를 구한다.

> 번외로 parameter initialization에 대해서 알아보면서 실제로 어떤 결과가 나오는지 확인해봤다. 실제로 parameter의 std가 작다면 layer를 거듭할수록 W의 영향이 작아져서 W가 0에 수렴하고 W 자체가 작은 값으로 gradient 또한 0이된다. std가 큰 경우 sigmoid나 tanh를 사용한다면 W가 수렴하지 않는다. 이로인해 또한 gradient를 구할 수 없다. 그러므로 이를 해결하기 위해 적절한 조치가 필요한데 그것이 바로 Xavier initialization과 He initialization이다.
> ~~이미지는 추후에 첨부~~
