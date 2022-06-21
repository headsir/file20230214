# Pandas 基础

## 一、Pandas 数据结构

### 1.1 Series 数据结构

```mermaid
graph LR
	id0((<font color = red size = 6>Series <br>数据结构</font>))
	id0-->id1
	id0-->id8
	id0-->id10
	id1[创建 Series]
		id2(传入列表)
			id4(数据标签默认从0开始)
			id5(指定索引,通过设置 index 参数自定义索引)			
		id3(传入字典)
			id6(key 是数据标签)
			id7(value 是数据值)
		
        id1 -->id2
            id2 -.- id4
            id2 -.- id5
        id1 -->id3
            id3 -.-id6
            id3 -.-id7
	id8[获取 Series 索引]-->id9(直接利用 index 方法获取 Series 索引值)
	id10[获取 Series 值]-->id11(直接利用 values 方法获取 Series 索引值)
	
```



#### 1.1.1 创建 Series

- 传入列表

  - 数据标签默认从0开始

    ```
    import pandas as pd
    s1 = pd.Series(["a","b","c","d"])
    print(s1)
    
    0    a
    1    b
    2    c
    3    d
    dtype: object
    ```

  - 指定索引,通过设置 index 参数自定义索引

    ```python
    import pandas as pd
    s2 = pd.Series([1,2,3,4],index = ["a","b","c","d"])
    print(s1)
    
    a    1
    b    2
    c    3
    d    4
    dtype: int64
    ```
    

- 传入字典

  字典的 key 值是数据标签，value 是数据值
  ```python
  import pandas as pd
  s3 = pd.Series({"a":1,"b":2,"c":3,"d":4})
  print(s3)
  
  a    1
  b    2
  c    3
  d    4
  dtype: int64
  ```

#### 1.1.2 获取 Series 索引

直接利用 index 方法获取 Series 索引值

```
s1.index

RangeIndex(start=0, stop=4, step=1)
```

```
s2.index

Index(['a', 'b', 'c', 'd'], dtype='object')
```

####   1.1.3  获取 Series 值

直接利用 values 方法获取 Series 索引值

```
s1.values

array(['a', 'b', 'c', 'd'], dtype=object)
```

```
s2.values

array([1, 2, 3, 4], dtype=int64)
```

### 1.2 DataFrame 表格型数据结构

```mermaid
graph LR
	id0((<font color = red size = 6>DataFrame <br>表格型数据结构</font>))
	id0-->id1
	id0-->id8
	id0-->id10
	id1[创建 DataFrame]
		id2(传入列表)
			id2_3(传入列表)
			id2_4(传入嵌套列表)
			id2_5(传入嵌套元组)	
            id2_6("指定行、列索引")
		id3(传入字典)
			id6(字典 key 值相当于列索引)
			id7(使用 index 参数自定义行索引)
		
        id1 -->id2
        	id2 -.- id2_3
            id2 -.- id2_4
            id2 -.- id2_5
            id2 -.- id2_6
        id1 -->id3
            id3 -.-id6
            id3 -.-id7
	id8["获取 DataFrame 行、列索引"]-->id9("1、利用 columns 方法获取 DataFrame 列索引 
									<br> 2、利用 index 方法获取 DataFrame 行索引")
	id10[获取 DataFrame 值]-->id11("详见后面【数据选择】")
	
```



#### 1.2.1 创建 DataFrame

- 传入列表

  ```
  import pandas as pd
  df1 = pd.DataFrame(["a","b","c","d"])
  print(df1)
  
     0
  0  a
  1  b
  2  c
  3  d
  ```

  - 传入嵌套列表

    ```
    df2 = pd.DataFrame([["a","A"],["b","B"],["c","C"],["d","D"]])
    print(df2)
    
       0  1
    0  a  A
    1  b  B
    2  c  C
    3  d  D
    ```

    

  - 传入嵌套元组

    ```
    df3 = pd.DataFrame([("a","A"),("b","B"),("c","C"),("d","D")])
    print(df3)
    
       0  1
    0  a  A
    1  b  B
    2  c  C
    3  d  D
    ```

  - 指定行、列索引

    通过设置 columns 参数自定义列索引，设置 index 参数自定义行索引

    ```
    # 设置列索引
    df31 = pd.DataFrame([["a","A"],["b","B"],["c","C"],["d","D"]], columns = ["小写","大写"])
    print(df31)
    
      小写 大写
    0  a  A
    1  b  B
    2  c  C
    3  d  D
    ```

    ```
    # 设置行索引
    df32 = pd.DataFrame([["a","A"],["b","B"],["c","C"],["d","D"]], index = ["一","二","三", "四"])
    print(df32)
    
       0  1
    一  a  A
    二  b  B
    三  c  C
    四  d  D
    ```

    ```
    # 行、列索引同时设置
    df33 = pd.DataFrame([["a","A"],["b","B"],["c","C"],["d","D"]], columns = ["小写","大写"],index = ["一","二","三", "四"])
    
    print(df33)
      小写 大写
    一  a  A
    二  b  B
    三  c  C
    四  d  D
    ```

