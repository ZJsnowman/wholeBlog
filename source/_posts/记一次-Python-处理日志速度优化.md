---
title: 记一次 Python 处理日志速度优化
date: 2018-05-15 09:45:02
tags: [数据分析]
---
通过 Python+ Pandas +regrex清洗日志,在此基础上优化代码提升速度<!--more-->

# 介绍
通过正则提取日志中的有用信息,存入 DataFrame中进行分析

# 公共代码
```Python
import pandas as pd
import os
import json
import time
import re

regex = re.compile(r"\[(.*?)\].*?\[(.*?)\].*?\[(.*?)\].*?\[(.*?)\].*?\[(.*?)\].*?\[(.*?)\].*?\[(.*?)\].*?(\{.*\})")
```

# 优化前
```python
start = time.time()
df_interface = pd.DataFrame()
interface_dir_path = '/Users/zhangjun/PycharmProjects/LogAnalysis/data/gateway_hour/interface'

files = os.listdir(interface_dir_path)
# tempList = []
for file in files:
    with open(interface_dir_path + '/' + file) as f:
        for line in f:
            matches = regex.search(line)
            if matches:
                LOG_TIME = matches.group(1)
                LOG_UNIQUE_ID = matches.group(4)
                api_list = LOG_UNIQUE_ID.split('_')[2:]
                LOG_API = '_'.join(api_list)
                LOG_PROTOCOL = matches.group(6)
                LOG_TYPE = matches.group(7)
                LOG_JSON_CONTENG = matches.group(8)
                result = {'time': LOG_TIME, 'unique_id': LOG_UNIQUE_ID, 'protocol': LOG_PROTOCOL, 'type': LOG_TYPE,
                          'api': LOG_API}

                if LOG_API == 'mbp-gateway_operator_v1_login' and LOG_TYPE == 'request':
                    request_json = json.loads(LOG_JSON_CONTENG)
                    request_header = request_json['requestHeader']
                    request_params = request_json['params']
                    result.update(request_header)
                    result.update(request_params)
                    s = pd.Series(result)
                    # tempList.append(s)
                    df_interface = df_interface.append(s, ignore_index=True)
    print(file + "清理完毕添加到 DataFrame")
# df_interface = pd.DataFrame(tempList)
end = time.time()
print('interface清理读取到 DF,耗时' + str(end - start))
```
耗时:13.6s

# 第一次优化
有原来的`df.append`改成 `list.append`.带来约四倍提升.非常有效,在另外一个处理脚本提高了8倍速度.非常感人.推荐这种优化.
```python
start = time.time()
df_interface = pd.DataFrame()
interface_dir_path = '/Users/zhangjun/PycharmProjects/LogAnalysis/data/gateway_hour/interface'

files = os.listdir(interface_dir_path)
tempList = []
for file in files:
    with open(interface_dir_path + '/' + file) as f:
        for line in f:
            matches = regex.search(line)
            if matches:
                LOG_TIME = matches.group(1)
                LOG_UNIQUE_ID = matches.group(4)
                api_list = LOG_UNIQUE_ID.split('_')[2:]
                LOG_API = '_'.join(api_list)
                LOG_PROTOCOL = matches.group(6)
                LOG_TYPE = matches.group(7)
                LOG_JSON_CONTENG = matches.group(8)
                result = {'time': LOG_TIME, 'unique_id': LOG_UNIQUE_ID, 'protocol': LOG_PROTOCOL, 'type': LOG_TYPE,
                          'api': LOG_API}

                if LOG_API == 'mbp-gateway_operator_v1_login' and LOG_TYPE == 'request':
                    request_json = json.loads(LOG_JSON_CONTENG)
                    request_header = request_json['requestHeader']
                    request_params = request_json['params']
                    result.update(request_header)
                    result.update(request_params)
                    s = pd.Series(result)
                    tempList.append(s)
                    # df_interface = df_interface.append(s, ignore_index=True)
    print(file + "清理完毕添加到 DataFrame")
df_interface = pd.DataFrame(tempList)
end = time.time()
print('interface清理读取到 DF,耗时' + str(end - start))

```
耗时:3.7s

# 第二次优化
1. 先将所有分散的文件聚合成一个文件`cat * > interface.log`
2. 然后在在第一次优化的基础上直接处理这个一个文件,而不是处理多个小文件

思路:本意是想在 IO 这块提升速度.一个文件只会有两次 IO, 而多个小文件,这 IO 操作太频繁影响处理速度.
实验证明这种方法意义不大.首先`cat`操作本身需要时间,在这第二步执行结果发现提升非常有限.**暂时结论**
是不推荐这种方法
