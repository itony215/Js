# Sum of two dimensional array

二維陣列相加，常常會變成字串相連，如下

```javascript
const datasets = [
          [65, 50, 62, 25, 18, 60],
          [35, 50, 38, 75, 82, 40],
          [35, 50, 38, 75, 82, 40],
        ]
        
console.log(datasets.reduce((a,b)=>{return a+b}))
//65,50,62,25,18,6035,50,38,75,82,4035,50,38,75,82,40...
```

所以必須提出來一個一個加

```javascript
const sumArray = (datasets) => {

  const [first, ...other] = datasets
  let totalList = []
  first.map((item, index) => {
    for (let i = 0; i < other.length; i++) {
      item += other[i][index] || 0
    }
    totalList.push(item)
  })
  return totalList
  
}

console.log(sumArray(datasets))
//[ 135, 150, 138, 175, 182, 140 ]
```

