things to be remind
=======
REF: [link](https://www.youtube.com/watch?v=iKKAvZ8JQBs&feature=youtu.be)
> ### 개발자스러움?
> 1. 회사에서 원하는 개발자 상
>   * 문제해결능력: 문제 해결을 위한 노력, 왜 이렇게 했는지, 왜 이걸 썼는지
>   * 원만한 협업  
>   * 성장 욕구, 노력, __가능성__
> 2. 개발자 문화에 익숙한가?
> 
> ### 잘 만든 git?
> 1. readme
> 2. git convention 지키기
> 3. 오픈소스 기여  
> 4. 잔디, 코드, 커밋 내역, ...  

### backend developer
REF: [link](https://www.youtube.com/watch?v=89bFo003oik)
* 데이터 사이언티스트가 백엔드 개발을 할 줄 모르면 할 수 있는 일이 많이 줄어든다
* 취업 시장에서 원하는 인재상 2가지:
    - cs 전공지식이 탄탄하거나
    - 개발을 진짜 잘하거나    
    
### commit시 주의사항
* commit message에 바꾼 내용을 전부 적어야함
* 한가지 일을 단위로 하는 것이 좋다
    * fix, refactor, add, ...
    * 안 좋은 예시: fix ~~ 라는 msg를 작성하고 실제로는 여러가지를 변경한 경우
    
### 작업 원칙!
1. .py로 작업
> * 반복적으로 사용되는 함수
> * 특정 기능 수행하는 class, 모델

2. .ipynb
> * 이것 저것 분석한 내용: analysis_{today_date}.ipynb 로 저장
    > * 해당 파일 내에서만 사용되는 함수는 따로 빼지 말고 그 안에서만 사용할 것 
> * 인사이트를 찾은 분석 내용: {topic}.ipynb 로 저장


### 데이터 분석 Tip
Q) trouble shooting에 효율적인 방법론?
- 데이터 종류별로 뭐가 좋다 하기 어려움 (같은데이터라도 다른특성 띨 수 있어서)
- 데이터 분석을 통해 특성을 정의하고 그에 맞는 방법을 search하는게 효율적
    - 어떤 스케일의 데이터 처리해야될 때?
    - 다중공선성이 높다 => 검색
    - feature selection: 수치, corr 어느정도
    - 데이터 특성 파악 후 해당특성 검색
__- 가장 중요한 것: domain 지식! 기반지식, 각각의 열들의 의미 이해__
- 변수간의 관계 파악해서 그런 특징 검색하는 게 가장 best
- __데이터, 비즈니스 필드에 대한 이해가 가장 중요__
- 변수간의 상호작용은 데이터로 확인할 수 있지만 데이터를 잘 아는사람은 그냥 알고있음
- 인과관계 없이도 상관관계가 있어보이면 인과관계가 있어보이는 데이터 찾아볼 수 있음

Q) ?
- 평가지표 100% 신뢰 x
- test 데이터 분포에 따라 다름,, test는 outlier가 없으면 outlier처리 안해도 잘나올수도 있고..
- 그래서 test case가 새로 업데이트될 때마다 model도 같이 update해야함

Q) Label encoding과 one hot encoding?
- 조심해서 해야함
- 모델이 오해하지 않게
- but 반드시 label encoding으로 해야하는 경우와 반드시 one hot encoding으로 해야하는 경우는 나눠야함

Q) DNN 명목변수 사용 가능?
- label encoding 후 적용
- Standard는 좀 이상하니 normalize로 적용
- 실제로는 float처럼 보여도 명목인 애들도 많아서..ㄱㅊㄱㅊ
