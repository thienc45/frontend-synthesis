# Các loại function hay dùng trong React

## Higher Order Function

### Callback function
**Callback Function (Hàm Callback)** là hàm được truyền như một đối số vào một hàm khác và được gọi khi hàm đó thực hiện một số thao tác nhất định. Callback thường được dùng trong các hàm xử lý bất đồng bộ, chẳng hạn như trong các hàm như `forEach`, `map`, `filter`, và `reduce`.



```js
const num = [2, 4, 6, 8]
const callback = (item, index) => {
  console.log('STT: ', index, 'la ', item)
}
num.forEach(callback)
```

### Currying

`findNumber` gọi là currying function vì nó return một function mới. Vậy nên chúng ta phải `()` 2 lần thì nó mới chạy hết code trong nó được.

```js
function findNumber(num) {
  return function (func) {
    const result = []
    for (let i = 0; i < num; i++) {
      if (func(i)) {
        result.push(i)
      }
    }
    return result
  }
}
findNumber(10)((number) => number % 2 === 1)
findNumber(20)((number) => number % 2 === 0)
findNumber(30)((number) => number % 3 === 2)
```

Cách viết ngắn gọn hơn với arrow function

```js
const findNumber = (num) => (func) => {
  const result = []
  for (let i = 0; i < num; i++) {
    if (func(i)) {
      result.push(i)
    }
  }
  return result
}
findNumber(10)((number) => number % 2 === 1)
findNumber(20)((number) => number % 2 === 0)
findNumber(30)((number) => number % 3 === 2)
```
