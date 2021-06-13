AI-related scraps
=======

jupyter notebook을 쓴 뒤로 용량문제로 github 관리가 어려워졌다,,  
최근에 취업준비를 하다보니 산업 동향과 기술동향을 따라갈 필요성을 느꼈다.  
1일 1 스크래핑 도전 ㅠ  

## 2021-06-13

### [How to Remove Multicollinearity Using Python](https://towardsdatascience.com/how-to-remove-multicollinearity-using-python-4da8d9d8abb2)  

#### Variance Inflation Factor
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