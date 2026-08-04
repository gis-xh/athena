---
tags: [GIS]
difficulty: 进阶
status: published
---

# GIS-Python

GIS 与 Python 结合的栅格数据处理与机器学习笔记。

## 环境安装

安装常用依赖：

```sh
pip install tensorflow seaborn
```

参考链接：

- [rasterio 1.2.3](https://pypi.org/project/rasterio/1.2.3/)
- [rasterio 安装文档](https://rasterio.readthedocs.io/en/latest/installation.html)
- [GDAL 3.2.2](https://pypi.org/project/GDAL/3.2.2/)
- [pyproj 升级说明](https://pyproj4.github.io/pyproj/stable/gotchas.html#upgrading-to-pyproj-2-from-pyproj-1)

### rasterio DLL load failed

导入 rasterio 时报错：

```text
ImportError: DLL load failed while importing _base: 找不到指定的程序。
```

原因是 rasterio 和 gdal 库的版本不匹配，可尝试以下步骤：

1. 卸载 rasterio 和 gdal 库：`conda remove rasterio gdal -y`
2. 重新安装并指定 gdal 版本为 2.x：`conda install rasterio gdal=2 -y`
3. 在导入 rasterio 之前先导入 gdal 模块：`from osgeo import gdal`
4. 重启 Python 解释器或 IDE，再次尝试导入 rasterio

## 分类评价指标

问题：sklearn.metrics 计算的准确率、精确度、召回率、F1 分数是用户精度吗？

答：sklearn.metrics 计算的准确率、精确度、召回率、F1 分数均基于用户指定的类别标签作为正例、其他类别作为负例。可以通过设置 `average` 参数选择不同的计算方式，例如 `average='binary'` 表示只计算二分类的结果，`average='macro'` 表示计算每个类别的结果并取平均，`average='micro'` 表示计算全局的结果。`confusion_matrix` 函数可以用来查看每个类别的真正例、假正例、真负例和假负例的数量。

## 栅格数据读取与显示

```python
import rasterio
import matplotlib.pyplot as plt
from rasterio.plot import show

img_fp = "E:\\geeDownloads\\tif\\S2_louang_namtha.tif"
full_dataset = rasterio.open(img_fp)

clipped_img = full_dataset.read([4,3,2])[:, :, :]
print(clipped_img.shape)
fig, ax = plt.subplots(figsize=(10,7))

show(clipped_img[:, :, :], ax=ax, transform=full_dataset.transform)
```

说明：

- `rasterio.open` 打开 GeoTIFF 文件并返回数据集对象。
- `read([4,3,2])` 读取第 4、3、2 三个波段（分别对应红、绿、蓝），得到形状为 `(3, height, width)` 的三维数组。
- `show` 将数组显示在图形对象上，并使用数据集对象的 `transform` 属性设置坐标系。

## 按矢量范围提取像素样本

从遥感影像中提取区域范围内的像素，转换为用于训练机器学习模型的特征向量：

```python
img_fp = "E:\\geeDownloads\\tif\\S2_louang_namtha.tif"

X = np.array([], dtype=np.int8).reshape(0,23)
y = np.array([], dtype=np.string_)

with rasterio.open(img_fp) as src:
    band_count = src.count
    for index, geom in enumerate(geoms):
        feature = [mapping(geom)]
        out_image, out_transform = mask(src, feature, crop=True)
        out_image_trimmed = out_image[:,~np.all(out_image == 0, axis=0)]
        out_image_trimmed = out_image_trimmed[:,~np.all(out_image_trimmed == 255, axis=0)]
        out_image_reshaped = out_image_trimmed.reshape(-1, band_count)
        y = np.append(y,[shapefile["class"][index]] * out_image_reshaped.shape[0])
        X = np.vstack((X,out_image_reshaped))
```

说明：

- `mask(src, feature, crop=True)` 按矢量几何范围裁剪影像，提取出指定区域内的像素。
- 去除值全为 0 和全为 255 的列，这些列没有意义，不能作为特征向量的一部分。
- 剩余像素按波段数重塑为特征向量；`y` 记录每个像素对应的类别标签，`X` 累积全部区域的特征向量。

## 参考

- [open-geo-tutorial（原创出处）](https://github.com/ceholden/open-geo-tutorial)