- 传入字典

  默认字典 key 值相当于列索引，可以利用 from_dict 函数中参数 orient 设置，默认 orient = 'columns'

  ```
  data ={"小写":["a","b","c","d"] , "大写" : ["A","B","C","D"]}
  df41 = pd.DataFrame(data)
  print(df41)
  
    小写 大写
  0  a  A
  1  b  B
  2  c  C
  3  d  D
  ```

  - 使用 index 参数自定义行索引

    ```
    data ={"小写":["a","b","c","d"] , "大写" : ["A","B","C","D"]}
    df42 = pd.DataFrame(data , index = ["一","二","三", "四"])
    print(df42)
    
      小写 大写
    一  a  A
    二  b  B
    三  c  C
    四  d  D
    ```

#### 1.2.2 获取 DataFrame 行、列索引

- 利用 columns 方法获取 DataFrame 列索引

  ```
  df2.columns
  
  RangeIndex(start=0, stop=2, step=1)
  ```

  ```
  df33.columns
  
  Index(['小写', '大写'], dtype='object')
  ```

- 利用 index 方法获取 DataFrame 行索引

  ```
  df2.index
  
  RangeIndex(start=0, stop=4, step=1)
  ```

  ```
  df33.index
  
  Index(['一', '二', '三', '四'], dtype='object')
  ```

#### 1.2.3  获取 DataFrame 值

​		详见后面【数据选择】内容

## 二、Pandas 获取数据

### 2.1 导入外部数据

主要用到 Pandas 里面的 read_x() 方法，x 表示导入文件格式

```mermaid
graph LR
	id2_1((<font color = red size = 6>导入<br>外部数据</font>))
		id2_1_1["导入 .xlsx 文件 <br> read_excel()"]
			id2_1_1-1(基本导入)
			id2_1_1-2(指定 sheet 导入:sheet_name)
			id2_1_1-3(指定行索引:index_col)
			id2_1_1-4(指定列索引:header)
			id2_1_1-5(指定导入列:usecols)
		id2_1_2["导入 .csv 文件 <br> read_csv()"]
			id2_1_2-1(直接导入)
			id2_1_2-2(指明分隔符:sep)
			id2_1_2-3(指明读取行数:nrows)
			id2_1_2-4(指明编码格式:encoding)
			id2_1_2-5(指定解析语言:engine)
			id2_1_2-6("其它:涉及行、列索引设置及指定导入某列，设定方法与导入 .xlsx 文件一致")
		id2_1_3["导入 .txt 文件 <br> read_table()"]
			id2_1_3-1("是将利用分隔符分开的文件导入的通用函数，还可以导入 .csv 文件，必须用sep指明分隔符。")
		id2_1_4["导入SQL文件 <br> read_sql()"]
			id2_1_4-1("利用 sqlalchemy 模块连接数据库")
			
		
	id2_1-->id2_1_1 
        id2_1_1-.->id2_1_1-1
        id2_1_1-.->id2_1_1-2
        id2_1_1-.->id2_1_1-3
        id2_1_1-.->id2_1_1-4
        id2_1_1-.->id2_1_1-5
     id2_1-->id2_1_2 
     	id2_1_2 -.->id2_1_2-1
     	id2_1_2 -.->id2_1_2-2
     	id2_1_2 -.->id2_1_2-3
     	id2_1_2 -.->id2_1_2-4
     	id2_1_2 -.->id2_1_2-5
     	id2_1_2 -.->id2_1_2-6
     id2_1-->id2_1_3
     	id2_1_3-.->id2_1_3-1
     id2_1-->id2_1_4
     	id2_1_4-.->id2_1_4-1
```



#### 2.1.1 导入 .xlsx 文件

导入方法：read_excel()

