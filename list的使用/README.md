## list的使用

```
import redis
import os

user_connection = redis.Redis(host='localhost', port=6379, password=os.environ['REDIS_HOST_PASSWORD'], decode_responses=True)

user_connection.ping()
```

```
res = user_connection.lpush("mylist", "two", "one")
print(res)

結果:============
2
```

```
res = user_connection.rpush("mylist", "three", "four")
print(res)

結果:=============
4
```

```
res = user_connection.lrange("mylist", 0, 2)
print(res)

結果:================
['one', 'two', 'three']
```

```
res = user_connection.lrange("mylist", 0, -1)
print(res)

結果:============================
['one', 'two', 'three', 'four']
```

```
res = user_connection.linsert("mylist", "after", "four", "five")
print(res)

結果:=========================
5
```

```
res = user_connection.lindex("mylist", 2)
print(res)

結果:=================
three
```

```
res = user_connection.ltrim("mylist", 1, 3)
print(res)

結果:======================
true
```

```
res = user_connection.lset("mylist", 1, "foo")
print(res)

結果:=====================
true
```

## 實際案例

```python
#加入50位學生,5個分數
import random
import redis
import os

user_connection = redis.Redis(host='localhost', port=6379, password=os.environ['REDIS_HOST_PASSWORD'], decode_responses=True)

user_connection.ping()

#檢查是否有學生分數
#如果沒有加入學生分數
if not user_connection.exists("name:1"):
    print("學生尚未加入")
    for i in range(50):
        key = f"name:{i}"
        scores = []
        for _ in range(5):
            scores.append(random.randint(50,100))
        user_connection.lpush(key,*scores)

#取出學生資料
for i in range(50):
        key = f"name:{i}"
        scores = user_connection.lrange(key,0,-1)
        print(key)
        print(scores)
        print("============")
        
#清除所有資料
#user_connection.flushdb()
```

