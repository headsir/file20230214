



[官方文档]https://www.pypandas.cn/docs/

[C语言中文网]http://c.biancheng.net/pandas/

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

```
pd.read_excel(io, sheet_name=0, header=0, names=None, index_col=None,
              usecols=None, squeeze=False,dtype=None, engine=None,
              converters=None, true_values=None, false_values=None,
              skiprows=None, nrows=None, na_values=None, parse_dates=False,
              date_parser=None, thousands=None, comment=None, skipfooter=0,
              convert_float=True, **kwds)
```

| 参数名称   | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| io         | 表示 Excel 文件的存储路径。                                  |
| sheet_name | 要读取的工作表名称。                                         |
| header     | 指定作为列名的行，默认0，即取第一行的值为列名；若数据不包含列名，则设定 header = None。若将其设置  为 header=2，则表示将前两行作为多重索引。 |
| names      | 一般适用于Excel缺少列名，或者需要重新定义列名的情况；names的长度必须等于Excel表格列的长度，否则会报错。 |
| index_col  | 用做行索引的列，可以是工作表的列名称，如 index_col = '列名'，也可以是整数或者列表。 |
| usecols    | int或list类型，默认为None，表示需要读取所有列。              |
| squeeze    | boolean，默认为False，如果解析的数据只包含一列，则返回一个Series。 |
| converters | 规定每一列的数据类型。                                       |
| skiprows   | 接受一个列表，表示跳过指定行数的数据，从头部第一行开始。     |
| nrows      | 需要读取的行数。                                             |
| skipfooter | 接受一个列表，省略指定行数的数据，从尾部最后一行开始。       |

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

  通过设置 encoding 参数指明文件编码格式，常见有 UTF-8、gbk

- 指定解析语言

  默认使用C语言作为解析语言，当文件路径路经或者文件名包含中文时，默认方式可能会报错。

  通过设置 engine 参数指明解析语言为 Python 可解决。

- 指明导入列数据类型

  通过设置 type 参数
  
  ```
  dtype={
      '网元ID': str,
      'cellId': str
  }
  ```
  
- 其它

  涉及行、列索引设置及指定导入某列，设定方法与导入 .xlsx 文件一致。
  
  **low_memory**
  
  ```
  low_memory : boolean, default True
  
  # 分块加载到内存，再低内存消耗中解析，但是可能出现类型混淆。
  # 确保类型不被混淆需要设置为False，或者使用dtype 参数指定类型。
  # 注意使用chunksize 或者iterator 参数分块读入会将整个文件读入到一个Dataframe，而忽略类型（只能在C解析器中有效）
  ```

#### 2.1.3 导入 .txt 文件

导入方法：read_table()

是将利用分隔符分开的文件导入的通用函数，还可以导入 .csv 文件，必须用sep指明分隔符。

#### 2.1.4 导入SQL文件

导入方法：read_read_sql()

利用 sqlalchemy 模块连接数据库

```
from sqlalchemy import create_engine
# 创建数据库连接,mysql用户名是root，密码是qazwsx，本地的数据库服务是localhost,数据库的名称试验库,数据库编码utf8
engine = create_engine('mysql+pymysql://root:qazwsx@localhost/试验库?charset=utf8')
```

利用 pymysql模块连接数据库

```
# 创建数据库连接,mysql用户名是root，密码是qazwsx，本地的数据库服务是localhost,数据库的名称试验库,数据库编码utf8
import pymysql
con = pymysql.connect(host=localhost, user=username, password=password, database=dbname, charset=‘utf8’, use_unicode=True)
```



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

- 单个值通过isna()/isnull()方法查看，缺失值返回 True

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

通过 dropna() 方法实现，默认删除含有缺失值的行，传入参数 how = "all" 只删除全为空值的行 , 参数 subset 选择列，参数 inplace 是否修改原表

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
df3["ENodeBID"] = df3["ENodeBID"].astype("string")

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
df.rename(columns = {"订单编号":"新订单编号","客户姓名":"新客户姓名"}, index = {1:"一",2:"二"}, inplace=True)

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

- 普通索引：通过传入==列名==选择数据，举例：df[["列1" , "列2"]],推荐使用：df.loc[:,["列1" , "列2"]]

- 位置索引：通过传入==具体位置==来选择数据，利用 iloc 方法，举例：df.iloc[: , [0,2]] ，逗号前是行，后是列

#### 4.1.2 选择连续的某几列

普通索引、位置索引都可以，推荐使用位置索引,位置索引中使用==位置区间==又称为切片索引。举例：df.iloc[: , 0:3]

#### 4.1.3 选择某一列某部分字符

- 正则选取：df[‘new_col’] = df[‘selected_col’].str.extract(‘正则表达式’, expand = True)

### 4.2 行选择

#### 4.2.1 选择某一行/某几行

- 普通索引：通过传入==行索引名==选择数据，利用 loc 方法，举例：df.loc[["行1" , "行2"]]
- 位置索引：通过传入具体==行序号==，利用 iloc 方法，举例：df.iloc[[0,2]]

#### 4.2.2 选择连续的某几行

切片索引：通过传入连续行的==位置区间==，利用 iloc 方法，举例：df.iloc[0:3]

#### 4.2.3 选择满足条件的行

布尔索引：通过传入一个或多个==判断条件==来选择数据，举例：df[(df["年龄"] < 200) & (df["列2"] < 102)]

- isin(列表)，包含在列表的行

  ```
  df=df[df["GCI"].isin(df_3["GCI"].tolist())]
  ```

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

行（列）用切片索引，列（行）用普通索引，这种交叉索引利用 ix 方法，举例：df.ix[0:3 , ["列1" , "列2"]]，

此方法已弃用

## 五、Pandas 数据操作

### 5.1 数值替换

利用 replace() 方法

- 一对一替换：replace(A , B)，举例：df["年龄"].replace(240 , 33 , inplace[^1] = True) ,df.replace(np.NaN[^2] ,  0)
- 多对一替换：replace([A,B] , C)，举例：df.replace([240 , 260 , 280] , 33)
- 多对多替换：replace({ "A" : "a" , "B" : "b"})，举例：df.replace({240 : 32 , 260 : 33 , 280 : 34})
- 某列部分字符替换：data_new["列名"]=data["列名"].str.replace("A","B")
- 正则表达式替换：data_new["列名"]=data["列名"].str.replace(A , B,regex=True)
- 根据一列值确定另一列值：df_new.loc[df_new["网元链路状态"]=="Broken","小区服务状态"]="断链"

### 5.2 数值排序

利用 sort_values() 方法，参数 by 指明排序的列

#### 5.2.1 按照一列数值排序

参数 ascending 默认值为 True ,表示升序，可以省略

举例：df.sort_values(by = ["列名"] , ascending = False)

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

