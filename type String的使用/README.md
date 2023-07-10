## type String的使用

```python
#設定full_name
user_connection.set("full_name", "john doe")

結果===========
true
```

```python
#檢查是否有key存在
user_connection.exists("full_name")
結果===========
1
```

```python
#取出key的值
user_connection.get("full_name")
結果:===============
'john doe'
```


```python
#覆蓋原來key的值
user_connection.set("full_name","徐國堂")
user_connection.get("full_name")
結果:=======================
'徐國堂'
```

```python
#設定有限定時間的key
#目前時間後100秒
user_connection.setex("important_key", 100, "important_value")
結果:====================
True
```

```python
#剩82秒
user_connection.ttl("important_key")
結果:====================
82
```

```python
#直接儲存dict
dict_data = {
    "employee_name": "Adam Adams",
    "employee_age": 30,
    "position": "Software Engineer",
}

user_connection.mset(dict_data)

結果:===============
True
```

```python
user_connection.mget("employee_name", "employee_age", "position", "non_existing")

結果:====================
user_connection.mget("employee_name", "employee_age", "position", "non_existing")
```

