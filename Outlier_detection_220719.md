# Outlier Detection_220719_조진환 박사님
- outlier detection의 concept을 알아야 함
- outlier detection: 개수가 적거나 대다수와 다름
<hr>
참고문헌

- Liu,Ting,Zhou_Isolation Forest_2008
- Liu,TIng,Zhou_Isolation-based Anomaly Detection_2012
- Guha et al_Robust Random Cut Forest based Anomaly Detection On Streams_2016
- Aggarwal_Outlier Analysis_2nd_2017
---

# Table of contents
1. [Anomaly Detection](#anomaly-detection)
1. [Isolation Forest](#isolation-forest)
---

# Anomaly Detection
Noise와 anomalies의 구분은 분석가의 주관에 따라 분류가 달라진다.

normal data로만 학습할 지 normal+abnormal data로 학습하는지에 따라 outlier 탐지 성능 차이가 크다.

outlier인 대상에 class를 'outlier이다' 와 'outlier가 아니다'로 나누는 게 아니라, 적당한 score를 준다. 1 $\sigma$범위는 몇 점, 2 $\sigma$ 범위는 몇 점, 3$\sigma$ 범위는 몇 점, 이런 식으로.

Difference from (supervised) classification:

- **class imbalance**: 보통 supervised learning을 할 때, class별 data의 개수는 동일하게 두는 편이다. 하지만 anomaly detection에서는 abnormal data의 수가 normal data에 비해 극히 작다.

- **False positive > False negative**: 암 진단의 경우, 정상인에게 암이라고 진단하는 것이 암환자에게 암이 아니라고 진단하는 것보다 낫다.(과잉 진단) 추가진단을 통해 걸러낼 수 있기 때문. 한번 정상이라고 진단하면 더 이상의 테스트가 들어가지 않기 때문.

- **contaminated normal class**: 일반적으로는 normal data로만 이루어진 data는 없다. abnormal data를 조금이라도 포함하고 있는 게 일반적이다.

- **partial training information**: semi-supervised or novelty detection. normal data로만 학습하는 경우도 있다.

difficulty: novelty detection(only normal data) > unsupervised learning(normal+abnormal data)

---

# Isolation Forest

Isolation Forest: 수학개념 없음(measure or distance 개념 없음)
- data의 공간(dim=n)에 hyperplane(dim=n-1)을 random하게 그려서 data들을 나눈다.(decision tree 생성) 이를 통해 outlier를 isolated 시키고, 그때까지 필요한 hyperplane의 개수를 path length라 하고 이걸 outlier-score로 주자.

- 필요한 hyperplane 개수는 최대 $2^n$개.

- Short path length means high susceptibility to isolation: path 길이가 짧다는 것의 의미는 isolated data일 확률이 높다는 뜻.

![isolated forest](images/isolated%20forest.jpg)

'윌리를 찾아라'에서 윌리 찾는 게 쉽지가 않다. 윌리는 정상인이니깐. 어떻게 하면 윌리를 쉽게 찾을 수 있을까?
- 윌리가 특이한 모자를 쓰고 있다면 쉽게 찾겠지만 보통의 모자를 쓰고 있다면 찾기 어렵다.

- 그럼 모자를 쓰고 벗을 때의 변화량을 감지해보면 어떨까?

- 변화량 감지 = path length를 미분

$L(T)$: the set of leaves on the decision tree where each leaf is a data.

- displacement of $x\in L(T)$: $\sum_{y\in L(T)\backslash\{x\}}[d_T(y)-d_{T_x}(y)]$ 

RRCF: Robust Random Cut Forest

- [내용 추가하자](https://hiddenbeginner.github.io/paperreview/2021/07/14/rrcf.html)

- [참고1](https://klabum.github.io/rrcf/anomaly-scoring.html)

- Guha 논문의 내용인듯.

path length 보다 collusive displacement가 민감도가 높다.

path length의 average를 계산하여 확률 계산?
