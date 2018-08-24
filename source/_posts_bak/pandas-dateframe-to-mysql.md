---
title: pandas dateframe to mysql
date: 2018-05-28 15:13:54
tags:
---
将 DF 数据序列化到 Mysql中<!--more-->

```python
import pandas as pd
from sqlalchemy import create_engine

df = pd.read_json('/Users/zhangjun/PycharmProjects/LogAnalysis/cleanData/2018-05-05.log', orient='records', lines=True)

host = '127.0.0.0'
port = 3306
db = 'db_log'
user = '***'
password = '***'

engine = create_engine(str(r"mysql+mysqldb://%s:" + '%s' + "@%s/%s?charset=utf8") % (user, password, host, db))  # 这里添加 charset=utf-8是为了解决编码问题

try:
    df.to_sql('tbl_login', con=engine, if_exists='replace', index=False)
except Exception as e:
    print(e)
```
# 指定数据类型
http://www.cnblogs.com/arkenstone/p/6271923.html
http://docs.sqlalchemy.org/en/latest/core/type_basics.html#generic-types
