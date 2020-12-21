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
