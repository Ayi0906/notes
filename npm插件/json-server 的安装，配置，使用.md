[Toc]

# json-server 的安装，配置，使用

### 安装

`$ npm install -g json-server`

## 配置

### 创建一个json文件

- 以这里的文件名为 `data.json`，但是一般最好为`db.json`，原因是调用的时候不管你这个文件名是什么，都是通过`db`获取全部数据的

```json
{
    "user": [{
        "name": "user1",
        "age": 12,
        "jobs": 3
    }, {
        "name": "user2",
        "age": 15,
        "jobs": 2
    }, {
        "name": "user3",
        "age": 21,
        "jobs": 1
    }, {
        "name": "user4",
        "age": 23,
        "jobs": 2
    }],
    "jobs": [{
            "id": 1,
            "name": "教师"
        }, {
            "id": 2,
            "name": "司机"
        },
        {
            "id": 3,
            "name": "程序员"
        }
    ]
}
```

### 调用`json-server`命令

`$ json-server --watch --port 3003 data.json`

#### 输出以下内容为成功

```javascript
  \{^_^}/ hi!

  Loading data.json
  Done

  Resources
  http://localhost:3003/user      
  http://localhost:3003/jobs      

  Home
  http://localhost:3003
```

### 进入浏览器获取数据

`http://127.0.0.1:3003/db` ：获取全部数据

`http://127.0.0.1:3003/user`：获取user集合

`http://127.0.0.1:3003/jobs`：获取jobs集合

- 以上获取数据的方法均为GET

### 选择获取数据

#### 获取指定的某个属性

`http://127.0.0.1:3003/jobs?name=教师`：获取jobs集合里name属性为教师的对象（以数组包含）,

这种方式只支持对象下的第一个数组们的匹配，内部多重数组无法使用。

比如下面可以通过`http://127.0.0.1:3003/color`访问color，但不能使用`http://127.0.0.1:3003/color/blue`访问blue

```json
[
  {
    "id": 1,
    "name": "教师"
  }
]
```

```json
"color": [{
        "blue": [{
            "lightblue": "#009ee0"
        }]
    }]
```

#### 获取某个限定范围的数据

**「注」过滤字段，只针对数组数据。（毕竟只有数组需要过滤）**

_gte: 大于等于

_lte: 小于等于

_ne: 不等于

_like: 包含

// 获取jobs集合下 id大于等于2且小于等于4的数据

```http
http://127.0.0.1:3003/jobs?id_get=2&idlte=4
```

```json
[
  {
    "id": 1,
    "name": "教师"
  },
  {
    "id": 2,
    "name": "司机"
  },
  {
    "id": 3,
    "name": "程序员"
  }
]
```

### 通过id快捷访问

`http://127.0.0.1:3003/jobs/2`：对于有id属性的对象，可以直接访问

```json
{
  "id": 2,
  "name": "司机"
}
```

