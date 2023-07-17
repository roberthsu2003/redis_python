## hash的使用

```
import redis
import os

user_connection = redis.Redis(host='localhost', port=6379, password=os.environ['REDIS_HOST_PASSWORD'], decode_responses=True)

user_connection.ping()

```

### [hash 官網操作](https://redis.io/docs/data-types/hashes/)

```
#選擇資料庫
user_connection.select(1)

結果:===============
true
```

```
#加入hash
user_connection.hset('myhash','field1',"foo")
結果 :====================
1
```

```
#加入hash欄位field2
user_connection.hset('myhash','field2',"world")
結果 :====================
1
```

```
#取出hash_fiel_value
hash_value = user_connection.hget('myhash','field1')
hash_value

結果:==============================
'foo'
```

```
#取出所有的keys
user_connection.keys("*")
結果:==============
['myhash']

```

```
#取出keys(pattern)
user_connection.keys("myhash*")
結果:==============
['myhash']
```

```
#取出所有hash的key內的欄位
user_connection.hkeys("myhash")
結果:=====================
['field1', 'field2']
```

```
#取出所有hash資料
user_connection.hgetall("myhash")
結果:=======================
{'field1': 'foo', 'field2': 'world'}
```

```
#取出hash沒有field的結果
# Get none-value
hashVal = user_connection.hget("myhash","field3")
print("沒有field3的欄位:",hashVal)
結果:==========================
沒有field3的欄位: None
```

```
# hexists:檢查keyname內的field是否存在
keyList= ['field1','field2','field3']
for keyname in keyList:
    hexists = user_connection.hexists('myhash',keyname)
    if hexists:
        print(keyname, "存在")
    else:
        print(keyname, "不存在")
        
        
結果:=================
field1 存在
field2 存在
field3 不存在
```

```
# hincrby: 加總hash內欄位的值
# 預設值是 1,
user_connection.hset('myhash','field3',20)
user_connection.hincrby('myhash','field3',10)
print("myhash,field3:",user_connection.hget('myhash','field3'))
結果:=======================
myhash,field3: 30
```

```
#hkeys: 取出hash內所有field的名稱
kL = user_connection.hkeys('myhash')
print("keyname myhash內所有的欄位",kL)
結果:======================
keyname myhash內所有的欄位 ['field1', 'field2', 'field3']
```

```
#hlen: Return the number of elements in hash ``name``
lenHash =user_connection.hlen('myhash')
print("All hash length:",lenHash)
結果:===============================
All hash length: 3
```

```
#hmget: Returns a list of values ordered identically to ``keys``
#hmget(self, name, keys), keys should be python list data structure
val =user_connection.hmget('myhash',['field1','field2','field3','fieldx'])
print("Get all redis-hash value list:",val)

結果:================================
Get all redis-hash value list: ['foo', 'world', '30', None]
```

```
#hmset:  Sets each key in the ``mapping`` dict to its corresponding value in the hash ``name``
#hset:使用mapping
hmDict={'field':'foo','field1':'bar'}
hmKeys=hmDict.keys()
user_connection.hset('hash',mapping=hmDict)
val = user_connection.hmget('hash',hmKeys)
print("Get hset value:",val)

結果:=====================
Get hset value: ['foo', 'bar']
```

```
#hdel: Delete ``key`` from hash ``name``
user_connection.hdel('hash','field')
print("Get delete result:",user_connection.hget('hash','field'))

結果:====================
Get delete result: None
```

```
#hvals:  Return the list of values within hash ``name``
val = user_connection.hvals('myhash')
print("Get redis-hash values with HVALS",val)

結果:=====================
Get redis-hash values with HVALS ['foo', 'world', '30']
```

```
#沒有欄位名才動作
#hsetnx: Set ``key`` to ``value`` within hash ``name`` if ``key`` does not exist.
# Returns 1 if HSETNX created a field, otherwise 0.

r=user_connection.hsetnx('myhash','field',2)
print("Check hsetnx execute result:",r," Value:",user_connection.hget('myhash','field'))

結果:===============================
Check hsetnx execute result: 1  Value: 2
```

```
#hsetnx: Set ``key`` to ``value`` within hash ``name`` if ``key`` does not exist.
# Returns 1 if HSETNX created a field, otherwise 0.

r=user_connection.hsetnx('myhash','field',20)
print("Check hsetnx execute result:",r," Value:",user_connection.hget('myhash','field'))

結果:=======================
Check hsetnx execute result: 0  Value: 2
```

```
#Empty db
user_connection.flushdb()

結果:======================
True
```