- 基本导入

  ```python
  import pandas as pd
  df = pd.read_excel(r"C:\Users\WT\数据分析20220612\对比Excel,轻松学习Python数据分析数据集\郑州电信FDDLTE基站信息表20220318.xlsx")
  print(df)
  
         地市  厂家         ECI  PLMN  MCC  MNC   制式    簇        子网  ENodeBID  ...  \
  0      郑州  中兴   979168-18     1  460   11  FDD   31  410133.0    979168  ...   
  1      郑州  中兴   979168-17     1  460   11  FDD   31  410133.0    979168  ...   
  2      郑州  中兴   979168-16     1  460   11  FDD   31  410133.0    979168  ...   
  3      郑州  中兴   979166-18     1  460   11  FDD   50  410133.0    979166  ...   
  4      郑州  中兴   979166-17     1  460   11  FDD   50  410133.0    979166  ...   
  
  [43503 rows x 64 columns]
  ```

- 指定 sheet 导入

  通过设置 sheet_name 参数指定导入特定 sheet 表

  ```python
  df1 = pd.read_excel(r"C:\Users\WT\数据分析20220612\对比Excel,轻松学习Python数据分析数据集\郑州电信FDDLTE基站信息表20220318.xlsx" , sheet_name = "子网")
  print(df1)
  
       城区        子网   城区.1  tac1    11264  Unnamed: 5    1    S1  Unnamed: 8  \
  0   中原区  410102.0    中原区  tac2  11265.0         NaN  2.0   S11         NaN   
  1   二七区  410103.0    二七区  tac3  11266.0         NaN  3.0  S111         NaN   
  2   管城区  410104.0  管城回族区  tac4  11267.0         NaN  4.0  S211         NaN   
  3   金水区  410105.0    金水区  tac5  11268.0         NaN  5.0  S221         NaN   
  4   上街区  410106.0    上街区  tac6  11269.0         NaN  6.0  S222         NaN   
  
  [70 rows x 18 columns]
  ```

- 指定行索引

  通过设置 index_col 参数指定特定行索引

  ```python
  df3 = pd.read_excel(r"C:\Users\WT\数据分析20220612\对比Excel,轻松学习Python数据分析数据集\郑州电信FDDLTE基站信息表20220318.xlsx" , index_col = 0)
  print(df3)
      厂家         ECI  PLMN  MCC  MNC   制式    簇        子网  ENodeBID   行政区域  ...  \
  地市                                                                       ...   
  郑州  中兴   979168-18     1  460   11  FDD   31  410133.0    979168    金水区  ...   
  郑州  中兴   979168-17     1  460   11  FDD   31  410133.0    979168    金水区  ...   
  郑州  中兴   979168-16     1  460   11  FDD   31  410133.0    979168    金水区  ...   
  郑州  中兴   979166-18     1  460   11  FDD   50  410133.0    979166    金水区  ...   
  郑州  中兴   979166-17     1  460   11  FDD   50  410133.0    979166    金水区  ...   
  
  [43503 rows x 63 columns]
  ```

  

- 指定列索引

  通过设置 header 参数指定特定列索引

  ```
  # 使用第一行作为列索引
  df3 = pd.read_excel(r"C:\Users\WT\数据分析20220612\对比Excel,轻松学习Python数据分析数据集\郑州电信FDDLTE基站信息表20220318.xlsx" , header = 0)
  
  print(df3)  
         地市  厂家         ECI  PLMN  MCC  MNC   制式    簇        子网  ENodeBID  ...  \
  0      郑州  中兴   979168-18     1  460   11  FDD   31  410133.0    979168  ...   
  1      郑州  中兴   979168-17     1  460   11  FDD   31  410133.0    979168  ...   
  2      郑州  中兴   979168-16     1  460   11  FDD   31  410133.0    979168  ...   
  3      郑州  中兴   979166-18     1  460   11  FDD   50  410133.0    979166  ...   
  4      郑州  中兴   979166-17     1  460   11  FDD   50  410133.0    979166  ...   
  
  [43503 rows x 64 columns]
  ```

  ```
  # 使用第二行作为列索引
  df4 = pd.read_excel(r"C:\Users\WT\数据分析20220612\对比Excel,轻松学习Python数据分析数据集\郑州电信FDDLTE基站信息表20220318.xlsx" , header = 2)
  
  print(df4)
         郑州  中兴   979168-17  1  460  11  FDD   31    410133  979168  ...  \
  0      郑州  中兴   979168-16  1  460  11  FDD   31  410133.0  979168  ...   
  1      郑州  中兴   979166-18  1  460  11  FDD   50  410133.0  979166  ...   
  2      郑州  中兴   979166-17  1  460  11  FDD   50  410133.0  979166  ...   
  3      郑州  中兴   979166-16  1  460  11  FDD   50  410133.0  979166  ...   
  4      郑州  中兴   979790-16  1  460  11  FDD   16  410129.0  979790  ...   
  
  [43501 rows x 64 columns]
  ```

  ```
  # 使用默认从0开始的数作为列索引
  df5 = pd.read_excel(r"C:\Users\WT\数据分析20220612\对比Excel,轻松学习Python数据分析数据集\郑州电信FDDLTE基站信息表20220318.xlsx" , header = None)
  print(df5)  
         0   1           2     3    4    5    6    7       8         9   ...  \
  0      地市  厂家         ECI  PLMN  MCC  MNC   制式    簇      子网  ENodeBID  ...   
  1      郑州  中兴   979168-18     1  460   11  FDD   31  410133    979168  ...   
  2      郑州  中兴   979168-17     1  460   11  FDD   31  410133    979168  ...   
  3      郑州  中兴   979168-16     1  460   11  FDD   31  410133    979168  ...   
  4      郑州  中兴   979166-18     1  460   11  FDD   50  410133    979166  ...   
  
  [43504 rows x 64 columns]
  ```

