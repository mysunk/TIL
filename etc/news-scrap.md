AI-related scraps
=======

## 2021-06-13

### [How to Remove Multicollinearity Using Python](https://towardsdatascience.com/how-to-remove-multicollinearity-using-python-4da8d9d8abb2)  

#### Variance Inflation Factor (VIF)
* multicollinearity를 파악하는 지표
* 최소가 1이며 클 수록 multicollinearity가 크다고 판단
* linear regression에서의 R^2를 이용한 지표

#### 처리방법
1. remove the feature (not recommended)
2. create new feature
    * 해당 글에서는 단순히 feature간의 difference만 남기고 원본 feature는 제거

#### Permutation feature importance
* 일반적으로 SVM model은 해석하기 어려운데, 해석하기 위한 지표
* 하나의 feature가 변할 시 그에 따른 error의 변화량을 이용해 feature importance를 측정한 지표
* error의 변화량이 클 시 해당 feature의 중요도가 큰 것
 
#### 의문점
* VIF가 5 이상인 feature를 제거했는데 제거함으로써 모델 성능이 향상되었는지 검증하지 않음
* VIF가 작으면 무조건 좋을까? 
    * VIF가 큰 feature 2개 vs 작은 feature 1개 검증 안됨
* 단순히 방법만 보여주고 검증은 하지 않아서 아쉽,, 직접 해보거나 더 찾아봐야할 듯

### [구글, 반도체 설계에 AI 적용..."수개월 걸리던 작업을 6시간 만에"](http://www.aitimes.com/news/articleView.html?idxno=138949)

* 보통 수율 향상은 제조, 생산 부분에서 이루어지나 설계 시간을 단축했다는 점이 혁신적
* 몇주 ~ 몇달 걸리는 칩 평면도 설계 작업을 6시간으로 단축

publishment: [A graph placement methodology for fast chip design](https://www.nature.com/articles/s41586-021-03544-w)
* 강화학습 기반의 graph convolutional neural network architecture

## 2021-06-14

### [“MRMR” Explained Exactly How You Wished Someone Explained to You](https://towardsdatascience.com/mrmr-explained-exactly-how-you-wished-someone-explained-to-you-9cf4ed27458b)

#### MRMR
* MRMR: Maximum Relevance — Minimum Redundancy 
* 최대 관련성과 최대 중복성
* 통용되는 feature selection 방법
* feature간의 redundancy를 제거한 minimal optimal subset을 찾는 알고리즘

Motivation: 
* Best K features 가 K best feature가 아님  

Algorithm:  
* User input needed: select K
1. Initialize: target과 가장 연관성이 높은 feature 1개를 select  
2. Select next feature: score가 가장 높은 feature를 select  
score = relevance(f | target) / redundancy(f | 현재까지 선택된 feature subset)  
해석: target과의 관련성은 높고 현재까지 선택된 feature subset과의 중복성은 낮은 feature를 고름  
3. Iterate until i = K

#### 의문점
* K는 어떻게 정할 것?

#### code snippet
```python
!pip install git+https://github.com/smazzanti/mrmr
from mrmr import mrmr_classif

selected_features = mrmr_classif(X, y, K = K)
```

## 2021-06-25
### [Fundamental Techniques of Feature Engineering for Machine Learning](https://towardsdatascience.com/feature-engineering-for-machine-learning-3a5e293a5114)

#### Feature engineering technique 요약
* Imputation
* Handling Outliers
* Binning
* Log Transform
* One-hot encodings
* Grouping Operations
* Feature Split
* Scaling
* Extracting Date

### 1. Imputation
#### 1-1. Numerical imputation
- 만약 해당 feature가 1과 NaN만 있다면 해당 부분은 0일 가능성이 높음 => 0으로 대체
   * ex) 고객 방문 여부와 같이 방문자만 labeling돼서 방문이 안 되면 값이 없는 경우
- 일반적으로는 0 혹은 중앙값으로 대치
#### 1-2. Categorical imputation
- 가장 많이 발생한 값으로 대치하는 것이 일반적
- 만약 변수들이 uniform하게 분포되어있는 경우, Others로 따로 빼는 것이 합리적

### 2. Handling Outliers
* 가장 좋은 방법은 시각화를 해보는 것
* 통계적 방법론은 빠르다는 장점이 있다
#### 2-1. 표준편차 기반
- z-score 기반으로 upper outlier와 lower outlier를 거름

#### 2-2. 백분위 기반
- 상위 x% 이상, 상위 x% 이하를 이상값으로 간주
- 표준편차 기반과 별 다를 바 없어 보임

#### 2-3. drop or cap
- 버리는 대신 limit를 두어, 그걸 넘을 시 해당 limit으로 replace함

### 3. Binning
- 범주 혹은 범위를 더 상위 개념으로 포괄함
- 어떻게보면 정보를 버리는 것이지만, robust하고 overfitting에 대응 가능함
- "Sacrifice information and regularize data"

### 4. Log Transform
- motivation: 15~20세의 차이는 65~70세와 같지 않음
- 로그 변환은 이러한 크기 차이를 정규화함
- 변환 후에는 분포가 정규 분포에 가까워짐

### 5. One-hot encoding
- 정보 손실 없이 범주형 데이터를 그룹화
```python
pd.get_dummies(data['column'])
```

### 6. 범주형 feature 그룹화
- 범주형 열 그룹화
   1. 가장 높은 빈도를 가진 label을 선택
   2. 피벗 테이블
      * ex) user별로 그룹화
   3. one-hot encoding 적용 후 feature별 그룹 적용

- 숫자형 feature 그룹화
   - 보통 합계 및 평균을 통해 그룹화

### 7. feature 분할
- 그룹화와 반대되는 개념
- ex) Luther N. Gonzalez 를 first name, middle name, last name으로 분할

### 8. Scaling
- 거리 기반 알고리즘에서 필요
- 표준화, 정규화

### 9. 날짜 분할
- 1996-10-04 를 1996, 1, 04 로 분할

## 2021-06-26
### [4 Tips for Advanced Feature Engineering and Preprocessing](https://towardsdatascience.com/4-tips-for-advanced-feature-engineering-and-preprocessing-ec11575c09ea)

Isolation Forest
- outlier detection방법 중 하나
- 어떤 sample이 얼마나 쉽게 isolation될 수 있는지 확인

```python
from sklearn.ensemble import IsolationForest
clf = IsolationForest(contamination=0.01, behaviour='new')
outliers = clf.fit_predict(data)
```

### [My secret sauce to be in top 2% of a kaggle competition](https://towardsdatascience.com/my-secret-sauce-to-be-in-top-2-of-a-kaggle-competition-57cff0677d3c)

### Identifying noisy features
