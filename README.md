# redis_python
## redis_server的安裝
[參考官網](https://redis.io/docs/getting-started/installation/)

## python操控redis
[參考來源](https://redis.readthedocs.io/en/stable/examples.html)


## 連線redis,密碼儲存於環境變數內

```python
import redis
import os

user_connection = redis.Redis(host='localhost', port=6379, password=os.environ['REDIS_HOST_PASSWORD'], decode_responses=True)

user_connection.ping()
user_connection.ping()

結果================
True
```

## 1. [string的使用](./String的使用/)
## 2. [list的使用](./list的使用/)
## 3. [hash的使用](./hash的使用/)
## 4. [set的使用](./set的使用/)
## 5. [json的使用](./json的使用/)

