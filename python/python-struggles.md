python struggles
=======
python trouble shooting  

### Rule
* for loop 등 dummy variable과 실제 사용되는 variable 구분하기! 어디서 꼬일지 모른다

### Pycharm module import시 reference를 찾지 못하는 문제
* 다음을 통해 system path에 directory 추가 시, pycharm이 못 찾을 수 있다
```
import sys
sys.path.insert(0, 'path-to-dir')
```
* python interpreter path에 'path-to-dir'을 추가하면 해결됨
* ref: [jetbrains issue link](https://intellij-support.jetbrains.com/hc/en-us/community/posts/360003198659-PyCharm-Fails-to-resolve-references-to-installed-modules-and-even-built-ins)

### NaN 제거 후에도 nan이 남아있는 문제
* 다음과 같이 empty array에 mean을 취하게 되면 shape (n,)인 nan array가 발생
* 해당 경우 예외처리 해줘야함
```
array([], shape = (0, n))
```

### Nan이 있는 데이터에서 Dataframe.sum 사용 시 주의사항
다음과 같이 연산됨
1. nan + value = value
2. nan + nan = 0
=> summation 전에 따로 처리 필요