- 插入行：利用纵轴拼接的方法变相插入行，拼接方法参考后面章节
- 插入列：
  - 利用 insert() 方法，举例：df.insert(插入位置 , 新列列名 , 插入列数据)
  - 索引方式插入列：举例：df["新列列名"] = [插入列数据]

### 5.10 行列互换

利用 .T 方法，举例：df.T

### 5.11 索引重塑

重塑：数据从表格型数据转换到树形数据的过程。

- 表格转树形：利用 stack() 方法 ,举例：df.stack
- 树形转表格：利用 unstack() 方法，举例：df.unstack()

### 5.12 长宽表转换

将比较长（很多行）的表转换为比较宽（很多列）的表。

宽表：

|      |  Company | Name | Sate2013 | Sate2014 | Sate2015 | Sate2016 |
| ---: | -------: | ---: | -------: | -------: | -------: | -------- |
|    0 |    Apple | 苹果 |     5000 |     5050 |     5050 | 5050     |
|    1 |   Google | 谷歌 |     3500 |     3800 |     3800 | 3800     |
|    2 | Facebook | 脸书 |     2300 |     2900 |     2900 | 2900     |

长表：

|      |  Company | Name |  level_2 | 0    |
| ---: | -------: | ---: | -------: | ---- |
|    0 |    Apple | 苹果 | Sate2013 | 5000 |
|    1 |    Apple | 苹果 | Sate2014 | 5050 |
|    2 |    Apple | 苹果 | Sate2015 | 5050 |
|    3 |    Apple | 苹果 | Sate2016 | 5050 |
|    4 |   Google | 谷歌 | Sate2013 | 3500 |
|    5 |   Google | 谷歌 | Sate2014 | 3800 |
|    6 |   Google | 谷歌 | Sate2015 | 3800 |
|    7 |   Google | 谷歌 | Sate2016 | 3800 |
|    8 | Facebook | 脸书 | Sate2013 | 2300 |
|    9 | Facebook | 脸书 | Sate2014 | 2900 |
|   10 | Facebook | 脸书 | Sate2015 | 2900 |
|   11 | Facebook | 脸书 | Sate2016 | 2900 |

#### 5.12.1 长表转换为宽表

常用的方法是透视表，参考后面章节

#### 5.12.2 宽表转换为长表

##### 1、利用 stack() 方法

> - 设置索引，举例：df.set_index(["Company" ,"Name"])
>
> |          |      | Sate2013 | Sate2014 | Sate2015 | Sate2016 |
> | -------: | ---: | -------: | -------: | -------: | -------: |
> |  Company | Name |          |          |          |          |
> |    Apple | 苹果 |     5000 |     5050 |     5050 |     5050 |
> |   Google | 谷歌 |     3500 |     3800 |     3800 |     3800 |
> | Facebook | 脸书 |     2300 |     2900 |     2900 |     2900 |
>
> - 连索引转换成行索引，举例：df.set_index(["Company" ,"Name"]).stack()
>
> ```
> Company   Name          
> Apple     苹果   Sate2013    5000
>                 Sate2014    5050
>                 Sate2015    5050
>                 Sate2016    5050
> Google    谷歌   Sate2013    3500
>                 Sate2014    3800
>                 Sate2015    3800
>                 Sate2016    3800
> Facebook  脸书   Sate2013    2300
>                 Sate2014    2900
>                 Sate2015    2900
>                 Sate2016    2900
> ```
>
> - 索引重置
>
> |      |  Company | Name |  level_2 |    0 |
> | ---: | -------: | ---: | -------: | ---: |
> |    0 |    Apple | 苹果 | Sate2013 | 5000 |
> |    1 |    Apple | 苹果 | Sate2014 | 5050 |
> |    2 |    Apple | 苹果 | Sate2015 | 5050 |
> |    3 |    Apple | 苹果 | Sate2016 | 5050 |
> |    4 |   Google | 谷歌 | Sate2013 | 3500 |
> |    5 |   Google | 谷歌 | Sate2014 | 3800 |
> |    6 |   Google | 谷歌 | Sate2015 | 3800 |
> |    7 |   Google | 谷歌 | Sate2016 | 3800 |
> |    8 | Facebook | 脸书 | Sate2013 | 2300 |
> |    9 | Facebook | 脸书 | Sate2014 | 2900 |
> |   10 | Facebook | 脸书 | Sate2015 | 2900 |
> |   11 | Facebook | 脸书 | Sate2016 | 2900 |

##### 2、利用 melt() 方法，推荐

举例：df.melt(id_vars = [不变的列] , var_name = "列转换行后的列名" , value_name = "新索引对应值的列名" )

```
df.melt(id_vars = ["Company" ,"Name"],var_name = "Year" , value_name = "Sale")
```

|      | Company  | Name |   Year   | Sale |
| :--: | :------: | :--: | :------: | :--: |
|  0   |  Apple   | 苹果 | Sate2013 | 5000 |
|  1   |  Google  | 谷歌 | Sate2013 | 3500 |
|  2   | Facebook | 脸书 | Sate2013 | 2300 |
|  3   |  Apple   | 苹果 | Sate2014 | 5050 |
|  4   |  Google  | 谷歌 | Sate2014 | 3800 |
|  5   | Facebook | 脸书 | Sate2014 | 2900 |
|  6   |  Apple   | 苹果 | Sate2015 | 5050 |
|  7   |  Google  | 谷歌 | Sate2015 | 3800 |
|  8   | Facebook | 脸书 | Sate2015 | 2900 |
|  9   |  Apple   | 苹果 | Sate2016 | 5050 |
|  10  |  Google  | 谷歌 | Sate2016 | 3800 |
|  11  | Facebook | 脸书 | Sate2016 | 2900 |

### 5.13 apply() 与 applymap() 函数

与匿名函数 lambda 结合使用

#### 5.13.1 apply() 函数

对某一行或列中的元素执行相同函数操作，举例：df[ "列名" ] .apply(lambda x:x+1)

举例1：

```
df["备注"] = df["工程状态"].apply(lambda x: "网管小区目标已删除" if pd.isna(x) else "")
```

举例2：

```
   def __traffic_fun(x):
        if x > 0:
            return "非零流量"
        else:
            return "零流量"

    df["是否零流量"] = df["gNB空口总业务流量（GB）"].apply(lambda x: __traffic_fun(x))
```



#### 5.13.2 applymap() 函数

对每一个元素执行相同的函数操作，举例：df.applymap(lamdba x:x+1)

### 5.14 设置数据格式

