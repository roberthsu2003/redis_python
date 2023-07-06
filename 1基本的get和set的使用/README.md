## 基本的get和set的使用

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