# 医药销售数据分析

# 1.提出问题

从销售数据中分析出以下业务指标： 1）月均消费次数2）月均消费金额3）客单价4）消费趋势

# 2.字段描述

>该表记录了一段时间内药品销售信息，包含7个字段，6598条信息，字段描述如下：

>购药时间：用户购买药品的时间包含星期

>社保卡号：用户购买使用社保卡号

>商品编码：销售商品编码

>商品名称：销售商品名称

>销售数量：商品销售数量

>应收金额：商品实际标价

>实收金额：用户实际支付商品价格



```python
#导入数据分析包
import pandas as pd
```


```python
#数据读取
Saledf=pd.read_excel('医院销售数据.xlsx',dtype=str)
Saledf.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>购药时间</th>
      <th>社保卡号</th>
      <th>商品编码</th>
      <th>商品名称</th>
      <th>销售数量</th>
      <th>应收金额</th>
      <th>实收金额</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-01 星期五</td>
      <td>001616528</td>
      <td>236701</td>
      <td>强力VC银翘片</td>
      <td>6</td>
      <td>82.8</td>
      <td>69</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-02 星期六</td>
      <td>001616528</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>1</td>
      <td>28</td>
      <td>24.64</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-06 星期三</td>
      <td>0012602828</td>
      <td>236701</td>
      <td>感康</td>
      <td>2</td>
      <td>16.8</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-11 星期一</td>
      <td>0010070343428</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>1</td>
      <td>28</td>
      <td>28</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-15 星期五</td>
      <td>00101554328</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>8</td>
      <td>224</td>
      <td>208</td>
    </tr>
  </tbody>
</table>
</div>




```python
#查看数据信息
Saledf.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 6578 entries, 0 to 6577
    Data columns (total 7 columns):
    购药时间    6578 non-null object
    社保卡号    6578 non-null object
    商品编码    6578 non-null object
    商品名称    6578 non-null object
    销售数量    6578 non-null object
    应收金额    6578 non-null object
    实收金额    6578 non-null object
    dtypes: object(7)
    memory usage: 359.8+ KB
    

# 3.数据清洗

## （1）选择子集 


```python
#选择子集 本次分析表中的字段在本次分析中都可用故在此不选择子集。一般选择子集可用loc函数通过索引来选择
Saledf.loc[0:4,'购药时间':'实收金额']
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>购药时间</th>
      <th>社保卡号</th>
      <th>商品编码</th>
      <th>商品名称</th>
      <th>销售数量</th>
      <th>应收金额</th>
      <th>实收金额</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-01 星期五</td>
      <td>001616528</td>
      <td>236701</td>
      <td>强力VC银翘片</td>
      <td>6</td>
      <td>82.8</td>
      <td>69</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-02 星期六</td>
      <td>001616528</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>1</td>
      <td>28</td>
      <td>24.64</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-06 星期三</td>
      <td>0012602828</td>
      <td>236701</td>
      <td>感康</td>
      <td>2</td>
      <td>16.8</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-11 星期一</td>
      <td>0010070343428</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>1</td>
      <td>28</td>
      <td>28</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-15 星期五</td>
      <td>00101554328</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>8</td>
      <td>224</td>
      <td>208</td>
    </tr>
  </tbody>
</table>
</div>



## （2）列重命名 


```python
#列重命名
#字典：旧列名和新列名对应关系
colnameDic={'购药时间':'销售时间'}
```


```python
Saledf.rename(columns=colnameDic,inplace=True)
'''
inplace=False，数据框本身不会变，而会创建一个改动后新的数据框，
默认的inplace是False
inplace=True，数据框本身会改动
'''
Saledf.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>销售时间</th>
      <th>社保卡号</th>
      <th>商品编码</th>
      <th>商品名称</th>
      <th>销售数量</th>
      <th>应收金额</th>
      <th>实收金额</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-01 星期五</td>
      <td>001616528</td>
      <td>236701</td>
      <td>强力VC银翘片</td>
      <td>6</td>
      <td>82.8</td>
      <td>69</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-02 星期六</td>
      <td>001616528</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>1</td>
      <td>28</td>
      <td>24.64</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-06 星期三</td>
      <td>0012602828</td>
      <td>236701</td>
      <td>感康</td>
      <td>2</td>
      <td>16.8</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-11 星期一</td>
      <td>0010070343428</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>1</td>
      <td>28</td>
      <td>28</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-15 星期五</td>
      <td>00101554328</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>8</td>
      <td>224</td>
      <td>208</td>
    </tr>
  </tbody>
