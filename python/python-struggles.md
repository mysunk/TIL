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


### DataFrame에 type별 NaN처리
```python
nan_cols = pd.isnull(train_test).sum(axis=0) > 0
for col in train_test.columns[nan_cols]:
  if train_test[col].dtypes == float:
    print('Float type')
    train_test[col] = train_test[col].fillna(train_test[col].median())
  elif train_test[col].dtypes == int:
    print('Int type')
    train_test[col] = train_test[col].fillna(train_test[col].median())
  else:
    print('Obj type')
    nan_idx = pd.isnull(train_test[col])
    train_test.loc[nan_idx,col] = train_test[col].mode()
```

* 위와같이 짜면 다음과같은 error 발생

```
/usr/local/lib/python3.7/dist-packages/pandas/core/indexing.py:1743: SettingWithCopyWarning: 
A value is trying to be set on a copy of a slice from a DataFrame.
Try using .loc[row_indexer,col_indexer] = value instead

See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
  isetter(ilocs[0], value)
/usr/local/lib/python3.7/dist-packages/ipykernel_launcher.py:5: SettingWithCopyWarning: 
A value is trying to be set on a copy of a slice from a DataFrame.
Try using .loc[row_indexer,col_indexer] = value instead

See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
  """
```

* __'A value is trying to be set on a copy of a slice from a DataFrame'__
* copy에 값이 넣어지므로 value를 넣으라는..
* dataframe에 series를 넣으면 이런문제가 생기는 듯
* int나 array나 list로 바꿔줘야 한다
* 다음과같이 바꿔주면 문제없다

```python
nan_cols = pd.isnull(train_test).sum(axis=0) > 0
for col in train_test.columns[nan_cols]:
  if train_test[col].dtypes == float:
    print('Float type')
    train_test[col] = train_test[col].fillna(train_test[col].median())
  elif train_test[col].dtypes == int:
    print('Int type')
    train_test[col] = train_test[col].fillna(train_test[col].median())
  else:
    print('Obj type')
    nan_idx = pd.isnull(train_test[col])
    train_test.loc[nan_idx,col] = train_test[col].mode().values[0]
```
