## 1 参考文章

1. [CSDN - 【办公自动化】全网最全 python 中 openpyxl 库用法](https://blog.csdn.net/m0_70214054/article/details/125570707)
2. [CSDN - Python 高效办公 1.1 第三方库 openpyxl 的安装](https://blog.csdn.net/weixin_63986098/article/details/124001914)



## 2 安装相关库

### 2.1 `openpyxl` 库

- `openpyxl` 库只能识别 `*.xlsx` 文件

- `openpyxl = open + py + xl`
- open：打开
- py：Python 的简写
- xl：Excel 的文件类型 xlsx 的前 2 位
- `openpyxl` 库 = 用 Python 打开 `*.xls` / `*.xlsx` 文件的库。

> 安装 `openpyxl` 库

```sh
pip install openpyxl
```



### 2.2 `tqdm` 库

- Tqdm 是一个进度条工具库。

> 安装 `tqdm` 库

```sh
pip install tqdm
```



## 3 使用 openpyxl 操作 Excel 表

```python
from openpyxl import *
# openpyxl 只能识别 *.xlsx 文件【此处按需修改】
filename = r'E:\Download\新闻与文化传播学院2022年硕士研究生入学考试拟录取结果公示.xlsx'
wb = load_workbook(filename)
# 按 sheet 名查看表格【此处按需修改】
sh1 = wb["拟录取汇总表"]
# 批量删除偶数列【此处按需修改】
for i in range(1,100):
    if i %2 == 0:
        sh1.delete_cols(i)
# 配置操作后的存储文件位置【此处按需修改】
savename = r'E:\Download\新闻与文化传播学院2022年硕士研究生入学考试拟录取结果公示(批量删除).xlsx'
wb.save(savename)
```

