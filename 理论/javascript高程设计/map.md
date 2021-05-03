### 要义盘点
- `创建映射`
- `使用自定义迭代器初始化映射`
- `set方法添加键值对`
- `size属性查询键值对数量`
- `get与has方法查询指定键的值`
- `delete与clear方法删除键值对`
- `键名为`NaN`或者键名为`+/-0`时可能会报错`
- `三种返回可迭代对象的情况`
#### 创建映射并初始化
`new Map()`:
```
        let m = new Map([
            ['key1', 'val1'],
            ['key2', 'val2'],
            ['key3', 'val3']
        ]);
```
#### 使用自定义迭代器初始化映射
```
        const m2 = new Map({
            [Symbol.iterator]: function* () {
                yield ['key1', 'val1'];
                yield ['key2', 'val2'];
                yield ['key3', 'val3'];
            }
        });
```

#### 添加键值对：使用set()方法
```
        let m = new Map().set('key1', 'val1').set('key2', 'val2');
        console.log(m.size); // 2
```


#### 查询键值对数量：size
```
        let m = new Map([
            ['key1', 'val1'],
            ['key2', 'val2'],
            ['key3', 'val3']
        ]);

        console.log(m.size); //3
```


#### 查询：get()与has()方法
```
        let m = new Map([
            ['key1', 'val1'],
            ['key2', 'val2'],
            ['key3', 'val3']
        ]);

        console.log(m.has('key1')); // true
        console.log(m.get('key2')); // val2
```

#### 删除：delete()与clear()方法
```
        let m = new Map([
            ['key1', 'val1'],
            ['key2', 'val2'],
            ['key3', 'val3']
        ]);

        m.delete('key2');
        console.log(m.size); // 2

        console.log(m.has('key2')); // false
        console.log(m.get('key2')); // undefined

        m.clear();
        console.log(m.size); // 0
```

#### 键名为`NaN`或者键名为`+/-0`时可能会报错
```
        let key1 = 0 / ""; // NaN
        let key2 = 0 / ""; // NaN
        console.log(key1 === key2); // false

        let m = new Map().set(key1, 'val1');
        console.log(m.get(key2)); // val1
```
```
        let key1 = +0; // NaN
        let key2 = -0; // NaN
        console.log(key1 === key2); // true

        let m = new Map().set(key1, 'val1');
        console.log(m.get(key2)); // val1
```

#### 返回可迭代对象：使用entries()方法
```
        let m = new Map().set('key1', 'val1').set('key2', 'val2').set('key3', 'val3');
        console.log(m.entries === m[Symbol.iterator]); //true
        for (let prop of m.entries()) {
            console.log(prop);
            console.log(Array.isArray(prop)); //true
        }
        // ['key1','val1']
        // ['key2','val2']
        // ['key3','val3']
```

#### 返回可迭代对象：使用数组扩展运算符
```
        let m = new Map().set('key1', 'val1').set('key2', 'val2').set('key3', 'val3');
        for (let prop of [...m]) {
            console.log(prop);
            console.log(Array.isArray(prop)); //true

        }
        // ['key1','val1']
        // ['key2','val2']
        // ['key3','val3']
```

#### 返回键或值迭代对象：使用keys()和values()方法
```
        let m = new Map().set('key1', 'val1').set('key2', 'val2').set('key3', 'val3');
        for (let prop of m.keys()) {
            console.log(prop);
        }
        // key1 
        // key2
        // key3
```
```
        let m = new Map().set('key1', 'val1').set('key2', 'val2').set('key3', 'val3');
        for (let prop of m.values()) {
            console.log(prop);
        }
        // val1 
        // val2
        // val3
```