- 指定导入列

  通过设置 usecols 参数指定导入列

  ```
  df6 = pd.read_excel(r"C:\Users\WT\数据分析20220612\对比Excel,轻松学习Python数据分析数据集\郑州电信FDDLTE基站信息表20220318.xlsx" , usecols = ["ECI"])
  print(df6)  
                ECI
  0       979168-18
  1       979168-17
  2       979168-16
  3       979166-18
  4       979166-17
  ...           ...
  43498    180998-5
  43499  741475-179
  43500  741475-180
  43501  741475-181
  43502  742530-178
  
  [43503 rows x 1 columns]
  ```

#### 2.1.2 导入 .csv 文件

导入方法：read_csv()

- 直接导入

  ```
  import pandas as pd
  df = pd.read_csv(r"C:\Users\WT\数据分析20220612\对比Excel,轻松学习Python数据分析数据集\loan.csv")
  print(df)
  
            用户ID  好坏客户  年龄          负债率      月收入  家属数量
  0            1     1  45     0.802982   9120.0   2.0
  1            2     0  40     0.121876   2600.0   1.0
  2            3     0  38     0.085113   3042.0   0.0
  3            4     0  30     0.036050   3300.0   0.0
  4            5     0  49     0.024926  63588.0   0.0
  
  [150000 rows x 6 columns]
  ```

- 指明分隔符

  read_csv()默认文件中的数据是以逗号分开的，但是有的文件不是用逗号分开的，会报错。

  通过 sep 参数指定分隔符，常见的分隔符除了逗号还有空格、制表符（\t）

- 指明读取行数

  通过设置 nrows 参数指明读取前 nrows 行。

- 指定编码格式

  通过设置 enc0ding 参数指明文件编码格式，常见有 UTF-8、gbk

- 指定解析语言

  默认使用C语言作为解析语言，当文件路径路经或者文件名包含中文时，默认方式可能会报错。

  通过设置 engine 参数指明解析语言为 Python 可解决。

- 其它

  涉及行、列索引设置及指定导入某列，设定方法与导入 .xlsx 文件一致。

#### 2.1.3 导入 .txt 文件

导入方法：read_table()

是将利用分隔符分开的文件导入的通用函数，还可以导入 .csv 文件，必须用sep指明分隔符。

#### 2.1.4 导入SQL文件

导入方法：read_read_sql()

利用 sqlalchemy 模块连接数据库

```
from sqlalchemy import create_engine
import pandas as pd

# 创建数据库连接,mysql用户名是root，密码是qazwsx，本地的数据库服务是localhost,数据库的名称试验库,数据库编码utf8
engine = create_engine('mysql+pymysql://root:qazwsx@localhost/试验库?charset=utf8')

# 读取mysql数据,使用pandas的read_sql()查询mysql表department中的数据
db = pd.read_sql(sql='select * from 试验库.information_bbu', con=engine)
print(db)

   序号      子网       网元                     网元名称     机房名称  
0      1  410117  4849673  5AZYZOSFF0009建设IDC机房06D  建设IDC机房   
1      2  410117  4849683    5AZYZOSFF0013建设威科姆0ED    建设威科姆   
2      3  410117  4849698  5AZYZOSFF0022建设谦祥万和城01D  建设谦祥万和城   
3      4  410117  4849706    5AZYZOSFF002A建设威科姆01D    建设威科姆   
4      5  410117  4849699  5AZYZOSFF0023建设天健湖机房13D  建设天健湖机房   

[740 rows x 12 columns]
```