</table>
</div>



# （3）缺失值处理

python缺失值有3种：

1）Python内置的None值

2）在pandas中，将缺失值表示为NA，表示不可用not available。

3）对于数值数据，pandas使用浮点值NaN（Not a Number）表示缺失数据。

None是Python的一种数据类型，NaN是浮点类型 两个都用作空值


```python
#缺失值处理
#删除缺失值
print('删除缺失值前大小',Saledf.shape)
```

    删除缺失值前大小 (6578, 7)
    


```python
#将字符串'nan'替换成 NAN
import numpy as np
Saledf.replace(to_replace='nan',value=np.nan,inplace=True)
```


```python
#删除列（销售时间，社保卡号）中为空的行
#how='any'在给定的任何一列中有缺失值就删除
Saledf=Saledf.dropna(subset=['销售时间','社保卡号'],how='any')
```


```python
print('删除缺失值后大小',Saledf.shape)
```

    删除缺失值后大小 (6575, 7)
    


```python
#删除缺失值，使得索引序号不连续，这里用reset_index重置索引
Saledf=Saledf.reset_index(drop=True)
Saledf
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>销售时间</th>
      <th>社保卡号</th>
      <th>商品编码</th>
      <th>商品名称</th>
      <th>销售数量</th>
      <th>应收金额</th>
      <th>实收金额</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-01 星期五</td>
      <td>001616528</td>
      <td>236701</td>
      <td>强力VC银翘片</td>
      <td>6</td>
      <td>82.8</td>
      <td>69</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-02 星期六</td>
      <td>001616528</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>1</td>
      <td>28</td>
      <td>24.64</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-06 星期三</td>
      <td>0012602828</td>
      <td>236701</td>
      <td>感康</td>
      <td>2</td>
      <td>16.8</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-11 星期一</td>
      <td>0010070343428</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>1</td>
      <td>28</td>
      <td>28</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-15 星期五</td>
      <td>00101554328</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>8</td>
      <td>224</td>
      <td>208</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2018-01-20 星期三</td>
      <td>0013389528</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>1</td>
      <td>28</td>
      <td>28</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2018-01-31 星期日</td>
      <td>00101464928</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>2</td>
      <td>56</td>
      <td>56</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2018-02-17 星期三</td>
      <td>0011177328</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>5</td>
      <td>149</td>
      <td>131.12</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2018-02-22 星期一</td>
      <td>0010065687828</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>1</td>
      <td>29.8</td>
      <td>26.22</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2018-02-24 星期三</td>
      <td>0013389528</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>4</td>
      <td>119.2</td>
      <td>104.89</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2018-03-05 星期六</td>
      <td>0010026389628</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>2</td>
      <td>59.6</td>
      <td>59.6</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2018-03-05 星期六</td>
      <td>00102285028</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>3</td>
      <td>84</td>
      <td>84</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2018-03-05 星期六</td>
      <td>0010077400828</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>1</td>
      <td>28</td>
      <td>24.64</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2018-03-07 星期一</td>
      <td>0010077400828</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>5</td>
      <td>140</td>
      <td>112</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2018-03-09 星期三</td>
      <td>0010079843728</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>6</td>
      <td>168</td>
      <td>140</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2018-03-15 星期二</td>
      <td>0010031328528</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>2</td>
      <td>56</td>
      <td>49.28</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2018-03-15 星期二</td>
      <td>00100703428</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>2</td>
      <td>56</td>
      <td>49.28</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2018-03-15 星期二</td>
      <td>0010712328</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>5</td>
      <td>140</td>
      <td>112</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2018-03-20 星期日</td>
      <td>0011668828</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>6</td>
      <td>168</td>
      <td>140</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2018-03-22 星期二</td>
      <td>0010066351928</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>1</td>
      <td>28</td>
      <td>28</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2018-03-23 星期三</td>
      <td>00102133328</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>6</td>
      <td>168</td>
      <td>140</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2018-03-24 星期四</td>
      <td>0010078873928</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>6</td>
      <td>168</td>
      <td>140</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2018-03-24 星期四</td>
      <td>00101924628</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>1</td>
      <td>28</td>
      <td>28</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2018-03-28 星期一</td>
      <td>0010075233228</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>6</td>
      <td>168</td>
      <td>140</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2018-03-29 星期二</td>
      <td>0013189428</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>1</td>
      <td>28</td>
      <td>28</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2018-04-05 星期二</td>
      <td>0010079849328</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>2</td>
      <td>56</td>
      <td>49.28</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2018-04-07 星期四</td>
      <td>0011652628</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>6</td>
      <td>168</td>
      <td>140</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2018-04-13 星期三</td>
      <td>0011005128</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>2</td>
      <td>56</td>
      <td>56</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2018-04-22 星期五</td>
      <td>0010344628</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>6</td>
      <td>168</td>
      <td>140</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2018-05-01 星期日</td>
      <td>0010070313828</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>6</td>
      <td>168</td>
      <td>140</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>6545</th>
      <td>2018-04-05 星期二</td>
      <td>00108945828</td>
      <td>2367011</td>
      <td>开博通</td>
      <td>2</td>
      <td>56</td>
      <td>49.28</td>
    </tr>
    <tr>
      <th>6546</th>
      <td>2018-04-05 星期二</td>
      <td>0011778628</td>
      <td>2367011</td>
      <td>开博通</td>
      <td>2</td>
      <td>56</td>
      <td>49.28</td>
    </tr>
    <tr>
      <th>6547</th>
      <td>2018-04-09 星期六</td>
      <td>0010028740928</td>
      <td>2367011</td>
      <td>开博通</td>
      <td>3</td>
      <td>84</td>
      <td>78</td>
    </tr>
    <tr>
      <th>6548</th>
      <td>2018-04-10 星期日</td>
      <td>0010015684428</td>
      <td>2367011</td>
      <td>开博通</td>
      <td>1</td>
      <td>28</td>
      <td>25</td>
    </tr>
    <tr>
      <th>6549</th>
      <td>2018-04-10 星期日</td>
      <td>0010039801328</td>
      <td>2367011</td>
      <td>开博通</td>
      <td>2</td>
      <td>56</td>
      <td>50</td>
    </tr>
    <tr>
      <th>6550</th>
      <td>2018-04-10 星期日</td>
      <td>0013437628</td>
      <td>2367011</td>
      <td>开博通</td>
      <td>1</td>
      <td>28</td>
      <td>25</td>
    </tr>
    <tr>
      <th>6551</th>
      <td>2018-04-12 星期二</td>
      <td>001616528</td>
      <td>2367011</td>
      <td>开博通</td>
      <td>1</td>
      <td>28</td>
      <td>28</td>
    </tr>
    <tr>
      <th>6552</th>
      <td>2018-04-13 星期三</td>
      <td>00101409528</td>
      <td>2367011</td>
      <td>开博通</td>
      <td>2</td>
      <td>56</td>
      <td>50</td>
    </tr>
    <tr>
      <th>6553</th>
      <td>2018-04-13 星期三</td>
      <td>0013406628</td>
      <td>2367011</td>
      <td>开博通</td>
      <td>2</td>
      <td>56</td>
      <td>50</td>
    </tr>
    <tr>
      <th>6554</th>
      <td>2018-04-14 星期四</td>
      <td>0010039287528</td>
      <td>2367011</td>
      <td>开博通</td>
      <td>2</td>
      <td>56</td>
      <td>50</td>
    </tr>
    <tr>
      <th>6555</th>
      <td>2018-04-15 星期五</td>
      <td>001006668328</td>
      <td>2367011</td>
      <td>开博通</td>
      <td>2</td>
      <td>56</td>
      <td>50</td>
    </tr>
    <tr>
      <th>6556</th>
      <td>2018-04-15 星期五</td>
      <td>0010018771328</td>
      <td>2367011</td>
      <td>开博通</td>
      <td>2</td>
      <td>56</td>
      <td>49.28</td>
    </tr>
    <tr>
      <th>6557</th>
      <td>2018-04-15 星期五</td>
      <td>0010028164128</td>
      <td>2367011</td>
      <td>心痛定</td>
      <td>2</td>
      <td>89.6</td>
      <td>79.6</td>
    </tr>
    <tr>
      <th>6558</th>
      <td>2018-04-15 星期五</td>
      <td>0010083726928</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>2</td>
      <td>11.2</td>
      <td>9.86</td>
    </tr>
    <tr>
      <th>6559</th>
      <td>2018-04-16 星期六</td>
      <td>0010035539928</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>2</td>
      <td>11.2</td>
      <td>9.86</td>
    </tr>
    <tr>
      <th>6560</th>
      <td>2018-04-17 星期日</td>
      <td>0011177328</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>2</td>
      <td>11.2</td>
      <td>9.86</td>
    </tr>
    <tr>
      <th>6561</th>
      <td>2018-04-18 星期一</td>
      <td>0010018771328</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>1</td>
      <td>5.6</td>
      <td>4.93</td>
    </tr>
    <tr>
      <th>6562</th>
      <td>2018-04-21 星期四</td>
      <td>0011137628</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>2</td>
      <td>11.2</td>
      <td>10</td>
    </tr>
    <tr>
      <th>6563</th>
      <td>2018-04-22 星期五</td>
      <td>0010018771328</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>1</td>
      <td>5.6</td>
      <td>5</td>
    </tr>
    <tr>
      <th>6564</th>
      <td>2018-04-24 星期日</td>
      <td>0010073294128</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>1</td>
      <td>5.6</td>
      <td>5.6</td>
    </tr>
    <tr>
      <th>6565</th>
      <td>2018-04-25 星期一</td>
      <td>0010019172628</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>1</td>
      <td>5.6</td>
      <td>5</td>
    </tr>
    <tr>
      <th>6566</th>
      <td>2018-04-25 星期一</td>
      <td>0010019192628</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>3</td>
      <td>16.8</td>
      <td>15.46</td>
    </tr>
    <tr>
      <th>6567</th>
      <td>2018-04-25 星期一</td>
      <td>0010039350528</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>2</td>
      <td>11.2</td>
      <td>9.86</td>
    </tr>
    <tr>
      <th>6568</th>
      <td>2018-04-26 星期二</td>
      <td>0010052558628</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>2</td>
      <td>11.2</td>
      <td>10</td>
    </tr>
    <tr>
      <th>6569</th>
      <td>2018-04-26 星期二</td>
      <td>00108945828</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>2</td>
      <td>11.2</td>
      <td>10</td>
    </tr>
    <tr>
      <th>6570</th>
      <td>2018-04-27 星期三</td>
      <td>0010060482828</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>1</td>
      <td>5.6</td>
      <td>5</td>
    </tr>
    <tr>
      <th>6571</th>
      <td>2018-04-27 星期三</td>
      <td>00107886128</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>10</td>
      <td>56</td>
      <td>54.8</td>
    </tr>
    <tr>
      <th>6572</th>
      <td>2018-04-27 星期三</td>
      <td>0010087865628</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>2</td>
      <td>11.2</td>
      <td>9.86</td>
    </tr>
    <tr>
      <th>6573</th>
      <td>2018-04-27 星期三</td>
      <td>0013406628</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>1</td>
      <td>5.6</td>
      <td>5</td>
    </tr>
    <tr>
      <th>6574</th>
      <td>2018-04-28 星期四</td>
      <td>0011926928</td>
      <td>2367011</td>
      <td>高特灵</td>
      <td>2</td>
      <td>11.2</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
<p>6575 rows × 7 columns</p>
</div>



# 5.数据一致化


```python
#将销售数据进行截取
'''
定义函数：分割销售日期，获取销售日期
输入：timeColSer 销售时间这一列，是个Series数据类型
输出：分割后的时间，返回也是个Series数据类型
'''
def splitSaletime(timeColSer):
    timeList=[]
    for value in timeColSer:
        dateStr=value.split(' ')[0]
        timeList.append(dateStr)     
    #将列表转行为一维数据Series类型
        timeSer=pd.Series(timeList)
    return timeSer