```mermaid
graph TB
	id["Pandas样式设置"]
        id-->id1["隐藏索引"]
        id-->id2["设置数据格式"]
        id-->id3["设置数据格式"]
        	id3-.->id3_1["数字格式"]
                id3_1.->id3_1-1["小数位数"]
                id3_1.->id3_1-2["千分位"]
                id3_1.->id3_1-3["百分位"]
                id3_1.->id3_1-4["金额符号"]
        	id3-.->id3_2["时间数据"]
        	id3-.->id3_3["NaN值"]
        id-->id4["颜色高亮设置"]
        	id4-.->id4_1["最大值"]
        	id4-.->id4_2["最小值"]
        	id4-.->id4_3["空值"]
        	id4-.->id4_4["区间范围"]
        	id4-.->id4_5["个性化设置"]
        id-->id5["色阶颜色设置"]
        	id5-.->id5_1["背景颜色设置"]
        	id5-.->id5_2["文本颜色设置"]
        id-->id6["数据条显示"]
        id-->id7["自定义函数使用"]
        	id7-.->id7_1["apply"]
        	id7-.->id7_2["applymap"]
        id-->id8["颜色设置范围选择"]
        	id8-.->id8_1["行范围设置"]
        	id8-.->id8_2["列范围设置"]
        	id8-.->id8_3["行和列范围设置"]
        id-->id9["共享样式"]
        id-->id10["导出样式到EXCEL"]	
```

#### 5.14.1 **隐藏索引**

用 `hide_index()` 方法可以选择隐藏索引，代码如下：

```javascript
df_consume.style.hide_index()
```

#### 5.14.2 隐藏列

用 `hide_columns()` 方法可以选择隐藏一列或者多列，代码如下：

```javascript
df_consume.style.hide_index().hide_columns(['性别','基金经理','上任日期','2021'])
```

#### 5.14.3 设置数据格式

在设置数据格式之前，需要注意下，所在列的数值的数据类型应该为数字格式，如果包含字符串、时间或者其他非数字格式，则会报错。

可以用 `DataFrame.dtypes` 属性来查看数据格式。

```javascript
df_consume.dtypes
```

数据格式主要包括字符串、数字和时间这三种常见的类型，此外，空值（NaN，NaT等）也是我们需要处理的数据类型之一。

- 对于==字符串==类型，一般不要进行格式设置；

- 对于==数字==类型，是格式设置用的最多的，包括设置小数的位数、千分位、百分数形式、金额类型等；

  ```
  data.apply(lambda x: format(x, '.2%'))
  ```

- 对于==时间==类型，经常会需要转换为字符串类型进行显示；

- 对于==空值==，可以通过 `na_rep` 参数来设置显示内容；

Pandas 中可以通过 `style.format()` 函数来对数据格式进行设置。

```javascript
format_dict = {'基金规模(亿)': '￥{0:.1f}', 
               '管理费': '{0:.2f}', 
               '托管费': '{0:.2f}', 
               '规模对应日期':lambda x: "{}".format(x.strftime('%Y%m%d')),
               '2018': '{0:.1%}', 
               '2019': '{0:.1%}', 
               '2020': '{0:.1%}', 
               '2021': '{0:.1%}'
                }

df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期','2021'])\
                .format(format_dict)
```

##### **空值设置**

使用 `na_rep` 设置空值的显示，一般可以用 `-`、`/`、`MISSING` 等来表示:

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期','2021'])\
                .format(format_dict,na_rep='-')
```

#### 5.14.4 颜色高亮设置

对于最大值、最小值、NaN等各类值的颜色高亮设置，pandas 已经有专门的函数来处理，配合 `axis` 参数可以对行或者列进行应用：

- highlight_max()
- highlight_min()
- highlight_null()
- highlight_between()

##### 最大值:highlight_max

通过 `highlight_max()`来高亮最大值，其中 `axis=0` 是按列进行统计：

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期',])\
                .format(format_dict)\
                .highlight_max(axis=0,subset=['2018','2019','2020'])
```

##### **最小值:highlight_min**

通过 `highlight_min()`来高亮最小值，其中 `axis=1` 是按行进行统计：

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期',])\
                .format(format_dict)\
                .highlight_min(axis=1,subset=['2018','2019','2020'])
```

##### NaN:**highlight_null**

通过 `highlight_null()`来高亮空值（NaN值）

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期',])\
                .format(format_dict)\
                .highlight_null()
```

##### 区间范围:**highlight_between**

`highlight_between()` 函数，对处于范围内的数据进行高亮显示。

`highlight_between()` 函数的使用参数如下：

```
Styler.highlight_between(subset=None, color='yellow', axis=0, left=None, right=None, inclusive='both', props=None)
```

`highlight_between()` 函数,对处于范围内的数据进行高亮显示，通过 `left` 和 `right` 参数来设置两边的范围。

需要注意下，`highlight_between()` 函数从 pandas 1.3.0版本开始才有，旧的版本可能不能使用哦。

下面示例中 对2018年至2020年的年度涨跌幅度 `-20%~+20%` 范围内的数据进行高亮标注.

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期',])\
                .format(format_dict)\
                .highlight_between(left=-0.2,right=0.2,subset=['2018','2019','2020'])
```

区间分别设置

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期',])\
                .format(format_dict)\
                .highlight_between(left=[-0.15,0,0],right=[0,0.2,0.4],subset=['2018','2019','2020'],axis=1)
```

##### **个性化设置**

`highlight_max()`、`highlight_min()`、`highlight_null()` 等函数的默认颜色设置，我们不一定满意，可以通过 `props` 参数来进行修改。

**字体颜色和背景颜色设置**

color 字体颜色

background-color 背景色

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期',])\
                .format(format_dict)\
                .highlight_min(axis=1,subset=['2018','2019','2020','2021'],props='color:black;background-color:#99ff66')\
                .highlight_max(axis=1,subset=['2018','2019','2020','2021'],props='color:black;background-color:#ee7621')\
                .highlight_null(props='color:white;background-color:darkblue')
```

**字体加粗以及字体颜色设置**

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期',])\
                .format(format_dict)\
                .highlight_between(left=-0.2,right=0.2,subset=['2018','2019','2020'],props='font-weight:bold;color:#ee7621')
```

#### 5.14.5 色阶颜色设置

##### **背景色阶颜色设置**

使用 `background_gradient()` 函数可以对背景颜色进行设置。

该函数的参数如下：

```
Styler.background_gradient(cmap='PuBu', low=0, high=0, axis=0, subset=None, text_color_threshold=0.408, vmin=None, vmax=None, gmap=None)
```

使用如下：

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期'])\
                .format(format_dict)\
                .background_gradient(cmap='Blues')
```

如果不对 `subset` 进行设置，`background_gradient` 函数将默认对所有数值类型的列进行背景颜色标注。

对 `subset` 进行设置后，可以选择特定的列或特定的范围进行背景颜色的设置。

```javascript
# 对基金规模以色阶颜色进行标注
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期'])\
                .format(format_dict)\
                .background_gradient(subset=['基金规模(亿)'],cmap='Blues')