### 2.2 了解数据

只有对数据充分熟悉，才能更好分析。

```mermaid
graph LR
	id2_2((<font color = red size = 6>了解数据</font>))
		id2_2_1(head 预览前几行)
		id2_2_2(shape 获取数据表的大小)
		id2_2_3(info 获取数据类型)
		id2_2_4(describe 获取数值分布情况)
	
	id2_2 --> id2_2_1
	id2_2 --> id2_2_2
	id2_2 --> id2_2_3
	id2_2 --> id2_2_4
```



#### 2.2.1  head 预览前几行

通过 head() 方法控制显示行数，默认显示前5行。

```
db.head()

序号	子网	网元	网元名称	机房名称	5G业务IP电信	5G业务IP联通	BBU经度	BBU纬度	软件版本	备注	更新时间
0	1	410117	4849673	5AZYZOSFF0009建设IDC机房06D	建设IDC机房	240E:0183:C00C:0000:0200::42BA	2408:8160:C100:0010:0200::42BA	113.535754	34.803838	V5.45.20.20	None	2022-05-12 10:14:13
1	2	410117	4849683	5AZYZOSFF0013建设威科姆0ED	建设威科姆	240E:0183:C00C:0000:0200::4206	2408:8160:C100:0010:0200::4206	113.556622	34.824446	V5.45.20.20	None	2022-05-12 10:14:13
2	3	410117	4849698	5AZYZOSFF0022建设谦祥万和城01D	建设谦祥万和城	240E:0183:C00C:0000:0200::416A	2408:8160:C100:0010:0200::416A	113.534413	34.833985	V5.45.20.20	None	2022-05-12 10:14:13
3	4	410117	4849706	5AZYZOSFF002A建设威科姆01D	建设威科姆	240E:0183:C00C:0000:0200::4212	2408:8160:C100:0010:0200::4212	113.556624	34.824469	V5.45.20.20	None	2022-05-12 10:14:13
4	5	410117	4849699	5AZYZOSFF0023建设天健湖机房13D	建设天健湖机房	240E:0183:C00C:0000:0200::415E	2408:8160:C100:0010:0200::415E	113.496189	34.814425	V5.45.20.20	None	2022-05-12 10:14:13
```

#### 2.2.2  shape 获取数据表的大小

通过 shape() 方法获取数据表的行、列数量,不会统计行索引和列索引。

```
db.shape

(740, 12)
```

#### 2.2.3  info 获取数据类型

获取每一列数据类型，不同数据类型分析思路不一样。

```
db.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 740 entries, 0 to 739
Data columns (total 12 columns):
 #   Column    Non-Null Count  Dtype         
---  ------    --------------  -----         
 0   序号        740 non-null    int64         
 1   子网        740 non-null    object        
 2   网元        740 non-null    object        
 3   网元名称      740 non-null    object        
 4   机房名称      740 non-null    object        
 5   5G业务IP电信  740 non-null    object        
 6   5G业务IP联通  740 non-null    object        
 7   BBU经度     740 non-null    object        
 8   BBU纬度     740 non-null    object        
 9   软件版本      740 non-null    object        
 10  备注        0 non-null      object        
 11  更新时间      740 non-null    datetime64[ns]
dtypes: datetime64[ns](1), int64(1), object(10)
memory usage: 69.5+ KB
```

#### 2.2.4  describe 获取数值分布情况

数据的分布情况，即均值是多少，最值是多少，方差及分位数分别又是多少。

```
db.describe()

序号
count	740.000000
mean	370.535135
std	213.825046
min	1.000000
25%	185.750000
50%	370.500000
75%	555.250000
max	748.000000
```

## 三、Pandas 数据预处理

处理不规整数据，主要有缺失数据、重复数据、异常数据。

### 3.1 缺失值

某些原因导致部分数据为空，一般有两种处理方式：

- 删除：含有缺失值的数据删除
- 填充：缺失部分数据用某个值代替

#### 3.1.1 缺失值查看

- 通过 info() 方法查看，对比非 null 数量

- 通过 isnull() 方法查看，缺失值返回 True

  ```
  db.isnull()
  序号	子网	网元	网元名称	机房名称	5G业务IP电信	5G业务IP联通	BBU经度	BBU纬度	软件版本	备注	更新时间
  0	False	False	False	False	False	False	False	False	False	False	True	False
  1	False	False	False	False	False	False	False	False	False	False	True	False
  2	False	False	False	False	False	False	False	False	False	False	True	False
  3	False	False	False	False	False	False	False	False	False	False	True	False
  4	False	False	False	False	False	False	False	False	False	False	True	False
  
  740 rows × 12 columns
  ```