```


```python
#获取“销售时间”这一列
timeSer=Saledf.loc[:,'销售时间']
#对字符串进行分割，获取销售日期
dateSer=splitSaletime(timeSer)
```


```python
#将修改后的值赋值给销售时间
Saledf.loc[:,'销售时间']=dateSer.values
Saledf.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>销售时间</th>
      <th>社保卡号</th>
      <th>商品编码</th>
      <th>商品名称</th>
      <th>销售数量</th>
      <th>应收金额</th>
      <th>实收金额</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-01</td>
      <td>001616528</td>
      <td>236701</td>
      <td>强力VC银翘片</td>
      <td>6</td>
      <td>82.8</td>
      <td>69</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-02</td>
      <td>001616528</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>1</td>
      <td>28</td>
      <td>24.64</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-06</td>
      <td>0012602828</td>
      <td>236701</td>
      <td>感康</td>
      <td>2</td>
      <td>16.8</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-11</td>
      <td>0010070343428</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>1</td>
      <td>28</td>
      <td>28</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-15</td>
      <td>00101554328</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>8</td>
      <td>224</td>
      <td>208</td>
    </tr>
  </tbody>
</table>
</div>




```python
#将销售时间修改为日期类型
'''
数据类型转换:字符串转换为日期
'''
#errors='coerce' 如果原始数据不符合日期的格式，转换后的值为空值NaT
#format 是原始数据中日期的格式
Saledf.loc[:,'销售时间']=pd.to_datetime(Saledf.loc[:,'销售时间'],format='%Y-%m-%d',errors='coerce')
```


