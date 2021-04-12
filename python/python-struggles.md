python struggles
=======
python trouble shooting  

### Rule
* for loop 등 dummy variable과 실제 사용되는 variable 구분하기! 어디서 꼬일지 모른다

### Pycharm module import시 reference를 못찾는 문제
* 다음을 통해 system path에 directory 추가 시, pycharm이 못 찾을 수 있다
```
import sys
sys.path.insert(0, 'path-to-dir')
```
* python interpreter path에 'path-to-dir'을 추가하면 해결됨
* ref: [jetbrains issue link](https://intellij-support.jetbrains.com/hc/en-us/community/posts/360003198659-PyCharm-Fails-to-resolve-references-to-installed-modules-and-even-built-ins)