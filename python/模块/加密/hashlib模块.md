```python
import hashlib

# Using hashlib.sha3_256() method
gfg = hashlib.sha3_256()
gfg.update('123'.encode("utf-8"))
# 使用 hexdigest() 方法获取加密后的密码，以十六进制字符串的形式返回
print(gfg.hexdigest())
```

