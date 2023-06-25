案例：

```
# Fernet算法是一种对称加密算法
from cryptography.fernet import Fernet


# 生成密钥
def generate_key():
    return Fernet.generate_key()


class Cipher:
    def __init__(self, secret_key):
        # isinstance 用来判断一个对象的变量类型,bytes字节
        if not isinstance(secret_key, bytes):
            secret_key = secret_key.encode()
        print(type(secret_key))
        self.cipher = Fernet(secret_key)

    # 加密
    def encrypt(self, data):
        # decode将其他编码方式转换成unicode编码方式,encode将unicode转换成其他编码方式
        encrypt_data = self.cipher.encrypt(data.encode("utf-8")).decode()
        return encrypt_data

    # 解密
    def decrypt(self, encrypt_data):
        decrypt_data = self.cipher.decrypt(encrypt_data.encode("utf-8")).decode()
        return decrypt_data


if __name__ == '__main__':
    # print(generate_key())
    sk = generate_key()
    print(sk)
    # sk = "Oe4PWwBs0oEKnYlmWUZcFehfHv2AWPTU8J7tH0un3YI="
    c = Cipher(sk)
    e = c.encrypt("asd")
    d = c.decrypt(e)
    print(e)
    print(d)
```

