[Toc]

# Array.prototype.reduce

# 示例一

- ```js
  let arr = [3, 5, 7, 9, 12, 28, 54]
  let i = 0
  arr.reduce((accu, cur, index, arr) => {
  	console.group(`-----${i++} start:-----`)
  	console.log('accu:' + accu)
  	console.log('cur:' + cur)
  	console.groupEnd()
  	return accu + cur
  })
  ```

- ```
  -----0 start:-----
    accu:3
    cur:5
  -----1 start:-----
    accu:8
    cur:7
  -----2 start:-----
    accu:15
    cur:9
  -----3 start:-----
    accu:24
    cur:12
  -----4 start:-----
    accu:36
    cur:28
  -----5 start:-----
    accu:64
    cur:54
  ```

# 示例二

- ```js
  let arr = [3, 5, 7, 9, 12, 28, 54]
  let j = 0
  arr.reduce((accu, cur, index, arr) => {
  	console.group(`-----${j++} start:-----`)
  	console.log('accu:' + accu)
  	console.log('cur:' + cur)
  	console.groupEnd()
  	return accu + cur
  }, 0)
  ```

- ```
  -----0 start:-----
    accu:0
    cur:3
  -----1 start:-----
    accu:3
    cur:5
  -----2 start:-----
    accu:8
    cur:7
  -----3 start:-----
    accu:15
    cur:9
  -----4 start:-----
    accu:24
    cur:12
  -----5 start:-----
    accu:36
    cur:28
  -----6 start:-----
    accu:64
    cur:54