```python
Saledf.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>销售时间</th>
      <th>社保卡号</th>
      <th>商品编码</th>
      <th>商品名称</th>
      <th>销售数量</th>
      <th>应收金额</th>
      <th>实收金额</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-01</td>
      <td>001616528</td>
      <td>236701</td>
      <td>强力VC银翘片</td>
      <td>6</td>
      <td>82.8</td>
      <td>69</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-02</td>
      <td>001616528</td>
      <td>236701</td>
      <td>清热解毒口服液</td>
      <td>1</td>
      <td>28</td>
      <td>24.64</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-06</td>
      <td>0012602828</td>
      <td>236701</td>
      <td>感康</td>
      <td>2</td>
      <td>16.8</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-11</td>
      <td>0010070343428</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>1</td>
      <td>28</td>
      <td>28</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-15</td>
      <td>00101554328</td>
      <td>236701</td>
      <td>三九感冒灵</td>
      <td>8</td>
      <td>224</td>
      <td>208</td>
    </tr>
  </tbody>
</table>
</div>




```python
Saledf.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 6575 entries, 0 to 6574
    Data columns (total 7 columns):
    销售时间    6552 non-null datetime64[ns]
    社保卡号    6575 non-null object
    商品编码    6575 non-null object
    商品名称    6575 non-null object
    销售数量    6575 non-null object
    应收金额    6575 non-null object
    实收金额    6575 non-null object
    dtypes: datetime64[ns](1), object(6)
    memory usage: 359.6+ KB
    


