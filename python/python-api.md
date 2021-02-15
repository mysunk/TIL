python api
=======
useful python api  

### re (regular expression)
* findall: 특정 패턴을 갖는 pair를 전부 return
```python
import re
test_str = 'abcd12ab34'
pairs = re.findall(r'(\w+?)(\d+)', test_str)
```

### matplotlib configure
```python
import matplotlib
font = {'size': 16, 'family':"NanumGothic"}
matplotlib.rc('font', **font)
```

### Decorator
* 대상 함수를 wrapping
* 반복적으로 실행되는 함수나 class를 재사용하기 위함
* 함수와 class 둘 다 가능하다
  
__Example__
```python
class Decorator_example:
    def __init__(self, f):
        self.func = f
    
    def __call__(self):
        print('Begin')
        self.func()
        print('End')

@ Decorator_example
def func():
    print('Something')
```

위와 같은 예제를 실행하면 다음과 같이 출력될 것이다.
```
Begin
Something
End
```