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

### DNN custom objective를 사용하는 model load시 생길 수 있는 문제
```
Traceback (most recent call last):
  File "C:\ProgramData\Anaconda3\envs\tf-gpu\lib\site-packages\IPython\core\interactiveshell.py", line 3418, in run_code
    exec(code_obj, self.user_global_ns, self.user_ns)
  File "<ipython-input-40-963e752b7d1e>", line 1, in <module>
    base_model = tf.keras.models.load_model(f"models/model_Q_{question_number}_{classifier}.h5",custom_objects=None, compile=True)
(...중략...)
ValueError: Unknown loss function:categorical_hinge_loss
```
* 모듈 입장에서 custom objective function을 알 수 없으므로 생기는 당연한 문제
* 해결법
    1. custom objective를 지정해준다
    2. 그럴 수 없다면 compile=False로 model을 load한다
* see also this [issue](https://github.com/keras-team/keras/issues/5916)