```python
'''
转换日期过程中不符合日期格式的数值会被转换为空值，
这里删除列（销售时间，社保卡号）中为空的行
'''
Saledf.dropna(subset=['销售时间','社保卡号'],how='any',inplace=True)
Saledf.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 6552 entries, 0 to 6574
    Data columns (total 7 columns):
    销售时间    6552 non-null datetime64[ns]
    社保卡号    6552 non-null object
    商品编码    6552 non-null object
    商品名称    6552 non-null object
    销售数量    6552 non-null object
    应收金额    6552 non-null object
    实收金额    6552 non-null object
    dtypes: datetime64[ns](1), object(6)
    memory usage: 409.5+ KB
    


```python
#将销售数量、应收金额、实收金额转化为浮点型
Saledf['销售数量']=Saledf['销售数量'].astype(float)
Saledf['应收金额']=Saledf['应收金额'].astype(float)
Saledf['实收金额']=Saledf['实收金额'].astype(float)
Saledf.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 6552 entries, 0 to 6574
    Data columns (total 7 columns):
    销售时间    6552 non-null datetime64[ns]
    社保卡号    6552 non-null object
    商品编码    6552 non-null object
    商品名称    6552 non-null object
    销售数量    6552 non-null float64
    应收金额    6552 non-null float64
    实收金额    6552 non-null float64
    dtypes: datetime64[ns](1), float64(3), object(3)
    memory usage: 409.5+ KB
    

