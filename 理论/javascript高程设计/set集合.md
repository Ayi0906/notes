## 创建集合
- 使用`new`或`Set`构造函数
- 构造函数接收一个可迭代对象
```
        const s1 = new Set(["val1", 'val2', 'val3']);
```

## .size属性
```
        const s1 = new Set(["val1", 'val2', 'val3']);
        console.log(s1.size); //3
```

## 自定义迭代器初始化集合
```
        const s2 = new Set({
            [Symbol.iterator]: function* () {
                yield 'val1';
                yield 'val2';
                yield 'val3';
            }
        });
        console.log(s2.size); // 3
```

## add()增加值
```
        const s1 = new Set(["val1", 'val2', 'val3']);
        console.log(s1.size); // 3
        s1.add('val4');
        console.log(s1.size); // 4
```

## has()查询值
```
        const s1 = new Set(["val1", 'val2', 'val3']);
        console.log(s1.size); // 3
        s1.add('val4');
        console.log(s1.size); // 4
        console.log(s1.has('val4')); // true
```

## delete()删除值
```
        s1.delete('val3');
        console.log(s1.size); // 3
```

## clear()清空集合
```
        s1.clear();
        console.log(s1.size); // 0
```

## values()/keys()/[Symbol.iterator]()/entries()/forEach()获取迭代器
```
        console.log(s2.values === s2[Symbol.iterator]); // true
        console.log(s2.keys === s2[Symbol.iterator]); // true

        for (let value of s2.values()) {
            console.log(value);
        }
        // val1
        // val2
        // val3

        for (let value of s2[Symbol.iterator]()) {
            console.log(value);
        }
        // val1
        // val2
        // val3
        
        for (let value of s2.entries()) {
            console.log(value);
        }
        // Array(2)  ['val1','val1']
        // Array(2)  ['val2','val2']
        // Array(2)  ['val3','val3']

        s2.forEach((item, index, arr) => {
            console.log(item + '-->' + index);
        });
        // val1-->val1
        // val2-->val2
        // val3-->val3

        console.log([...s2]);
        // Array(3)  ['val1','val2','val3']
```