#### 3.1.2 缺失值删除

通过 dropna() 方法实现，默认删除含有缺失值的行，传入参数 how = "all" 只删除全为空值的行。

#### 3.1.3 缺失值填充

- 通过 fillna(“填充值”) 方法对数据表中所有缺失值填充，
- 通过 fillna({"列1" : "填充值" , "列2":"填充值"}) 方法指定特定列名，对特定列空值填充。

### 3.2 重复值

- 通过 drop_duplicates() 方法对所有值进行重复值判断，且保留第一个值
- 通过参数 subset = ["列1","列2"]指定特定列名进行重复值判断，且保留第一个值
- 通过参数 keep 定义保留哪一个值，默认 first  第一个值， last 最后一个值，False 全部删除。

### 3.3 异常值

异常值：相比正常数据而言过高或过低的数据

#### 3.3.1 异常值检测

- 经验：根据业务经验划定不同指标的正常范围，超过该范围的值算作异常值
- 箱型图：通过绘制箱型图，把大于（小于）箱型图上边缘（下边缘）的点称为异常值
- 服从正态分布：数据服从正态分布，可以利用3σ原则（一个数值与平均值之间的偏差超过3倍标准差，就认为这个值是异常值）

#### 3.3.2 异常值处理

- 最常用的处理方式就是删除
- 把异常值当作缺失值来填充
- 把异常值当作特殊情况，研究异常值出现的原因。

### 3.4、数据类型转换

#### 3.4.1 数据类型

| 类型           | 说明                                               |
| -------------- | -------------------------------------------------- |
| int            | 整型数，即整数                                     |
| float          | 浮点数，即含有小数点的数                           |
| object         | Python 对象类型，用 O 表示                         |
| string_        | 字符串类型，经常用 S 表示，S10表示长度为10的字符串 |
| unicode_       | 固定长度的 unicode 类型，跟字符串定义方式一样      |
| datetime64[ms] | 表示时间格式                                       |

info() 获取每一列数据类型，dtype() 获取某一列数据类型。

```python
df1.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 70 entries, 0 to 69
Data columns (total 18 columns):
 #   Column       Non-Null Count  Dtype  
---  ------       --------------  -----  
 0   城区           12 non-null     object 
 1   子网           17 non-null     float64
 2   城区.1         17 non-null     object 
 3   tac1         7 non-null      object 
 4   11264        7 non-null      float64
 5   Unnamed: 5   0 non-null      float64
 6   1            11 non-null     float64
 7   S1           11 non-null     object 
 8   Unnamed: 8   0 non-null      float64
 9   Unnamed: 9   0 non-null      float64
 10  0            70 non-null     int64  
 11  51           70 non-null     int64  
 12  Unnamed: 12  0 non-null      float64
 13  CELLID取值区间   9 non-null      float64
 14  Unnamed: 14  0 non-null      float64
 15  子网.1         29 non-null     float64
 16  OMMB         29 non-null     object 
 17  拓扑ID         29 non-null     object 
dtypes: float64(10), int64(2), object(6)
memory usage: 10.0+ KB
```

```python
df1["城区"].dtype

dtype('O')
```

#### 3.4.2 类型转换

利用 astype() 方法对数据类型进行转换。

```
df3["ENodeBID"].dtype

dtype('int64')
```

```
df3["ENodeBID"].astype("string")

0        979168
1        979168
2        979168
3        979166
4        979166
          ...  
43498    180998
43499    741475
43500    741475
43501    741475
43502    742530
Name: ENodeBID, Length: 43503, dtype: string
```

### 3.5 索引设置

索引是查找数据的依据，设置索引的目的是便于我们查找数据。

#### 3.5.1 为无索引表添加索引

没有索引，默认会从 0 开始的自然数做索引,通过 columns 、index 参数分别传入列索引、行索引。

```
    0   1    2          3
0  A1  张1  101 2022-05-06
1  A2  张2  102 2022-05-07
2  A3  张3  103 2022-05-08
3  A4  张4  104 2022-05-09
4  A5  张5  104 2022-05-10
```

```
df.columns = ["订单编号","客户姓名","唯一识别码","成交时间"]
df.index = [1,2,3,4,5]
print(df)
  
  订单编号 客户姓名  唯一识别码       成交时间
1   A1   张1    101 2022-05-06
2   A2   张2    102 2022-05-07
3   A3   张3    103 2022-05-08
4   A4   张4    104 2022-05-09
5   A5   张5    104 2022-05-10
```

