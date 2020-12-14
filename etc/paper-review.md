paper review
=======
## household characteristic inference
### 1. Revealing household characteristics from smart meter data  (2014, ELSEVIER Energy)
[click here](https://www.sciencedirect.com/science/article/pii/S0360544214011748)
* manual로 피쳐 추출(출퇴근시간대 등을 고려)
* biased random guess보다 성능이 나음을 보임으로써 smart meter data로부터 household characteristic을 infer 가능함을 보임 (feasibility)

### 2. Deep Learning-Based Socio-Demographic Information Identification From Smart Meter Data (2018, IEEE Transactions on Smart Grid)
[click here](https://ieeexplore.ieee.org/document/8291011)
* Depp CNN을 통한 자동화된 특징 추출
* CNN으로 얻을 수 있는 장점이 데이터 특성과 잘 맞음 (time shift property 등)
* Target characteristics  
![image2-1](https://ieeexplore.ieee.org/mediastore_new/IEEE/content/media/5165411/8694132/8291011/kang.t3-2805723-small.gif)
* Used features  
![image2-2](https://ieeexplore.ieee.org/mediastore_new/IEEE/content/media/5165411/8694132/8291011/kang2-2805723-small.gif)
* Result  
![image2-3](https://ieeexplore.ieee.org/mediastore_new/IEEE/content/media/5165411/8694132/8291011/kang.t4-2805723-small.gif)

### 3. Time-Frequency Feature Combination Based Household Characteristic Identification Approach Using Smart Meter Data (2020, IEEE Transactions on Smart Grid)
[click here](https://ieeexplore.ieee.org/document/9042298)
* Time feature 뿐만 아니라 frequency feature를 사용
* Target characteristics  
![image3-1](https://ieeexplore.ieee.org/mediastore_new/IEEE/content/media/28/9079167/9042298/wang.t4-2981916-small.gif)
* Used features  
    + time domain  
    ![image3-2](https://ieeexplore.ieee.org/mediastore_new/IEEE/content/media/28/9079167/9042298/wang.t1-2981916-small.gif)
    - frequency domain  
    ![image3-3](https://ieeexplore.ieee.org/mediastore_new/IEEE/content/media/28/9079167/9042298/wang.t2-2981916-small.gif)  
* Result  
![image3-4](https://ieeexplore.ieee.org/mediastore_new/IEEE/content/media/28/9079167/9042298/wang.t5-2981916-small.gif)