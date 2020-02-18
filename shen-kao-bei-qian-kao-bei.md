# 深拷貝淺拷貝

简单的对象可以这样拷贝，`JSON.parse(JSON.stringify(obj))`

```javascript
function copyArr(arr){
        return arr.map((e)=>{
            if(typeof e === 'object'){
                return Object.assign({},e)
            }else{
                return e
            }
        })
    }
```



