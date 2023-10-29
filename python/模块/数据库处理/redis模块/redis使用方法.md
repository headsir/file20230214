参考：https://www.cnblogs.com/liqiangwei/p/14354374.html

## 安装模块

```
pip install redis
```

## python直连redis数据库

```python
import redis

# 直接连接redis
conn = redis.Redis(host='localhost', port=6379, password='foobared', encoding='utf-8')

# 设置键值：13253931266=“999”且超时时间为10s （值写入到redis时会自动转字符串）
conn.set("13253931266", 999, ex=10)

# 根据键获取值：如果存在取值（获取到的是字节类型）；不存在则返回None
value = conn.get('13253931266')
print(value)
```

## 使用redis连接池

redis-py使用connection pool来管理对一个redis server的所有连接，避免每次建立、释放连接的开销。默认，每个Redis实例都会维护一个自己的连接池。

可以直接建立一个连接池，然后作为参数Redis，这样就可以实现多个Redis实例共享一个连接池

```python
import redis    # 导入redis模块，通过python操作redis 也可以直接在redis主机的服务端操作缓存数据库

# host是redis主机，需要redis服务端和客户端都起着 redis默认端口是6379
# 加上decode_responses=True，写入的键值对中的value为str类型，不加这个参数写入的则为字节类型
pool = redis.ConnectionPool(host='localhost', port=6379,password='foobared' decode_responses=True)
r = redis.Redis(connection_pool=pool)

r.set('gender', 'male')     # key是"gender" value是"male" 将键值对存入redis缓存
print(r.get('gender'))      # gender 取出键male对应的值
```

