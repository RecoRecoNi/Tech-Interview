# Normalization
## 💡 Normalization의 동작은?
`Nomalization`은 모든 데이터 포인트가 동일한 정도의 스케일(중요도)로 반영되도록 해주는 작업입니다.

**1. Min-Max Normalization (최소-최대 정규화)**

$$
\frac {x - x_{min}} {x_{max} - x_{min}}
$$

- 모든 feature에 대해 각각의 최소값 0, 최대값 1로, 그리고 다른 값들은 0과 1 사이의 값으로 변환합니다.
- **이상치(outlier)에 너무 많은 영향을 받습니다**
</br>

**2. Standardization(표준화) - Z-Score Normalization (Z-점수 정규화)**

$$
\frac {x - \mu} {\sigma} (\mu : 평균, \sigma : 표준편차)
$$

- Z-점수 정규화는 이상치(outlier) 문제를 피하는 데이터 정규화 전략입니다.
- 하지만 정확히 동일한 척도로 정규화된 데이터를 생성하지는 않습니다.
- 데이터의 표준편차가 크면(값이 넓게 퍼져있으면) 정규화되는 값이 0에 가까워집니다

</br>


## 📑 꼬리질문
### 정규화를 통해 값을 Scaling하는 이유에 대해서 설명해주세요.

<p align="center"><img src="https://github.com/RecoRecoNi/Tech-Interview/assets/70088803/2481a07e-7ebf-45e5-9d0b-54c9f14ac35a" height="300" width="600px"></p>

특성들의 스케일이 매우 다르면 오른쪽 그림과 같이 비용 함수 공간의 모양이 길쭉한(elongated) 모양일 수 있습니다.
- 이 경우 경사 하강법의 수렴속도가 훨씬 오래 걸립니다.
  <p align="center"><img src="https://github.com/RecoRecoNi/Tech-Interview/assets/70088803/f2c30888-5315-49d3-a27d-da20a7c9a79c" height="300" width="500px"></p>
  
- 또한 특성 스케일링을 진행하게 되면, local minimum에서 더 빨리 빠져나올 수 있습니다.

Ex) “주택”의 정보가 담긴 데이터에서 Feature로 방의 개수, 얼마나 오래 전에 지어졌는지 같은 것들이 포함될 수 있습니다. 그리고 여기서 머신러닝 알고리즘을 통해 어느 집이 가장 적합한지 예측한다고 가정을 해보겠습니다.

→ 얼마나 오래전에 지어졌는지의 숫자가 더 스케일이 크기 때문에 연도로 데이터가 좌지우지되는 상황이 발생 할 수 있습니다.

</br>


### Batch Normalization에 대해서 설명해주세요.
딥러닝 학습 과정에서 각 레이어 마다 입력값의 분산이 달라지는 현상(Internal Covariance Shift)을 막고, 정규화의 장점(수렴 속도, local minimum 탈출)을 취하기 위해 Layer마다 미니배치 단위로 정규화를 진행하는 기법입니다. 이 때, 데이터를 계속 단순 정규화하게 되면 함수의 비선형 성질을 잃게 되는 문제가 발생합니다. 이를 위해 정규화 이후 Scale 및 Shift 연산을 수행하게 됩니다.
<p align="center"><img src="https://github.com/RecoRecoNi/Tech-Interview/assets/70088803/6345c73f-419f-40a9-8a89-70e310d579fd" height="300" width="500px"></p>


이 과정이 별도의 과정으로 떼어진 것이 아니라, 신경망 안에 포함되어 함께 학습됩니다. 즉, **정규화 이후 scale, shift을 수행하는 인자 $\gamma, \beta$도 학습 가능한 변수로, Backprop에 의해 학습**됩니다.

정규화 시 분모에 포함된 $\epsilon$은 계산할 때 0으로 나눠지는 문제가 발생하는 것을 막기 위한 아주 작은 숫자를 의미합니다.

</br>

### Batch Norm의 학습시와 인퍼런스시의 사용되는 통계량의 차이는 무엇일까요?
학습시에는 배치 단위의 데이터를 기준으로 통계량(평균,분산)을 계산하고, 인퍼런스 시에는 학습시 배치마다 얻었던 통계량의 이동 평균을 사용하여 각 평균과 분산을 구합니다.
인퍼런스 시에는 배치가 아닌 하나의 데이터나 이미지를 처리해야 하므로 추론 과정에서 배치마다 평균과 분산을 계산하기가 어렵기 때문입니다.
이 때 사용할 고정된 평균과 분산은 학습 과정에서 **이동 평균(moving average)** 또는 **지수 평균(exponential average)** 을 통하여 계산한 값입니다. 즉, 학습 하였을 때의 최근 N개에 대한 평균 값을 고정값으로 사용하는 것입니다.
이동 평균을 하면 N개 이전의 평균과 분산은 미반영 되지만 지수 평균을 사용하면 전체 데이터가 반영됩니다.


</br>

## 📚 Reference
[github blog - [딥러닝] 정규화? 표준화? Normalization? Standardization? Regularization?](https://realblack0.github.io/2020/03/29/normalization-standardization-regularization.html)

[핸즈온 머신러닝 2판 - 4장 모델 훈련]

[티스토리 - [Deep Learning] Batch Normalization (배치 정규화)](https://eehoeskrap.tistory.com/430)

[Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift](https://arxiv.org/pdf/1502.03167.pdf)

[[Deep Learning] Batch Normalization(배치 정규화)](https://eehoeskrap.tistory.com/430)

[테크 블로그 - 정규화 쉽게 이해하기](http://hleecaster.com/ml-normalization-concept/)

[티스토리 - 딥러닝 용어정리, Batch Normalization, 배치 정규화 설명](https://light-tree.tistory.com/139)

[블로그 - 배치 정규화](https://gaussian37.github.io/dl-concept-batchnorm/)

[티스토리 - [개념 정리] Batch Normalization in Deep Learning - part 1.](https://cvml.tistory.com/5)
