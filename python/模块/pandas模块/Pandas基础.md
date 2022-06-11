# Pandas 基础

## 一、Pandas 数据结构

### 1.1 Series 数据结构

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

  字典 key 值相当于列索引

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

