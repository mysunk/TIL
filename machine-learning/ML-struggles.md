machine learning struggles
=======
machine learning trouble shooting  

### Early stopping시 생길 수 있는 문제
* 다음과 같은 error는 early stopping시 생길 수 있다/ 
```python
if expected_num_weights != len(weights): 
TypeError: object of type 'NoneType' has no len()
```
* 참고: [link](https://stackoverflow.com/questions/61176084/typeerror-object-of-type-nonetype-has-no-len-when-using-restore-best-weight)
* validation loss중 best를 찾을 수 없어서 생기는 문제
* 보통 loss가 NaN인 경우가 많다
* 학습 과정을 확인해보면 다음과 같음
```python
Epoch 9/100
 128/3324 [>.............................]
 - ETA: 0s - loss: nan
2688/3324 [=======================>......]
 - ETA: 0s - loss: nan
3324/3324 [==============================]
 - 0s 24us/sample - loss: nan - val_loss: nan
Epoch 10/100
 128/3324 [>.............................]
 - ETA: 0s - loss: nan
2688/3324 [=======================>......]
 - ETA: 0s - loss: nan
``` 
* parameter tuning중에 이런 문제가 생겼으면 잘못된 parameter가 들어가 있을 가능성이 있음