```

此外，可以通过对 low 和 high 值的设置，可以来调节背景颜色的范围，low 和 high 分别是压缩 低端和高端的颜色范围，其数值范围一般是 0~1 ，各位可以调试下。

```javascript
# 对基金规模以色阶颜色进行标注
# 通过对 low 和 high 值的设置，可以来调节背景颜色的范围
# low 和 high 分别是压缩 低端和高端的颜色范围，其数值范围一般是 0~1 ，各位可以调试下
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期'])\
                .format(format_dict)\
                .background_gradient(subset=['基金规模(亿)'],cmap='Blues',low=0.3,high=0.9)
```

当数据范围比较大时，可以通过设置 vmin 和 vmax 来设置最小和最大的颜色的设置起始点。

比如下面，基金规模在20亿以下的，颜色最浅，规模70亿以上的，颜色最深，20~70亿之间的，颜色渐变。

```javascript
# 对基金规模以色阶颜色进行标注
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期'])\
                .format(format_dict)\
                .background_gradient(subset=['基金规模(亿)'],cmap='Blues',vmin=20,vmax=70)
```

通过 `gmap` 的设置，可以方便的按照某列的值，对行进行全部的背景设置

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期'])\
                .format(format_dict)\
                .background_gradient(cmap='Blues',gmap=df_consume['基金规模(亿)'])
```

`gmap` 还可以以矩阵的形式对数据进行样式设置，如下：

```javascript
df_gmap = df_consume.loc[:2,['基金名称','管理费','基金规模(亿)','2020']]

gmap = np.array([[1,2,3], [2,3,4], [3,4,5]])  # 3*3 矩阵，后面需要进行颜色设置的形状也需要是 3*3，需要保持一致
df_gmap.style.background_gradient(axis=None, gmap=gmap,
    cmap='Blues', subset=['管理费','基金规模(亿)','2020']
)
```

上面 gmap 是 `3*3 矩阵`，后面需要进行颜色设置的形状也需要是 `3*3`，需要保持一致。

需要注意的是 颜色设置是根据 `gmap`中的值来设置颜色深浅的，而不是根据 `DataFrame` 中的数值来的。

这个在某些特定的情况下可能会用到。

##### **文本色阶颜色设置**

类似于背景色阶颜色设置，文本也是可以进行颜色设置的。

使用 `text_gradient()` 函数可以实现这个功能，其参数如下：

```
Styler.text_gradient(cmap='PuBu', low=0, high=0, axis=0, subset=None, vmin=None, vmax=None, gmap=None)
```

`text_gradient()` 函数的用法跟 `background_gradient()` 函数的用法基本是一样的。

下面演示两个使用案例，其他的用法参考 `background_gradient()` 函数。

**某列的文本色阶显示**

```javascript
# 对基金规模以色阶颜色进行标注

df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期'])\
                .format(format_dict)\
                .text_gradient(subset=['基金规模(亿)'],cmap='RdYlGn')
```

**全部表格的文本色阶显示**

```javascript
# 通过 `gmap` 的设置，可以方便的按照某列的值，对行进行全部的文本颜色设置

df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期'])\
                .format(format_dict)\
                .text_gradient(cmap='RdYlGn',gmap=df_consume['基金规模(亿)'])
```

#### **5.14.6 数据条显示**

数据条的显示方式，可以同时在数据表格里对数据进行可视化显示，这个功能咱们在 Excel 里也是经常用到的。

在 pandas 中，可以使用 `DataFrame.style.bar()` 函数来实现这个功能，其参数如下：

```
Styler.bar(subset=None, axis=0, color='#d65f5f', width=100, align='left', vmin=None, vmax=None)
```

示例代码如下：

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期'])\
                .format(format_dict)\
                .bar(subset=['基金规模(亿)','2018','2021'],color=['#99ff66','#ee7621'])
```

##### **设置对其方式**

上面这个可视化效果，对于正负数值的区别，看起来总是有点别扭。

可以通过设置 `aligh` 参数的值来控制显示方式：

- left: 最小值从单元格的左侧开始。
- zero: 零值位于单元格的中心。
- mid: 单元格的中心在（max-min）/ 2，或者如果值全为负（正），则零对齐于单元格的右（左）。

将显示设置为 `mid` 后，符合大部分人的视觉审美观，代码如下：

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期'])\
                .format(format_dict)\
                .bar(subset=['基金规模(亿)','2018','2021'],color=['#99ff66','#ee7621'],align='mid')
```

关于颜色设置，`color=['#99ff66','#ee7621']`， `color`可以设置为单个颜色，所有的数据只显示同一个颜色，也可以设置为包含两个元素的list或tuple形式，左边的颜色标注负数值，右边的颜色标注正数值。

#### **5.14.7 自定义函数的使用**

通过 `apply` 和 `applymap` 函数，用户可以使用自定义函数来进行样式设置。

其中：

- `apply` 通过axis参数，每一次将一列或一行或整个表传递到DataFrame中。对于按列使用 axis=0, 按行使用 axis=1, 整个表使用 axis=None。
- `applymap` 作用于范围内的每个元素。

##### **apply**

先自定义了函数`max_value()`，用来找到符合条件的最大值，`apply` 使用的示例代码如下：

**按列设置样式**

```javascript
def max_value(x, color='red'):
    return np.where(x == np.nanmax(x.to_numpy()), f"color: {color};background-color:yellow", None)

# axis =0 ，按列设置样式
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期',])\
                .format(format_dict)\
                .apply(max_value,axis=0,subset=['2018','2019','2020','2021'])
```

**按行设置样式**

```javascript
# axis =1 ，按行设置样式
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期',])\
                .format(format_dict)\
                .apply(max_value,axis=1,subset=['2018','2019','2020','2021'])
```

**按整个表格设置样式**

按整个表格设置样式时，需要注意的是，整个表格的数据类型需要是一样的，不然会报错。

示例代码如下：

```javascript
# axis = None ，按整个表格设置样式
# 注意，整个表格的数据类型需要是一样的，不然会报错

df_consume_1 = df_consume[['2018','2019','2020','2021']]
# df_consume_1
df_consume_1.style.hide_index().apply(max_value,axis=None)
```

##### **applymap**

继续上面的数据表格，我们来自定义一个函数，对于基金的年度涨跌幅情况，年度上涨以橙色背景标注，下跌以绿色背景标注，NaN值以灰色背景标注。

由于 `applymap` 是作用于每个元素的，因此该函数不需要 `axis` 这个参数来进行设置，示例代码如下：

