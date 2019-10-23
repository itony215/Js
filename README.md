---
description: Be Careful about this
---

# this

## Example

```javascript
var user = {
    name: 'Tony',
    greet: function() {
        console.log('Hi, I am ' + this.name);
    }
};
setInterval(user.greet, 1000);
```

this always binds to the “current owner” of that function 

1. window, when called as a function 

2. Object to which . is applied, when called as a method 

3. Creating object, when called as a constructor



