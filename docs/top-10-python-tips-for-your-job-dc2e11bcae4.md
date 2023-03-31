# 适合您工作的 10 大 Python 技巧

> 原文：<https://medium.com/analytics-vidhya/top-10-python-tips-for-your-job-dc2e11bcae4?source=collection_archive---------20----------------------->

## 将这些片段添加到您的代码中并升级！

![](img/dc3e1dd63393ec1e2b82e7729487f235.png)

如果你问任何一个 Python 程序员关于 Python 的优势，他会说简洁和高可读性是最有影响力的。Python 努力让开发人员对其更简单、更简洁的语法感到满意。

极客们注意了！在这篇文章中，我整理了一些我发现的技巧。

## 提示#1 范围词典

最新版本的 python 不支持范围字典。但是这里有一个技巧

```
class RangeDict(dict):
    def __getitem__(self, item):
        if not isinstance(item, range):
            for key in self:
                if item in key:
                    return self[key]
            raise KeyError(item)
        else:
            return super().__getitem__(item)
```

要使用它，创建一个像这样的范围字典—

```
sample_dict = RangeDict({
    range(200,500): 20,
    range(100,200):10,
    range(0,100):5
})print(sample_dict[30]) #5
```

## 技巧#2 并行处理

根据可以并行化的任务，并行处理可以奇迹般地加快执行速度。

```
import concurrent.futureswith concurrent.futures.ProcessPoolExecutor() as executer:
    executer.map(<function_name>, <args>, chunksize=<int>)
```

或者

```
results = []
with concurrent.futures.ProcessPoolExecutor() as executer:
    results_map = executer.map(<function_name>, <args>, chunksize=<int>)
    for res in results_map:
        results.append(res)
```

另一个关于类的例子—

```
import concurrent.futures

class Concurrent:

    @staticmethod
    def execute_concurrently(function, kwargs_list):
        results = []
        with concurrent.futures.ProcessPoolExecutor() as executor:
            for _, result in zip(kwargs_list, executor.map(function, kwargs_list)):
                results.append(result)
        return results
```

但是如果你想在像 AWS lambda 这样的无服务器环境中使用它，应该使用线程池——

```
import concurrent.futures

class Concurrent:

    @staticmethod
    def execute_concurrently(function, kwargs_list):
        results = []
        with concurrent.futures.ThreadPoolExecutor(max_workers=10) as executor:
            futures = [executor.submit(function, kwargs) for kwargs in kwargs_list]
        for future in concurrent.futures.as_completed(futures):
            results.append(future.result())
        return results
```

参考—[https://github . com/Mozilla-services/remote-settings-lambdas/pull/103/commits/e 124220 BF 1 f 395 F3 CBC be 9 AE 98 e 75 ad 20 a 389 C5 b](https://github.com/mozilla-services/remote-settings-lambdas/pull/103/commits/e124220bf1f395f3cbcbe9ae98e75ad20a389c5b)

当长程序运行时，你可以拿一个咖啡杯四处逛逛，并在程序完成时通过系统铃声得到通知！

```
import winsound
winsound.PlaySound("SystemExit", winsound.SND_ALIAS)
```

## 技巧#3 数据帧值计数()作为字典

你们一定都使用过这个 value_counts 方法来检查或评估结果。但是，下面是如何将这个值用作其他内容的字典。

```
var = df[col].value_counts()
dict(zip(var.keys().tolist(), var.tolist()))
```

## 技巧 4 转置一个 2D 阵列

```
arr = [['x1', 'y1', 'z1'],['x2', 'y2', 'z2']]
print(list(zip(*arr)))
```

## 提示#5 破解 excel 文件的密码

```
from xlrd import *
import win32com.client
import pandas as pdxl = win32.com.client.Dispatch("Excel.Application")
filename, password = r'path to xlsx file', 'password'
xlwb = xl.Workbooks.Open(filename, False, True, None, password)
df = pd.DataFrame(list(xlwb.heets(1).Range("A1:D5").Value))
print(df)
```

## 技巧#6 从剪贴板创建数据帧，并为测试生成虚拟数据

```
pd.read_clipboard()###########################pd.util.testing.N = 10 #rows
pd.util.testing.K = 5 #columns
pd.util.testing.makeDataFrame()
```

## 技巧 7 将多个文件解压到一起

```
import os, zipfile
dir_name = os.path.abspath('input_files)
for item in os.listdir(os.chdir(dir_name)):
    if item.endswith(".zip"):
        file_path = os.path.abspath(item)
        zipfile.ZipFile(file_path).extractall(os.path.join(dir_name, os.path.abspath(file_path.split('.')[0])))
        os.remove(os.path.abspath(item)) ## comment this line if you do not want the original zip files to be removed after extraction
```

## 技巧#8 宇宙魔方 OCR 和 Poppler

关于宇宙魔方 OCR 库的细节可以在这里找到——https://github.com/UB-Mannheim/tesseract/wiki

下面将为 python 开发安装包装器

```
pip install tesseract
```

关于波普勒的细节可以在这里找到——(也应该添加到路径)【https://blog.alivate.com.au/poppler-windows 

其他 OCR 库包括 *camelot-py、tabula-py、EasyOCR*

## 技巧 9 安装 python 包时跳过 SSL 验证

```
pip install — trusted-host pypi.org — trusted-host files.pythonhosted.org
```

## 提示#10 Conda 环境覆盖

有时，如果环境存在于您的系统中，或者作为在执行主程序之前设置环境的脚本的一部分，您可能需要覆盖它。

```
conda create --force -y --nae py_env_cmbs --file python\<conda_env.yaml or txt>
```

## 附加提示按指定顺序排列字典键

在处理 JSON 对象时，这个技巧非常方便。

```
original_dict = {'key3':'value3','key1':'value1','key2':'value2'}
order_keys = ['key1','key2','key3']
ordered_dict = {k:original_dict[k] for k in order_keys}
print(ordered_dict)
```