```javascript
def color_returns(val):
    if val >=0:
        color = '#EE7621'  # light red
    elif val <0:
        color =  '#99ff66' # light green  '#99ff66'
    else:
        color = '#FFFAFA'  # ligth gray
    return f'background-color: {color}'

format_dict = {'基金规模(亿)': '￥{0:.1f}', 
               '管理费': '{0:.2f}', 
               '托管费': '{0:.2f}', 
               '规模对应日期':lambda x: "{}".format(x.strftime('%Y%m%d')),
               '2018': '{0:.1%}', 
               '2019': '{0:.1%}', 
               '2020': '{0:.1%}', 
               '2021': '{0:.1%}'
                }

df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期',])\
                .format(format_dict)\
                .applymap(color_returns,subset=['2018','2019','2020','2021'])
```

#### **5.14.8 颜色设置范围选择**

在使用 `Style` 中的函数对表格数据进行样式设置时，对于有 `subset` 参数的函数，可以通过设置 行和列的范围来控制需要进行样式设置的区域。

##### **对行(row)进行范围设置**

```javascript
df_consume_1.style.applymap(color_returns,subset=pd.IndexSlice[1:5,])
```

##### **对列(column)进行范围设置**

```javascript
df_consume_1.style.applymap(color_returns,subset=['2019','2020'])
```

##### **对行和列同时进行范围设置**

```javascript
df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期',])\
                .format(format_dict)\
                .applymap(color_returns,subset=pd.IndexSlice[1:5,['2018','2019','2020']])
```

#### **5.14.9 共享样式**

对于pandas 中样式设置后的共享复用，目前支持通过 `Styler.export()` 导出样式，然后通过 `Styler.use()` 来使用导出的样式。

不过经过阳哥的测试，简单的样式导出与使用是可以的。但稍微复杂一些的情况，目前的pandas版本是不太好用的。

##### **简单样式**

示例如下，先保存当前样式：

```javascript
df_consume_1 = df_consume[['2018','2019','2020','2021']]
# df_consume_1
style1 = df_consume_1.style.hide_index()\
                .highlight_min(axis=1,subset=['2018','2019','2020','2021'],props='color:black;background-color:#99ff66')\
                .highlight_max(axis=1,subset=['2018','2019','2020','2021'],props='color:black;background-color:#ee7621')\
                .highlight_null(props='color:white;background-color:darkblue')
style1
```

使用保存的样式：

```javascript
df_fund_1 = df_fund[['2018','2019','2020','2021']]

df_fund_1.style.use(style1.export())
```

##### **复杂样式**

当样式设置较多时，比如同时隐藏索引、隐藏列、设置数据格式、高亮特定值等，这个时候有些操作在导出后使用时并没有效果。

测试如下，先保存样式：

```javascript
style3 = df_consume.style.hide_index()\
                .hide_columns(['性别','基金经理','上任日期',])\
                .format(format_dict)\
                .highlight_min(axis=1,subset=['2018','2019','2020','2021'],props='color:black;background-color:#99ff66')\
                .highlight_max(axis=1,subset=['2018','2019','2020','2021'],props='color:black;background-color:#ee7621')\
                .highlight_null(props='color:white;background-color:darkblue')
style3
```

使用保存的样式：

```javascript
df_fund.style.use(style3.export())
```

从上面来看，我们希望的样式效果，并没有很好的实现。

所以，针对较为复杂的样式，还是乖乖的复制代码使用吧

#### **5.14.10 导出样式到Excel**

导出样式到 Excel 中，这个功能还是比较实用的。

DataFrames 使用 `OpenPyXL` 或`XlsxWriter` 引擎可以将样式导出到 Excel 工作表。

不过，这个功能目前也还是处于不断完善过程中，估计有时候有些内容会没有效果。

大家可以在使用过程中来发现其中的一些问题。

来看一个案例：

```javascript
df_consume.style.hide_index().hide_columns(['性别','基金经理','上任日期',]
 ).format(format_dict).highlight_min(axis=1,subset='2018','2019','2020','2021'],props='color:black;background-color:#99ff66'
     ).highlight_max(axis=1,subset=['2018','2019','2020','2021'],props='color:black;background-color:#ee7621').highlight_null(props='color:white;background-color:darkblue').to_excel('style_export.xlsx',engine = 'openpyxl')
```

可以看出，跟共享样式里有些相同的问题，比如隐藏索引、隐藏列、设置数据格式等效果并没有实现。

### 5.15 数据筛选

#### 查询函数`query`

query()的第二个参数 inplace 默认 false

- 单一条件

  df.query(条件)	df.query("Quantity == 95")

- 在多个条件
  - and：回在满足两个条件的所有记录	df.query("Quantity == 95 and UnitPrice == 182")
  - or（|）：返回满足任意条件的所有记录     df.query("Quantity == 95 or UnitPrice == 182")
  - not：否定条件   df.query("not(Quantity == 95)")

- 日期过滤

  数据类型`dateTime64 [ns]`

  - 字符串转化日期：pd.to_datetime(df["OrderDate"], format="%Y-%m-%d")

    ```
    # 获得八月份的所有记录
    df.query("OrderDate.dt.month == 8")
    
    # 提取2021年8月订购日为15或以上的所有订单
    df.query("OrderDate.dt.month == 8 and OrderDate.dt.year == 2021 and OrderDate.dt.day >=15")
    
    # 直接传递一个符合日期格式的字符串，它会自动的转换并且比较，日期格式必须为 XXXX-XX-XX
    df.query("OrderDate >= '2021-08-15' and OrderDate <= '2021-08-31'")
    ```

笔记：

@变量 、 notna() 非空、isna()空值

```
df = df_01.query("日期>=@date1  and 日期<=@date2  and 客服口径 == 1 and 业务地市.notna()")
```

### 5.16 获取最大值索引

```
# 获取最大值和最小值的索引
idxmax = df['market_cap'].idxmax()
idxmin = df['market_cap'].idxmin()
```

### 5.17 获取每列的最大值

df.max()

### 5.18 按照固定行数分割文件

```
df = pd.DataFrame([
    [1, 2, 3, 4],
    [5, 6, 7, 8],
    [9, 10, 11, 12],
    [13, 14, 15, 16],
    # [17, 18, 19, 20]
], columns=["一", "二", "三", "四"]
)
# 获取总行数
数据总行数 = df.shape[0]
数据分割行数 = 2
数据分割个数, 最后一个数据行数 = 数据总行数 // 数据分割行数, 数据总行数 % 数据分割行数
for i in range(数据分割个数):
    df1 = df.iloc[i * 数据分割行数:i * 数据分割行数 + 数据分割行数]
    print(df1)
if 最后一个数据行数 != 0:
    d = df.iloc[数据总行数 - 最后一个数据行数:数据总行数]
    print(d)
```

### 5.19 行、列分别求和

```
# 行求和
df["汇总"] = df.sum(axis=1)
# 列求和
df.loc["合计"] = df.sum(axis=0)
```

### 5.20 数据转换

#### 转换成字典：to_dict

```python
df = pd.DataFrame({'col1': [1, 2],'col2': [0.5, 0.75]},index=['row1', 'row2'])
```