# 6.数据排序


```python
#按销售日期进行升序排列
'''
by：按哪几列排序
ascending=True 表示升序排列，
ascending=False表示降序排列
na_position='first'表示排序的时候，把空值放到前列，这样可以比较清晰的看到哪些地方有空值
官网文档：https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.sort_values.html
'''
Saledf=Saledf.sort_values(by='销售时间',ascending=True,na_position='first')
Saledf.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>销售时间</th>
      <th>社保卡号</th>
      <th>商品编码</th>
      <th>商品名称</th>
      <th>销售数量</th>
      <th>应收金额</th>
      <th>实收金额</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-01</td>
      <td>001616528</td>
      <td>236701</td>
      <td>强力VC银翘片</td>
      <td>6.0</td>
      <td>82.8</td>
      <td>69.0</td>
    </tr>
    <tr>
      <th>1475</th>
      <td>2018-01-01</td>
      <td>00107891628</td>
      <td>861456</td>
      <td>酒石酸美托洛尔片(倍他乐克)</td>
      <td>2.0</td>
      <td>14.0</td>
      <td>12.6</td>
    </tr>
    <tr>
      <th>1306</th>
      <td>2018-01-01</td>
      <td>001616528</td>
      <td>861417</td>
      <td>雷米普利片(瑞素坦)</td>
      <td>1.0</td>
      <td>28.5</td>
      <td>28.5</td>
    </tr>
    <tr>
      <th>3859</th>
      <td>2018-01-01</td>
      <td>0010073966328</td>
      <td>866634</td>
      <td>硝苯地平控释片(欣然)</td>
      <td>6.0</td>
      <td>111.0</td>
      <td>92.5</td>
    </tr>
    <tr>
      <th>3888</th>
      <td>2018-01-01</td>
      <td>0010014289328</td>
      <td>866851</td>
      <td>缬沙坦分散片(易达乐)</td>
      <td>1.0</td>
      <td>26.0</td>
      <td>23.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#重命名行名(index):重新排序后的索引值是之前的行号，需要修改成从0到N按顺序的索引值
Saledf=Saledf.reset_index(drop=True)
Saledf.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>销售时间</th>
      <th>社保卡号</th>
      <th>商品编码</th>
      <th>商品名称</th>
      <th>销售数量</th>
      <th>应收金额</th>
      <th>实收金额</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-01</td>
      <td>001616528</td>
      <td>236701</td>
      <td>强力VC银翘片</td>
      <td>6.0</td>
      <td>82.8</td>
      <td>69.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-01</td>
      <td>00107891628</td>
      <td>861456</td>
      <td>酒石酸美托洛尔片(倍他乐克)</td>
      <td>2.0</td>
      <td>14.0</td>
      <td>12.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-01</td>
      <td>001616528</td>
      <td>861417</td>
      <td>雷米普利片(瑞素坦)</td>
      <td>1.0</td>
      <td>28.5</td>
      <td>28.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-01</td>
      <td>0010073966328</td>
      <td>866634</td>
      <td>硝苯地平控释片(欣然)</td>
      <td>6.0</td>
      <td>111.0</td>
      <td>92.5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-01</td>
      <td>0010014289328</td>
      <td>866851</td>
      <td>缬沙坦分散片(易达乐)</td>
      <td>1.0</td>
      <td>26.0</td>
      <td>23.0</td>
    </tr>
  </tbody>
</table>
</div>



# 7.异常值处理


```python
Saledf.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>销售数量</th>
      <th>应收金额</th>
      <th>实收金额</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>6552.000000</td>
      <td>6552.00000</td>
      <td>6552.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.384158</td>
      <td>50.43025</td>
      <td>46.266972</td>
    </tr>
    <tr>
      <th>std</th>
      <td>2.374754</td>
      <td>87.68075</td>
      <td>81.043956</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-10.000000</td>
      <td>-374.00000</td>
      <td>-374.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.000000</td>
      <td>14.00000</td>
      <td>12.320000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.000000</td>
      <td>28.00000</td>
      <td>26.500000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2.000000</td>
      <td>59.60000</td>
      <td>53.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>50.000000</td>
      <td>2950.00000</td>
      <td>2650.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#删除异常值：通过条件判断筛选出数据
