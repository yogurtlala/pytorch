# 1. 导入需要用到的相关库
# 导入 pandas 库，用于数据处理和分析
import pandas as pd
# 导入 numpy 库，用于科学计算和多维数组操作
import numpy as np
# 从 lightgbm 模块中导入 LGBMClassifier 类
from lightgbm import LGBMClassifier


# 2. 读取训练集和测试集
# 使用 read_excel() 函数从文件中读取训练集数据，文件名为 'traindata-new.xlsx'
train = pd.read_excel('./data/data280993/traindata-new.xlsx')
# 使用 read_excel() 函数从文件中读取测试集数据，文件名为 'testdata-new.xlsx'
test = pd.read_excel('./data/data280993/testdata-new.xlsx')

# 3 特征工程
# 3.1 test数据不包含 DC50 (nM) 和 Dmax (%)，将train数据中的DC50 (nM) 和 Dmax (%)删除
train = train.drop(['DC50 (nM)', 'Dmax (%)'], axis=1)

# 3.2 将object类型的数据进行目标编码处理
for col in train.columns[2:]:
    if train[col].dtype == object or test[col].dtype == object:
        train[col] = train[col].isnull()
        test[col] = test[col].isnull()

# 4. 加载决策树模型进行训练
model = LGBMClassifier(verbosity=-1)
model.fit(train.iloc[:, 2:].values, train['Label'])
pred = model.predict(test.iloc[:, 1:].values, )

# 5. 保存结果文件到本地
pd.DataFrame(
    {
        'uuid': test['uuid'],
        'Label': pred
    }
).to_csv('submit.csv', index=None)