![image-20230717115124421](imge/Pandas基础.assets/image-20230717115124421.png)

```python
# 'dict', 'list', 'series', 'split', 'tight', 'records', 'index'
print(f"{'dict':=^100}")
print(df.to_dict("dict"))
...
```

```python
================================================dict=====================================
{'col1': {'row1': 1, 'row2': 2}, 'col2': {'row1': 0.5, 'row2': 0.75}}
================================================list=====================================
{'col1': [1, 2], 'col2': [0.5, 0.75]}
===============================================series====================================
{'col1': row1    1
row2    2
Name: col1, dtype: int64, 'col2': row1    0.50
row2    0.75
Name: col2, dtype: float64}
===============================================split=====================================
{'index': ['row1', 'row2'], 'columns': ['col1', 'col2'], 'data': [[1, 0.5], [2, 0.75]]}
===============================================tight=====================================
{'index': ['row1', 'row2'], 'columns': ['col1', 'col2'], 'data': [[1, 0.5], [2, 0.75]], 'index_names': [None], 'column_names': [None]}
==============================================records====================================
[{'col1': 1, 'col2': 0.5}, {'col1': 2, 'col2': 0.75}]
===============================================index=====================================
{'row1': {'col1': 1, 'col2': 0.5}, 'row2': {'col1': 2, 'col2': 0.75}}
```





## 六、Pandas 数据分组

利用 groupby() 方法，数据类型为 DataFrameGroupBy

### 6.1 分组键是列名

- 单列分组：df.groupby("列名").count()，举例：`df1.groupby("Year").sum()`
- 多列分组：df.groupby(["列名1" ,"列名2"  ]).count()，举例：`df1.groupby(["Name" ,"Year"]).sum()`

### 6.2 分组键是 Series

- 单列分组：df.groupby(df["列名"]).count()，举例：`df.groupby(df["Year"]).sum()`
- 多列分组：df.groupby([df["列名1"] ,df["列名2"]  ]).count()，举例：`df.groupby([df["Name"] ,df["Year"]]).sum()`

### 6.3 aggregate 方法

#### 6.3.1 同时多种汇总方式

df.groupby("列名").aggregate( [ "count" , "sum"])

#### 6.3.2 不同列不同汇总方式

df.groupby("列名").aggregate( {"列3" : "count" , "列5" : "sum"})

### 6.4 对分组后结果重置索引

将 DataFrameGroupBy 类型转换为 DataFrame 形式，利用重置索引 reset_index() 方法

### 6.5 分组后表头获取

```
# 获取表头
df_Nc_group_columns = []
for i in df.columns:
    for j in i:
        if j != "" and j != '流量(GB)':
        	df_Nc_group_columns.append(j)
df.columns = df_Nc_group_columns
```

### 6.6 笔记

- 参数说明

  ```
  DataFrame.sum(axis = None,skipna = None,level = None,numeric_only = None,min_count = 0,**kwargs)
  
  参数说明:
      axis：axis = 1表示行，axis = 0表示列，默认为None（无）
      skipna：布尔型，表示计算结果是否排除NaN/Null值，默认值为None
      level：表示索引层级，默认为None
      numeric_only：仅数字，布尔型，默认值为None
      min_count：表示执行操作所需的数目，整型，默认为0
      **kwargs：要传递给函数的附加关键字参数。
      返回值：返回Series对象或DataFrame对象。行或列求和数据
  ```

- 分组筛选最大值行

  ```
  import pandas as pd
  data = {'year':[2016,2016,2017,2017,2017,2018,2018],
          'num':[2,5,4,7,8,90,78],
          'name':['a','b','c','d','e','f','g']}
  df = pd.DataFrame(data)
  #  筛选最大值所在的行
  df_groupby = df[['year','num']].groupby(by='year',as_index=False).max()
  #  对原始数据合并
  df_merge = pd.merge(df_groupby,df,on=['year','num'],how='left')
  ```

  

## 七、Pandas 数据透视

利用 pivot_table() 方法，pd.pivot_table(*data*, *values=None*, *index=None*, *columns=None*, *aggfunc='mean'*, *fill_value=None*, *margins=False*, *dropna=True*, *margins_name='All'*)，参数如下：

| 参数         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| data         | 必选参数，设定需要操作的  DataFrame                          |
| values       | 可选参数，对应 Excel 中值的那个框，用于指定汇总计算字段      |
| index        | 必选参数，对应 Excel 中行的那个框，用于指定行字段，作为结果DataFrame的行索引 |
| columns      | 必选参数，对应 Excel 中列的那个框，用于指定列字段，作为结果DataFrame的列索引 |
| aggfunc      | 用于指定汇总计算的方式，可以用字典对不同列不同方式汇总       |
| fill_value   | 用于对空值的填充， 默认不填充                                |
| dropna       | True 默认值  如果列的值都是NaN，将被删除；False时，被保留    |
| margins      | False 默认值 True时，会添加行/列的总计                       |
| margins_name | 'All'  默认值 margins = True 时，设定margins 行/列的名称     |

## 八、Pandas 多表拼接

### 8.1 横向拼接

横向将两个表依据公共列拼接在一起，水平拼接，利用 merge() 方法。

```
merge(left,right,how="inner",on=None,left_on=None,right_on=None,left_index=False,right_index=False,
    sort=False,suffixes=("_x", "_y"),copy=True,indicator=False,validate=None,)
```

| 参数        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| left        | 左表                                                         |
| right       | 右表                                                         |
| how         | 连接方式，inner、left、right、outer，默认为inner             |
| on          | 用于连接的列名称                                             |
| left_on     | 左表用于连接的列名                                           |
| right_on    | 右表用于连接的列名                                           |
| left_index  | 是否使用左表的行索引作为连接键，默认False                    |
| right_index | 是否使用右表的行索引作为连接键，默认False                    |
| sort        | 默认为False，将合并的数据进行排序                            |
| copy        | 默认为True，总是将数据复制到数据结构中，设置为False可以提高性能 |
| suffixes    | 存在相同列名时在列名后面添加的后缀，默认为(’_x’, ‘_y’)       |
| indicator   | 显示合并数据中数据来自哪个表                                 |
| validate    | 参数:validate : str, optional  <br>If specified, checks if merge is of specified type. <br> “one_to_one” or “1:1”: check if merge keys are unique in both left and right datasets.  <br/>“one_to_many” or “1:m”: check if merge keys are unique in left dataset. <br/>“many_to_one” or “m:1”: check if merge keys are unique in right dataset. <br/> “many_to_many” or “m:m”: allowed, but does not result in checks. |

#### 8.1.1 连接表

待连接的两个表根据同名列连接，默认取交集，举例：pd.merge(df1,df2,on = "连接键")

#### 8.1.2 连接键

