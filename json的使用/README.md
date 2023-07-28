## json的使用
### [官方範例](https://developer.redis.com/howtos/redisjson/using-python/)

### [JSONpath](https://redis.io/docs/data-types/json/path/)

### JSONpath

- 使用redis cli

```json
#json樣本

{
   "inventory": {
      "headphones": [
         {
            "id": 12345,
            "name": "Noise-cancelling Bluetooth headphones",
            "description": "Wireless Bluetooth headphones with noise-cancelling technology",
            "wireless": true,
            "connection": "Bluetooth",
            "price": 99.98,
            "stock": 25,
            "free-shipping": false,
            "colors": ["black", "silver"]
         },
         {
            "id": 12346,
            "name": "Wireless earbuds",
            "description": "Wireless Bluetooth in-ear headphones",
            "wireless": true,
            "connection": "Bluetooth",
            "price": 64.99,
            "stock": 17,
            "free-shipping": false,
            "colors": ["black", "white"]
         },
         {
            "id": 12347,
            "name": "Mic headset",
            "description": "Headset with built-in microphone",
            "wireless": false,
            "connection": "USB",
            "price": 35.01,
            "stock": 28,
            "free-shipping": false
         }
      ],
      "keyboards": [
         {
            "id": 22345,
            "name": "Wireless keyboard",
            "description": "Wireless Bluetooth keyboard",
            "wireless": true,
            "connection": "Bluetooth",
            "price": 44.99,
            "stock": 23,
            "free-shipping": false,
            "colors": ["black", "silver"]
         },
         {
            "id": 22346,
            "name": "USB-C keyboard",
            "description": "Wired USB-C keyboard",
            "wireless": false,
            "connection": "USB-C",
            "price": 29.99,
            "stock": 30,
            "free-shipping": false
         }
      ]
   }
}
```

### 建立json

- redis CLI

```json
JSON.SET store $ '{"inventory":{"headphones":[{"id":12345,"name":"Noise-cancelling Bluetooth headphones","description":"Wireless Bluetooth headphones with noise-cancelling technology","wireless":true,"connection":"Bluetooth","price":99.98,"stock":25,"free-shipping":false,"colors":["black","silver"]},{"id":12346,"name":"Wireless earbuds","description":"Wireless Bluetooth in-ear headphones","wireless":true,"connection":"Bluetooth","price":64.99,"stock":17,"free-shipping":false,"colors":["black","white"]},{"id":12347,"name":"Mic headset","description":"Headset with built-in microphone","wireless":false,"connection":"USB","price":35.01,"stock":28,"free-shipping":false}],"keyboards":[{"id":22345,"name":"Wireless keyboard","description":"Wireless Bluetooth keyboard","wireless":true,"connection":"Bluetooth","price":44.99,"stock":23,"free-shipping":false,"colors":["black","silver"]},{"id":22346,"name":"USB-C keyboard","description":"Wired USB-C keyboard","wireless":false,"connection":"USB-C","price":29.99,"stock":30,"free-shipping":false}]}}'
```

### 操作json
- 使用*會傳出list

```
JSON.GET store $.inventory.*

結果:=====================
"[[{\"id\":12345,\"name\":\"Noise-cancelling Bluetooth headphones\",\"description\":\"Wireless Bluetooth headphones with noise-cancelling technology\",\"wireless\":true,\"connection\":\"Bluetooth\",\"price\":99.98,\"stock\":25,\"free-shipping\":false,\"colors\":[\"black\",\"silver\"]},{\"id\":12346,\"name\":\"Wireless earbuds\",\"description\":\"Wireless Bluetooth in-ear headphones\",\"wireless\":true,\"connection\":\"Bluetooth\",\"price\":64.99,\"stock\":17,\"free-shipping\":false,\"colors\":[\"black\",\"white\"]},{\"id\":12347,\"name\":\"Mic headset\",\"description\":\"Headset with built-in microphone\",\"wireless\":false,\"connection\":\"USB\",\"price\":35.01,\"stock\":28,\"free-shipping\":false}],[{\"id\":22345,\"name\":\"Wireless keyboard\",\"description\":\"Wireless Bluetooth keyboard\",\"wireless\":true,\"connection\":\"Bluetooth\",\"price\":44.99,\"stock\":23,\"free-shipping\":false,\"colors\":[\"black\",\"silver\"]},{\"id\":22346,\"name\":\"USB-C keyboard\",\"description\":\"Wired USB-C keyboard\",\"wireless\":false,\"connection\":\"USB-C\",\"price\":29.99,\"stock\":30,\"free-shipping\":false}]]"
```

```
JSON.GET store $.inventory.headphones[*].name

結果:========================
"[\"Noise-cancelling Bluetooth headphones\",\"Wireless earbuds\",\"Mic headset\"]"
```

-  .. recursive descent operator

```
JSON.GET store $..name

結果:===================

"[\"Noise-cancelling Bluetooth headphones\",\"Wireless earbuds\",\"Mic headset\",\"Wireless keyboard\",\"USB-C keyboard\"]"
```


```
JSON.GET store $..headphones[0:2].name

結果:=========================
"[\"Noise-cancelling Bluetooth headphones\",\"Wireless earbuds\"]"
```

- Filter expressions ?()

```
JSON.GET store $..headphones[?(@.price<70&&@.wireless==true)]

結果 :============================
"[{\"id\":12346,\"name\":\"Wireless earbuds\",\"description\":\"Wireless Bluetooth in-ear headphones\",\"wireless\":true,\"connection\":\"Bluetooth\",\"price\":64.99,\"stock\":17,\"free-shipping\":false,\"colors\":[\"black\",\"white\"]}]"
```

```
JSON.GET store '$.inventory.*[(@.connection=="Bluetooth")].name'

結果:===================================
"[\"Noise-cancelling Bluetooth headphones\",\"Wireless earbuds\",\"Wireless keyboard\"]"

```

### Update JSON examples

```
JSON.GET store $..headphones[0].price
結果:================================
"[99.98]"

JSON.SET store $..headphones[0].price 78.99
結果:====================================
"OK"

JSON.GET store $..headphones[0].price
結果 :==================================
"[78.99]"
```

```
JSON.SET store $.inventory.*[?(@.price>49)].free-shipping true
結果:============================
"OK"

JSON.GET store $.inventory.*[?(@.free-shipping==true)].name
結果:=======================================
"[\"Noise-cancelling Bluetooth headphones\",\"Wireless earbuds\"]"


```

### JSON.ARRAPPEND

```
JSON.GET store $..headphones[0].colors
結果:=======================
"[[\"black\",\"silver\"]]"

JSON.ARRAPPEND store $..headphones[0].colors '"pink"'
結果:===================================
1) "3"

JSON.GET store $..headphones[0].colors
結果:==================================
"[[\"black\",\"silver\",\"pink\"]]"
```

