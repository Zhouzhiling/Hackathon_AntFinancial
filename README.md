# Hackathon_AntFinancial

---7/16---

#### result_716_1000.csv

版本特点：

删除66个特征

XGBoost L2 Lambda = 1

max_depth = 4

```python
#手动剔除变量
#a = [str(i) for i in range(20,25)]
b = ['20','22','24','26','32','34','48','50','52','53']        
c = [str(i) for i in range(64,72)]
d = [str(i) for i in range(111,155)]
```

##### TEST：

本地验证score = 0.3593916192721781

线上测试score = 0.3006

------

#### result_716_1000_V2.csv

版本特点：

修改未知标签的样本为负样本:（abs(label))

##### TEST：

五轮score = 0.36/0.36/0.32/0.46...

本地验证score = 0.3744718014761344

**线上测试score = 0.36(best)**



#### result_716_1000_V3.csv

版本特点：

- 回归→分类
- 归一化删除
- 加入过采样的负样本9000个。

五轮score = 0.37/0.46/0.46/0.51/0.54...

本地验证score = 0.4739



#### result_716_1400_reverse.csv

版本特点：

- 调参

- ```python
  param = {
          'booster': 'gbtree',
          'objective': 'binary:logistic',  # 二分类问题
          'max_depth': 5,  # 构建树的深度，越大越容易过拟合
          'lambda':1,  # 控制模型复杂度的权重值的L2正则化项参数，参数越大，模型越不容易过拟合。
          'gamma':0.1,
          'subsample':0.8,
          'colsample_bytree':0.8,
          'seed' : 1,
          # 'min_child_weight':4,
          # 这个参数默认是 1，是每个叶子里面 h 的和至少是多少，对正负样本不均衡时的 0-1 分类而言
          # ，假设 h 在 0.01 附近，min_child_weight 为 1 意味着叶子节点中最少需要包含 100 个样本。
          # 这个参数非常影响结果，控制叶子节点中二阶导的和的最小值，该参数值越小，越容易 overfitting。
          'silent': 0,  # 设置成1则没有运行信息输出，最好是设置为0.
          'eta': eta,  # 如同学习率
          'eval_metric': 'auc'
      }
  ```



五轮score = 0.5051 + 0.5188+0.5205+0.5281+0.4182

本地验证score = 0.4981

线上测试score = 0.0031



##### result_716_1400_V2

版本更新：1-result_716_1400_reverse.score



##### result_716_1800.csv

**不用过采样**的9000条数据

##### 预测结果

max = 0.80789

min = 0.00379

median = 0.004304

线下测试score大概在0.40左右

**线上测试score = 0.3878(best)**



##### result_716_1800_V2.csv

线下测试score大概在0.36855088978862793



##### result_717_1000.csv

版本说明：

light gbm，参数参考716_1800

```python
    param = {
        'boosting_type': 'gbdt',
        'objective': 'binary',  # 二分类问题
        'max_depth': 5,  # 构建树的深度，越大越容易过拟合
        'lambda':1,  # 控制模型复杂度的权重值的L2正则化项参数，参数越大，模型越不容易过拟合。
        'gamma':0.1,
        'subsample':0.8,
        'colsample_bytree':0.8,
        'seed' : 1,
        'scale_pos_weight':1,
        # 'min_child_weight':4,
        # 这个参数默认是 1，是每个叶子里面 h 的和至少是多少，对正负样本不均衡时的 0-1 分类而言
        # ，假设 h 在 0.01 附近，min_child_weight 为 1 意味着叶子节点中最少需要包含 100 个样本。
        # 这个参数非常影响结果，控制叶子节点中二阶导的和的最小值，该参数值越小，越容易 overfitting。
         'verbosity': 1,  # 设置成1则没有运行信息输出，最好是设置为0.
        'eta': eta,  # 如同学习率
        'eval_metric': 'auc'
    }
```



线下测试score大概在0.42



##### result_717_1000_V2.csv

用LightGBM的grid search调整了参数，具体如下：

```python
# Best parameters found by grid search are: {'lambda_l2': 0.1, 'learning_rate': 0.1, 'max_depth': 7, 'num_leaves': 63}
# Best score(auc) is: 0.9825989250931323
param = {
        'boosting_type': 'gbdt',
        'objective': 'binary',  # 二分类问题
        # max_depth, gamma, subsample+colsample_bytree, 
        'max_depth': 7,  # 构建树的深度，越大越容易过拟合
        'lambda_l2': 0.1,
        'num_leaves': 63,
        'gamma':0.1,
        'subsample':0.8,
        'colsample_bytree':0.8,
        'seed' : 1,
        'scale_pos_weight':1,
        'verbosity': 0,  # 设置成1则没有运行信息输出，最好是设置为0.
        'eta': eta,  # 如同学习率
        'metric': 'auc'
    }
```

线下测试的score在0.4558，分别在0.469/0.463/0.445/0.454/0.447

max = 0.87, min = 0.000189

线上score = 0.38



##### result_717_1000_V3

模型参数：XGBoost用LGBT的参数



##### result_717_1400_ensemble(1400上传)

result_717_1000_V3 + result_717_1000_V2

XGBoost + Light GBM

然鹅效果不好