##### 8.1.2.1 连接键是普通列

- 有公共列
  - 默认以公共列作为连接键，单列同名，举例：pd.merge(df1,df2)
  - 用 on 指定连接键，多列同名，举例：pd.merge(df1,df2,on = "连接键")
- 没有公共列
  - 分别指定左右连接键，参数 left_on 指明左表连接键列名，right_on 指明右表连接键列名，举例：pd.merge(df1,df2, left_on = "编号" , right_on = "学号")

##### 8.1.2.2 连接键是索引列

使用参数 left_index、right_index，举例：pd.merge(df1,df2, left_index = True , right_index = True)

##### 8.1.2.3 连接键是索引列 + 普通列

举例：pd.merge(df1,df2, left_index = True , right_on = "学号")

#### 8.1.3 连接方式

参数 how 指明连接方式。

##### 8.1.3.1 内连接（inner）

取两个表的公共部分(交集)，默认方式，举例：pd.merge(df1,df5,on = ["学号"] ,how = "inner")

|      | 名次 | 姓名 | 学号 | f_成绩 | e_成绩 |
| ---: | ---: | ---: | ---: | -----: | -----: |
|    0 |    1 | 小张 |  100 |    650 |    586 |
|    1 |    1 | 小张 |  100 |    650 |    602 |
|    2 |    2 | 小王 |  101 |    600 |    691 |
|    3 |    3 | 小李 |  102 |    578 |    645 |
|    4 |    3 | 小李 |  102 |    578 |    676 |

##### 8.1.3.2 左连接（left）

左表为基础，右表往上拼，空白信息用 NaN 填充，举例：pd.merge(df1,df5,on = ["学号"] ,how = "left")

|      | 名次 | 姓名 | 学号 | f_成绩 | e_成绩 |
| ---: | ---: | ---: | ---: | -----: | -----: |
|    0 |    1 | 小张 |  100 |    650 |  586.0 |
|    1 |    1 | 小张 |  100 |    650 |  602.0 |
|    2 |    2 | 小王 |  101 |    600 |  691.0 |
|    3 |    3 | 小李 |  102 |    578 |  645.0 |
|    4 |    3 | 小李 |  102 |    578 |  676.0 |
|    5 |    4 | 小赵 |  103 |    550 |    NaN |

##### 8.1.3.3 右连接（right）

右表为基础，左表往上拼，空白信息用 NaN 填充，举例：pd.merge(df1,df5,on = ["学号"] ,how = "right")

|      | 名次 | 姓名 | 学号 | f_成绩 | e_成绩 |
| ---: | ---: | ---: | ---: | -----: | -----: |
|    0 |    1 | 小张 |  100 |    650 |    586 |
|    1 |    2 | 小王 |  101 |    600 |    691 |
|    2 |    3 | 小李 |  102 |    578 |    645 |
|    3 |    1 | 小张 |  100 |    650 |    602 |
|    4 |    3 | 小李 |  102 |    578 |    676 |

##### 8.1.3.4 外连接（outer）

两个表合并（并集），空白信息用 NaN 填充，举例：pd.merge(df1,df5,on = ["学号"] ,how = "outer")

|      | 名次 | 姓名 | 学号 | f_成绩 | e_成绩 |
| ---: | ---: | ---: | ---: | -----: | -----: |
|    0 |    1 | 小张 |  100 |    650 |  586.0 |
|    1 |    1 | 小张 |  100 |    650 |  602.0 |
|    2 |    2 | 小王 |  101 |    600 |  691.0 |
|    3 |    3 | 小李 |  102 |    578 |  645.0 |
|    4 |    3 | 小李 |  102 |    578 |  676.0 |
|    5 |    4 | 小赵 |  103 |    550 |    NaN |

#### 8.1.4 重复列名处理

遇到列名重复，pd.merge() 方法会自动列名后面添加后缀 \_x、\_y、\_z,自动调整。

参数 suffixes 可以自定义重复列名，举例：pd.merge(df1,df5,on = ["学号"] ,how = "outer" , suffixes = [ " _L " , " _ R "])

### 8.2 纵向拼接

垂直拼接，利用 concat() 方法，举例：pd.concat([df1 ,df2] , ignore_index = True )

- 参数 ignore_index 索引设置，默认 False 保留原表索引

## 九、Pandas 数据导出

### 9.1 导出为 .xlsx 文件

利用 to_excel() 方法。

```
DataFrame.to_excel(excel_writer, sheet_name='Sheet1', na_rep='', float_format=None, columns=None, header=True, index=True, index_label=None, startrow=0, startcol=0, engine=None, merge_cells=True, encoding=None, inf_rep='inf', verbose=True, freeze_panes=None) 
```

| 参数名称     | 描述说明                                                     |
| ------------ | ------------------------------------------------------------ |
| excel_wirter | 文件路径或者 ExcelWrite 对象。                               |
| sheet_name   | 指定要写入数据的工作表名称。                                 |
| na_rep       | 缺失值的表示形式。                                           |
| float_format | 它是一个可选参数，用于格式化浮点数字符串。                   |
| columns      | 指要写入的列。                                               |
| header       | 写出每一列的名称，如果给出的是字符串列表，则表示列的别名。   |
| index        | 表示要写入的索引。                                           |
| index_label  | 引用索引列的列标签。如果未指定，并且 hearder 和 index 均为为 True，则使用索引名称。如果 DataFrame  使用 MultiIndex，则需要给出一个序列。 |
| startrow     | 初始写入的行位置，默认值0。表示引用左上角的行单元格来储存 DataFrame。 |
| startcol     | 初始写入的列位置，默认值0。表示引用左上角的列单元格来储存 DataFrame。 |
| engine       | 它是一个可选参数，用于指定要使用的引擎，可以是 openpyxl 或 xlsxwriter。 |

#### 9.1.1 设置文件导出路径

通过参数 excel_writer 设置文件导出路径，举例：df.to_excel(excel_writer = "文件保存路径")

#### 9.1.2 设置 Sheet 名称

通过参数 sheet_name 设置，举例：df.to_excel(excel_writer = "文件保存路径" , sheet_name = "表1")

#### 9.1.3 设置索引

通过参数 index 设置，默认 index = True ,默认从 0 开始自然索引，index =False 去掉索引。

#### 9.1.4 设置导出的列

通过参数 columns 指定要导出的列

#### 9.1.5 设置编码格式

通过参数 encoding 设置，一般选择” utf-8 “

#### 9.1.6 缺失值（Nan）处理

通过参数 na_rep 设置填充值

#### 9.1.7 无穷值（inf）处理

通过参数 inf_rep 设置填充值

### 9.2 导出为 .csv 文件

利用 to_csv() 方法。

#### 9.2.1 设置文件导出路径

通过参数 path_or_buf 设置文件导出路径，举例：df.to_excel(path_or_buf = "文件保存路径")

