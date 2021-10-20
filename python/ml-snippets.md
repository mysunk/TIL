시간 절약을 위한 code snippet 모음
=======

# context
>* [preprocessing](#preprocessing)  
>* [regression](#regression)  
>* [plot dnn history](#plot-dnn-history)  
>* [hyperopt](#hyperopt)
>* [classification](#classification)

## preprocessing
```python
from sklearn.preprocessing import LabelEncoder
import numpy as np
cat_features = ['continent', 'position', 'prefer_foot']
le_dict = dict()

def preprocessing(df, phase = 'train'):

  df['contract_until'] = df['contract_until'].apply(lambda x : int(x[-4:])).astype(int)

  df  = df.drop(['id','name'],axis = 1)

  for col in cat_features:
    if phase == 'train':
      encoder = LabelEncoder()
      unique_arr = np.unique(df[col])
      encoder.fit(unique_arr)
      le_dict[col] = encoder
    else:
      # train에 없는 게 있다면 replace
      encoder = le_dict[col]
      df[col] = df[col].apply(lambda x: x if x in encoder.classes_ else 'UNKNOWN')
    df[col] = encoder.transform(df[col])
  
  return df

train = preprocessing(train_raw.copy())
test = preprocessing(test_raw.copy(), phase = 'test')
```

## regression
```python
from sklearn.model_selection import KFold
import numpy as np
import lightgbm as lgb
from sklearn.metrics import mean_absolute_error, mean_squared_error
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, BatchNormalization
import tensorflow as tf
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import ExtraTreesRegressor
from xgboost import XGBRegressor
from catboost import CatBoostRegressor
import xgboost as xgb
import catboost as cat


def train_model(train_x, train_y, n_fold, fold_rs=0, categorical_feature = None, log_transform = False, regressor = 'lgb', metric = 'mae', verbose = False):

    valid_probs = np.zeros((train_y.shape))
    if log_transform:
      train_y = np.log(train_y)

    # -------------------------------------------------------------------------------------
    # Kfold cross validation

    models = []
    k_fold = KFold(n_splits=n_fold, shuffle=True, random_state=fold_rs)
    # split train, validation set
    for train_idx, val_idx in k_fold.split(train_x):

        # input 데이터 형식이 dataframe일 때와 array일 때를 구분
        if type(train_x) == pd.DataFrame:
            X = train_x.iloc[train_idx, :]
            valid_x = train_x.iloc[val_idx, :]
        elif type(train_x) == np.ndarray:
            X = train_x[train_idx, :]
            valid_x = train_x[val_idx, :]
        else:
            print('Unknown data type for X')
            return -1, -1

        y = train_y[train_idx]
        valid_y = train_y[val_idx]

        if regressor == 'rf':
          model=RandomForestRegressor(n_jobs = -1, criterion = metric)
          model.fit(X, y)

        elif regressor == 'ext':
          params = {
            'criterion':metric,
            'random_state':0,
            'n_jobs':-1
          }
          model = ExtraTreesRegressor(**params)
          model.fit(X, y)

        elif regressor == 'dnn':
          model = Sequential()
          model.add(Dense(64, input_shape=(X.shape[1],), activation='relu'))
          model.add(BatchNormalization())
          model.add(Dense(32, activation='relu'))
          model.add(BatchNormalization())
          model.add(Dense(1, activation='linear'))

          opt = tf.keras.optimizers.Adam(learning_rate=0.1)
          model.compile(loss=metric, optimizer=opt, metrics=[metric])
          es = tf.keras.callbacks.EarlyStopping(monitor='val_loss', patience=10, restore_best_weights=True)
          history = model.fit(X, y, epochs=1000, batch_size=128, verbose=1, validation_data = (valid_x, valid_y), shuffle = True, callbacks = [es])

        elif regressor == 'lgb':
          # 기본 파라미터
          params = {
                  'n_jobs': -1,
                  'objective':metric
              }
          d_train = lgb.Dataset(X, y, categorical_feature=categorical_feature)
          d_val = lgb.Dataset(valid_x, valid_y, categorical_feature=categorical_feature)
          # run training
          model = lgb.train(
              params,
              train_set=d_train,
              num_boost_round=10000,
              valid_sets=[d_train, d_val],
              early_stopping_rounds=100,
              verbose_eval=verbose,
          )
        elif regressor == 'cat':
          params = {
              'random_state': 0,
              'depth': 10,
              'learning_rate': 0.1,
              'iterations':10000,
              'silent' : True,
              'eval_metric':metric.upper(),
              'early_stopping_rounds':100,
              'loss_function':metric.upper()
          }
          model = CatBoostRegressor(**params)
          model.fit(X, y, eval_set = (valid_x, valid_y))

        elif regressor == 'xgb':
          xgb_params = {
              'n_jobs': -1,
              'eval_metric': metric
          }
          model = XGBRegressor(**xgb_params)
          model.fit(X, y, eval_set = (valid_x, valid_y))

        # cal valid prediction
        valid_prob = model.predict(valid_x)
        valid_probs[val_idx] = np.ravel(valid_prob)

        models.append(model)

    if log_transform:
      train_y, valid_probs = np.exp(train_y), np.exp(valid_probs)
    if metric == 'mae':
      loss = mean_absolute_error(train_y, valid_probs)
    elif metric == 'rmse':
      loss = mean_squared_error(train_y, valid_probs, squared = False)
    elif metric == 'mse':
      loss = mean_squared_error(train_y, valid_probs, squared = True)
    return models, valid_probs, loss
```

## plot dnn history
```python
import matplotlib.pyplot as plt
import numpy as np

# plot history
def plot_history(histories, key='loss'):
    plt.figure(figsize=(10,5))

    for name, history in histories:
      val = plt.plot(history.epoch, history.history['val_'+key],
                   '--', label=name.title()+' Val')
      plt.plot(history.epoch, history.history[key], color=val[0].get_color(),
             label=name.title()+' Train')
      if key == 'loss':
        idx = np.argmin(history.history['val_'+key])
      else:
        idx = np.argmax(history.history['val_'+key])
      best_tr = history.history[key][idx]
      best_val = history.history['val_'+key][idx]
      
      print('Train {} is {:.3f}, Val {} is {:.3f}'.format(key, best_tr,key, best_val))

    plt.xlabel('Epochs')
    plt.ylabel(key)
    plt.legend()

    plt.xlim([0,max(history.epoch)])
    plt.show()

plot_history([('baseline', history)],key='loss')
```

## hyperopt
```python
from sklearn.metrics import mean_absolute_error
from functools import partial
from hyperopt import hp, tpe, fmin, Trials, STATUS_OK, STATUS_FAIL


np.random.seed(0)

search_space = {
          'objective': 'regression_l1',
          'verbose': -1,
          'n_jobs':-1,
          # 'min_child_weight':         hp.loguniform('min_child_weight',    np.log(1e-4), np.log(1e-2)),
          'learning_rate':            hp.loguniform('learning_rate',    np.log(0.0001), np.log(0.2)),
          'max_depth':                -1,
          # 'num_leaves':               hp.quniform('num_leaves',       16, 64, 1),
          # 'min_data_in_leaf':		    hp.quniform('min_data_in_leaf',	10, 40, 1),	# overfitting 안되려면 높은 값
          'reg_alpha':                hp.uniform('reg_alpha',0, 1),
          'reg_lambda':               hp.uniform('reg_lambda',0, 1),
          'colsample_bytree':         hp.uniform('colsample_bytree', 0.8, 1.0),
          'colsample_bynode':		    hp.uniform('colsample_bynode',0.8,1.0),
          # 'bagging_freq':			    hp.quniform('bagging_freq',	0,10,1),
          # 'tree_learner':			    hp.choice('tree_learner',	['serial','feature','data','voting']),
          # 'subsample':                hp.uniform('subsample', 0.8, 1.0),
          # 'boosting':			        hp.choice('boosting', ['gbdt']),
          # 'max_bin':			        hp.quniform('max_bin',		200,300,1), # overfitting 안되려면 낮은 값
      }

def make_param_int(param, key_names):
    for key, value in param.items():
        if key in key_names:
            param[key] = int(param[key])
    return param

def obj_fun(params):
  params = make_param_int(params, ['max_depth','num_leaves','min_data_in_leaf',
                                'min_child_weight','bagging_freq','max_bin'])
  models, valid_predictions, loss = train_model(train_x, train_y, params, 10, categorical_feature=None, log_transform = True)
  return {'loss': loss, 'params': params, 'status': STATUS_OK}

def process(trials, algo, max_evals):
    try:
      result = fmin(fn = obj_fun, space=search_space, algo=algo, max_evals=max_evals, trials=trials)
    except Exception as e:
      return {'status': STATUS_FAIL,
                'exception': str(e)}
    return result, trials

bayes_trials = Trials()
tuning_algo = tpe.suggest  # -- bayesian opt
# tuning_algo = tpe.rand.suggest # -- random search

result = process(bayes_trials, tuning_algo, 100)
```

## classification
```python
import pandas as pd
import numpy as np
from tqdm import tqdm
from sklearn.metrics import *
from sklearn.model_selection import KFold
import lightgbm as lgb
from catboost import CatBoostClassifier, Pool
import matplotlib.pyplot as plt
import matplotlib

def f_pr_auc(probas_pred, y_true):
    """
    lightgbm custom loss functions

    [Input]
    probas_pred: 예측 결과
    y_true: 실제값

    [Output]
    metric 이름, auc 값, whether maximize or minimize (True is maximize)
    """

    labels = y_true.get_label()
    p, r, _ = precision_recall_curve(labels, probas_pred)
    score = auc(r, p)
    return "pr_auc", score, True


def lgb_train_model(train_x, train_y, params, n_fold, fold_rs=0, categorical_feature = ['model_start','model_end']):
    """
    lightgbm cross validation with given data

    [Input]
    train_x, train_y: 학습에 사용될 data와 label
    params: lightgbm parameter
    n_fold: k-fold cross validation의 fold 수
    fold_rs: fold를 나눌 random state

    [Output]
    models: fold별로 피팅된 model
    valid_probs: validation set에 대한 예측 결과
    """

    valid_probs = np.zeros((train_y.shape))

    # -------------------------------------------------------------------------------------
    # Kfold cross validation

    models = []
    k_fold = KFold(n_splits=n_fold, shuffle=True, random_state=fold_rs)
    # split train, validation set
    for train_idx, val_idx in k_fold.split(train_x):

        # input 데이터 형식이 dataframe일 때와 array일 때를 구분
        if type(train_x) == pd.DataFrame:
            X = train_x.iloc[train_idx, :]
            valid_x = train_x.iloc[val_idx, :]
        elif type(train_x) == np.ndarray:
            X = train_x[train_idx, :]
            valid_x = train_x[val_idx, :]
        else:
            print('Unknown data type for X')
            return -1, -1

        y = train_y[train_idx]
        valid_y = train_y[val_idx]

        d_train = lgb.Dataset(X, y, categorical_feature=categorical_feature)
        d_val = lgb.Dataset(valid_x, valid_y, categorical_feature=categorical_feature)

        # run training
        model = lgb.train(
            params,
            train_set=d_train,
            num_boost_round=1000,
            valid_sets=d_val,
            feval=f_pr_auc,
            early_stopping_rounds=100,
            verbose_eval=False,
        )

        # cal valid prediction
        valid_prob = model.predict(valid_x)
        valid_probs[val_idx] = valid_prob

        models.append(model)

    return models, valid_probs


def cat_train_model(train_x, train_y, params, n_fold, fold_rs=0):
    """
    catboost cross validation with given data

    [Input]
    train_x, train_y: 학습에 사용될 data와 label
    params: catboost parameter
    n_fold: k-fold cross validation의 fold 수
    fold_rs: fold를 나눌 random state

    [Output]
    models: fold별로 피팅된 model
    valid_probs: validation set에 대한 예측 결과
    """

    valid_probs = np.zeros((train_y.shape))
    models = []
    # -------------------------------------------------------------------------------------
    # Kfold cross validation
    k_fold = KFold(n_splits=n_fold, shuffle=True, random_state=fold_rs)
    for train_idx, val_idx in k_fold.split(train_x):
        # split train, validation set
        if type(train_x) == pd.DataFrame:
            X = train_x.iloc[train_idx, :]
            valid_x = train_x.iloc[val_idx, :]
        elif type(train_x) == np.ndarray:
            X = train_x[train_idx, :]
            valid_x = train_x[val_idx, :]
        else:
            print('Unknown data type for X')
            # return -1, -1
        y = train_y[train_idx]
        valid_y = train_y[val_idx]

        train_dataset = Pool(data=X,
                             label=y,
                             cat_features=['model_start', 'model_end'])

        valid_dataset = Pool(data=valid_x,
                             label=valid_y,
                             cat_features=['model_start', 'model_end'])

        cbm_clf = CatBoostClassifier(**params)

        cbm_clf.fit(train_dataset,
                    eval_set=valid_dataset,
                    verbose=False,
                    plot=False,
                    )

        # cal valid prediction
        valid_prob = cbm_clf.predict_proba(valid_x)
        valid_probs[val_idx] = valid_prob[:, 1]

        models.append(cbm_clf)

    return models, valid_probs

lgb_param = {
    'force_col_wise': True,
    'objective': 'binary',
    'tree_learner': 'data',
    'boosting': 'gbdt',
    'metrics': 'auc',
    'random_state': 0,
    'verbose': -1,
    'max_depth': -1,
    'n_jobs': -1,
}

cat_param = {
    'custom_loss': 'AUC',
    'depth': 10,
    'iterations': 1000,
    'thread_count': 6
}

models_lgb, valid_probs_lgb = lgb_train_model(train_X, train_y, lgb_param, n_fold = 10, fold_rs=0)
auc_score_lgb = roc_auc_score(train_y, valid_probs_lgb)
print('Lightgbm validation score:: {:.5f}'.format(auc_score_lgb))

print('Catboost model fitting...')
models_cat, valid_probs_cat = cat_train_model(train_X, train_y, cat_param, n_fold = 10, fold_rs=0)
auc_score_cat = roc_auc_score(train_y, valid_probs_cat)
print('Catboost validation score:: {:.5f}'.format(auc_score_cat))

# lightgbm 모델과 catboost 모델을 앙상블
weight = [0.6, 0.4] # 앙상블 weight 지정
auc_score_ens = roc_auc_score(train_y, valid_probs_lgb * weight[0] + valid_probs_cat * weight[1])
print('Ensemble validation score:: {:.5f}'.format(np.max(auc_score_ens)))

# 결과 plot
import matplotlib.pyplot as plt
import matplotlib
font = {'size': 16, 'family':"Malgun Gothic"}
matplotlib.rc('font', **font)
plt.rcParams['axes.unicode_minus'] = False

plt.bar(range(3), [auc_score_lgb, auc_score_cat, auc_score_ens], color = 'tab:gray')
plt.bar([2], [auc_score_ens], color = 'tab:pink')
plt.xticks(range(3),['LGB','CAT','ENS'])
plt.title('10-fold cv 성능 비교')
plt.ylabel('AUC')
plt.show()
```