#### 3.5.2 重新设置索引

一般指行索引的设置，利用 set_index() 方法重新设置索引。

```
df.set_index(["客户姓名","唯一识别码"])

					订单编号	成交时间
客户姓名	唯一识别码		
张1		101			A1	2022-05-06
张2		102			A2	2022-05-07
张1		103			A3	2022-05-08
张4		104			A4	2022-05-09
张1		104			A5	2022-05-10
```

层次化索引:一个表中用多列来做索引的方式

```
df = pd.read_excel(r"D:/桌面/3.xlsx" )
print(df)

print(df)
  Z1 Z2  C1  C2
0  A  a   1   2
1  A  b   3   4
2  B  a   5   6
3  B  b   7   8
```

```
df.set_index(["Z1","Z2"])

		C1	C2
Z1	Z2		
A	a	1	2
	b	3	4
B	a	5	6
	b	7	8
```

#### 3.5.3 重命名索引

利用 rename() 方法修改行索引及列索引名。

```
df.rename(columns = {"订单编号":"新订单编号","客户姓名":"新客户姓名"}, index = {1:"一",2:"二"})

新订单编号	新客户姓名	唯一识别码	成交时间
一	A1	张1	101	2022-05-06
二	A2	张2	102	2022-05-07
3	A3	张1	103	2022-05-08
4	A4	张4	104	2022-05-09
5	A5	张1	104	2022-05-10
```

#### 3.5.4 重置索引

主要用在层次化索引中，重置索引是将索引列当作一个 columns 进行返回。

利用 reset_index() 方法，`reset_index(level = None , drop = False , inplace = False)`

level参数：指定转化列，默认全部索引列

drop参数：指定是否删除原索引，默认不删除

inplace参数：指定是否修改原数据表

层次化索引:

```
df1 = df.set_index(["Z1","Z2"])
print(df1)

       C1  C2
Z1 Z2        
A  a    1   2
   b    3   4
B  a    5   6
   b    7   8

df1.reset_index()
Z1	Z2	C1	C2
0	A	a	1	2
1	A	b	3	4
2	B	a	5	6
3	B	b	7	8

df1.reset_index(level = 0)
	Z1	C1	C2
Z2			
a	A	1	2
b	A	3	4
a	B	5	6
b	B	7	8

df1.reset_index(drop = True)
	C1	C2
0	1	2
1	3	4
2	5	6
3	7	8
```

## 四、Pandas 数据选择

常规数据选择：列选择、行选择、行列同时选择

### 4.1 列选择

#### 4.1.1 选择某一列/某几列

- 普通索引：通过传入==列名==选择数据，举例：df[["列1" , "列2"]]

- 位置索引：通过传入==具体位置==来选择数据，利用 iloc 方法，举例：df.iloc[: , [0,2]] ，逗号前是行，后是列

#### 4.1.2 选择连续的某几列

普通索引、位置索引都可以，推荐使用位置索引,位置索引中使用==位置区间==又称为切片索引。举例：df.iloc[: , 0:3]

### 4.2 行选择

#### 4.2.1 选择某一行/某几行

- 普通索引：通过传入==行索引名==选择数据，利用 loc 方法，举例：df.loc[["行1" , "行2"]]
- 位置索引：通过传入具体==行序号==，利用 iloc 方法，举例：df.iloc[[0,2]]

#### 4.2.2 选择连续的某几行

切片索引：通过传入连续行的==位置区间==，利用 iloc 方法，举例：df.iloc[0:3]

#### 4.2.3 选择满足条件的行

布尔索引：通过传入一个或多个==判断条件==来选择数据，举例：df[(df["年龄"] < 200) & (df["列2"] < 102)]

### 4.3 行列同时选择

#### 4.3.1 普通索引 + 普通索引

同时传入行和列的索引名称进行数据选择，利用 loc 方法，举例：df.loc[["一" , "二"] , ["列1" , "列3"]]

#### 4.3.2 位置索引 + 位置索引

同时传入行、列索引的位置获取数据，利用 iloc 方法，举例：df.iloc[[0 , 2] , [ 1 , 3]]

#### 4.3.3 布尔索引 + 普通索引

通过布尔索引选择行，通过普通索引选择列，举例：`df[df["年龄"] < 200][["列1" , "列2"]]`

#### 4.3.4 切片索引 + 切片索引

同时传入行、列索引的位置区间进行数据选择，举例：df.iloc[0:3 , 2:4]

