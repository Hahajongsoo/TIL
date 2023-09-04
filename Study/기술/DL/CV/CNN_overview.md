# CNN 개론

## 이미지 파일
### gray scale
이미지 파일은 0~255의 정수(int8)로 이루어진 pixel 들로 구성된 matrix이다.  0으로 갈수록 어두워진다.
 ### RGB
 또는 이미지 파일이 RGB등의 채널을 가져 색을 가질 수도 있다. 라이브러리에 따라서 RGB를 읽는 방식이 다르다. opencv imread의 경우 BGR로 읽는다.
## 이미지 필터
이미지 파일에 filter 연산을 적용하면 필터의 종류에 따라 연산 결과인 이미지가 달라지게 된다. sharpen filter의 경우 edge가 선명해지고 blur filter의 경우 이미지가 뭉게진다.  필터연산은 정방행렬의 window 혹은 kernel에 이미지를 pixel-wise로 곱한 결과를 합하여 새로운 이미지의 pixel로 둔다. 슬라이딩하며 이미지의 모든 부분에 연산하도록 한다.
## Convolution 연산
이미지를 분류하기위해 이미지를 1차원의 벡터로 만들어 pixel 정보를 그대로 사용하여 모델을 학습시키려는 시도들이 있었다. 하지만 이러한 경우 이미지에 있는 지역적, 공간적 정보들을 고려하지 않게 된다. 또한 모든 pixel들을 그대로 고려하기 때문에 overfitting 될 수도 있다. 이미지의 지역적, 공간적 특징들을 고려하기 위해서 사용된 NN이 CNN이다. 앞서 말한 필터 연산을 이미지 전체에 슬라이딩 하며 수행하는 것이 convolution과 유사하여 CNN이라 이름 붙여진 듯 하다.
기본적으로 convolution을 수행하면 결과 이미지가 작아지게 되는데, 이는 압죽된 지역적, 공간적 정보로 볼 수 있다. 그렇다면 convolution을 수행할 수록 원래 이미지에서 점점 더 큰 부분을 의미한다거나, convolution을 수행할 수록 간단한 단위의 특징에서 점점 더 복잡한 단위의 특징으로 계층적인 학습을 한다고 볼 수 있다. 
### padding 
convolution을 수행할 수록 output size가 점점 작아지게 된다. 이런 경우 원하는 만큼 convolution을 수행하지 못하게 된다. input과 동일한 사이즈를 유지하기 위해서 원래 이미지 주변을 임의의 값으로 채워 둘러 싼다.(0, 가장자리와 유사한 값 등등 여러 방법이 있다.) 이렇게 하면 colvolution을 수행해도 이미지 크기가 줄어들지 않는다.
### stride
슬라이딩을 할 때 몇 칸씩 움직이느냐에 따라 output size가 달라진다. 움직이는 정도를 stride라고 하는데 output size는 input size를 stride로 나눈 값이 된다.

## CNN
CNN(Convolution Neural Network)는 앞서 말한 필터가 고정되지 않고 각 픽셀 값들에 대신 weight가 들어간 형태의 NN이다. 따라서 CNN은 이 필터들을 학습한다. 하나의 layer에 여러개의 필터가 있을 수 있으며 필터의 channel은  input의 channel과 동일하다. 그리고 output의 channel은 필터의 개수와 같다. 
### Max Pooling
pooling은 이미지나 feature map의 특정 부분에 대한 값을 하나의 대표 값으로 바꿔가면서 output의 size를 줄이는 layer로 대표 값을 정하는 방법에는 최대값, 평균 등 여러 방법이 있으나 보통 최대 값을 사용하는 max pooling을 사용한다. pooling을 사용하면 parameter 개수가 감소하므로 계산 비용이 감소하는 효과를 볼 수 있다. 그리고 직관적으로 우리는 이미지에서 해당 특성이 정확히 어디에 위치하는지 보다는 해당  특성이 존재하는지에 더 반응하므로, 해당 특성이 얼마나 크게 있는지를 나타내는 max pooling이 이미지 분류에 의미있게 동작하는 것을 납득할 수 있다.
### CNN의 구조
CNN은 filter를 곱하는 convolution 연산과 pooling이 반복되는 feature extration 부분이 있고 그 끝에 마지막 결과를 flatten이나 gloval average pooling으로 1차원으로 펼쳐 fully connected layer로 들어가 마지막에 정해진 특징들 모두를 고려하는 연산을 수행하고 그것으로 classification을 진행하게 된다.