#### 9.2.2 设置分隔符号

通过参数 sep 指明分隔符号，常用分隔符：逗号、空格、制表符、分号等

#### 9.2.3 设置编码格式

通过参数 encoding 设置，默认为 UTF-8 ，中文会乱码，一般使用 utf-8-sig 、gbk

#### 9.2.3 在新建文件中添加多个Sheet

利用 ExcelWriter() 函数，参数 engine 设置所使用的包，举例：

```python
writer = pd.ExcelWriter(r"D:/桌面/4.xlsx",engine = "xlsxwriter")
df1.to_excel(writer,sheet_name = "df1",index = False)
df2.to_excel(writer,sheet_name = "df2",index = False)
df3.to_excel(writer,sheet_name = "df3",index = False)
writer.save()
```

#### 9.2.4 在已存在文件中添加Sheet

利用 openpyxl 模块

```
from openpyxl import load_workbook
path_file = r"D:/桌面/11.xlsx"
book = load_workbook(path_file)
writer = pd.ExcelWriter(path_file,engine="openpyxl")
# 在ExcelWriter的源代码中，它初始化空工作簿并删除所有工作表，
# writer.book = book将原来表里面的内容保存到writer中
writer.book = book
df1.to_excel(writer,sheet_name = "df8",index = False)
df2.to_excel(writer,sheet_name = "df7",index = False)
df3.to_excel(writer,sheet_name = "df6",index = False)
writer.save()

# 推荐使用：
book = load_workbook(path_file)  # 保存Excel表原始数据
with pd.ExcelWriter(path_file, engine="openpyxl") as writer:
    writer.book = book
    data.to_excel(excel_writer=writer, sheet_name=sheeetname, index=False)  # index=False,忽略行标签
    writer.save()
```

## 9.3 导出mysql数据库

语法：

```
DataFrame.to_sql(name, con, schema=None, if_exists='fail', index=True, index_label=None, chunksize=None, dtype=None)
```

### 1.name

该name为SQL表的名字，这是必须输入的参数，指定写入的表。

### 2.con:

con为python连接sql的sqlalchemy.engine，该参数也为必须输入的参数，可以使用SQLAlchemy数据库支持的连接引擎。该引擎可以引入：

```
from sqlalchemy import create_engine
import pymysql

#创建引擎
engine=create_engine('mysql+pymysql://用户名:密码@主机名/数据库?charset=utf8')
```

### 3.schema

​		指定架构（如果database flavor支持此功能）。如果没有，则使用默认架构。pandas中get_schema()方法是可以编写sql的写入框架的，没用传入的话就是普通的Dataframe读入形式。

### 4.if_exists

该参数为当存在表格时我们应该选择数据以怎样的方式写入到这张表格之中，共有三种方式选择：

- fail：当存在表格时候自动弹出错误ValueError
- replace：将原表里面的数据给替换掉
- append：将数据插入到原表的后面

###  5.index

默认为True等于存在第一行，列名为index的列，也可以先设定好行索引为哪一列防止插入的时报错

### 6.index_label

索引列的列标签。如果未给定任何值（默认值）且index为True，则使用索引名称。如果数据帧使用多索引，则应给出序列。也就是如果设定的index为True，可以给index设定列名。

### 7.chunksize

 一次将按此大小成批写入行。默认情况下，将一次写入所有行。可以设定一次写入的数量，避免一次写入数据量过大导致数据库崩溃。

### 8.dtype

指定列的数据类型。键是列名，值是sqlite3模式的SQLAlchemy类型或字符串。可以去 sqlalchemy 的官方文档查看所有的sql数据类型：

```
from sqlalchemy import create_engine
import sqlalchemy
import pymysql
import pandas as pd
import datetime
from sqlalchemy.types import INT,FLOAT,DATETIME,BIGINT
date_now=datetime.datetime.now()
data={'id':[888,889],
                       'code':[1003,1004],
                        'value':[2000,2001],
                        'time':[20220609,20220610],
                        'create_time':[date_now,date_now],
                        'update_time':[date_now,date_now],
                         'source':['python','python']}
insert_df=pd.DataFrame(data)
schema_sql={ 'id':INT,
             'code': INT,
             'value': FLOAT(20),
             'time': BIGINT,
             'create_time':  DATETIME(50),
             'update_time':  DATETIME(50)
                 }
insert_df.to_sql('create_two',engine,if_exists='replace',index=False,dtype=schema_sql)
```





## 十、日常笔记

### 10.1 求两列的差集[^3]

举例：np.setdiffid(arr1,arr2)

### 10.2 Python Excel库对比

| 模块      | WIN  | MAC  | PY2  | PY3  | .xls | .xlsx | .xlsm | 读   | 写   | 修改 |
| --------- | ---- | ---- | ---- | ---- | ---- | ----- | ----- | ---- | ---- | ---- |
| xlrd      | √    | √    | √    | √    | √    | √     |       | √    | ×    | ×    |
| xlwt      | √    | √    | √    | √    | √    | ×     |       | ×    | √    | √    |
| xlutils   | √    | √    | √    | √    | √    | ×     |       | ×    | ×    | √    |
| xlwings   | √    | √    | √    | √    | √    | √     |       | √    | √    | √    |
| openpyxl  | √    | ×    | √    | √    | ×    | √     |       | √    | √    | √    |
| xlswriter | √    | √    | √    | √    | ×    | √     |       |      | √    | ×    |
| win32com  | √    | √    | √    | √    | √    | √     | √     | √    | √    | √    |
| DataNitro | √    | ×    | √    | √    | √    | √     |       | —    | —    | —    |
| pandas    | √    | √    | √    | √    | √    | √     |       | √    | √    | ×    |

备注：空格位置不确定

### 10.3 pandas 行数、列数

pd.shape 返回元组(行数，列数)

### 10.4 计算两行数值的差

DataFrame.diff(periods=1, axis=0)

参数：

- periods：移动的幅度，int类型，默认值为1。
- axis：移动的方向，{0 or ‘index’, 1 or ‘columns’}，如果为0或者’index’，则上下移动，如果为1或者’columns’，则左右移动。

返回值

- diffed：DataFrame类型

### 10.5 列绝对值

```
diff_df["汇总/TB"].abs()
```



[^1]: inplace参数：是否改变原始表数据
[^2]: np.NaN:对缺失值的一种表示方法
[^3]: 差集：返回在 arr1 数组中存在，但是在 arr2 数组中不存在的元素

# 数据算法

## 一、冒泡排序

冒泡排序是一种简单的排序算法，它也是一种稳定排序算法。其实现原理是重复扫描待排序序列，并比较每一对相邻的元素，当该对元素顺序不正确时进行交换。一直重复这个过程，直到没有任何两个相邻元素可以交换，就表明完成了排序

