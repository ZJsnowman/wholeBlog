---
title: 记一次 Python 处理日志速度优化
date: 2018-05-15 09:45:02
tags: [数据分析,Python优化]
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
实验证明这种方法意义不大.首先`cat`操作本身需要时间,在这第二步执行结果发现提升非常有限.
**暂时结论**是不推荐这种方法

## 第三个优化
在做正则匹配的时候,加一个字符串判断,而不是每行都做正则匹配,当文本很长的时候,正则匹配非常耗时.
而用 python 原生的`in`来做判断可以提速,非常明显.
```python
            if '[request]' in line:
                request_match = regrex_request.search(line)
            if '[response]' in line:
                response_match = regrex_response.search(line)
            if '[all_cost]' in line:
                all_cost_match = regrex_all_cost.search(line)
            if 'send][cost]' in line:
                channel_cost_match = regrex_channel_cost.search(line)
```


## 第四次优化
多进程处理.一般 python代码都只使用一个 CPU. 利用`ProcessPoolExecutor`可以轻松实现多进程处理,非常适合计算型任务(Wall Time ≈ CPU Total Time)
```python
import concurrent.futures
with concurrent.futures.ProcessPoolExecutor() as executor:
    image_files = glob.glob("*.jpg")
    for image_file, thumbnail_file in zip(image_files, executor.map (make_image_thumbnail, image_files)):  

import globimport osfrom PIL 
import Image
def make_image_thumbnail(filename):
    # 缩略图会被命名为"<original_filename>_thumbnail.jpg"
    base_filename, file_extension = os.path.splitext(filename)
    thumbnail_filename = f"{base_filename}_thumbnail{file_extension}"

    # 创建和保存缩略图
    image = Image.open(filename)
    image.thumbnail(size=(128, 128))
    image.save(thumbnail_filename, "JPEG")    
    return thumbnail_filename# 循环文件夹中所有JPEG图像，为每张图像创建缩略图
```
上述方法非常适合下面的任务:
- 从一堆XML，CSV和JSON文件中解析数据。

- 对大量图片数据做预处理，建立机器学习数据集。

**这里总结一个处理上述任务的一个套路**
1. 首先获得你想处理的文件（或其它数据）的列表

2. 写一个辅助函数，能够处理上述文件的单个数据

3. 使用for循环调用辅助函数，处理每一个单个数据，一次一个。

学习链接:https://mp.weixin.qq.com/s/6eSQuRa_3Wvq8Nc3zd48Gg
## 七个提升 Python性能的好习惯
- 使用局部变量
- 减少函数调用次数
- 采用映射替代条件查找
- 直接迭代序列元素
- 采用生成器表达式替代列表解析
- 先编译后调用
- 模块编程习惯

[七个提升Python 性能的好习惯](https://mp.weixin.qq.com/s/lH_B3BFBrmzuKZp7djoxOA)