## CV Task - Image Classification
CNN은 크게 앞서 말한 feature extractor인 back bone과 나머지 부분으로 이루어진다고 볼 수 있다. 마지막 부분에 어떤 역할을 수행하게 하느냐에 따라 task가 달라진다.
마지막 부분에 모든 input을 고려하게 하는 classifier를 붙이게 되면 해당 모델을 classification을 수행하게 된다.
### dataset
tensorflow에는 이미지 데이터를 불러오기 쉽게 해주는 함수들이 존재한다. 디렉토리를 label별로 두고 label명을 명시하면 그대로 이미지와 label을 가져오게 할 수도 있고, dataframe을 이용하여 dataframe에 명시된 파일 경로와 label과 추가 정보를 가져오게 할 수도 있다. pytorch의 경우에도 dataset을 customize하여 원하는 형태의 dataset을 만들어 tensorflow에서 유용한 함수의 형태로 dataset을 만들어줄 수 있다.
## CV Task - Object Detection
object detection의 경우 세가지를 동시에 해결해야한다.

- single object detection
	- localization : 어떤 location에 object가 존재하는지
	- classification : 해당 object의 class가 무엇인지
- multiple object detection
	- single object detection 에 여러 object가 있는 형태(single class)나 여러 object에 여러 class가 존재하여 이를 분류하는 형태의 문제이다.
### dataset
dataset은 이미지와 해당 object의 위치와 class에 대한 label data가 txt파일로 주어진다.

## CV Task - Object Tracking
object detection에 (localization, classification, multi object)에 Object ID가 추가된 형태의 task이다.  object id가 포함되어야 다음 프레임에서도 해당 object가 어디에 있는지 추적할 수 있다. 
### labelling tool
동영상은 사진의 연속인데 그 수 많은 이미지에 전부 하나하나 labelling을 할 수는 없다. interpolation을 이용하여 데이터의 분포에서 가장 자연스러운 분포를 찾아 예측하는 semi-auto labeling을 이용한다.
## CV Task - Segmentation

- semantic segmentation: 모든 pixel에 대해서 어떠한 class에 대한 여부 확률이 높은지에 대한 output을 출력하는 task
- instance segmentation: object의 위치 localization하고 어떤 pixel이 어떤 class에 속하는지에 대한 output을 출력하는 task

### dataset
dataset은 이미지와 class와 segemtation 영역의 pixel 값에 대한 json 파일로 주어진다.

## Image Preprocessing
이미지는 다양한 크기와 색상, 값들로 다양한 분포를 가지기 때문에 이를 일정한 분포로 바꿔주어야할 필요가 있다. 그렇기 때문에 Resize, Color, Normalization에 대한 처리를 해준다. 

## Data Imbalance
data의 class가 imbalance한 경우 많은 class만 잘 맞히는 방향으로 학습하게된다. 혹은 전부 많은 class라고 예측하더라도 accuracy는 올라가기 때문에 적은 양의 class를 틀리는 것을 신경쓰지 않게 된다. 
이런 경우 oversampling이나 augmentation을 적용할 수 있다.
augmentation을 적용할 때 class의 비율을 맞춰주기 위해서 한 class에만 적용하면 특정 augmentation에 대한 정보가 data에 있으면 높은 확률로 해당 class라고 예측하게 돼버린다. class의 비율을 균일하게 할 수는 없지만 data의 다양성이 증가하기 때문에 더 나은 성능을 기대할 수 있게된다.
### focal loss 
data가 적은 class에 대한 loss를 크게 적용하여 해당 class에 대해서 틀렸을 때 학습이 더 이루어지게 하는 기법이다. 가중치를 부여해 적은 비중의 class에 민감하게 한다.