#查询条件
querySer=Saledf.loc[:,'销售数量']>0
#应用查询条件
print('删除异常值前：',Saledf.shape)
Saledf=Saledf.loc[querySer,:]
print('删除异常值后：',Saledf.shape)
```

    删除异常值前： (6552, 7)
    删除异常值后： (6509, 7)
    

# 7.数据建模

** (1)月均消费次数 **


```python
#月均消费次数
#月均消费次数=总消费次数/月份数 其中总消费次数中同一天内同一人的消费算做一次消费
#总消费次数通过.drop_duplicates 删除重复数据
kpi1_Df=Saledf.drop_duplicates(subset=['销售时间','社保卡号'])
totalI=kpi1_Df.shape[0]
print('总消费次数=',totalI)
```

    总消费次数= 5345
    


```python
#消费月份数
Saledf['销售时间'].max()
```




    Timestamp('2018-07-19 00:00:00')




```python
Saledf['销售时间'].min()
```




    Timestamp('2018-01-01 00:00:00')




```python
#按销售时间升序排序
kpi1_Df=kpi1_Df.sort_values(by='销售时间',ascending=True)
#重命名列名（index）
kpi1_Df.reset_index(drop=True,inplace=True)
kpi1_Df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>销售时间</th>
      <th>社保卡号</th>
      <th>商品编码</th>
      <th>商品名称</th>
      <th>销售数量</th>
      <th>应收金额</th>
      <th>实收金额</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-01</td>
      <td>001616528</td>
      <td>236701</td>
      <td>强力VC银翘片</td>
      <td>6.0</td>
      <td>82.8</td>
      <td>69.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-01</td>
      <td>0013448228</td>
      <td>861507</td>
      <td>苯磺酸氨氯地平片(安内真)</td>
      <td>1.0</td>
      <td>9.5</td>
      <td>8.5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-01</td>
      <td>0012697828</td>
      <td>861464</td>
      <td>复方利血平片(复方降压片)</td>
      <td>4.0</td>
      <td>10.0</td>
      <td>9.4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-01</td>
      <td>0010616728</td>
      <td>865099</td>
      <td>硝苯地平片(心痛定)</td>
      <td>2.0</td>
      <td>3.4</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-01</td>
      <td>0010060654328</td>
      <td>861458</td>
      <td>复方利血平氨苯蝶啶片(北京降压0号)</td>
      <td>1.0</td>
      <td>10.3</td>
      <td>9.2</td>
    </tr>
  </tbody>
</table>
</div>




```python
#销售时间最大值
startTime=kpi1_Df.loc[0,'销售时间']
startTime
```




    Timestamp('2018-01-01 00:00:00')




```python
#销售时间最小值
endTime=kpi1_Df.loc[totalI-1,'销售时间']
endTime
```




    Timestamp('2018-07-19 00:00:00')




```python
#计算天数
daysI=(endTime-startTime).days
#月份数：运算符“//”表示取整除
#返回商的整数部分，例如9//2输出的结果是4
monthsI=daysI//30
print('月份数：',monthsI)
```

    月份数： 6
    


```python
#月消费次数
kpi1_I=totalI//monthsI
print('业务指标1：月均消费次数',kpi1_I )
```

    业务指标1：月均消费次数 890
    

**(2)月均消费金额**


```python
#月均消费金额=总消费金额/月份数
#总消费金额
totalMoneyF=Saledf.loc[:,'实收金额'].sum()
monthMoneyF=totalMoneyF/monthsI
print('业务指标2：月均消费金额=',monthMoneyF)
```

    业务指标2：月均消费金额= 50672.494999999624
    

**(3)客单价**


```python
#客单总消费金额/总消费次数
'''
totalMoneyF 为总消费金额
totalI 为总消费次数

'''
pct=totalMoneyF/totalI 
print('业务指标3：客单价',pct)
```

    业务指标3：客单价 56.882127221702106
    
