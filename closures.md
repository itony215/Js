---
description: >-
  Closure is a function using variables defined in outer function that has
  returned
---

# closures

## Example

```javascript
/* high-order function */
function createShift(i) {
    return function(j) { // closure
        return i + j;
    }
}

var shift = createShift(100);
shift(3) // 103
```

If it accesses data outside, those data are kept in memory \(after outer function returns\)

## Bad Closures

```javascript
var trs = document.querySelectorAll('tr');
for(var i = 0; i < trs.length; i++) {
    var tr = trs[i];
    tr.onmouseover = function() {
        tr.classList.add('row-over');
    };
}
```

####  All handlers add class to the same last

#### Fix

```javascript
var trs = document.querySelectorAll('tr');
for(var i = 0; i < trs.length; i++) {
    var tr = trs[i];
    tr.onmouseover = function() {
        this.classList.add('row-over');
    };
}
```

[https://codepen.io/itony215/pen/qPrpyP](https://codepen.io/itony215/pen/qPrpyP)