#### 4.3.5 切片索引 + 普通索引

行（列）用切片索引，列（行）用普通索引，这种交叉索引利用 ix 方法，举例：df.ix[0:3 , ["列1" , "列2"]]

## 五、Pandas 数据操作

### 5.1 数值替换

利用 replace() 方法

- 一对一替换：replace(A , B)，举例：df["年龄"].replace(240 , 33 , inplace[^1] = True) ,df.replace(np.NaN[^2] ,  0)
- 多对一替换：replace([A,B] , C)，举例：df.replace([240 , 260 , 280] , 33)
- 多对多替换：replace({ "A" : "a" , "B" : "b"})，举例：df.replace({240 : 32 , 260 : 33 , 280 : 34})

### 5.2 数值排序

利用 sort_values() 方法，参数 by 指明排序的列

#### 5.2.1 按照一列数值排序

参数 ascending 默认值为 True ,表示升序，可以省略

举例：df.sort_values(by = ["列名"] , ascending = Flase)

#### 5.2.2 按照有缺失值的列排序

参数 na_position 默认值为 last , 表示缺失值显示在最后，可以省略

举例：df.sort_values(by = ["列名"] , na_position = "first")

#### 5.2.3 按照多列数值排序

参数值以列表的形式指明要排序的多列列名及每列的排序方式

举例：df.sort_values(by = ["列名1" , "列名2"] , ascending = [ True , False])

### 5.3 数值排名

数值排名会新增一列用来存放数据的排名，排名从 1 开始。

利用 rank() 方法

- 参数 ascending 默认值为 True ,表示升序，可以省略
- 参数 method 用来指明排列值有重复值时的处理情况，参数值及说明如下：

| method参数值 | 说明                                           |
| ------------ | ---------------------------------------------- |
| average      | 当待排名的数值有重复值时，返回重复值的平均排名 |
| first        | 按值在所有的待排列数据中出现的先后顺序排名     |
| min          | 当待排名的数值有重复值时，返回重复值的最佳排名 |
| max          | 与min相反，取重复值对应的最大排名              |

举例：df["列名1"] , rank(method = "max")

### 5.4 数值删除

利用 drop() 方法，对数据表中一些无用的数据进行删除操作。

#### 5.4.1 删除列

- 直接传入待删除的列名，参数 axis = 1,表示删除列，举例：df.drop(["列1" , "列2"] , axis = 1)
- 直接传入待删除列的位置，参数 axis = 1,表示删除列，举例：df.drop(df.columns[ [4 ,5] ] , axis = 1)
- 将待删除列名以列表的形式传给 columns 参数，举例：df.drop(columns = [ "列1" , "列2" ])

#### 5.4.2 删除行

- 直接传入待删除的行索引名，参数 axis = 0,表示删除行，举例：df.drop(["行1" , "行2"] , axis = 0)
- 直接传入待删除的行号，参数 axis = 0,表示删除行，举例：df.drop(df.index[[0 , 1]] , axis = 0)
- 将待删除行名以列表的形式传给 index 参数，举例：df.drop( index = [ "行1" , "行2"])

#### 5.4.3 删除特定行

删除满足某个条件的行，我们直接把相反条件的数据筛选出来，作为新的数据源。

### 5.5 数值计数

计算某个值在一系列数值中出现的次数。

利用 value_counts() 方法，参数 normalize = True ,表示显示不同值出现的占比，参数 sort = False ,表示不按计数值降序排列。

举例：df["行1"].value_counts(normalize = True , sort = False)

### 5.6 唯一值获取

获取某一列重复值。

- 参考 【Pandas 数据预处理-重复值 】章节
- 利用 unique() 方法，举例：df["列1"] . unique()

### 5.7 数值查找

查看数据表中的数据是否包含某个值或某些值。

利用 isin() 方法，包含返回True，否则返回False，举例：某列 df["列1"].isin([ 31 , 21])  或  全表 df.isin([ " A1" ,"A2"])

### 5.8 区间切分

将一系列数值分成若干份。

- 指明切分区间，利用 cut() 方法，参数 bins 指明切分区间，结果是左开右闭区间，举例：pd.cut(df["列2"] , bins = [ 0 , 3 , 6 , 10])
- 指明切分个数，利用 qcut() 方法，每个组里的数据个数尽可能相等，举例：pd.qcut(df["列1"] , 3)

### 5.9 插入新的行或列

- 插入行：





[^1]: inplace参数：是否改变原始表数据
[^2]: np.NaN:对缺失值的一种表示方法
