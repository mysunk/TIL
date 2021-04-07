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

### 다른 프로젝트의 함수를 불러오는 법
* 해당 함수가 module안에 있어야 함
    * module화 하는 법: __init__.py empty 파일을 추가 
```
import sys
sys.path.insert(0, '/the/folder/path/name-package/')
from name-package import name-module
```